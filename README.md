# Marwa Ben Salem
**AI / ML / DL Engineer · Data Scientist**
Building production-grade ML systems — evidence-grounded LLMs, calibrated quantile models, geospatial ML — with the MLOps surface (observability, supply-chain, drift) that keeps them honest.

📫 `marwabensalem30@gmail.com`  ·  🌐 [linkedin.com/in/marwa-ben-salem](www.linkedin.com/in/marwa-ben-salem-573465b6) 

---

## Featured projects

### 🧠 [ResumeForge](https://github.com/MarwaBS/ResumeForge) — Production RAG system for grounded resume generation

Evidence-first LLM pipeline with hallucination guards, seniority-calibrated outputs, prompt versioning, drift detection, and a `$/resume` cost meter.

**Stack:** FastAPI · Streamlit · Pydantic v2 · Redis · arq · Mongo · FAISS / Qdrant · OpenAI · OTel · Prometheus · Helm · Docker
**Highlights:** `LLMProtocol` + `VectorStoreProtocol` abstractions · 1,326 tests · 7-job CI (Trivy · SBOM · perf regression gate) · retrieval eval with `recall@k / MRR / nDCG` + bootstrap CIs · multi-pod-safe JWT revocation · Redis-backed daily cost circuit-breaker
**Live demo:** [resumeforge-bg29.onrender.com](https://resumeforge-bg29.onrender.com)

---

### 📊 [nyc-real-estate-predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) — NYC real-estate price-zone classification + regression

4-model comparison (XGBoost · LightGBM · Random Forest · multi-task PyTorch TabNet) on 4,504 cleaned NYC listings. **Honest R² = 0.815** — documented the removal of the `PRICE_PER_SQFT` leakage that inflated v1 to R² = 0.997.

**Stack:** XGBoost · PyTorch · scikit-learn Pipelines · Optuna · SMOTE-ENN · SHAP · FastAPI · Streamlit · DVC · MLflow
**Highlights:** Macro F1 = 0.724 with per-class threshold tuning · SHAP explainability · fairness-by-borough analysis (diagnostic) · `assert_no_leakage()` enforced in CI · 64 tests / 88% coverage · Dockerfile multi-stage + Trivy + SBOM + Dependabot
**Live demo:** `docker compose up --build` *(or try the Streamlit app locally)*

---

### 💼 [high-pay-salary-predictor](https://github.com/MarwaBS/high-pay-salary-predictor) — US high-paying jobs salary quantile predictor

End-to-end data science pipeline on BLS OEWS + Census microdata. Multi-quantile XGBoost (P10/P50/P90) returns a calibrated uncertainty range — point-estimate R² is the wrong frame on a truncated cohort.

**Stack:** XGBoost quantile · FastAPI · Streamlit · Prometheus · Redis · Hugging Face Spaces · GHCR
**Highlights:** 149 tests · 3-job CI (matrix 3.11+3.12) with `Trivy` scan gating GHCR push · `APIKeyHeader` auth + env-driven CORS + proxy-aware `slowapi` rate limiting · cluster-wide drift monitor · scheduled retraining workflow
**Live demo:** [huggingface.co/spaces/MarwaBS/high-pay-salary-predictor](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)

---

## What I focus on

| Lane | Bar | Portfolio signal |
|---|---|---|
| **LLM / Agentic** | Evidence-grounded generation, not prompt engineering | ResumeForge |
| **ML / DL Engineering** | Honest metrics (no leakage), reproducible pipelines | nyc-real-estate-predictor |
| **Data Science** | Calibrated uncertainty, fairness-aware analysis | high-pay-salary-predictor |
| **MLOps** | CI-gated supply chain (Trivy · SBOM · Dependabot) across all 3 repos | every project |

## Tech

**Languages:** Python 3.12 (primary) · SQL · Bash
**ML:** scikit-learn · XGBoost · LightGBM · PyTorch · Optuna · SHAP
**LLM / AI:** OpenAI · LangChain · FAISS · Qdrant · Sentence-Transformers · custom `LLMProtocol` abstraction
**Serving:** FastAPI · Pydantic v2 · Streamlit · Uvicorn · arq · Redis · Mongo
**Infra:** Docker (multi-stage) · Kubernetes / Helm · Hugging Face Spaces · Render · GHCR · docker-compose
**Observability:** OpenTelemetry · Prometheus · structured JSON logs · locust perf regression
**CI / DevEx:** GitHub Actions · ruff · mypy (strict) · bandit · pytest · Trivy · CycloneDX SBOM · Dependabot

## Currently

- Sharpening interview prep across the 3 target lanes.
- Open to **Senior AI Engineer · ML/DL Engineer · Data Scientist** roles.
- Based remote/open to relocation.

<sub>Last updated: 2026-04-18 · Repos green CI-verified on HEAD · MIT-licensed</sub>
