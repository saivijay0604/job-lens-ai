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

## Docker

- [ ] Write `docker-compose.yml` — FastAPI + Firestore emulator + Pub/Sub emulator
- [ ] Write `.env.example` with all required environment variables
- [ ] Verify full local dev environment runs with `docker-compose up`

---

## Terraform (Infrastructure as Code)

- [ ] Write `main.tf` — GCP project, APIs, services
- [ ] Write `cloud_run.tf` — Cloud Run service definition
- [ ] Write `gke.tf` — GKE cluster
- [ ] Write `secret_manager.tf` — secrets
- [ ] Write `iam.tf` — service accounts and roles
- [ ] Create `dev` and `prod` environments

---

## Kubernetes

- [ ] Write `service.yaml` — Kubernetes service
- [ ] Write `ingress.yaml` — ingress for external access
- [ ] Write `configmap.yaml` — non-secret config
- [ ] Set up GKE cluster via Terraform

---

## CI/CD (Cloud Build)

- [ ] Write Cloud Build steps for: build and push Docker image to Artifact Registry
- [ ] Write Cloud Build steps for: deploy to Cloud Run or GKE on merge to main

---

## Web UI

- [ ] Build PDF upload tab in the resume input form
- [ ] Display interview questions with category and difficulty filters
- [ ] Display model answers section
- [ ] Build job history dashboard with status tracking

---

## Observability & Security

- [ ] Add structured logging with request IDs for all API requests
- [ ] Add rate limiting on AI endpoints
