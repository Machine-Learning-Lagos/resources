# **10-Week Generative AI Study Group Curriculum**

**Prerequisites for the Generative AI Study Group**

* Strong proficiency in Python (lists, dicts, functions, modules).

* Solid software engineering fundamentals (Git, CLI).

* Experience making API requests in Python (e.g., the `requests` library). [Requests docs](https://requests.readthedocs.io/?utm_source=chatgpt.com). [requests.readthedocs.io](https://requests.readthedocs.io/?utm_source=chatgpt.com)

* Basic knowledge of a Python web framework like [Flask](https://flask.palletsprojects.com/?utm_source=chatgpt.com) or [FastAPI](https://fastapi.tiangolo.com/?utm_source=chatgpt.com). [Flask Documentation+1](https://flask.palletsprojects.com/?utm_source=chatgpt.com)

* Ability to set up Python environments and install packages with `pip` or `conda`.

* High-level conceptual understanding of ML / neural networks.

* Intuitive grasp of vectors & gradients (linear algebra & calculus).

* Familiarity with [Jupyter Notebooks](https://jupyter.org/?utm_source=chatgpt.com) or [Google Colab](https://colab.google/?utm_source=chatgpt.com) (Colab is handy for quick experiments). [jupyter.org+1](https://jupyter.org/?utm_source=chatgpt.com)

---

## **Weeks 1–2: Laying the Foundation**

**Goal:** Understand core principles of Generative AI and the components of LLMs.

### **Week 1: Introduction to Generative AI**

**Topics:** What is Generative AI; differences from discriminative ML; applications and history.  
 **Resources:**

* Video: “What is Generative AI?” — *Google Cloud Tech* (YouTube). [https://www.youtube.com/watch?v=G2fqAlgmoPo](https://www.youtube.com/watch?v=G2fqAlgmoPo&utm_source=chatgpt.com). [YouTube](https://www.youtube.com/watch?v=G2fqAlgmoPo&utm_source=chatgpt.com)

* Reading / short course: “Introduction to Generative AI” — *Google Cloud / Cloud Skills Boost* (overview & guided micro-course). [https://cloud.google.com/use-cases/generative-ai](https://cloud.google.com/use-cases/generative-ai?utm_source=chatgpt.com) and [https://www.cloudskillsboost.google/course\_templates/536](https://www.cloudskillsboost.google/course_templates/536?utm_source=chatgpt.com). [Google Cloud+1](https://cloud.google.com/use-cases/generative-ai?utm_source=chatgpt.com)  
   **Assignment:** Present one inspiring application; set up Python dev environment and install `transformers`, [PyTorch](https://pytorch.org/?utm_source=chatgpt.com) or [TensorFlow](https://www.tensorflow.org/?utm_source=chatgpt.com). [PyTorch+1](https://pytorch.org/?utm_source=chatgpt.com)

### **Week 2: How Large Language Models Work**

**Topics:** High-level neural nets; tokens, embeddings, Transformer architecture.  
 **Resources:**

* Visual explainer: **The Illustrated Transformer** — Jay Alammar (excellent walkthrough). [https://jalammar.github.io/illustrated-transformer/](https://jalammar.github.io/illustrated-transformer/?utm_source=chatgpt.com). [jalammar.github.io](https://jalammar.github.io/illustrated-transformer/?utm_source=chatgpt.com)

* Intro reading on LLMs: **What are Large Language Models (LLMs)?** — Cohere blog. [https://cohere.com/blog/large-language-models](https://cohere.com/blog/large-language-models?utm_source=chatgpt.com). [Cohere](https://cohere.com/blog/large-language-models?utm_source=chatgpt.com)  
   **Assignment:** Load a pre-trained tokenizer (e.g., via Hugging Face `transformers`) and experiment with tokenization and decoding. [Transformers docs & install](https://huggingface.co/docs/transformers/en/index?utm_source=chatgpt.com). [Hugging Face](https://huggingface.co/docs/transformers/en/index?utm_source=chatgpt.com)

---

## **Weeks 3–4: Prompting & APIs**

**Goal:** Learn to control models via prompting and using provider APIs.

### **Week 3: The Art of Prompt Engineering**

**Topics:** Zero-/one-/few-shot prompting, clarity & formatting, chain-of-thought techniques.  
 **Resources:**

* Course: *ChatGPT Prompt Engineering for Developers* — DeepLearning.AI (short course). [https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/?utm_source=chatgpt.com). [DeepLearning.ai](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/?utm_source=chatgpt.com)

* Guide: OpenAI’s official **Prompt engineering** guide. [https://platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering?utm_source=chatgpt.com). [OpenAI Platform](https://platform.openai.com/docs/guides/prompt-engineering?utm_source=chatgpt.com)  
   **Assignment:** Pick a problem (e.g., complex-paragraph summarization) and solve it with zero/one/few-shot prompts; compare outputs.

### **Week 4: Working with Generative AI APIs**

**Topics:** Using APIs from OpenAI, Google (Gemini), Anthropic; secure API key handling; making calls & processing responses. (Note: your curriculum mentions using Gemini; see Google’s docs for Gemini.)  
 **Resources:**

* OpenAI API docs & quickstart (Python examples & repo): [https://platform.openai.com/docs/](https://platform.openai.com/docs/?utm_source=chatgpt.com) and the [OpenAI quickstart GitHub (Python)](https://github.com/openai/openai-quickstart-python?utm_source=chatgpt.com). [OpenAI Platform+1](https://platform.openai.com/docs/?utm_source=chatgpt.com)

* Google Gemini / Google Cloud generative AI docs (search Google Cloud docs or Cloud console for Gemini access — Google’s GenAI docs vary by release). [Google Cloud](https://cloud.google.com/use-cases/generative-ai?utm_source=chatgpt.com)  
   **Assignment:** Build a small CLI app that takes a user question and queries an LLM API to generate an answer.

---

## **Weeks 5–6: First Projects**

**Goal:** Build end-to-end apps and explore open-source models.

### **Week 5: Project 1 — Text Generation App**

**Topics:** Scaffold a web app (Flask / FastAPI), integrate an LLM, handle input/output.  
 **Resources:**

* Tutorials: OpenAI quickstarts and community tutorials like “Build a Python app with the OpenAI API” (see OpenAI quickstart repo). [GitHub](https://github.com/openai/openai-quickstart-python?utm_source=chatgpt.com)  
   **Project:** Creative Story Idea Generator (input: genre \+ character → output: short story prompt).

### **Week 6: Exploring Open-Source Ecosystem**

**Topics:** Hugging Face Hub — finding, downloading, and running open models locally for generation/summarization.  
 **Resources:**

* Hugging Face Course / LLM Course / Learn pages. [https://huggingface.co/learn](https://huggingface.co/learn?utm_source=chatgpt.com) and [https://huggingface.co/huggingface-course](https://huggingface.co/huggingface-course?utm_source=chatgpt.com). [Hugging Face+1](https://huggingface.co/learn?utm_source=chatgpt.com)  
   **Project:** Swap your Week 5 app to use a free Hugging Face model (via `transformers` \+ `pipeline`) instead of a paid API.

---

## **Weeks 7–8: Advanced Topics**

**Goal:** Explore image generation, multimodality, and grounding LLMs with private data.

### **Week 7: Project 2 — Image Generation & Multimodality**

**Topics:** How Stable Diffusion / DALL·E work; prompting images; multimodal models.  
 **Resources:**

* Video: *How AI Image Generators Work (Stable Diffusion / DALL-E)* — Computerphile (YouTube). [https://www.youtube.com/watch?v=1CIpzeNxIhU](https://www.youtube.com/watch?v=1CIpzeNxIhU&utm_source=chatgpt.com) (and related Computerphile videos). [YouTube+1](https://www.youtube.com/watch?v=1CIpzeNxIhU&utm_source=chatgpt.com)

* Tools & APIs: Stability AI platform & docs (Stable Diffusion APIs). [https://platform.stability.ai/](https://platform.stability.ai/?utm_source=chatgpt.com) and API docs. [platform.stability.ai+1](https://platform.stability.ai/?utm_source=chatgpt.com)

* OpenAI image docs (DALL·E): [https://platform.openai.com/docs/guides/images](https://platform.openai.com/docs/guides/images?utm_source=chatgpt.com). [OpenAI Platform](https://platform.openai.com/docs/guides/images?utm_source=chatgpt.com)  
   **Project:** Build an "AI Blog Post Title Image Generator" that creates an image from a post title.

### **Week 8: Project 3 — Retrieval-Augmented Generation (RAG)**

**Topics:** Why LLMs hallucinate; using embeddings \+ vector DBs to ground answers (RAG pattern).  
 **Resources:**

* Reading: “What is Retrieval-Augmented Generation (RAG)?” — Pinecone RAG guide. [https://www.pinecone.io/learn/retrieval-augmented-generation/](https://www.pinecone.io/learn/retrieval-augmented-generation/?utm_source=chatgpt.com). [Pinecone](https://www.pinecone.io/learn/retrieval-augmented-generation/?utm_source=chatgpt.com)

* Vector DBs / embeddings: Pinecone docs and Hugging Face embedding models (see Hugging Face Learn). [Hugging Face+1](https://huggingface.co/huggingface-course?utm_source=chatgpt.com)  
   **Project:** Build a Q\&A chatbot that answers questions about a single PDF (ingest PDF → create embeddings → query \+ generate).

---

## **Week 9: Focused Project Development**

**Goal:** To consolidate all the skills learned by building a more complex, multi-faceted generative AI application of your choice. This week is dedicated to hands-on building.

* **Project Ideas:**  
  * A code review assistant that provides suggestions on a block of code.  
  * A personalized trip itinerary planner.  
  * A tool that generates marketing copy for a product based on a description.  
* **Process:** Work individually or in pairs. The focus is on implementing a complete, functional application. The group should hold regular check-ins to help each other with challenges.

---

## **Week 10: Capstone Project Showcase**

#### **Week 10: Capstone Project Showcase**

**Goal:** To present your Week 9 project to the group, demonstrating its functionality and sharing the challenges and learnings from the development process.

* **Activity:** Each member or team will conduct a 10-15 minute presentation and demo of their project.  
* **Discussion:** After presentations, the group will discuss what they've learned over the 10 weeks, what they found most interesting, and what they plan to build next.

