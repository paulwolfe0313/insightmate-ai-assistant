# Data Ingestion Plan – InsightMate AI Assistant

## Overview
This document outlines how InsightMate ingests, processes, and stores documents to prepare them for use in retrieval-augmented generation (RAG) pipelines. It covers source formats, parsing strategies, chunking methods, and embedding storage.

---

## 1. Supported Input Formats

| Format | Source | Example |
|--------|--------|---------|
| PDF    | Local upload, SharePoint | QBRs, internal reports, audit docs |
| CSV    | Local upload, cloud sync | Ticket logs, HR records, summaries |
| DOCX   | Optional (via Office365/SharePoint) | Policies, notes |
| API    | REST (e.g., SharePoint list, internal dashboard) | Metrics, live report feeds |

---

## 2. Preprocessing Workflow

### Step-by-Step Flow:
1. **File Uploaded or Pulled**
   - Local: via web dashboard
   - Remote: via scheduled API pull (e.g. SharePoint)

2. **Parsing & Cleaning**
   - PDF: Use `PyMuPDF` or `pdfplumber` to extract structured text
   - CSV: Use `pandas` to clean, format columns
   - DOCX: Use `python-docx` to extract text from sections

3. **Text Chunking**
   - Use `LangChain.TextSplitter` with custom settings:
     - Chunk size: 500–1000 tokens
     - Overlap: 100 tokens (to preserve context)
   - Remove empty chunks or boilerplate content

4. **Embedding**
   - Use `HuggingFaceEmbeddings` or `OpenAIEmbeddings`
   - Embed each chunk as a vector

5. **Storage**
   - Vectors: stored in `pgvector` (PostgreSQL) or Pinecone
   - Metadata: document name, chunk index, timestamp, source path
   - Use UUIDs for each document upload session

---

## 3. File Metadata Tracking

Each ingestion includes:
- `doc_id`: Unique document UUID
- `doc_type`: PDF / CSV / API
- `source_path`: Upload path or API origin
- `upload_time`: UTC timestamp
- `user_id`: (optional) to associate file with Slack/web user

---

## 4. Scheduled Ingestion (For SharePoint/API)

- Use a background scheduler (e.g. `APScheduler`, `Celery`, or `cron`)
- Query SharePoint API using MS Graph tokens
- Store fetched content in `/data/tmp/` before processing
- Auto-clean processed files on success

---

## 5. Example Scripts

| File | Purpose |
|------|---------|
| `ingest_pdf.py` | Load and process PDF files |
| `ingest_csv.py` | Parse and clean CSV datasets |
| `ingest_sharepoint.py` | Pull and sync SharePoint docs |
| `embed_chunks.py` | Common function to embed and insert into vector DB |

---

## 6. Error Handling

- Catch and log malformed documents
- Skip over blank or duplicate chunks
- Store all ingestion logs in `/logs/ingestion_log.json`

---

## 7. Security & Cleanup

- All uploads are scanned for PII patterns (regex-based)
- Temporary files are auto-deleted after processing
- Embeddings do not contain raw text (unless user toggles "store text")

---
