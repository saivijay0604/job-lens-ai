# Vijay — Task Assignment
## Project: Job-Lens AI

Both Vijay and Praneetha work on ALL topics. Tasks within each topic are split between them.

---

## Python & FastAPI

- [ ] Initialize Python project with `requirements.txt`
- [ ] Set up FastAPI app skeleton with `GET /health` endpoint
- [ ] Configure environment variables and `config.py`
- [ ] Build resume input handler for plain text (`input_type: text`)
- [ ] Build structured form input handler
- [ ] Normalize all input types into a unified profile schema (`schemas.py`)
- [ ] Implement `POST /profile` and `PUT /profile/{profile_id}`
- [ ] Implement `POST /analyze` — run full analysis (match score + gaps + questions + answers)
- [ ] Implement `GET /jobs` and `DELETE /jobs/{job_id}`
- [ ] Implement `GET /jobs/{job_id}/questions` with category and difficulty filters
- [ ] Write tests: `test_profile.py`, `test_questions.py`

---

## GCP

- [ ] Set up GCP project and enable required APIs (Firestore, Cloud Run, Vertex AI, Secret Manager)
- [ ] Set up Cloud Storage bucket for PDF uploads
- [ ] Build `storage_client.py` — upload and download files from GCS
- [ ] Store all secrets (API keys, credentials) in Secret Manager
- [ ] Set up Pub/Sub topic and subscription for async job analysis
- [ ] Update Firestore job status as processing progresses (`pending → completed`)

---

## AI / Vertex AI / Gemini

- [ ] Set up Vertex AI client (`vertex_client.py`) with service account credentials
- [ ] Design and write prompt templates:
  - `match_prompt.py` — match score and skill gap analysis
  - `gap_prompt.py` — skill gap recommendations
- [ ] Build `ai_service.py` — call Gemini and parse structured JSON responses
- [ ] Add retry logic and error handling for AI API calls
- [ ] Learn and apply: prompt engineering, token control, structured JSON output

---

## Docker

- [ ] Write `Dockerfile` for the FastAPI app
- [ ] Test Docker build and verify app runs in container
- [ ] Write `docker-compose.yml` — FastAPI + Firestore emulator + Pub/Sub emulator
- [ ] Write `.env.example` with all required environment variables
- [ ] Verify full local dev environment runs with `docker-compose up`

---

## Terraform (Infrastructure as Code)

- [ ] Write `main.tf` — GCP project, APIs, services
- [ ] Write `cloud_run.tf` — Cloud Run service definition
- [ ] Write `gke.tf` — GKE cluster
- [ ] Write `firestore.tf` — Firestore database
- [ ] Write `storage.tf` — Cloud Storage bucket
- [ ] Write `pubsub.tf` — Pub/Sub topic and subscription
- [ ] Write `secret_manager.tf` — secrets
- [ ] Write `iam.tf` — service accounts and roles
- [ ] Write `variables.tf` — input variables
- [ ] Set up Terraform state in GCS backend
- [ ] Create `dev` and `prod` environments

---

## Kubernetes

- [ ] Write `deployment.yaml` — Kubernetes deployment
- [ ] Write `service.yaml` — Kubernetes service
- [ ] Write `ingress.yaml` — ingress for external access
- [ ] Write `configmap.yaml` — non-secret config
- [ ] Write `hpa.yaml` — Horizontal Pod Autoscaler
- [ ] Set up GKE cluster via Terraform
- [ ] Test deployments on GKE cluster

---

## CI/CD (Cloud Build)

- [ ] Write Cloud Build steps for: lint and test on PR
- [ ] Write Cloud Build steps for: build and push Docker image to Artifact Registry
- [ ] Write Cloud Build steps for: deploy to Cloud Run or GKE on merge to main
- [ ] Add smoke tests post-deployment

---

## Web UI

- [ ] Build resume input form (text input tab and structured form tab)
- [ ] Build job description input field and analyze button
- [ ] Display match score and skill gap results
- [ ] Build PDF upload tab in the resume input form
- [ ] Display interview questions with category and difficulty filters
- [ ] Display model answers section
- [ ] Build job history dashboard with status tracking

---

## Observability & Security

- [ ] Integrate Cloud Logging for all AI calls
- [ ] Add input validation and sanitization on all endpoints
- [ ] Add structured logging with request IDs for all API requests
- [ ] Add rate limiting on AI endpoints
