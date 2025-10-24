# Product Requirements Document (PRD): "DocuQuery" API

## 1.1. Overview

### Project: DocuQuery API

#### Summary

DocuQuery is an internal, backend-first API service that allows developers and other technical users to ask natural language questions against a specific, private set of documents (e.g., technical documentation, knowledge bases, or internal wikis) and receive a synthesized, accurate answer.

#### Problem

Technical information is often scattered across hundreds of documents. Developers spend significant time using Ctrl+F, keyword search, or asking colleagues for information that exists but is hard to find. This slows down onboarding and day-to-day productivity.

#### Solution

We will build a Retrieval-Augmented Generation (RAG) system. This system will ingest and "index" our documentation into a vector database. 
It will then expose an API that:
- retrieves the most relevant document snippets based on a user's question 
- augments a Large Language Model (LLM) with this context to generate a precise, trustworthy answer.

## 1.2. Goals & Objectives

| Type | Goal | Metric (KPI) |
| :--- | :--- | :--- |
| **User** | Get accurate, synthesized answers to technical questions instantly. | 90% of answers are rated "Helpful" by users (via a simple feedback mechanism). |
| **User** | Trust the answers by verifying the sources. | 95% of all answers must return valid, correct source document references. |
| **Business** | Reduce developer time spent searching for information. | Decrease in "time-to-answer" for common support questions from hours to seconds. |
| **Technical** | Build a scalable, low-latency query system. | p95 query latency < 3 seconds. |
| **Technical** | Create a reliable and automated data ingestion pipeline. | New/updated documents are queryable within 15 minutes of being "published" or uploaded. |

## 1.3. Target Personas

1. Developer: The primary consumer of the API.

- Needs: Wants to ask questions like, "What's the correct way to implement the auth service retry logic?" or "Where is the staging DB configuration?"
- Pain Point: Hates switching context to read through 10 different Confluence pages or Markdown files.

2. Admin (System Administrator / DevOps): The primary operator of the system.

- Needs: Wants to manage the "knowledge set." Needs to upload new documents, update existing ones, and remove old ones.
- Pain Point: Doesn't want a manual, error-prone process. Needs the ingestion to be automated and observable.

### 1.4. Features & Requirements (Epics & User Stories)

#### Epic 1: Data Ingestion Pipeline (For "Admin")

As an Admin, I need to reliably manage the document corpus so that Deva always gets answers from the most up-to-date information.

- User Story 1.1 (Batch Upload): As an Admin, I want to upload a .zip file containing Markdown (.md) documents via a secure API endpoint so I can add a new set of knowledge.
- User Story 1.2 (Automated Processing): As the system, when a .zip file is uploaded, I must automatically:Unzip the file.Load all .md documents.Chunk the documents into small, semantically meaningful pieces (e.g., ~500 tokens with overlap).Generate vector embeddings for each chunk using an embedding model (e.g., sentence-transformers).Store the chunks, their metadata (e.g., source filename), and their vectors in a Vector Database (e.g., PostgreSQL w/ pgvector).
- User Story 1.3 (Idempotency): As the system, if an Admin re-uploads a document with the same name, I must overwrite the old document's chunks to avoid stale data and duplicates.
- User Story 1.4 (Processing Status): As an Admin, I want an endpoint to check the processing status of my upload batch (e.g., PENDING, PROCESSING, COMPLETED, FAILED).

#### Epic 2: Query API (For "Developer")

As a Developer, I want to ask a question to the API and get a single, trustworthy answer with its sources.

- User Story 2.1 (Query Endpoint): As a Developer, I want to POST a JSON payload {"question": "..."} to a /query endpoint.
- User Story 2.2 (Successful Response): As a Developer, when I send a valid question, I want to receive a 200 OK JSON response containing:answer: The natural language answer (string).sources: A list of objects, each with filename and chunk_id (or text snippet), so I can verify the information.
- User Story 2.3 (Validation): As the system, if a Developer sends a request with an empty or missing question field, I must return a 422 Unprocessable Entity error.

### 1.5. Non-Functional Requirements (NFRs)

- Performance:Query Latency (p95): < 3 seconds. This includes vector search, LLM inference, and network time.
- Ingestion Throughput: The pipeline must process 100MB of .md files in under 5 minutes.
- Scalability:The /query API must be stateless and horizontally scalable (e.g., deployable as multiple container instances).The vector database must support indexing up to 1 million vectors.
- Reliability:API: 99.9% uptime.
- Ingestion: The ingestion pipeline must be idempotent and retry-safe (e.g., using Airflow or Celery).
- Security:All API endpoints (ingestion and query) must be secured via API Key or OAuth2.
- Observability:The system must log structured JSON.The /query endpoint must have metrics for latency, error rate, and LLM token usage.

### 1.6. Out of Scope

- A graphical user interface (GUI). This is a backend-only project.
- Fine-tuning our own LLM. We will use a pre-trained model via an API (like OpenAI/Gemini 1.5 flash) or a hosted open-source model (like Llama 3).
- Support for non-text file types (e.g., PDFs, images, videos). The v1 is Markdown-only.