# Python + GCP + Vertex AI — Learning Roadmap
### For Golang Backend Developers

---

## Goal

In 3–4 months, build a production-ready system:

> User uploads a document → Python processes it → Vertex AI/Gemini summarizes it → Everything runs on GCP

You already know Go. You are not learning Python from zero. You are learning Python as a **backend + AI + cloud automation language**.

---

## Go → Python Mental Map

| Go | Python |
|----|--------|
| `struct` | `class` / `dataclass` |
| `map[string]string` | `dict` |
| `goroutine` | `asyncio`, `threads` |
| `go mod` | `venv`, `pip`, `poetry` |
| `net/http` | `FastAPI` / `Flask` |
| `error != nil` | `try/except` |

---

## 4-Month Roadmap

---

### Month 1 — Python for Backend Developers

**What to learn:**

1. Python syntax
2. Data types — `list`, `tuple`, `dict`, `set`
3. Functions
4. OOP in Python
5. Exception handling
6. File handling
7. Virtual environments
8. Python packages
9. Type hints
10. Async Python basics
11. REST APIs using FastAPI
12. Testing with pytest

**Resources:**
- Python tutorial → https://docs.python.org/3/tutorial/
- FastAPI docs → https://fastapi.tiangolo.com/
- Pytest docs → https://docs.pytest.org/

---

### Month 2 — GCP Core Skills

**What to learn:**

| GCP Service | Purpose |
|-------------|---------|
| Cloud Storage | Store files, documents, images, JSON output |
| Cloud Run | Deploy containerized Python APIs |
| Artifact Registry | Store Docker images |
| Cloud Build | CI/CD build pipeline |
| IAM | Service accounts, roles, permissions |
| Secret Manager | Store API keys and credentials |
| Cloud Logging & Monitoring | Debug deployed applications |
| Pub/Sub | Async event-driven processing |
| BigQuery | Store analytics and data output |
| Firestore / Cloud SQL | Store app-level metadata |

> Google Cloud's Python client libraries are the recommended way to call GCP APIs from Python. They handle auth and reduce boilerplate.

**Resources:**
- GCP Python libraries → https://cloud.google.com/python/docs/reference
- gcloud CLI → https://cloud.google.com/sdk
- GCP auth guide → https://cloud.google.com/docs/authentication/client-libraries

---

### Month 3 — AI, LLM, and Vertex AI Basics

> Do not jump into model training. First learn how LLM applications work.

**Core AI/LLM concepts to understand:**

1. What is AI vs ML vs GenAI
2. What is an LLM
3. Tokens
4. Prompt engineering
5. Temperature, top-p, max tokens
6. Embeddings
7. Vector search
8. RAG — Retrieval-Augmented Generation
9. Function calling / tool calling
10. Model evaluation
11. Safety filters
12. Hallucination control
13. Cost and latency optimization

> Vertex AI is Google Cloud's ML/AI platform for working with models like Gemini and deploying AI applications.
> The Vertex AI Python SDK supports evaluation, prompt management, prompt optimization, and agent development.

**Resources:**
- Vertex AI docs → https://cloud.google.com/vertex-ai/docs
- Vertex AI Python SDK → https://cloud.google.com/python/docs/reference/aiplatform/latest
- Vertex GenAI SDK → https://cloud.google.com/python/docs/reference/vertexai/latest
- GenAI learning path → https://www.skills.google/paths/118
- ML/AI training → https://cloud.google.com/learn/training/machinelearning-ai

---

### Month 4 — Build Production-Level AI App

**What to build:**

1. Dockerize the Python app
2. Deploy FastAPI to Cloud Run
3. Use service account auth
4. Store files in Cloud Storage
5. Call Vertex AI/Gemini from Python
6. Save metadata in Firestore or Cloud SQL
7. Add logs and error handling
8. Add async processing using Pub/Sub
9. Add CI/CD using Cloud Build

---

## Main Project — Enterprise Document Intelligence Platform

### What it does

User uploads a PDF, resume, invoice, or any document. The system:

1. Stores the file in GCP
2. Extracts the text
3. Sends it to Vertex AI/Gemini
4. Returns structured output:
   - Summary
   - Key points
   - Action items
   - Q&A from document
   - Risk/issues detection
   - Searchable document history

---

### Architecture

```
User
  |
  v
Python FastAPI AI Service
  |
  +---> Cloud Storage       (store uploaded files)
  |
  +---> Vertex AI / Gemini  (summarize, Q&A, analysis)
  |
  +---> Firestore           (store metadata + results)
  |
  +---> Cloud Logging       (observability)
  |
  +---> Pub/Sub             (async background processing)
```

---

### Tech Stack

**Python handles:**
- File parsing
- AI prompts
- Vertex AI calls
- Embeddings
- RAG pipeline

**GCP services used:**
- Cloud Run
- Cloud Storage
- Vertex AI
- Firestore
- Secret Manager
- Cloud Build
- Pub/Sub
- Cloud Logging

---

## Project Phases

---

### Phase 1 — Basic Python FastAPI Service

**Build these endpoints:**

```
POST /summarize
POST /ask
GET  /health
```

**Input:**
```json
{
  "text": "long document text here"
}
```

**Output:**
```json
{
  "summary": "...",
  "key_points": ["...", "..."],
  "action_items": ["...", "..."]
}
```

**What you learn:** Python, FastAPI, REST API, JSON handling, error handling

---

