# Praneetha — Task Assignment
## Project: Job-Lens AI

Both Vijay and Praneetha work on ALL topics. Tasks within each topic are split between them.

---

## Python & FastAPI

- [ ] Set up `.env` file structure and `docker-compose.yml` for local development
- [ ] Initialize Git repo with `.gitignore`
- [ ] Build PDF upload handler using `pdfplumber` or `PyMuPDF`
- [ ] Build `pdf_service.py` — extract text from uploaded PDFs
- [ ] Build `profile_service.py` — profile normalization logic
- [ ] Implement `GET /profile/{profile_id}`
- [ ] Implement `GET /jobs/{job_id}` and `PUT /jobs/{job_id}/status`
- [ ] Implement `GET /jobs/{job_id}/answers` — return model answers for all questions
- [ ] Write tests: `test_analyze.py`

---

## GCP

- [ ] Set up IAM — service accounts and roles for Cloud Run, Firestore, GCS, Vertex AI
- [ ] Build `firestore_client.py` — Firestore read/write for profiles and jobs
- [ ] Save analysis results to Firestore under job history
- [ ] Upload PDF files to Cloud Storage, store GCS path reference in Firestore
- [ ] Move heavy AI processing to background worker via Pub/Sub
- [ ] Return `job_id` immediately on `POST /analyze`, process in background

---

## AI / Vertex AI / Gemini

- [ ] Design and write prompt template:
  - `interview_prompt.py` — interview question and model answer generation
- [ ] Learn and apply: embeddings, RAG basics, hallucination control, cost optimization
- [ ] Test and validate AI responses — ensure structured JSON output is parsed correctly
- [ ] Add safety filters and model evaluation checks

---

## Data Engineering

### BigQuery Analytics
- [ ] Set up BigQuery dataset and tables for job analytics
- [ ] Stream match scores, skill gaps, and job trends into BigQuery after every analysis
- [ ] Build `bigquery_client.py` — insert and query BigQuery from Python
- [ ] Create views for: top missing skills, average match scores by job title, application trends over time

### Data Pipeline
- [ ] Build a data transformation pipeline to clean and normalize resume and job description data before AI analysis
- [ ] Standardize skill names (e.g. `k8s` → `Kubernetes`, `GCP` → `Google Cloud Platform`)
- [ ] Build `pipeline_service.py` — pre-processing layer between input and AI service

### Embeddings & Vector Search
- [ ] Generate embeddings for resume profiles and job descriptions using Vertex AI Embeddings API
- [ ] Store embeddings in Firestore or a vector store
- [ ] Build similarity search — find the most relevant past jobs for a given resume
- [ ] Use embeddings to improve match scoring accuracy

### Data Export
- [ ] Build `GET /export/jobs` endpoint — export full job history as CSV or JSON
- [ ] Schedule daily export of analytics data from Firestore to BigQuery using Cloud Scheduler
- [ ] Store exported files in Cloud Storage with date-partitioned folder structure

### Batch Processing
- [ ] Build `POST /analyze/batch` endpoint — accept multiple job descriptions at once
- [ ] Publish each job as a separate Pub/Sub message for parallel async processing
- [ ] Track batch status — return overall progress and per-job results
- [ ] Use Dataflow or Cloud Run Jobs for large-scale batch analysis
