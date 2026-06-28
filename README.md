# Marwa Bensalem

**Senior AI/ML Engineer** · Production LLM Systems · RAG · MLOps

I build production AI systems that verify their own outputs. Four years running enterprise HRIS/payroll at ADP taught me what breaks at 3am — and why most "AI tools" aren't production systems. Now I build the kind that are.

📫 `marwabensalem30@gmail.com` · 🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem) · 📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

---

## Shipped systems

### ⚖️ [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) · [Live demo →](https://huggingface.co/spaces/MarwaBS/job-decision-engine)
Deterministic 5-signal job scorer with a bounded LLM reasoning layer. Same input → same output, always — determinism verified to 1e-9 across local, CI, and HuggingFace production. 261 hermetic tests in ~1s. Evaluation gate locked until 50 real outcomes accumulate (no fake metrics).

**Stack:** Pydantic v2 · sentence-transformers · OpenAI GPT-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · HuggingFace Spaces

**Engineering signals:** Architecture-as-Contract · append-only audit log · protocol-based LLM/DB abstractions · CI: privacy audit → tests → lint → auto-deploy

---

### 🧠 [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform) · [Live demo →](https://resumeforge-bg29.onrender.com)
Multi-tenant RAG infrastructure grounding every generated claim in the user's own input. Full production observability: hallucination guards, drift detection, cost circuit-breaker. Published as [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) on PyPI.

**Stack:** FastAPI · LangChain · OpenAI · FAISS/Qdrant · Redis · MongoDB · Pydantic v2 · OpenTelemetry · Prometheus · Docker · Kubernetes · Helm · GitHub Actions

**Engineering signals:** 7-job CI (Trivy scan · CycloneDX SBOM · perf regression gate · Dependabot · privacy audit) · vendor-neutral LLMProtocol + VectorStoreProtocol · Helm + HPA on Kubernetes · 90%+ test coverage

---

### 📊 [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor)
4-model ensemble (XGBoost · LightGBM · Random Forest · PyTorch) on 4,504 NYC listings. **Honest R² = 0.815** — I found and documented my own data leakage that inflated v1 to R²=0.997, then extracted the fix as [`schema-firewall`](https://pypi.org/project/schema-firewall/) on PyPI.

**Stack:** XGBoost · PyTorch · scikit-learn · Optuna · SHAP · FastAPI · Streamlit · DVC · MLflow · Docker

**Engineering signals:** ADR-001 documents the leakage removal · CI-enforced leakage guard (test_no_leakage.py) · sealed external benchmark on NYC.gov 2024 data · 88%+ test coverage gate

---

### 💼 [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)
Multi-quantile XGBoost (P10/P50/P90) on BLS OEWS + Census microdata. Calibrated uncertainty, not point estimates. Distributed Redis-backed drift monitor, scheduled weekly retraining pipeline, Prometheus observability, Kubernetes manifests.

**Stack:** XGBoost · FastAPI · Streamlit · Redis · Prometheus · Docker · Kubernetes · GitHub Actions · HuggingFace

**Engineering signals:** 170+ tests · composite model provenance string · /predict p99 < 200ms SLO enforced in CI · Dependabot + pip-audit CVE gate · scheduled retraining via GitHub Actions

---

## Open-source libraries

| Package | What it does |
|---|---|
| [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) | Vendor-neutral RAG + LLM serving infrastructure: swappable LLM protocol and vector store, cached embedding index, OpenTelemetry tracing |
| [`schema-firewall`](https://pypi.org/project/schema-firewall/) | Three checks that catch the data leakage and schema bugs that slip past peer review — documented against JAMA, Nature Communications, and Kaggle Santander patterns |

---

## Stack

```
LLM / AI      Python · LangChain · OpenAI API · HuggingFace · sentence-transformers · XGBoost · PyTorch · scikit-learn
MLOps         Docker · Kubernetes · Helm · GitHub Actions · Prometheus · OpenTelemetry · DVC · MLflow
Backend       FastAPI · Pydantic v2 · Redis · MongoDB · FAISS · Qdrant
Security      Trivy · CycloneDX SBOM · Dependabot · pip-audit
```

---

*Open to founding-engineer or staff IC roles in production AI and MLOps. NYC.*
