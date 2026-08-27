<div align="center">

# 🧾 Enterprise Invoice & Receipt Review System

**An enterprise-grade document intelligence platform combining automated OCR extraction, LLM field validation, deterministic financial rules, and human-in-the-loop exception workflows.**

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-19.0-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Azure AI](https://img.shields.io/badge/Azure_AI-Services-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)](https://azure.microsoft.com)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com)

<br />

<a href="#-overview">Overview</a> •
<a href="#-key-features">Features</a> •
<a href="#-system-architecture">Architecture</a> •
<a href="#-technology-stack">Tech Stack</a> •
<a href="#-getting-started">Getting Started</a> •
<a href="#-documentation">Documentation</a>

<br />

<img src="docs/images/dashboard-preview.png" alt="Invoice Review Dashboard UI" width="100%" style="border-radius: 8px; box-shadow: 0 4px 20px rgba(0,0,0,0.15);" />

</div>

---

## 📌 Overview

The **Enterprise Invoice & Receipt Review System** bridges the gap between probabilistic AI extractions and deterministic accounting requirements. Designed for high-volume accounts payable (AP) workflows, it automates line-item processing, multi-currency detection, and tax verification while retaining human oversight for financial exceptions.

### Processing Pipeline

```mermaid
flowchart LR
    A[📄 Upload Document] --> B[🏷️ Classification]
    B --> C[🔍 Azure Extraction]
    C --> D[⚖️ Rules Engine]
    D --> E[⚠️ Exception Gate]
    E --> F[👤 Human Review UI]
    F -->|Approved| G[💾 ERP Export]
    F -->|Rejected| H[🚫 Archive / Flag]
    G & H --> I[📜 Immutable Audit Trail]
    
    style A fill:#3776AB,stroke:#fff,color:#fff
    style D fill:#009688,stroke:#fff,color:#fff
    style F fill:#0078D4,stroke:#fff,color:#fff
    style I fill:#2496ED,stroke:#fff,color:#fff