# Job-Lens_AI

---

## What Is This Project?

A production-grade AI platform that takes your resume and a job description, then gives you:

- A match score showing how well you fit the role
- Skill gap analysis — exactly what you are missing
- Tailored interview questions grouped by category and difficulty
- Model answers based on your own experience

This is not a simple text generator. It is a full backend system with AI, database, file processing, REST API, web UI, Docker, Kubernetes, Terraform, and GCP deployment.

---

## The Problem It Solves

Job applications are blind. You paste your resume, submit, and hope. You do not know:

- How well you actually match the job
- What skills are blocking you from getting shortlisted
- What questions the interviewer will ask
- How to answer those questions using your own experience

This platform solves all of that before you even apply.

---

## Core Features

---

### 1. Resume Input — Three Ways

**Option A — Paste plain text**
Paste your resume directly as text into the API or UI.

**Option B — Upload PDF**
Upload your PDF resume. The system extracts the text automatically.

**Option C — Structured form**
Fill in a form with:
- Name
- Skills (list)
- Work experience (company, role, duration, description)
- Projects (name, tech stack, description)
- Education

All three options produce the same result. The system normalizes your input into a unified profile.

---

### 2. Job Match Score

Paste a job description. The system compares it against your profile and returns:

```json
{
  "match_score": 74,
  "match_level": "Strong Match",
  "matched_skills": ["Python", "FastAPI", "Docker", "GCP"],
  "missing_skills": ["Kubernetes", "Terraform", "Pub/Sub"],
  "experience_match": "3 of 5 required areas covered",
  "summary": "You are a strong match for the backend requirements but lack infrastructure and DevOps skills listed in the job."
}
```

---

### 3. Skill Gap Analysis

Beyond just listing missing skills, the system tells you:

```json
{
  "critical_gaps": ["Kubernetes", "Terraform"],
  "nice_to_have_gaps": ["BigQuery", "Pub/Sub"],
  "recommendations": [
    "Learn Kubernetes basics — focus on deployments, services, and ingress",
    "Learn Terraform for GCP — start with Cloud Run and Cloud Storage modules",
    "These two skills appear in 80% of similar job postings"
  ]
}
```

---

### 4. Interview Questions — Categorized and Leveled

The system generates interview questions based on the job description and your profile.

**Categories:**
- Technical — coding, system design, tools, architecture
- Behavioral — teamwork, conflict, leadership, ownership
- Situational — what would you do if, how would you handle

**Difficulty levels:**
- Easy — warm-up questions, basic concepts
- Medium — real-world scenarios, depth of knowledge
- Hard — system design, edge cases, senior-level thinking

**Example output:**
```json
{
  "technical": [
    {
      "question": "How would you design a document processing pipeline on GCP?",
      "difficulty": "hard",
      "why_asked": "Job requires GCP + document processing experience"
    }
  ],
  "behavioral": [
    {
      "question": "Tell me about a time you had to learn a new technology quickly.",
      "difficulty": "medium",
      "why_asked": "Role requires fast ramp-up on Python and AI tools"
    }
  ],
  "situational": [
    {
      "question": "If a Pub/Sub message fails to process, how would you handle it?",
      "difficulty": "hard",
      "why_asked": "Job description mentions async processing with Pub/Sub"
    }
  ]
}
```

---

### 5. Model Answers Based on Your Experience

For each question, the system generates a suggested answer using your actual experience from your resume.

```json
{
  "question": "Tell me about a time you had to learn a new technology quickly.",
  "model_answer": "At [Company], I was asked to build a Go microservice within two weeks despite having no prior Go experience. I focused on the official docs and built a working REST API by day 5. By the end of the sprint, the service was deployed and handling production traffic. This taught me how to learn by building rather than just reading."
}
```

---

### 6. Profile Storage — Save Once, Reuse Always

Your profile is saved in the database. Every time you analyze a new job, you just paste the job description. Your profile is already there.

- Update your profile anytime
- Add new skills, projects, or experience
- The system always uses your latest profile

---

### 7. Job Application History

Every job you analyze is saved with:

| Field | Description |
|-------|-------------|
| `job_id` | Unique ID |
| `company_name` | Extracted or entered manually |
| `job_title` | Extracted from job description |
| `match_score` | Your match percentage |
| `analyzed_at` | Timestamp |
| `status` | saved, applied, interviewing, rejected, offer |
| `notes` | Your personal notes on this job |

You can come back to any job, see your previous analysis, update the status, and track your entire job search in one place.

