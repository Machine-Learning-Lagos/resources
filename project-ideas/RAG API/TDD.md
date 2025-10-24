# Technical Design Document: DocuQuery API (v1.0)

## Introduction

This document outlines the technical design for the **DocuQuery API**, a backend service that provides a Retrieval-Augmented Generation (RAG) capability. It is the technical "how-to" for implementing the features defined in the Product Requirements Document (PRD).

The system will ingest a corpus of Markdown (`.md`) documents, process them into a searchable vector store, and expose a secure API to answer natural language questions based **only** on the content of that corpus.

The architecture is broken into two primary, asynchronous flows:

1.  **Ingestion Flow:** An asynchronous, idempotent pipeline for adding/updating documents.
2.  **Query Flow:** A low-latency, synchronous API for asking questions.

## System Architecture

The system is a decoupled, microservices-oriented architecture designed for scalability and reliability.

### Ingestion Flow (Asynchronous)

1.  **Client (Admin)** `POST /v1/documents/upload` with a `.zip` file to the **API Service (FastAPI)**.
2.  The API Service validates the request, generates a `job_id`, and immediately uploads the raw `.zip` file to **Object Storage (S3/MinIO)**.
3.  The API Service enqueues a job in the **Task Queue (RabbitMQ)** with the `job_id` and the file's S3 path.
4.  A **Celery Worker (Ingestion Service)**, running as a separate process, polls the queue and dequeues the job.
5.  The Worker downloads the file from S3, unzips it, and processes each `.md` file:
      * **Extract:** Reads the text content.
      * **Chunk:** Splits the text into small, overlapping semantic chunks (e.g., 500 tokens).
      * **Embed:** Uses the `sentence-transformers` model to create a vector embedding for each chunk.
      * **Load:** Inserts the chunk text, vector, and metadata into the **Vector Database (PostgreSQL w/ pgvector)**. This is done idempotently based on the filename.
6.  The Worker updates the job status in the **Database** (e.g., in the `documents` table).

### Query Flow (Synchronous)

1.  **Client (Developer)** `POST /v1/query` with a `{"question": "..."}` to the **API Service (FastAPI)**.
2.  The API Service validates the request payload.
3.  The API Service uses the *same* `sentence-transformers` model to create an embedding of the user's question.
4.  The API Service executes a similarity search query against the **Vector Database** to retrieve the `top-k` (e.g., top 5) most relevant text chunks.
5.  The API Service constructs a prompt using a template, injecting the retrieved chunks (the "context") and the user's question. (e.g., *"Context: [...chunks...] \\n\\n Question: [...question...] \\n\\n Answer:"*).
6.  The API Service sends this prompt to the **External LLM API (e.g., OpenAI)**.
7.  The LLM API returns the synthesized answer.
8.  The API Service formats the final JSON response, including the answer and the source metadata from the retrieved chunks, and returns it to the client.

## Component Deep-Dive

  * **API Service (FastAPI):**
      * The main, stateless web service.
      * Handles all client-facing requests, authentication (API Key), and request validation (Pydantic).
      * Orchestrates the synchronous `query` flow.
      * Dispatches jobs for the asynchronous `ingestion` flow.
  * **Object Storage (MinIO / AWS S3):**
      * Durable storage for incoming raw `.zip` files.
      * Decouples the API from the compute-intensive workers. The API request finishes in milliseconds, even for a 1GB upload.
  * **Task Queue (Celery & RabbitMQ):**
      * Manages the queue of ingestion jobs.
      * Provides reliability: If an ingestion worker crashes, the job is retried automatically.
      * Allows scaling: We can add more Celery workers to increase ingestion throughput without touching the API service.
  * **Ingestion Worker (Celery Worker):**
      * A separate, containerized Python process.
      * Contains all the "heavy" logic: text parsing, chunking (e.g., using `langchain.text_splitter`), and embedding generation.
      * This component can be scaled independently, potentially on machines with more CPU/RAM.
  * **Vector Database (PostgreSQL + pgvector):**
      * The single source of truth for our queryable data.
      * `pgvector` is a Postgres extension that provides vector storage and efficient similarity search (e.g., HNSW, IVFFlat).
      * We choose this over a dedicated vector DB (like Milvus) for simplicity, as it's a mature, familiar RDBMS we can use for all our data (jobs, metadata, *and* vectors).
  * **Embedding Model (`sentence-transformers`):**
      * **Model:** `all-MiniLM-L6-v2` (384 dimensions).
      * **Reason:** Small, fast, and high-performing. It runs locally within the API service and the Ingestion Worker.
  * **Generative LLM (OpenAI API):**
      * **Model:** `gpt-4o-mini`.
      * **Reason:** Excellent balance of cost, speed, and reasoning. It's used as a "pay-as-you-go" external service.

## Database Schema (PostgreSQL)

We will use two main tables.

**1. `documents` Table:** Tracks the status of ingested files for idempotency and status checks.

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp"; -- Enable UUID generation

