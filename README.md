# Enterprise Invoice & Receipt Review System

> AI-powered invoice and receipt processing with automated validation and human-in-the-loop review.

<p align="center">
  <img src="docs/images/dashboard-preview.png" alt="Invoice Review Dashboard" width="100%">
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#documentation">Documentation</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white">
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white">
  <img src="https://img.shields.io/badge/Azure_AI-0078D4?style=flat-square&logo=microsoftazure&logoColor=white">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white">
</p>

---

## Overview

The **Enterprise Invoice & Receipt Review System** is an end-to-end document intelligence platform designed to automate invoice and receipt processing while keeping accounting professionals in control of final decisions.

The platform combines:

**Azure Document Intelligence** for document extraction  
**Azure OpenAI** for intelligent document processing  
**Deterministic validation rules** for financial checks  
**Human review workflows** for final decisions  
**Audit trails** for traceability and accountability

### Processing Flow

```text
Upload
  ↓
Classify
  ↓
Extract
  ↓
Validate
  ↓
Detect Exceptions
  ↓
Human Review
  ↓
Approve / Reject
  ↓
Audit Trail
````

---

## Features

### Automated Document Extraction

Processes multilingual invoices and fuel receipts while handling:

* Scanned documents
* Variable layouts
* Poor scan quality
* Complex tables
* Layout inconsistencies

### Deterministic Financial Validation

Automatically identifies accounting exceptions such as:

* Duplicate line items
* Missing purchase orders
* Tax mismatches
* Invalid totals
* Missing required fields

### Human-in-the-Loop Review

Accounting professionals can:

* Review the original document
* Inspect extracted fields
* Investigate validation failures
* Override automated results
* Approve or reject documents
* Track every decision

---

## Architecture

<p align="center">
  <img src="docs/images/architecture.png" alt="System Architecture" width="90%">
</p>

The system separates **AI interpretation** from **financial validation**.

```text
                    DOCUMENT
                       │
                       ▼
          Azure Document Intelligence
                       │
                       ▼
              Structured Data
                       │
              ┌────────┴────────┐
              ▼                 ▼
        Azure OpenAI       Rules Engine
              │                 │
              └────────┬────────┘
                       ▼
                Exception Detection
                       │
                       ▼
                 Human Review
                       │
              ┌────────┴────────┐
              ▼                 ▼
            Approve           Reject
              │                 │
              └────────┬────────┘
                       ▼
                  Audit Trail
```

---

## Technology Stack

| Layer                 | Technology                           |
| --------------------- | ------------------------------------ |
| Backend               | FastAPI · Python 3.12+               |
| Frontend              | React 19 · TypeScript · Tailwind CSS |
| AI                    | Azure OpenAI                         |
| Document Intelligence | Azure Document Intelligence          |
| Database              | SQLite                               |
| Build                 | Vite · pnpm · uv                     |
| Deployment            | Docker                               |

---

## Repository Structure

```text
.
├── backend/
│   ├── app/
│   ├── scripts/
│   └── pyproject.toml
│
├── frontend/
│   ├── src/
│   └── vite.config.ts
│
├── samples/
├── docs/
├── Dockerfile
├── .env.example
└── README.md
```

---

# Getting Started

## Prerequisites

* Python 3.12+
* Node.js 22+
* pnpm 11.3+
* uv
* Docker (optional)

## Environment Variables

Create your environment file:

```bash
cp .env.example .env
```

Configure Azure services:

```env
AZURE_DOCUMENT_INTELLIGENCE_ENDPOINT=https://your-resource.cognitiveservices.azure.com/
AZURE_DOCUMENT_INTELLIGENCE_KEY=your-key

AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/openai/v1/
AZURE_OPENAI_DEPLOYMENT=gpt-5.6-terra
AZURE_OPENAI_API_KEY=your-key

ALLOWED_ORIGIN=http://localhost:5173
```

> Never commit `.env` files or Azure credentials to Git.

---

## Backend

```bash
cd backend
uv sync --locked --no-dev
```

Initialize the database:

```bash
uv run --locked --no-sync python -c "from app.main import create_app; app = create_app(); print('Database initialized')"
```

Start the API:

```bash
uv run --locked --no-sync uvicorn app.main:create_app \
  --factory \
  --host 0.0.0.0 \
  --port 8000
```

API:

```text
http://localhost:8000
```

Swagger:

```text
http://localhost:8000/docs
```

---

## Frontend

```bash
cd frontend
pnpm install --frozen-lockfile
pnpm dev
```

Application:

```text
http://localhost:5173
```

---

## Docker

Build:

```bash
docker build -t invoice-review:latest .
```

Run:

```bash
docker run \
  -p 8000:8000 \
  -p 5173:5173 \
  -e AZURE_DOCUMENT_INTELLIGENCE_ENDPOINT="..." \
  -e AZURE_DOCUMENT_INTELLIGENCE_KEY="..." \
  -e AZURE_OPENAI_ENDPOINT="..." \
  -e AZURE_OPENAI_DEPLOYMENT="..." \
  -e AZURE_OPENAI_API_KEY="..." \
  invoice-review:latest
```

---

# Development

### Backend

Lint:

```bash
uv run --locked --no-sync ruff check app scripts
```

### Frontend

```bash
pnpm lint
pnpm build
pnpm exec tsc -b
```

### Development Principles

* Keep business rules deterministic and testable.
* Use `uv.lock` for dependency management.
* Do not commit secrets or uploaded documents.
* Keep production configuration environment-based.
* Preserve auditability for financial decisions.

---

# Security

Because the platform processes financial documents, production deployments should apply appropriate security controls.

* Never commit API keys or credentials.
* Restrict CORS origins.
* Protect uploaded documents.
* Use managed secrets in production.
* Persist production data outside ephemeral containers.
* Apply access controls to document storage.
* Maintain audit logs for important decisions.

For enterprise deployments, consider integrating with an identity provider such as Microsoft Entra ID.

---

# Documentation

| Document                                          | Description                 |
| ------------------------------------------------- | --------------------------- |
| [`architecture.md`](docs/architecture.md)         | System architecture         |
| [`azure-deploy.md`](docs/azure-deploy.md)         | Azure deployment            |
| [`pricing.md`](docs/pricing.md)                   | Infrastructure and pricing  |
| [`api-and-pipeline.md`](docs/api-and-pipeline.md) | API and processing pipeline |
| [`client-brief.md`](docs/client-brief.md)         | Product requirements        |

---

## Project Philosophy

> **Automate the work, not the accountability.**

AI extracts and interprets documents.

Deterministic rules validate financial data.

Humans make the final decision.

The audit trail records what happened.

---

<p align="center">
  <strong>AI-powered document processing for modern financial operations.</strong>
</p>
```

### One important change

The biggest improvement is to use **real screenshots instead of ASCII UI mockups**.

For GitHub, I would structure the top of the README like this:

```text
┌──────────────────────────────────────────────┐
│                                              │
│     ENTERPRISE INVOICE REVIEW SYSTEM         │
│                                              │
│     AI-powered financial document review     │
│                                              │
│              [ Dashboard Screenshot ]        │
│                                              │
└──────────────────────────────────────────────┘

Features

[ Automated Extraction ] [ Validation ] [ Human Review ]

                 Architecture

             [ Clean Diagram ]

              Getting Started
```


