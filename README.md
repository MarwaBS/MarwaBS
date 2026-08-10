# Marwa Bensalem

**Senior AI/ML Engineer** · Production LLM Systems · RAG · MLOps

I build production AI systems that check their own outputs. Four years on enterprise HRIS/payroll at ADP taught me what breaks at 3am. Most "AI tools" are not production systems. I build the kind that are.

📫 `marwabensalem30@gmail.com` · 🌐 [linkedin.com/in/marwabensalem](https://www.linkedin.com/in/marwabensalem) · 📦 PyPI: [schema-firewall](https://pypi.org/project/schema-firewall/) · [rag-llm-infra](https://pypi.org/project/rag-llm-infra/)

| Shipped | What it is | Proof |
|---|---|---|
| [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) | Price model that caught its own data leakage | 307 tests · 89.89% · 71 planted defects, all caught |
| [schema-firewall](https://pypi.org/project/schema-firewall/) | Published PyPI package. 3 checks, 499 lines | 126 tests · 97.10% · 20 of 20 defects caught |
| [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) | Serves P10/P50/P90 ranges, not point estimates | 658 tests · 92.84% · 0 quantile crossings |
| [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform) | Reference RAG service on my own published package | 253 tests · 98.15% |
| [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) | Job scorer with a bounded LLM layer | 350 tests in ~3s · LLM capped at 25% of the score |

Every number above comes from a committed file in its repo, and each repo fails its own build if its docs and that file disagree. This page is a hand-kept copy of those numbers, so treat the repo as the source of truth.

---

## Shipped systems

*Each entry below is a failure I found in my own work, and what I changed. Skip to the bold numbers if you are short on time.*

### 📊 [NYC Real Estate Predictor](https://github.com/MarwaBS/nyc-real-estate-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/nyc-real-estate-predictor)

My first version scored R² = 0.997. It was wrong. `PRICE_PER_SQFT` was in the feature list, so the model was reading the target back out of its own input. I wrote that up as ADR-001 and removed the feature. The honest number came back at **0.835**, or 0.814 ± 0.028 across 20 seeds. Then I pulled the guard out into [`schema-firewall`](https://pypi.org/project/schema-firewall/) so it could not happen to me again.

Four more things this repo taught me the hard way.

Every estimator was built with `n_jobs=-1`. Thread count decides the order the float sums land, so two runs of one commit on the same CI runner scored val R² 0.7740 and 0.7719. The top two candidates sat 0.0029 apart. That noise was enough to flip which model shipped, so a Linux runner published LightGBM while my laptop published XGBoost. The fix was `n_jobs=1` and recording the choice instead of re-deriving it every run.

The headline is also scored against a capped target. IQR bounds are fitted on train, correctly, but they apply to every row, so 72 of 906 test prices are clipped before scoring. Against listed prices the same model gets **0.7883**. Both numbers sit next to each other in the README, because only quoting the better one would be a lie by omission.

The gate that watches the external benchmark only ever compared R². An R² over 100 rows reads exactly like one over 18,321, so a run that quietly scored a different set of rows would have passed. I replayed it against the committed result and it returned pass at 12.7%, 42.9%, 50.0% and 99.5% loss of the scored population. It now bands the row count and requires the set of drop reasons to match. A live run then moved the population 0.9%, which is the real drift the band has to tolerate.

My leakage guard checks feature *names* for the word price. That is all it can do. Two of my columns, ZIP and sublocality, are target encoded, so they are built from price and the guard cannot see them. The README used to read that guard as proof of no price derived columns. It now says both facts: the guard is a name check, and those two columns are fitted on the train split only, inside the pipeline, so a row never contributes to its own encoding. A test holds that sentence to the encoder config.

**Stack:** XGBoost · LightGBM · scikit-learn · SHAP · category-encoders · FastAPI · Streamlit · MLflow · Docker

**Engineering signals**

- **307 tests**, 85% coverage gate, 89.89% actual
- **71-mutation harness.** Each one breaks a single behaviour and the build fails unless a named test catches it. Every entry exists because a gate turned out to be walkable, so the registry is a list of what a green suite had already missed
- SHA256-manifest model registry, so the live Space serves the audited artifacts, checked weekly
- External benchmark against public NYC.gov 2024 Rolling Sales, **18,321 real sales** under a sealed schema contract. CI fails if the recomputed score, the number of rows scored, or the set of reasons rows were dropped leaves its band

---

### 🔥 [schema-firewall](https://github.com/MarwaBS/schema-firewall) · [PyPI →](https://pypi.org/project/schema-firewall/)

Three checks: `check_leakage`, `check_schema`, `check_stateless`. 499 lines. Three dependencies. Four Python versions in CI.

The interesting part is not the checks. It is that this library has shipped three fail-opens of its own, and all three are recorded in the changelog rather than quietly patched. In 0.2.0 a speed change capped tail sampling to the 20 highest-variance columns. That re-opened a hole 0.1.3 had closed. A cross-row edit on a low-variance column was missed on about 18 of 20 seeds. A second shared a NaN sampling budget across columns, so the dirtiest column spent it and the column that leaked went unchecked. A third only sampled the frame the caller passed in, never the frame the pipeline returned, so winsorising a derived ratio slipped through on 14 of 40 seeds and a `duplicated()` flag on 40 of 40.

That is why `tools/planted_defects.py` exists. It registers 20 failure modes, turns each behaviour off in a throwaway copy, and requires the named tests to go red. 20 of 20 caught, controls green. A suite test fails the build if the registry, the docs and the tests drift apart.

The claim is scoped on purpose. The check runs from the registry outwards, so it is a coverage floor for those 20 modes. Not a completeness proof. Not a mutation score.

**Stack:** numpy · pandas · scikit-learn, and nothing else, by design

**Engineering signals**

- **126 tests**, 97.10% branch coverage
- The 500-line budget, the dependency count and the public surface are each pinned by a test, so the design limits cannot rot quietly
- Used downstream as a pinned dependency by the NYC benchmark. That pin stays at 0.1.3 by recorded decision, because 0.2.x changed the MI binning and the threshold would need re-measuring first

---

### ⚖️ [Job Decision Engine](https://github.com/MarwaBS/Job_Decision_Engine) · [Live demo →](https://huggingface.co/spaces/MarwaBS/job-decision-engine)

Job scorer with a fixed core and a bounded LLM layer. On the default LLM-absent path, which is what the public demo runs, the same input gives the same output every time, verified to 1e-9 in local and CI runs. With a key set, one signal comes from a live model and that path is not deterministic. The LLM signal is capped at 25% of the score. The UI banner is checked against a live API ping at boot, so it cannot claim an LLM that is not answering. 350 isolated tests in about 3 seconds. The evaluation gate stays locked until 50 real outcomes arrive, so no metric is invented.

**Stack:** Pydantic v2 · sentence-transformers · OpenAI GPT-4o · MongoDB Atlas · Streamlit · Docker · GitHub Actions · HuggingFace Spaces

**Engineering signals**

- Append-only audit log. Every decision is recorded with its signals and weights, so any past verdict can be rebuilt
- Protocol-based LLM and DB interfaces
- CI runs a privacy audit, then tests and lint, then deploys
- Weekly security re-scan and a live-Space health probe

---

### 💼 [Salary Quantile Predictor](https://github.com/MarwaBS/high-pay-salary-predictor) · [Live demo →](https://huggingface.co/spaces/MarwaBS/high-pay-salary-predictor)

One multi-quantile XGBoost model serving P10, P50 and P90 from a single artifact, on BLS OEWS and US Census microdata. It returns calibrated ranges, not point estimates. The served interval is widened by a cross-conformal margin estimated on train-only folds, and the route that applies it is pinned by tests. Drop the margin and the suite goes red instead of quietly narrowing the interval.

It also ships a Redis-backed distributed drift monitor with a familywise-corrected alarm rate, weekly scheduled retraining, Prometheus observability and Kubernetes manifests.

Training repeats bit-identically on the same machine. Across machines the drift is unmeasured, and the repo says so.

The repo used to record that its classifier lost to a logistic baseline, 0.6735 against 0.68. I found the comparison was unfair to my own model: the baseline was fitted once, on one split, while the classifier was scored across five. Refit on the same five splits they are **0.6958 ± 0.0075** and **0.6901 ± 0.0071**, a gap smaller than either spread. So neither one ranks better, and the shortfall I had been publishing was an artifact of how I measured, not a property of the model.

**Stack:** XGBoost · FastAPI · Streamlit · Redis · Prometheus · Docker · Kubernetes · GitHub Actions · HuggingFace

**Engineering signals**

- **658 tests**, 88% coverage gate, 92.84% actual
- **Every published metric is pinned to the file that produced it.** Corrupt a number in the README, the model card or the design record and CI fails
- Every hyper-parameter has a committed producer and a recorded search, read as a tie rather than a win because the margin sits inside build-to-build noise
- Artifact integrity gate that refuses to start on a digest mismatch or a missing manifest
- `/predict`, `/predict/batch`, `/drift` and `/metrics` all behind the same key, with auth resolving before the rate limiter
- **p99 under 200ms** on `/predict`, enforced in CI, plus Dependabot and a pip-audit CVE gate

---

### 🧠 [Production RAG Platform](https://github.com/MarwaBS/production-rag-platform)

Runnable reference RAG service built on my published [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) package. Swappable vector store. API-key-protected data plane. Prometheus metrics and structured JSON logs. The retrieval-recall eval gate in CI can actually fail.

A committed measurement in this repo did not reproduce. The recall curve came back at 0.75 on my machine every time and 0.8333 in CI, on code that had not changed. The cause was not the retrieval. At ten thousand documents, 11 of the 12 queries tie **exactly** at the rank-3 cut, because distractors built from the same vocabulary hash into the same buckets as the gold document and the vectors come out identical. `np.argpartition` orders equal scores arbitrarily, so one gold sat at rank 4 on one machine and rank 3 on another. The number I had published was a property of the tie order, not of the system. A gold now has to beat the fourth score outright, which is the same value whichever tied document holds the slot. The curve is still 0.75. It is just defined now.

Moving to the 0.2 line of my own library broke the Qdrant backend, and my local run could not see it. That release removed the default collection, because `add()` replaces the collection's contents and the library stopped guessing a name two services could share. Neither optional backend is installed on my machine, so the test took the boot-guard branch and reported green. The CI job that installs them caught it. That is the value of a job that builds the thing for real.

The README draws a clear public/private line. A separate private product is built on the same design and stays private. Everything claimed in this repo runs with `pip install`.

**Stack:** FastAPI · rag-llm-infra · Pydantic v2 · NumPy retrieval (FAISS/Qdrant optional) · SentenceTransformers (optional) · OpenAI (optional) · Prometheus · Docker · Kubernetes · Helm · GitHub Actions

**Engineering signals**

- **253 tests**, 93% coverage gate, 98.15% actual
- The test step starts the run itself and reads the report that run wrote, so a skipped or faked suite fails instead of passing green
- The chunk window, the eval floors and the scale curve are derived by committed scripts. CI re-runs each producer and fails unless it returns the committed values
- CI starts the built image in the configuration the Helm chart deploys, requires a missing key and a wrong key to both return 401 before it will exercise the keyed routes, then publishes the image it scanned
- A separate job installs the optional backends and requires each one to construct, because a backend that is only ever refused is a backend nobody has run
- Vendor-neutral LLM and vector-store interfaces. The ingress refuses to serve without TLS and an API key

---

## Open-source libraries

| Package | What it does |
|---|---|
| [`rag-llm-infra`](https://pypi.org/project/rag-llm-infra/) | Vendor-neutral RAG and LLM serving infrastructure: swappable LLM interface and vector store, cached embedding index, budget-aware multi-provider fallback, OpenTelemetry tracing. 0.2 added the credential, the request and corpus bounds, and the `py.typed` marker, so callers get their types checked instead of `Any` |
| [`schema-firewall`](https://pypi.org/project/schema-firewall/) | The three checks above, written against JAMA, Nature Communications and Kaggle Santander patterns |

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
