# Marwa Bensalem

**Senior AI/ML Engineer** · Production LLM Systems · RAG · MLOps

I build production AI systems that verify their own outputs. Four years running enterprise HRIS/payroll at ADP taught me what breaks at 3am — and why most "AI tools" aren't production systems. Now I build the kind that are.

📫 `marwabensalem30@gmail.com` · 🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem) · 📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

---

## Shipped systems

### ⚖️ [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) · [Live demo →](https://huggingface.co/spaces/MarwaBS/job-decision-engine)
Job scorer with a deterministic core and a bounded LLM reasoning layer. On the LLM-free path (the public demo): same input → same output, every time — verified to 1e-9 in local and CI runs. The LLM signal is capped at 25% of the score, and the UI banner is reconciled against a live API ping at boot — it can never claim an LLM that isn't actually answering. 350 hermetic tests in under 3 seconds. Evaluation gate stays locked until 50 real outcomes accumulate (no fake metrics).

**Stack:** Pydantic v2 · sentence-transformers · OpenAI GPT-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · HuggingFace Spaces

**Engineering signals:** append-only audit log — every decision recorded with its exact signals and weights, so any past verdict can be re-derived · protocol-based LLM/DB abstractions · CI: privacy audit → tests → lint → auto-deploy to the Space · weekly scheduled security re-audit + live-Space health probe

---

### 🧠 [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform)
Runnable reference RAG service built on my published [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) package: swappable vector store, API-key-protected data plane, Prometheus metrics, structured JSON logs, and a retrieval-recall eval gate in CI that can actually fail. The README draws an explicit public/private boundary — the private flagship built on the same design (ResumeForge) stays private; everything claimed in this repo runs with `pip install`.

**Stack:** FastAPI · rag-llm-infra · OpenAI · FAISS/Qdrant (optional extras) · Pydantic v2 · Prometheus · Docker · Kubernetes · Helm · GitHub Actions

**Engineering signals:** CI: tests → retrieval-recall eval floor → Trivy image scan → CycloneDX SBOM · vendor-neutral LLMProtocol + VectorStoreProtocol · Helm chart with HPA, ingress off by default · optional backends fail fast with the exact `pip install` hint

---

### 📊 [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/nyc-real-estate-predictor)
XGBoost · LightGBM · Random Forest compared on a validation split (fixed hyperparameters — no tuning) on 4,526 NYC listings. **Honest test R² = 0.835** (0.814 ± 0.028 over 20 seeds) — I found and documented my own data leakage that had inflated v1 to R² = 0.997 (ADR-001), then extracted the fix as [`schema-firewall`](https://pypi.org/project/schema-firewall/) on PyPI. Anyone can re-verify me: the external benchmark re-runs against public NYC.gov 2024 Rolling Sales data (18,321 real sales) under a sealed schema contract.

**Stack:** XGBoost · LightGBM · scikit-learn · SHAP · category-encoders · FastAPI · Streamlit · MLflow · Docker

**Engineering signals:** CI-enforced leakage guard (test_no_leakage.py) · SHA256-manifest model registry — the live Space serves exactly the audited artifacts, checked weekly by a drift guard · 78% coverage gate (~89% actual) · 29-mutation harness proving every gate can fail · reproducible external benchmark in CI

---

### 💼 [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)
Multi-quantile XGBoost (P10/P50/P90) on BLS OEWS + US Census microdata. Calibrated uncertainty, not point estimates. Redis-backed distributed drift monitor, weekly scheduled retraining pipeline, Prometheus observability, Kubernetes manifests.

**Stack:** XGBoost · FastAPI · Streamlit · Redis · Prometheus · Docker · Kubernetes · GitHub Actions · HuggingFace

**Engineering signals:** 197 tests · 88% coverage gate (92% actual) · /predict p99 < 200ms SLO enforced in CI · release artifacts machine-checked against the serving config · Dependabot + pip-audit CVE gate · auto-deploy to the Space with a weekly drift guard

---

## Open-source libraries

| Package | What it does |
|---|---|
| [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) | Vendor-neutral RAG + LLM serving infrastructure: swappable LLM protocol and vector store, cached embedding index, budget-aware multi-provider fallback, OpenTelemetry tracing |
| [`schema-firewall`](https://pypi.org/project/schema-firewall/) | Three checks that catch the data leakage and schema bugs that slip past peer review — documented against JAMA, Nature Communications, and Kaggle Santander patterns |

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
