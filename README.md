# Marwa Bensalem
**AI/ML Engineer · Production Systems · MLOps**
Building production-grade ML systems — evidence-grounded LLMs, calibrated quantile
models, hybrid decision engines — with the MLOps surface (observability,
supply-chain, drift) that keeps them honest.


📫 `marwabensalem30@gmail.com`  ·  🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem)  ·  📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)


---


## Featured projects


### 🧠 Production RAG Platform — evidence-grounded LLM generation
**Runnable reference service + deployment:** [production-rag-platform](https://github.com/MarwaBS/production-rag-platform)  ·  **Open infra on PyPI:** [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)  ·  **Live demo:** [resumeforge-bg29.onrender.com](https://resumeforge-bg29.onrender.com)


A production retrieval-augmented generation service that grounds every generated
claim in the user's own input, served behind a typed FastAPI gateway with full
delivery infrastructure.


**Stack:** FastAPI · Pydantic v2 · Redis · FAISS / Qdrant · OpenAI · OpenTelemetry · Prometheus · Helm / Kubernetes · Docker
**Highlights:** a **runnable reference service** (FastAPI: typed config · Prometheus `/metrics` · `/health` + `/ready` · integration tests) built on the published `rag-llm-infra` package — vendor-neutral `LLMProtocol` + `VectorStoreProtocol` (NumPy default · FAISS / Qdrant optional), whose own CI enforces **two eval gates** (retrieval recall@1/MRR + generation faithfulness) · **this repo's CI/CD:** lint · mypy · integration tests · Helm lint + render · hadolint · **Trivy image scan → CycloneDX SBOM → push to GHCR** · Helm chart (Deployment / HPA / PDB / Ingress / ServiceAccount) · OpenTelemetry tracing + structured JSON logs
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

