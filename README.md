# Marwa Bensalem
**AI / ML / DL Engineer · Data Scientist**
Building production-grade ML systems — evidence-grounded LLMs, calibrated quantile
models, hybrid decision engines — with the MLOps surface (observability,
supply-chain, drift) that keeps them honest.

📫 `marwabensalem30@gmail.com`  ·  🌐 [linkedin.com/in/marwa-ben-salem-573465b6](https://www.linkedin.com/in/marwa-ben-salem-573465b6)  ·  📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

---

## Featured projects

### 🧠 Production RAG Platform — evidence-grounded LLM generation
**Runnable reference service + deployment:** [production-rag-platform](https://github.com/MarwaBS/production-rag-platform)  ·  **Open infra on PyPI:** [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)  ·  **Live demo:** [resumeforge-bg29.onrender.com](https://resumeforge-bg29.onrender.com)

A production retrieval-augmented generation service that grounds every generated
claim in the user's own input, served behind a typed FastAPI gateway with full
delivery infrastructure.

**Stack:** FastAPI · Pydantic v2 · Redis · FAISS / Qdrant · OpenAI · OpenTelemetry · Prometheus · Helm / Kubernetes · Docker
**Highlights:** a **runnable reference service** (FastAPI: typed config · Prometheus `/metrics` · `/health` + `/ready` · integration tests) built on the published `rag-llm-infra` package · **Dockerfile that builds + publishes to GHCR (CD)** + Helm chart (Deployment / HPA / PDB / Ingress / ServiceAccount) · vendor-neutral `LLMProtocol` + `VectorStoreProtocol` (FAISS / NumPy / Qdrant) with **two CI quality gates** (retrieval recall@1/MRR + generation faithfulness) · OpenTelemetry tracing + structured JSON logs
> The product's proprietary generation logic stays private — but both the reusable library (`rag-llm-infra`, on PyPI) and a runnable, CI/CD'd reference deployment (`production-rag-platform`) are public and inspectable.

---

### ⚖️ [Job_Decision_Engine](https://github.com/MarwaBS/Job_Decision_Engine) — deterministic decision system
Hybrid 5-signal weighted scorer + bounded LLM reasoning layer with locked weights,
an append-only audit log, and an evaluation gate intentionally inert until real
outcomes accrue. **Determinism formally verified to `1e-9` across local, CI, and Hugging Face Spaces.**

**Stack:** Pydantic v2 · sentence-transformers · OpenAI · MongoDB Atlas · Streamlit · Docker · GitHub Actions · Hugging Face Spaces
**Highlights:** Architecture-as-Contract (immutable design doc + execution rules) · hermetic invariant test suite · `decision_trace` with counterfactual sensitivity replay + dominant-signal coding · Protocol-based Mongo / LLM / embedding abstractions · CI with auto-deploy to Hugging Face Spaces on merge
**Live demo:** [huggingface.co/spaces/MarwaBS/job-decision-engine](https://huggingface.co/spaces/MarwaBS/job-decision-engine)

---

### 📊 [nyc-real-estate-predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) — price-zone classification + regression
4-model comparison (XGBoost · LightGBM · Random Forest · multi-task PyTorch) on
4,504 cleaned NYC listings. **Honest R² = 0.815** — documents the removal of the
`PRICE_PER_SQFT` leakage that had inflated v1 to R² = 0.997.

**Stack:** XGBoost · PyTorch · scikit-learn Pipelines · Optuna · SHAP · FastAPI · Streamlit · DVC · MLflow
**Highlights:** macro-F1 = 0.724 with per-class threshold tuning (+0.020 over argmax) · SHAP explainability · fairness-by-borough diagnostic · `assert_no_leakage()` enforced in CI · multi-stage Docker + Trivy + CycloneDX SBOM + Dependabot
**Run:** `docker compose up --build` *(or the Streamlit app locally)*

---

### 💼 [high-pay-salary-predictor](https://github.com/MarwaBS/high-pay-salary-predictor) — salary quantile predictor
End-to-end pipeline on BLS OEWS + Census microdata. Multi-quantile XGBoost
(P10 / P50 / P90) returns a calibrated uncertainty range — point-estimate R² is the
wrong frame on a truncated `≥ $100K` cohort; quantile coverage is the right one.

**Stack:** XGBoost (quantile) · FastAPI · Streamlit · Prometheus · Redis · Hugging Face Spaces · GHCR
**Highlights:** `APIKeyHeader` auth + env-driven CORS + proxy-aware `slowapi` rate limiting · cluster-wide drift monitor over a Redis sliding window · test-matrix CI (3.11 + 3.12) with a Trivy gate on GHCR push · scheduled retraining workflow
**Live demo:** [huggingface.co/spaces/MarwaBS/high-pay-salary-predictor](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)

---

## Open-source

### 🔥 [schema-firewall](https://github.com/MarwaBS/schema-firewall) · [pypi.org/project/schema-firewall](https://pypi.org/project/schema-firewall/)
> *Three checks that catch the leakage and schema bugs that slip past peer review.*

Validation library extracted from `nyc-real-estate-predictor`'s firewall layer after
the v1 → v2 R² collapse (0.997 → 0.815) traced to a single feature-leakage line.
Drop-in, exceptions over warnings, no silent degradation.

```bash
pip install schema-firewall
```

| Check | Catches |
|---|---|
| `check_leakage` | feature-target correlation that mirrors the label (the `PRICE_PER_SQFT` class) |
| `check_schema` | post-outcome columns leaking into training inputs |
| `check_stateless` | dataset-wide transforms applied outside cross-validation folds |

### 🧩 [rag-llm-infra](https://github.com/MarwaBS/rag-llm-infra) — RAG/LLM infrastructure ([on PyPI](https://pypi.org/project/rag-llm-infra/))
**`pip install rag-llm-infra`** — a vendor-neutral RAG/LLM serving layer: `LLMProtocol`
(OpenAI / Anthropic-stub / Mock) with a budget-aware multi-provider `FallbackLLM`,
`VectorStoreProtocol` (FAISS / NumPy / Qdrant), a cached embedding index, a FastAPI
serving layer (`/index` · `/query`), OpenTelemetry + structured logging, and **two
CI quality gates** — retrieval (recall@1 / MRR) and generation faithfulness
(groundedness). Typed, packaged (hatchling), Docker-shipped, **green CI**.

---

## What I focus on

| Lane | Bar | Portfolio signal |
|---|---|---|
| **LLM / RAG Systems** | Evidence-grounded generation + vendor-neutral infra, not prompt engineering | production-rag-platform · rag-llm-infra |
| **ML Systems Design** | Deterministic core + bounded LLM + verified reproducibility | Job_Decision_Engine |
| **ML / DL Engineering** | Honest metrics (no leakage), reproducible pipelines | nyc-real-estate-predictor |
| **Data Science** | Calibrated uncertainty, fairness-aware analysis | high-pay-salary-predictor |
| **MLOps & Tooling** | CI-gated supply chain (Trivy · SBOM · Dependabot) + shipped library | every project + schema-firewall |

## Tech

**Languages:** Python 3.12 (primary) · SQL · Bash
**ML:** scikit-learn · XGBoost · LightGBM · PyTorch · Optuna · SHAP · `schema-firewall` (own)
**LLM / AI:** OpenAI · LangChain · FAISS · Qdrant · Sentence-Transformers · custom `LLMProtocol` abstraction
**Serving:** FastAPI · Pydantic v2 · Streamlit · Uvicorn · Redis · MongoDB
**Infra:** Docker (multi-stage) · Kubernetes / Helm · Hugging Face Spaces · Render · GHCR · docker-compose
**Observability:** OpenTelemetry · Prometheus · structured JSON logs
**CI / DevEx:** GitHub Actions · ruff · mypy (strict) · bandit · pytest · Trivy · CycloneDX SBOM · Dependabot

## Currently

- Maintaining `schema-firewall` as a public PyPI library extracted from production code.
- Building vendor-neutral RAG/LLM infrastructure (`rag-llm-infra`, `production-rag-platform`).

<sub>Last updated: 2026-06-04 · Repos CI-verified on HEAD · MIT-licensed</sub>