---

## How It Works — Full Flow

```
User provides resume (text / PDF / form)
        |
        v
System extracts and normalizes profile
        |
        v
Profile saved in Firestore
        |
        v
User pastes job description
        |
        v
Vertex AI / Gemini analyzes job vs profile
        |
        v
Returns:
  - Match score
  - Skill gaps
  - Interview questions (categorized + leveled)
  - Model answers using your experience
        |
        v
Results saved in Firestore under job history
        |
        v
User views results via API or Web UI
```

---

## Architecture

```
Browser / Postman
        |
        v
  [ Web UI — HTML/JS ]     [ REST API — FastAPI ]
        |                          |
        +----------+---------------+
                   |
                   v
         Python FastAPI Service
         (runs on Cloud Run / Kubernetes)
                   |
        +----------+----------+----------+----------+
        |          |          |          |          |
        v          v          v          v          v
  Vertex AI   Firestore   Cloud      Secret     Cloud
  / Gemini    (profiles   Storage    Manager    Logging
  (AI calls)  + jobs)     (PDF       (API       (observ-
                          files)     keys)      ability)
                                        |
                                   Pub/Sub
                                (async processing)
```

---

## API Endpoints

### Profile

| Method | Endpoint | What It Does |
|--------|----------|--------------|
| `POST` | `/profile` | Create profile (text, PDF, or form) |
| `GET` | `/profile/{profile_id}` | Get saved profile |
| `PUT` | `/profile/{profile_id}` | Update profile |

### Job Analysis

| Method | Endpoint | What It Does |
|--------|----------|--------------|
| `POST` | `/analyze` | Analyze job vs profile, returns full result |
| `GET` | `/jobs` | List all analyzed jobs |
| `GET` | `/jobs/{job_id}` | Get full analysis for a specific job |
| `PUT` | `/jobs/{job_id}/status` | Update job status (applied, interviewing, etc.) |
| `DELETE` | `/jobs/{job_id}` | Delete a job from history |

### Interview Prep

| Method | Endpoint | What It Does |
|--------|----------|--------------|
| `GET` | `/jobs/{job_id}/questions` | Get all interview questions for a job |
| `GET` | `/jobs/{job_id}/questions?category=technical` | Filter by category |
| `GET` | `/jobs/{job_id}/questions?difficulty=hard` | Filter by difficulty |
| `GET` | `/jobs/{job_id}/answers` | Get model answers for all questions |

### Utility

| Method | Endpoint | What It Does |
|--------|----------|--------------|
| `GET` | `/health` | Service health check |

---

## Input and Output Examples

### Create Profile — Plain Text

**Request:**
```json
{
  "input_type": "text",
  "content": "John Doe. Backend Engineer. Skills: Python, Go, Docker, FastAPI, PostgreSQL. Experience: 3 years at TechCorp building REST APIs..."
}
```

### Create Profile — Structured Form

**Request:**
```json
{
  "input_type": "form",
  "name": "John Doe",
  "title": "Backend Engineer",
  "skills": ["Python", "Go", "Docker", "FastAPI", "PostgreSQL"],
  "experience": [
    {
      "company": "TechCorp",
      "role": "Backend Engineer",
      "duration": "2021 - 2024",
      "description": "Built REST APIs serving 1M requests/day using Go and PostgreSQL"
    }
  ],
  "projects": [
    {
      "name": "Order Processing Service",
      "tech": ["Go", "Kafka", "PostgreSQL"],
      "description": "Async order processing system handling 50K orders/day"
    }
  ],
  "education": "B.Tech Computer Science, 2021"
}
```

### Analyze a Job

**Request:**
```json
{
  "profile_id": "profile-abc-123",
  "job_description": "We are looking for a Senior Python Engineer with experience in FastAPI, GCP, Kubernetes, Terraform, and Vertex AI..."
}
```

