<div align="center">

<br />

# ⚖️ RayCtify

### *Achieving Algorithmic Equilibrium.*

**A premium AI auditing framework that detects, neutralizes, and evaluates demographic bias in financial loan approval models — in real time.**

<br />

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-async-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-Vite-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev)
[![Cloud Run](https://img.shields.io/badge/Google_Cloud-Run-4285F4?style=flat-square&logo=googlecloud&logoColor=white)](https://cloud.google.com/run)
[![Firebase](https://img.shields.io/badge/Firebase-Hosting-FFCA28?style=flat-square&logo=firebase&logoColor=black)](https://firebase.google.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Zero Retention](https://img.shields.io/badge/Data_Policy-Zero_Retention-red?style=flat-square)](#-security--data-privacy-policy)

<br />

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Local Setup](#-local-setup)
- [Deployment](#-deployment)
- [Security & Data Privacy](#-security--data-privacy-policy)
- [Institutional Guardrails](#-institutional-guardrails)
- [Contributing](#-contributing)

---

## 🧠 Overview

Financial institutions have long relied on Machine Learning models to automate loan approval decisions. But these models carry a silent, systemic risk: **proxy bias** — the tendency of an AI to learn hidden correlations between legally permissible financial metrics and protected demographic identifiers such as Age, Gender, Ethnicity, or ZIP Code.

RayCtify was built to solve this **at the architectural level**. Rather than simply flagging biased outputs after the fact, RayCtify acts as a **Glass Box wrapper** around any Black Box model. It intercepts the model's input pipeline, surgically zeroes out the influence of sensitive parameters, and forces every prediction to rest solely on standardized, defensible financial anchors — Income, Credit Score, DTI Ratio, and Loan Amount.

> The result is a loan decision engine that is not just accurate, but **auditable, equitable, and institutionally compliant**.

---

## ✨ Key Features

| Pillar | Module | Description |
|:---:|---|---|
| 🔬 | **The Interceptor** | Ingests `.pkl`, `.onnx`, `.pmml` models, detects sensitive feature dependencies, and exports a fully healed `RayCtifiedWrapper` with zero data retention. |
| 🔭 | **The Model Auditor** | Decomposes feature importance into a normalized 100% influence matrix with True-Sign Preservation — showing precisely whether each feature helped or hurt the applicant. |
| ⚡ | **The Arena** | Batch stress-testing engine for hundreds of records. Runs side-by-side delta comparisons and surfaces explicit **Bias Penalty** scores per applicant. |
| 🛡️ | **Institutional Guardrails** | Hard-coded, vectorized banking policies that override ML model anomalies — including automatic rejection of deep subprime scores below 500. |
| 🔒 | **Zero-Retention Architecture** | All processing occurs in-memory. Uploaded models and applicant data are garbage-collected immediately after evaluation, leaving no trace on disk. |
| 📐 | **DTI Standardization** | Full alignment with the industry-standard `dti_ratio` metric, carrying its mandated 20% baseline weight across the entire reporting matrix. |

---

## 🏗️ Architecture

RayCtify is organized into three primary operational pillars, each serving a distinct function in the bias audit lifecycle.

```
┌──────────────────────────────────────────────────────────────────┐
│                        RayCtify Platform                         │
│                                                                  │
│   ┌─────────────────┐  ┌──────────────────┐  ┌───────────────┐  │
│   │  THE INTERCEPTOR │  │  THE MODEL AUDIT │  │   THE ARENA   │  │
│   │  (Model Healing) │  │  (Introspection) │  │ (Batch Tests) │  │
│   └────────┬─────────┘  └────────┬─────────┘  └──────┬────────┘  │
│            └─────────────────────┴────────────────────┘           │
│                                  │                                │
│                    ┌─────────────▼──────────────┐                │
│                    │   FastAPI Backend (GCR)     │                │
│                    │   Zero-Retention | Async    │                │
│                    └─────────────┬──────────────┘                │
│                                  │                                │
│                    ┌─────────────▼──────────────┐                │
│                    │   React Frontend (Firebase) │                │
│                    │   Vite | Tailwind | Framer  │                │
│                    └────────────────────────────┘                │
└──────────────────────────────────────────────────────────────────┘
```

### 🔬 Pillar I — The Interceptor *(Model Healing)*

The Interceptor is the core remediation engine. It accepts any serialized model file and performs dynamic introspection on the underlying Scikit-Learn estimator to build a complete map of its feature dependencies. Upon detecting sensitive parameters (e.g., `demographic_segment`, `zip_code_cluster`, `age_band`), it wraps the estimator in a `RayCtifiedWrapper` — a custom proxy class that intercepts every call to `predict()` and `predict_proba()`, silently zeroing the weights of demographic features before the computation proceeds.

### 🔭 Pillar II — The Model Auditor *(Granular Introspection)*

The Model Auditor operates at the level of a single applicant record, giving compliance officers a mathematically precise view into *why* the model reached its decision. It extracts the raw `coef_` or `feature_importances_` array from the estimator and normalizes each feature's contribution to a clean 100% influence scale. Critically, it applies **True-Sign Preservation** — the signed direction of each feature's impact is retained throughout normalization, preventing the audit report from misrepresenting a detrimental feature as beneficial.

### ⚡ Pillar III — The Arena *(Batch Stress-Testing)*

The Arena is designed for institutional-scale validation. It accepts a batch of hundreds of applicant records and runs each one through both the original Base model and the RayCtified model in a vectorized, parallel pipeline. For every record it computes a **Bias Penalty** — the exact delta between the biased score and the equitable score — giving risk and compliance teams a quantified measure of how severely demographic noise was distorting outcomes.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend Framework** | React 18 (Vite) |
| **UI Styling** | Tailwind CSS |
| **Animations** | Framer Motion |
| **Frontend Hosting** | Firebase Hosting |
| **Backend Runtime** | Python 3.11 |
| **API Framework** | FastAPI (async) |
| **ML Engine** | Scikit-Learn |
| **Data Processing** | Pandas, NumPy |
| **Backend Hosting** | Google Cloud Run |

---

## 🚀 Local Setup

### Prerequisites

Ensure you have the following installed before proceeding: Python `>= 3.11`, Node.js `>= 18.x`, `pip`, and `npm`. The Google Cloud SDK (`gcloud`) and Firebase CLI are required for deployment only.

### 1 · Clone the Repository

```bash
git clone https://github.com/your-org/rayctify.git
cd rayctify
```

### 2 · Backend Setup

```bash
# Navigate to the backend directory
cd backend

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate       # On Windows: venv\Scripts\activate

# Install all Python dependencies
pip install -r requirements.txt

# Start the FastAPI development server
uvicorn main:app --reload --port 8000
```

> ✅ The API will be available at `http://localhost:8000`  
> 📖 Interactive Swagger UI at `http://localhost:8000/docs`

### 3 · Frontend Setup

```bash
# Navigate to the frontend directory
cd ../frontend

# Install Node dependencies
npm install

# Start the Vite development server
npm run dev
```

> ✅ The frontend will be available at `http://localhost:5173`

### 4 · Environment Variables

Create a `.env` file in the `/backend` directory:

```env
# Base URL for the FastAPI backend (used by the React frontend)
VITE_API_BASE_URL=http://localhost:8000

# Google Cloud project ID (required for Cloud Run deployment)
GCP_PROJECT_ID=your-gcp-project-id
```

---

## ☁️ Deployment

### Backend — Google Cloud Run

RayCtify's backend is designed for Google Cloud Run, which provides containerized, stateless, auto-scaling execution — a critical property for the Zero-Retention security model.

```bash
# Authenticate with Google Cloud
gcloud auth login
gcloud config set project YOUR_GCP_PROJECT_ID

# Build and submit the Docker image to Google Container Registry
gcloud builds submit --tag gcr.io/YOUR_GCP_PROJECT_ID/rayctify-api

# Deploy to Cloud Run
gcloud run deploy rayctify-api \
  --image gcr.io/YOUR_GCP_PROJECT_ID/rayctify-api \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 1Gi \
  --timeout 120
```

> 💡 Once deployed, copy the service URL provided by Cloud Run and set it as `VITE_API_BASE_URL` in your frontend environment configuration.

### Frontend — Firebase Hosting

```bash
# Create a production build
cd frontend
npm run build

# Install the Firebase CLI if not already present
npm install -g firebase-tools

# Authenticate and initialize Firebase (select 'Hosting')
firebase login
firebase init hosting

# Deploy the production build to Firebase
firebase deploy --only hosting
```

---

## 🔒 Security & Data Privacy Policy

RayCtify was architected from the ground up with a **Zero-Retention** guarantee. No applicant data, no uploaded model files, and no prediction artifacts are ever written to disk, stored in a database, or logged to any external service.

All processing occurs entirely within the **in-memory request lifecycle** of the FastAPI application. Uploaded `.pkl`, `.onnx`, or `.pmml` files are deserialized into Python objects in RAM, processed, and then explicitly garbage-collected via `gc.collect()` at the conclusion of each request handler. The Cloud Run execution environment is stateless and ephemeral by design — no data persists between container invocations.

This architecture ensures compliance with data minimization principles under **GDPR**, **CCPA**, and applicable financial privacy regulations, making RayCtify safe for use with real applicant data in regulated institutional environments.

> ⚠️ **Disclaimer:** RayCtify does not provide legal compliance certification. Organizations operating in regulated jurisdictions should conduct their own independent review in consultation with qualified legal and compliance counsel.

---

## 🏦 Institutional Guardrails

The following hard-coded policies are enforced by the backend API and **cannot be overridden by the ML model** under any circumstances.

| Policy | Rule | Enforcement Method |
|---|---|---|
| **Deep Subprime Hard Stop** | Credit Score `< 500` | Vectorized NumPy mask returns `[1.0, 0.0]` probability, bypassing the model entirely. |
| **DTI Standardization** | `dti_ratio` at 20% baseline weight | Enforced across both the React feature catalog and FastAPI input schema simultaneously. |

---

## 🤝 Contributing

Contributions that improve auditing precision, expand model format support, or strengthen compliance tooling are welcome. Please open an issue to discuss your proposal before submitting a pull request.

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# After making changes, run the test suite
pytest backend/tests/

# Submit a pull request against the `main` branch
```

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">

<br />

*Built to make algorithmic fairness an engineering standard, not an afterthought.*

<br />

</div>