### Phase 2 — Connect Vertex AI / Gemini

**Flow:**
```
Receive text
  -> prepare prompt
  -> call Gemini via Vertex AI SDK
  -> parse response
  -> return clean JSON
```

**What you learn:** Prompt design, Gemini SDK, token control, AI response formatting

---

### Phase 3 — Add Cloud Storage

**New endpoint:**
```
POST /upload
```

**Flow:**
```
User uploads PDF
  -> store file in Cloud Storage
  -> extract text
  -> summarize using Vertex AI
  -> save result
```

**What you learn:** GCS buckets, Python storage client, service account auth, file processing

---

### Phase 4 — Add Database (Firestore)

**Store per document:**

| Field | Description |
|-------|-------------|
| `document_id` | Unique ID |
| `file_name` | Original file name |
| `gcs_path` | Path in Cloud Storage |
| `summary` | AI-generated summary |
| `created_at` | Timestamp |
| `status` | Processing status |
| `user_question` | Last question asked |
| `ai_answer` | Last AI answer |

**What you learn:** Metadata storage, querying history, document tracking

---

### Phase 5 — Deploy to GCP

**Deploy to Cloud Run using:**
- Dockerfile
- Artifact Registry
- Service accounts
- Secret Manager
- Cloud Logging

**What you learn:** Deployment, IAM, containerization, GCP production workflow

---

### Final Folder Structure

```
enterprise-document-intelligence-platform/
│
├── app/
│   ├── main.py              # FastAPI app + all routes
│   ├── vertex_client.py     # Vertex AI / Gemini calls
│   ├── storage_client.py    # Cloud Storage upload/download
│   ├── firestore_client.py  # Firestore read/write
│   ├── prompts.py           # Prompt templates
│   ├── schemas.py           # Pydantic request/response models
│   ├── config.py            # Settings from environment
│   └── utils.py             # Text extraction helpers
│
├── tests/
│   └── test_main.py         # pytest tests
│
├── infra/
│   ├── cloudbuild.yaml      # CI/CD pipeline
│   └── deploy.sh            # Manual deploy script
│
├── Dockerfile
├── requirements.txt
├── .env.example
└── README.md
```

---

## Weekly Plan

---

### Week 1 — Python Basics

**Learn:** Syntax, functions, lists/dicts, classes, exceptions, file handling

**Build:** CLI text analyzer
```
Input:  document.txt
Output: word count, top keywords, basic summary placeholder
```

---

### Week 2 — FastAPI

**Learn:** FastAPI, Pydantic, REST APIs, pytest, logging

**Build:** FastAPI service with `/summarize` and `/health`

---

### Week 3 — GCP Basics

**Learn:** gcloud CLI, Cloud Storage, IAM, service accounts, Cloud Run

**Build:** Upload a file to Cloud Storage using Python

---

### Week 4 — Vertex AI Basics

**Learn:** Gemini model call, prompting, token limits, structured JSON response

**Build:** Document summarizer using Vertex AI

---

### Week 5 — Database

**Learn:** Firestore, store document metadata, query results

**Build:** Document history API

---

### Week 6 — Async Processing

**Learn:** Pub/Sub, background document processing, status tracking

**Build:** Upload document → publish event → AI service processes async

---

### Week 7 — Production Deployment

**Learn:** Docker, Cloud Build, Artifact Registry, Cloud Run, logs, monitoring

**Build:** Fully deployed AI document assistant on GCP

---

## Mini Projects (Build Before Final Project)

---

### 1. Python File Analyzer
- Input: `.txt` or `.csv` file
- Output: line count, word count, top 10 words, file size
- Skills: Python basics, file handling

### 2. FastAPI Notes API
- Endpoints: `POST /notes`, `GET /notes`, `GET /notes/{id}`, `DELETE /notes/{id}`
- Skills: FastAPI, REST, Pydantic

### 3. GCP File Upload App
- Upload files to Cloud Storage from Python
- Skills: GCP auth, buckets, Python client library

### 4. Vertex AI Text Summarizer
- Input: long text
- Output: summary, key points, action items
- Skills: Vertex AI, Gemini, prompting

---

## What NOT to Learn Initially

Avoid these until you have one working AI app:

- Deep math
- Training models from scratch
- TensorFlow internals
- Complex ML algorithms
- Kubernetes
- Heavy MLOps tools

---

## Topics Covered by This Project

**Python:** FastAPI, Pydantic, type hints, async endpoints, file handling, API testing, package management, error handling, logging

**GCP:** Cloud Run, Cloud Storage, IAM, service accounts, Secret Manager, Cloud Build, Artifact Registry, Firestore, Pub/Sub, Cloud Logging

**AI / Vertex AI:** Gemini API, prompt engineering, summarization, Q&A, RAG basics, embeddings, response parsing, AI safety, evaluation, cost optimization

---

## Resume Bullet (After Completion)

> Built a cloud-native AI Document Intelligence Platform using Python FastAPI, Google Cloud Run, Cloud Storage, Firestore, and Vertex AI/Gemini. Designed a microservice architecture where the Python AI service processed documents, generated summaries, answered user questions, and stored metadata in GCP. Deployed containerized services using Docker and Cloud Run with IAM-based service account authentication, centralized logging, and scalable serverless infrastructure.

---

## Target Roles After This

- Backend Engineer with AI
- Cloud Engineer
- AI Application Developer
- Platform Engineer
- GenAI Engineer
- Backend LLM Engineer