**Response:**
```json
{
  "job_id": "job-xyz-456",
  "match_score": 68,
  "match_level": "Good Match",
  "matched_skills": ["Python", "FastAPI", "Docker"],
  "missing_skills": ["Kubernetes", "Terraform", "Vertex AI"],
  "skill_gaps": {
    "critical": ["Kubernetes", "Terraform"],
    "nice_to_have": ["Vertex AI", "Pub/Sub"],
    "recommendations": [
      "Learn Kubernetes — focus on deployments and services",
      "Learn Terraform for GCP infrastructure"
    ]
  },
  "interview_questions": {
    "technical": [
      {
        "question": "How do you manage secrets in a Kubernetes deployment?",
        "difficulty": "medium",
        "model_answer": "Based on your Docker experience, you can relate this to environment variables but explain Kubernetes Secrets and how they integrate with Secret Manager on GCP..."
      }
    ],
    "behavioral": [
      {
        "question": "Describe a time you improved the performance of a system.",
        "difficulty": "medium",
        "model_answer": "From your resume: At TechCorp, you built APIs serving 1M requests/day. Talk about what optimizations you made to reach that scale..."
      }
    ],
    "situational": [
      {
        "question": "If your Cloud Run service is timing out under load, how do you debug it?",
        "difficulty": "hard",
        "model_answer": "Walk through Cloud Logging, metrics, concurrency settings, and cold start analysis..."
      }
    ]
  }
}
```

---

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| API Framework | FastAPI (Python) | Fast, async, auto-generates Swagger docs |
| AI Model | Gemini via Vertex AI | Best LLM on GCP, structured JSON output |
| PDF Extraction | pypdf | Extract text from uploaded resumes |
| Database | Firestore | Serverless NoSQL, stores profiles and jobs |
| File Storage | Cloud Storage | Stores uploaded PDF resumes |
| Async Processing | Pub/Sub | Decouple heavy AI analysis from API response |
| Secrets | Secret Manager | Store Gemini API keys and credentials |
| Observability | Cloud Logging | Centralized logs |
| Containerization | Docker | Package the app for any environment |
| Orchestration | Kubernetes (GKE) | Run containers at scale in production |
| Infrastructure | Terraform | Provision all GCP resources as code |
| Deployment | Cloud Run / GKE | Serverless or Kubernetes deployment |
| CI/CD | Cloud Build | Automated build, test, and deploy pipeline |
| Web UI | HTML + JS (served by FastAPI) | Simple browser interface |

---

## Folder Structure

```
ai-job-intelligence/
│
├── app/
│   ├── main.py                  # FastAPI app — all routes
│   ├── routers/
│   │   ├── profile.py           # Profile create, get, update
│   │   ├── analyze.py           # Job analysis endpoint
│   │   ├── jobs.py              # Job history endpoints
│   │   └── questions.py         # Interview questions endpoints
│   ├── services/
│   │   ├── ai_service.py        # All Vertex AI / Gemini calls
│   │   ├── profile_service.py   # Profile normalization logic
│   │   └── pdf_service.py       # PDF text extraction
│   ├── clients/
│   │   ├── vertex_client.py     # Vertex AI SDK wrapper
│   │   ├── firestore_client.py  # Firestore read/write
│   │   └── storage_client.py    # Cloud Storage upload/download
│   ├── prompts/
│   │   ├── match_prompt.py      # Prompt for job match scoring
│   │   ├── gap_prompt.py        # Prompt for skill gap analysis
│   │   └── interview_prompt.py  # Prompt for interview questions
│   ├── schemas.py               # Pydantic request/response models
│   ├── config.py                # Environment variable settings
│   └── static/
│       └── index.html           # Simple web UI
│
├── tests/
│   ├── test_profile.py
│   ├── test_analyze.py
│   └── test_questions.py
│
├── infra/
│   └── terraform/
│       ├── main.tf              # GCP project, APIs, services
│       ├── cloud_run.tf         # Cloud Run service definition
│       ├── gke.tf               # Kubernetes cluster on GCP
│       ├── firestore.tf         # Firestore database
│       ├── storage.tf           # Cloud Storage bucket
│       ├── pubsub.tf            # Pub/Sub topic and subscription
│       ├── secret_manager.tf    # Secrets
│       ├── iam.tf               # Service accounts and roles
│       └── variables.tf         # Input variables
│
├── k8s/
│   ├── deployment.yaml          # Kubernetes deployment
│   ├── service.yaml             # Kubernetes service
│   ├── ingress.yaml             # Ingress for external access
│   ├── configmap.yaml           # Non-secret config
│   └── hpa.yaml                 # Horizontal pod autoscaler
│
├── Dockerfile                   # Container definition
├── docker-compose.yml           # Local development setup
├── requirements.txt             # Python dependencies
├── .env.example                 # Environment variable template
└── README.md                    # Project documentation
```

---

## Local Development Setup

```
docker-compose up
```

Runs everything locally:
- FastAPI service on port 8000
- Firestore emulator
- Pub/Sub emulator

