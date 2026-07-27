# Marwa Bensalem

**Senior AI/ML Engineer** · Production LLM Systems · RAG · MLOps

I build production AI systems that verify their own outputs. Four years running enterprise HRIS/payroll at ADP taught me what breaks at 3am — and why most "AI tools" aren't production systems. Now I build the kind that are.

📫 `marwabensalem30@gmail.com` · 🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem) · 📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

---

## Shipped systems

### ⚖️ [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) · [Live demo →](https://huggingface.co/spaces/MarwaBS/job-decision-engine)
Job scorer with a deterministic core and a bounded LLM reasoning layer. On the LLM-free path (the public demo): same input → same output, every time — verified to 1e-9 in local and CI runs. The LLM signal is capped at 25% of the score, and the UI banner is reconciled against a live API ping at boot — it can never claim an LLM that isn't actually answering. 350 hermetic tests in ~5-10 seconds. Evaluation gate stays locked until 50 real outcomes accumulate (no fake metrics).

**Stack:** Pydantic v2 · sentence-transformers · OpenAI GPT-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · HuggingFace Spaces

**Engineering signals:** append-only audit log — every decision recorded with its exact signals and weights, so any past verdict can be re-derived · protocol-based LLM/DB abstractions · CI: privacy audit → tests → lint → auto-deploy to the Space · weekly scheduled security re-scan + live-Space health probe

---

### 🧠 [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform)
Runnable reference RAG service built on my published [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) package: swappable vector store, API-key-protected data plane, Prometheus metrics, structured JSON logs, and a retrieval-recall eval gate in CI that can actually fail. The README draws an explicit public/private boundary — a private flagship built on the same design stays private; everything claimed in this repo runs with `pip install`.

**Stack:** FastAPI · rag-llm-infra · OpenAI · FAISS/Qdrant (optional extras) · Pydantic v2 · Prometheus · Docker · Kubernetes · Helm · GitHub Actions

**Engineering signals:** CI: tests → retrieval-recall eval floor → Trivy image scan → CycloneDX SBOM · vendor-neutral LLMProtocol + VectorStoreProtocol · Helm chart with HPA, ingress off by default · optional backends fail fast with the exact `pip install` hint

---

### 📊 [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/nyc-real-estate-predictor)
XGBoost · LightGBM · Random Forest compared on a validation split (fixed hyperparameters — no tuning) on 4,526 NYC listings. **Honest test R² = 0.835** (0.814 ± 0.028 over 20 seeds) — I found and documented my own data leakage that had inflated v1 to R² = 0.997 (ADR-001), then extracted the fix as [`schema-firewall`](https://pypi.org/project/schema-firewall/) on PyPI. Anyone can re-verify me: the external benchmark re-runs against public NYC.gov 2024 Rolling Sales data (18,321 real sales) under a sealed schema contract.

**Stack:** XGBoost · LightGBM · scikit-learn · SHAP · category-encoders · FastAPI · Streamlit · MLflow · Docker

**Engineering signals:** CI-enforced leakage guard (test_no_leakage.py) · SHA256-manifest model registry — the live Space serves exactly the audited artifacts, checked weekly by a drift guard · 220 tests, 78% coverage gate (88.85% actual) · **29-mutation harness** — CI breaks one behaviour at a time and fails the build unless a named test catches it; it started at 6 of 15 caught, which is how I learned a passing suite proves nothing · reproducible external benchmark in CI

---

### 💼 [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)
One multi-quantile XGBoost model (P10/P50/P90 from a single artifact) on BLS OEWS + US Census microdata. Calibrated uncertainty, not point estimates: the served interval is widened by a cross-conformal margin estimated on train-only folds, and the route that applies it is pinned by tests — dropping the margin turns the suite red instead of silently narrowing the interval. Redis-backed distributed drift monitor with a familywise-corrected alarm rate, weekly scheduled retraining, Prometheus observability, Kubernetes manifests.

**Stack:** XGBoost · FastAPI · Streamlit · Redis · Prometheus · Docker · Kubernetes · GitHub Actions · HuggingFace

**Engineering signals:** 509 tests · 88% coverage gate (91.33% actual) · **every published metric is pinned to `model_metrics.json`** — corrupt a number in the README or model card and CI fails · every hyper-parameter has a committed producer and a recorded search, read as a tie rather than a win because the margin sits inside build-to-build noise · artifact integrity gate that refuses to start on a digest mismatch *or* a missing manifest · `/predict`, `/drift` and `/metrics` all behind the same key, auth resolving before the rate limiter · /predict p99 < 200ms SLO enforced in CI · training reproduces bit-identically from a clean tree · Dependabot + pip-audit CVE gate

---

## Open-source libraries

| Package | What it does |
|---|---|
| [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) | Vendor-neutral RAG + LLM serving infrastructure: swappable LLM protocol and vector store, cached embedding index, budget-aware multi-provider fallback, OpenTelemetry tracing |
| [`schema-firewall`](https://pypi.org/project/schema-firewall/) | Three checks — `check_leakage`, `check_schema`, `check_stateless` — for the data leakage and schema bugs that slip past peer review, documented against JAMA, Nature Communications, and Kaggle Santander patterns. v0.2.1 restored exhaustive tail sampling after a 0.2.0 optimisation quietly capped it to the 20 highest-variance columns and re-opened a fail-open — a cross-row edit on a low-variance column was being missed on 18 of 20 seeds |

---

## Stack

```
LLM / AI      Python · OpenAI API · HuggingFace · sentence-transformers · XGBoost · PyTorch · scikit-learn
MLOps         Docker · Kubernetes · Helm · GitHub Actions · Prometheus · OpenTelemetry · DVC · MLflow
Backend       FastAPI · Pydantic v2 · Redis · MongoDB · FAISS · Qdrant
Security      Trivy · CycloneDX SBOM · Dependabot · pip-audit
```

---

*Open to founding-engineer or staff IC roles in production AI and MLOps. NYC.*