CREATE TYPE ingestion_status AS ENUM ('PENDING', 'PROCESSING', 'COMPLETED', 'FAILED');

CREATE TABLE documents (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    filename TEXT NOT NULL UNIQUE,
    status ingestion_status NOT NULL DEFAULT 'PENDING',
    error_message TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

**2. `chunks` Table:** Stores the vector data.

```sql
CREATE EXTENSION IF NOT EXISTS "vector"; -- Enable pgvector

CREATE TABLE chunks (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    document_id UUID NOT NULL REFERENCES documents(id) ON DELETE CASCADE,
    chunk_text TEXT NOT NULL,
    metadata JSONB, -- e.g., {"source_filename": "auth.md", "chunk_index": 1}
    
    -- The embedding. 384 dimensions for 'all-MiniLM-L6-v2'
    embedding vector(384) NOT NULL 
);

-- CRITICAL: An index for fast similarity search
CREATE INDEX ON chunks 
USING HNSW (embedding vector_cosine_ops);
```

## API Contract (OpenAPI 3.0.0 Specification)

All endpoints are secured by an `API-Key` in the request header.

```yaml
openapi: 3.0.0
info:
  title: DocuQuery API
  version: 1.0.0

security:
- ApiKeyAuth: []

paths:
  /v1/documents/upload:
    post:
      summary: Upload a knowledge base
      description: Accepts a .zip file of .md documents for asynchronous processing.
      operationId: upload_documents
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              type: object
              properties:
                file:
                  type: string
                  format: binary
      responses:
        '202':
          description: File accepted for processing.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Job'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /v1/documents/{job_id}/status:
    get:
      summary: Get ingestion job status
      operationId: get_job_status
      parameters:
        - name: job_id
          in: path
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: The status of the job.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/JobStatus'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /v1/query:
    post:
      summary: Ask a question
      description: Submits a natural language query and gets a synthesized answer.
      operationId: query_knowledgebase
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/QueryRequest'
      responses:
        '200':
          description: Successful query.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/QueryResponse'
        '422':
          $ref: '#/components/responses/UnprocessableEntity'
        '401':
          $ref: '#/components/responses/Unauthorized'

components:
  securitySchemes:
    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-KEY

  schemas:
    Job:
      type: object
      properties:
        job_id:
          type: string
          format: uuid
        filename:
          type: string
        status:
          type: string
          enum: [PENDING]

    JobStatus:
      type: object
      properties:
        job_id:
          type: string
          format: uuid
        status:
          type: string
          enum: [PENDING, PROCESSING, COMPLETED, FAILED]
        message:
          type: string

    QueryRequest:
      type: object
      required: [question]
      properties:
        question:
          type: string
          example: "How do I implement auth retries?"
    
    QueryResponse:
      type: object
      properties:
        answer:
          type: string
          example: "To implement auth retries, you should..."
        sources:
          type: array
          items:
            $ref: '#/components/schemas/Source'
    
    Source:
      type: object
      properties:
        filename:
          type: string
          example: "auth_service.md"
        snippet:
          type: string
          example: "...the retry logic is configured in the main..."

  responses:
    Unauthorized:
      description: API key is missing or invalid.
    NotFound:
      description: The requested resource was not found.
    UnprocessableEntity:
      description: Validation error.
```

## Deployment & Observability

  * **Deployment:** The system will be fully containerized using **Docker**. A `docker-compose.yml` file will orchestrate the `fastapi` service, `rabbitmq` service, `celery-worker` service, and `postgres` service for local development. For production, these services will be deployed as separate **Kubernetes (K8s)** Deployments, allowing the API and workers to be scaled independently.
  * **Observability:**
      * **Logging:** Structured JSON logging will be implemented in FastAPI and Celery.
      * **Metrics (Prometheus):** A `/metrics` endpoint will be exposed on the API service to track:
          * `query_latency_seconds` (Histogram)
          * `http_requests_total` (Counter)
          * `llm_response_time_seconds` (Histogram)
      * **Tracing (OpenTelemetry):** Traces will be configured for the `/v1/query` endpoint to provide a full breakdown of the request lifecycle: `Request -> Embedding -> Vector Search -> LLM API Call -> Response`. This is critical for debugging latency.

## Risks & Mitigations

1.  **Risk:** Poor retrieval quality (irrelevant chunks).
      * **Mitigation:** Experiment with chunking strategies (size, overlap). If performance is still poor, evaluate a more powerful (but slower) embedding model.
2.  **Risk:** LLM Hallucinations (making up answers).
      * **Mitigation:** Strict prompt engineering (e.g., "Answer *only* from the context provided. If the answer is not in the context, say 'I don't know.'").
3.  **Risk:** External LLM API Latency.
      * **Mitigation:** Implement a **Redis cache** for the `/v1/query` endpoint. Identical questions will return a cached response, saving time and cost.
4.  **Risk:** Ingestion pipeline failure.
      * **Mitigation:** Celery's built-in retry mechanisms will be used. The `documents` table `status` and `error_message` fields will provide visibility for debugging failed jobs.