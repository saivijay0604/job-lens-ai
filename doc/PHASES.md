# Job-Lens AI — Phase-Wise Task Plan

---

## Phase 1 — Project Setup & Foundation

- [ ] Initialize Python project with `pyproject.toml` or `requirements.txt`
- [ ] Set up FastAPI app skeleton with health check endpoint (`GET /health`)
- [ ] Configure environment variables and `.env` file structure
- [ ] Set up Docker and `docker-compose.yml` for local development
- [ ] Initialize Git repo with `.gitignore`
- [ ] Set up GCP project, enable required APIs (Firestore, Cloud Run, Vertex AI, Secret Manager)

---

## Phase 2 — Resume Ingestion & Profile Management

- [ ] Build resume input handler for plain text (`input_type: text`)
- [ ] Build PDF upload handler using `pdfplumber` or `PyMuPDF`
- [ ] Build structured form input handler
- [ ] Normalize all three input types into a unified profile schema
- [ ] Implement `POST /profile` — create and save profile to Firestore
- [ ] Implement `GET /profile/{profile_id}` — fetch profile
- [ ] Implement `PUT /profile/{profile_id}` — update profile
- [ ] Upload PDF files to Cloud Storage, store reference in Firestore

---

## Phase 3 — AI Integration (Vertex AI / Gemini)

- [ ] Set up Vertex AI client with service account credentials via Secret Manager
- [ ] Design prompt templates for:
  - Match score and skill gap analysis
  - Interview question generation (technical, behavioral, situational)
  - Model answer generation using resume context
- [ ] Build AI service layer to call Gemini and parse structured JSON responses
- [ ] Add retry logic and error handling for AI API calls

---

## Phase 4 — Job Analysis Engine

- [ ] Implement `POST /analyze` — run full analysis (match score + gaps + questions + answers)
- [ ] Save analysis results to Firestore under job history
- [ ] Implement `GET /jobs` — list all analyzed jobs for a profile
- [ ] Implement `GET /jobs/{job_id}` — get full analysis for a job
- [ ] Implement `PUT /jobs/{job_id}/status` — update job status
- [ ] Implement `DELETE /jobs/{job_id}` — delete job from history

---

## Phase 5 — Interview Prep Endpoints

- [ ] Implement `GET /jobs/{job_id}/questions` — return all questions
- [ ] Add filtering by `category` (technical, behavioral, situational)
- [ ] Add filtering by `difficulty` (easy, medium, hard)
- [ ] Implement `GET /jobs/{job_id}/answers` — return model answers for all questions

---

## Phase 6 — Async Processing with Pub/Sub

- [ ] Set up Pub/Sub topic and subscription for async job analysis
- [ ] Move heavy AI processing to background worker via Pub/Sub
- [ ] Return `job_id` immediately on `POST /analyze`, process in background
- [ ] Update Firestore job status as processing progresses (`pending → completed`)

---

## Phase 7 — Web UI

- [ ] Build minimal HTML/JS frontend
- [ ] Resume input form (text, PDF upload, structured form)
- [ ] Job description input and analyze button
- [ ] Display match score, skill gaps, and recommendations
- [ ] Display interview questions with category and difficulty filters
- [ ] Display model answers
- [ ] Job history dashboard with status tracking

---

## Phase 8 — Observability & Security

- [ ] Integrate Cloud Logging for all API requests and AI calls
- [ ] Add structured logging with request IDs
- [ ] Store all secrets (API keys, credentials) in Secret Manager
- [ ] Add input validation and sanitization on all endpoints
- [ ] Add rate limiting on AI endpoints

---

## Phase 9 — Infrastructure as Code (Terraform)

- [ ] Write Terraform modules for:
  - Cloud Run service
  - Firestore database
  - Cloud Storage bucket
  - Pub/Sub topic and subscription
  - Secret Manager secrets
  - IAM roles and service accounts
- [ ] Set up Terraform state in GCS backend
- [ ] Create `dev` and `prod` environments

---

## Phase 10 — Kubernetes & CI/CD

- [ ] Write Kubernetes manifests (Deployment, Service, Ingress, ConfigMap, HPA)
- [ ] Set up GKE cluster via Terraform
- [ ] Build CI/CD pipeline (GitHub Actions or Cloud Build):
  - Lint and test on PR
  - Build and push Docker image to Artifact Registry
  - Deploy to Cloud Run or GKE on merge to main
- [ ] Add smoke tests post-deployment

---

## Milestone Summary

| Phase | Deliverable |
|-------|-------------|
| 1 | Running FastAPI app in Docker |
| 2 | Profile CRUD with all 3 input types |
| 3 | AI integration returning structured JSON |
| 4 | Full job analysis saved to Firestore |
| 5 | Interview prep endpoints with filters |
| 6 | Async processing via Pub/Sub |
| 7 | Working Web UI |
| 8 | Logging, secrets, and security hardened |
| 9 | Full infra provisioned via Terraform |
| 10 | Deployed to GCP with CI/CD pipeline |
