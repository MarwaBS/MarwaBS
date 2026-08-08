# Marwa Bensalem

**Senior AI/ML Engineer** · Production LLM Systems · RAG · MLOps

I build production AI systems that verify their own outputs. Four years running enterprise HRIS/payroll at ADP taught me what breaks at 3am — and why most "AI tools" aren't production systems. Now I build the kind that are.

📫 `marwabensalem30@gmail.com` · 🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem) · 📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

---

## Shipped systems

### ⚖️ [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) · [Live demo →](https://huggingface.co/spaces/MarwaBS/job-decision-engine)
Job scorer with a deterministic core and a bounded LLM reasoning layer. On the LLM-free path (the public demo): same input → same output, every time — verified to 1e-9 in local and CI runs. The LLM signal is capped at 25% of the score, and the UI banner is reconciled against a live API ping at boot — it can never claim an LLM that isn't actually answering. 350 hermetic tests in about 3 seconds. Evaluation gate stays locked until 50 real outcomes accumulate (no fake metrics).

**Stack:** Pydantic v2 · sentence-transformers · OpenAI GPT-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · HuggingFace Spaces

**Engineering signals:** append-only audit log — every decision recorded with its exact signals and weights, so any past verdict can be re-derived · protocol-based LLM/DB abstractions · CI: privacy audit → tests → lint → auto-deploy to the Space · weekly scheduled security re-scan + live-Space health probe

---

### 🧠 [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform)
Runnable reference RAG service built on my published [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) package: swappable vector store, API-key-protected data plane, Prometheus metrics, structured JSON logs, and a retrieval-recall eval gate in CI that can actually fail. The README draws an explicit public/private boundary — a separate private product built on the same design stays private; everything claimed in this repo runs with `pip install`.

**Stack:** FastAPI · rag-llm-infra · Pydantic v2 · NumPy retrieval (FAISS/Qdrant optional) · SentenceTransformers (optional) · OpenAI (optional) · Prometheus · Docker · Kubernetes · Helm · GitHub Actions

**Engineering signals:** the test step is a checker that starts the run itself and reads the report *that run wrote*, so a skipped or forged suite fails instead of passing green · the chunk window and the eval floors are derived by committed scripts and re-derived byte-identical in CI · CI starts the built image and exercises its API, then publishes the image it scanned rather than rebuilding one · vendor-neutral LLMProtocol + VectorStoreProtocol · the ingress refuses to render without TLS and an API key · optional backends fail fast with the exact `pip install` hint

---

### 📊 [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/nyc-real-estate-predictor)
XGBoost · LightGBM · Random Forest compared on a validation split (fixed hyperparameters — no tuning) on 4,526 NYC listings. **Honest test R² = 0.835** (0.814 ± 0.028 over 20 seeds) — I found and documented my own data leakage that had inflated v1 to R² = 0.997 (ADR-001), then extracted the fix as [`schema-firewall`](https://pypi.org/project/schema-firewall/) on PyPI. Anyone can re-verify me: the external benchmark re-runs against public NYC.gov 2024 Rolling Sales data (18,321 real sales) under a sealed schema contract.

**Stack:** XGBoost · LightGBM · scikit-learn · SHAP · category-encoders · FastAPI · Streamlit · MLflow · Docker

**Engineering signals:** CI-enforced leakage guard (test_no_leakage.py) · SHA256-manifest model registry — the live Space serves exactly the audited artifacts, checked weekly by a drift guard · 304 tests, 85% coverage gate (89.89% actual) · **68-mutation harness** — CI breaks one behaviour at a time and fails the build unless a named test catches it; entries were added each time a gate turned out to be walkable, so the registry is a record of what a passing suite had already missed · reproducible external benchmark in CI

---

### 💼 [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)
One multi-quantile XGBoost model (P10/P50/P90 from a single artifact) on BLS OEWS + US Census microdata. Calibrated uncertainty, not point estimates: the served interval is widened by a cross-conformal margin estimated on train-only folds, and the route that applies it is pinned by tests — dropping the margin turns the suite red instead of silently narrowing the interval. Redis-backed distributed drift monitor with a familywise-corrected alarm rate, weekly scheduled retraining, Prometheus observability, Kubernetes manifests.

**Stack:** XGBoost · FastAPI · Streamlit · Redis · Prometheus · Docker · Kubernetes · GitHub Actions · HuggingFace

**Engineering signals:** 648 tests · 88% coverage gate (92.81% actual) · **every published metric is pinned to the file that produced it** — corrupt a number in the README, the model card or the design record and CI fails, and the README's data findings are recomputed from the CSV · every hyper-parameter has a committed producer and a recorded search, read as a tie rather than a win because the margin sits inside build-to-build noise · artifact integrity gate that refuses to start on a digest mismatch *or* a missing manifest · `/predict`, `/predict/batch`, `/drift` and `/metrics` all behind the same key, auth resolving before the rate limiter · /predict p99 < 200ms SLO enforced in CI · training reproduces bit-identically **on the same machine** — across machines the drift is unmeasured, and the repo says so rather than claiming otherwise · Dependabot + pip-audit CVE gate

---

### 🔥 [schema-firewall](https://github.com/MarwaBS/schema-firewall) · [PyPI →](https://pypi.org/project/schema-firewall/)
Three checks — `check_leakage`, `check_schema`, `check_stateless` — for the leakage and schema bugs that pass peer review. 496 lines of implementation under a 500-line budget a test enforces, three dependencies, four Python versions in CI. `tools/planted_defects.py` registers 20 failure modes, and running it disables each behaviour in a throwaway copy and requires the named tests to go red — 20 of 20 caught, controls green. A suite test fails the build if the registry, the docs and the tests drift apart. The claim is deliberately scoped: the check runs from the registry outwards, so it is a coverage floor for those 20 modes — not a completeness proof, and not a mutation score.

**Stack:** numpy · pandas · scikit-learn — and nothing else, by design

**Engineering signals:** 125 tests at 97.06% branch coverage · LoC budget, dependency count and public surface all test-enforced, so the design constraints cannot rot silently · consumed downstream as a pinned dependency by the NYC benchmark, which re-runs it weekly against public NYC.gov data · the pin stays at 0.1.3 by documented decision, because 0.2.x changed the MI binning and the threshold would need re-measuring first

---

## Open-source libraries

| Package | What it does |
|---|---|
| [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) | Vendor-neutral RAG + LLM serving infrastructure: swappable LLM protocol and vector store, cached embedding index, budget-aware multi-provider fallback, OpenTelemetry tracing |
| [`schema-firewall`](https://pypi.org/project/schema-firewall/) | The three checks above, documented against JAMA, Nature Communications, and Kaggle Santander patterns. v0.2.1 restored exhaustive tail sampling after a 0.2.0 optimisation quietly capped it to the 20 highest-variance columns and re-opened a fail-open — a cross-row edit on a low-variance column was being missed on 18 of 20 seeds |

---

## Stack

```
LLM / AI      Python · OpenAI API · HuggingFace · sentence-transformers · XGBoost · scikit-learn
MLOps         Docker · Kubernetes · Helm · GitHub Actions · Prometheus · OpenTelemetry · MLflow
Backend       FastAPI · Pydantic v2 · Redis · MongoDB · FAISS · Qdrant
Security      Trivy · CycloneDX SBOM · Dependabot · pip-audit
```

---

*Open to founding-engineer or staff IC roles in production AI and MLOps. NYC.*