Access the API at `http://localhost:8000`
Access Swagger docs at `http://localhost:8000/docs`
Access the Web UI at `http://localhost:8000/ui`

---

## GCP Deployment — Step by Step

### Step 1 — Provision infrastructure with Terraform
```
cd infra/terraform
terraform init
terraform plan
terraform apply
```

This creates:
- GKE cluster
- Firestore database
- Cloud Storage bucket
- Pub/Sub topic
- Secret Manager secrets
- Service accounts with correct IAM roles

### Step 2 — Build and push Docker image
```
docker build -t gcr.io/<project-id>/ai-job-intelligence:latest .
docker push gcr.io/<project-id>/ai-job-intelligence:latest
```

### Step 3 — Deploy to Kubernetes
```
kubectl apply -f k8s/
```

### Step 4 — CI/CD with Cloud Build
Every push to main branch automatically:
- Runs tests
- Builds Docker image
- Pushes to Artifact Registry
- Deploys to GKE

---

## Firestore Data Model

### profiles collection
```
profiles/
  {profile_id}/
    name: string
    title: string
    skills: array
    experience: array
    projects: array
    education: string
    raw_text: string
    created_at: timestamp
    updated_at: timestamp
```

### jobs collection
```
jobs/
  {job_id}/
    profile_id: string
    job_title: string
    company_name: string
    job_description: string
    match_score: number
    match_level: string
    matched_skills: array
    missing_skills: array
    skill_gaps: map
    interview_questions: map
    status: string
    notes: string
    analyzed_at: timestamp
```

---

## Environment Variables

```
GCP_PROJECT_ID          — your GCP project ID
GCP_REGION              — e.g. us-central1
GCS_BUCKET_NAME         — bucket for PDF uploads
GEMINI_MODEL            — gemini-1.5-pro
FIRESTORE_DATABASE       — (default) or named database
PUBSUB_TOPIC            — topic for async analysis jobs
```

---

## What You Learn Building This Project

| Area | Skills |
|------|--------|
| Python | FastAPI, Pydantic, routers, services pattern, async, file handling |
| AI / Vertex AI | Prompt engineering, structured JSON output, multi-step AI pipelines |
| GCP | Cloud Run, GKE, Firestore, Cloud Storage, Pub/Sub, Secret Manager, IAM |
| Infrastructure | Terraform — provision real GCP resources as code |
| Kubernetes | Deployments, services, ingress, HPA, configmaps |
| Docker | Multi-stage builds, docker-compose for local dev |
| CI/CD | Cloud Build pipeline — test, build, push, deploy |
| Testing | pytest with mocks — test AI features without real API calls |

---

## Project Phases — Build Order

| Phase | What You Build | What You Learn |
|-------|---------------|----------------|
| 1 | FastAPI skeleton — health, profile CRUD, hardcoded responses | Python, FastAPI, Pydantic, REST |
| 2 | PDF extraction + profile normalization | File handling, pypdf, data modeling |
| 3 | Vertex AI integration — match score + skill gaps | Prompt engineering, Gemini SDK |
| 4 | Interview questions + model answers | Multi-step AI prompts, response parsing |
| 5 | Firestore — save profiles and job history | GCP Firestore, data persistence |
| 6 | Cloud Storage — PDF upload and storage | GCS Python client, service accounts |
| 7 | Web UI — simple browser interface | HTML, JS, FastAPI static files |
| 8 | Docker + docker-compose local setup | Containerization, local dev workflow |
| 9 | Terraform — provision GCP infrastructure | Infrastructure as code, GCP resources |
| 10 | Kubernetes — deploy to GKE | K8s deployments, services, ingress, HPA |
| 11 | Cloud Build CI/CD pipeline | Automated testing and deployment |

---

## Resume Bullet (After Completion)

> Built a production-grade AI Job Intelligence Platform using Python FastAPI, Vertex AI/Gemini, Firestore, and Cloud Storage. The system analyzes resumes against job descriptions to generate match scores, skill gap reports, and categorized interview questions with model answers. Deployed on GKE using Kubernetes with Terraform-provisioned GCP infrastructure, Docker containerization, and an automated Cloud Build CI/CD pipeline.

---

## Why This Project Is Powerful

- It solves a real problem you face right now
- It covers every layer — AI, backend, database, storage, infra, deployment
- Terraform + Kubernetes + Docker is what every senior engineer is expected to know
- The AI features are non-trivial — multi-step prompts, structured output, context-aware answers
- After building this, you can walk into any interview and explain every component in detail
