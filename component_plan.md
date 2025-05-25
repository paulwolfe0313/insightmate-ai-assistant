# Component Plan – InsightMate AI Assistant

## Overview
This document outlines the modular components of InsightMate, including tools, configurations, and their responsibilities. This helps ensure clean separation of concerns and a reusable, scalable architecture.

---

## 1. User Interfaces

### A. Slack Bot
- **Purpose:** Natural language interface for quick queries.
- **Libraries:** Slack Bolt (Python) or Slack Events + FastAPI
- **Endpoints:** `/summarize`, `/ask`, `/search`

### B. Web Dashboard
- **Purpose:** Provide a visual interface for uploading docs, asking questions, and viewing summaries.
- **Tech Stack:** Next.js, Tailwind CSS
- **Features:** File upload, chat interface, summary view, trace logs

---

## 2. API Layer

### FastAPI Backend
- **Purpose:** Serve API routes to Slack/Web, route inputs to LLM pipelines, manage auth, and handle user sessions.
- **Libraries:** FastAPI, Uvicorn, Pydantic
- **Endpoints:** `POST /query`, `GET /sources`, `POST /upload`

---

## 3. AI Pipeline (LLM + RAG)

### LangChain or LlamaIndex
- **Purpose:** Orchestrate retrieval, generation, and evaluation.
- **Components:**
  - Document Loader
  - Text Splitter (chunking strategy)
  - Embedding Generator (OpenAI / Hugging Face)
  - Vector Retriever
  - QA Chain with PromptTemplate
- **Config Files:** `rag_config.yaml`, `prompt_templates.md`

---

## 4. Data Ingestion Layer

### Input Types:
- PDF, CSV, SharePoint (via REST API)

### Pipeline:
- **Scripts:** `ingest_pdf.py`, `ingest_csv.py`, `sharepoint_ingest.py`
- **Processing:** Normalize → Chunk → Embed → Store in Vector DB
- **Libraries:** PyMuPDF, Pandas, Requests, LangChain

---

## 5. Vector Store

### pgvector / Pinecone / Weaviate
- **Purpose:** Efficient storage & retrieval of document embeddings.
- **Decision Factors:**
  - Use `pgvector` for local/Postgres compatibility
  - Use `Pinecone` if distributed scalability is required

---

## 6. LLM Serving

### Options:
- **OpenAI API** (for early prototyping)
- **vLLM or TGI** (for hosting local OSS models like LLaMA 3, Mistral)
- **PEFT/LoRA** (if fine-tuning needed)

---

## 7. Observability

### Tools:
- LangSmith or Traceloop for tracing and prompt performance.
- Custom logs for prompt inputs/outputs, latency, error handling.

---

## 8. Infrastructure

### Docker
- Containerize backend, frontend, and ingestion scripts.

### Terraform
- Deploy infrastructure on AWS (ECS, S3, Lambda, IAM)

### GitHub Actions
- Automate builds, test pipelines, deployment routines.

---

## 9. Security & Auth

### Measures:
- Slack auth via OAuth
- JWT tokens for API
- Role-based access control for internal teams
- Environment variable secrets with AWS Parameter Store

---
