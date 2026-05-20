# Marwa Bensalem
**AI / ML / DL Engineer · Data Scientist**
Building production-grade ML systems — evidence-grounded LLMs, calibrated quantile models, hybrid decision engines — with the MLOps surface (observability, supply-chain, drift) that keeps them honest.

📫 `marwabensalem30@gmail.com`  ·  🌐 [linkedin.com/in/marwa-ben-salem-573465b6](https://www.linkedin.com/in/marwa-ben-salem-573465b6)  ·  📦 [pypi.org/user/MarwaBS](https://pypi.org/project/schema-firewall/)

---

## Featured projects

### 🧠 [ResumeForge](https://github.com/MarwaBS/ResumeForge) — Production RAG system for grounded resume generation

Evidence-first LLM pipeline with hallucination guards, seniority-calibrated outputs, prompt versioning, drift detection, and a `$/resume` cost meter.

**Stack:** FastAPI · Streamlit · Pydantic v2 · Redis · arq · Mongo · FAISS / Qdrant · OpenAI · OTel · Prometheus · Helm · Docker
**Highlights:** `LLMProtocol` + `VectorStoreProtocol` abstractions · 1,800 tests passing at 81.62% coverage · 7-job CI (Trivy · SBOM · perf regression gate) · retrieval eval with `recall@k / MRR / nDCG` + bootstrap CIs · multi-pod-safe JWT revocation · Redis-backed daily cost circuit-breaker
**Live demo:** [resumeforge-bg29.onrender.com](https://resumeforge-bg29.onrender.com)

---

### ⚖️ [Job_Decision_Engine](https://github.com/MarwaBS/Job_Decision_Engine) — Deterministic decision system for job applications

Hybrid 5-signal weighted scorer + bounded LLM reasoning layer. Locked weights, append-only audit log, intentionally-inert evaluation pending real outcomes. **Determinism formally verified to `1e-9` precision across local, CI, and production.**

**Stack:** Pydantic v2 · sentence-transformers · OpenAI gpt-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · Hugging Face Spaces
**Highlights:** Architecture-as-Contract (immutable design doc + execution rules) · 261 hermetic invariant tests in 1.10s · 3-job CI (privacy audit · tests · auto-deploy to HF) · `decision_trace` with counterfactual sensitivity replays + dominant-signal coding · Protocol-based Mongo / LLM / embedding abstractions · frozen 3-environment determinism proof artifact
**Live demo:** [huggingface.co/spaces/MarwaBS/job-decision-engine](https://huggingface.co/spaces/MarwaBS/job-decision-engine)

---

### 📊 [nyc-real-estate-predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) — NYC real-estate price-zone classification + regression

4-model comparison (XGBoost · LightGBM · Random Forest · multi-task PyTorch TabNet) on 4,504 cleaned NYC listings. **Honest R² = 0.815** — documented the removal of the `PRICE_PER_SQFT` leakage that inflated v1 to R² = 0.997.

**Stack:** XGBoost · PyTorch · scikit-learn Pipelines · Optuna · SMOTE-ENN · SHAP · FastAPI · Streamlit · DVC · MLflow
**Highlights:** Macro F1 = 0.724 with per-class threshold tuning · SHAP explainability · fairness-by-borough analysis (diagnostic) · `assert_no_leakage()` enforced in CI · 64 tests / 88% coverage · Dockerfile multi-stage + Trivy + SBOM + Dependabot
**Live demo:** `docker compose up --build` *(or run the Streamlit app locally)*

---

### 💼 [high-pay-salary-predictor](https://github.com/MarwaBS/high-pay-salary-predictor) — US high-paying jobs salary quantile predictor

End-to-end data science pipeline on BLS OEWS + Census microdata. Multi-quantile XGBoost (P10/P50/P90) returns a calibrated uncertainty range — point-estimate R² is the wrong frame on a truncated cohort.

**Stack:** XGBoost quantile · FastAPI · Streamlit · Prometheus · Redis · Hugging Face Spaces · GHCR
**Highlights:** 149 tests · 3-job CI (matrix 3.11 + 3.12) with `Trivy` scan gating GHCR push · `APIKeyHeader` auth + env-driven CORS + proxy-aware `slowapi` rate limiting · cluster-wide drift monitor · scheduled retraining workflow
**Live demo:** [huggingface.co/spaces/MarwaBS/high-pay-salary-predictor](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)

---

## Open-source packages

### 🔥 [schema-firewall](https://github.com/MarwaBS/schema-firewall) · [pypi.org/project/schema-firewall](https://pypi.org/project/schema-firewall/)

> *Three checks that catch the leakage and schema bugs that slip past peer review.*

Production-tested validation library extracted from the `nyc-real-estate-predictor` firewall layer after the v1 → v2 R² collapse (0.997 → 0.815) traced to a single feature-leakage line. Three drop-in checks — no silent degradation, exceptions over warnings.

```bash
pip install schema-firewall
```

| Check | Catches |
|---|---|
| `check_leakage` | feature-target correlation that mirrors the label (the `PRICE_PER_SQFT` class of bugs) |
| `check_schema` | post-outcome columns leaking into training inputs |
| `check_stateless` | dataset-wide transforms applied outside cross-validation folds |

**Stack:** Python 3.10+ · MIT · minimal dependencies · zero silent fallback
**Why it exists:** every ML system ships at least one of these three bugs at least once. The library makes the check cheap enough that you have no excuse not to run it on every commit.

---

## What I focus on

| Lane | Bar | Portfolio signal |
|---|---|---|
| **LLM / Agentic** | Evidence-grounded generation, not prompt engineering | ResumeForge |
| **ML Systems Design** | Deterministic core + bounded LLM + verified reproducibility | Job_Decision_Engine |
| **ML / DL Engineering** | Honest metrics (no leakage), reproducible pipelines | nyc-real-estate-predictor |
| **Data Science** | Calibrated uncertainty, fairness-aware analysis | high-pay-salary-predictor |
| **MLOps & Tooling** | CI-gated supply chain (Trivy · SBOM · Dependabot · privacy audit) + shipped library | every project + schema-firewall |

## Tech

**Languages:** Python 3.12 (primary) · SQL · Bash
**ML:** scikit-learn · XGBoost · LightGBM · PyTorch · Optuna · SHAP · `schema-firewall` (own)
**LLM / AI:** OpenAI · LangChain · FAISS · Qdrant · Sentence-Transformers · custom `LLMProtocol` abstraction
**Serving:** FastAPI · Pydantic v2 · Streamlit · Uvicorn · arq · Redis · Mongo
**Infra:** Docker (multi-stage) · Kubernetes / Helm · Hugging Face Spaces · Render · GHCR · docker-compose
**Observability:** OpenTelemetry · Prometheus · structured JSON logs · locust perf regression
**CI / DevEx:** GitHub Actions · ruff · mypy (strict) · bandit · pytest · Trivy · CycloneDX SBOM · Dependabot

## Currently

- Shipping `schema-firewall` as a public PyPI library extracted from production code.
- Open to **Senior AI Engineer · ML / DL Engineer · ML Systems Engineer · Senior Data Scientist** roles.
- Remote-first; open to relocation.

<sub>Last updated: 2026-05-20 · Repos green CI-verified on HEAD · MIT-licensed</sub>
