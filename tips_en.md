# 🧠 Exam Heuristics Tips (EN)

Quick guide to spot correct/incorrect choices in Databricks GenAI‑style questions.

## Bias
- ❌ If an option says “remove training data/remove information,” it’s bad practice.
- ✅ Prefer “tag and filter at retrieval,” “balance datasets,” “audit bias,” “use fairness metrics.”
- Eliminate if: it proposes deleting information, ignoring bias metrics, or training only on a homogeneous subset.
- Likely correct if: it suggests tagging + Vector Search/metadata filters, bias evaluation, and balanced datasets.

## Hallucinations
- ❌ “Increase temperature” or “trust the LLM blindly.”
- ✅ “Use RAG with verified sources,” “fact verification,” “LLM‑as‑a‑judge,” “cite sources,” “improve/structure prompts.”
- Eliminate if: no external context, ignores citations, or only tweaks temperature as the fix.
- Likely correct if: adds RAG, verifies with sources, cites docs, or adds fact‑checking guardrails.

## Cost
- ❌ “Always use the largest model” or “don’t measure tokens.”
- ✅ “Use smaller models when applicable,” “response caching,” “prompt compression,” “monitor tokens and cost/request,” “scale‑to‑zero.”
- Eliminate if: no cost/token measurement, or fixed 24/7 infra without need.
- Likely correct if: instruments cost metrics, uses auto‑scaling, and selects model by cost/benefit.

## Latency
- ❌ “Add unnecessary steps without parallelism.”
- ✅ “Parallelize chains,” “use embeddings/Vector Search to reduce context,” “serve real‑time with auto‑scaling,” “optimize batch/quantization (if self‑hosted).”
- Eliminate if: loads the entire corpus into the prompt or uses a single unsized queue.
- Likely correct if: reduces context via semantic retrieval and uses auto‑scalable infra.

## Availability
- ❌ “Single endpoint without scaling/observability.”
- ✅ “Auto‑scaling,” “A/B or canary,” “monitoring + alerts,” “controlled fallbacks.”
- Eliminate if: no monitoring or redundancy.
- Likely correct if: endpoints with auto‑scaling, A/B, and Inference Table capture.

## Drift
- ❌ “Ignore drift” or “retrain without evidence.”
- ✅ “Lakehouse Monitoring (profile/drift/custom),” “retrain triggers with thresholds,” “versioning and comparison.”
- Eliminate if: arbitrary retraining or no baseline comparison.
- Likely correct if: thresholds + retraining plan based on metrics.

## Quality
- ❌ “Measure only one metric” or “only loss/perplexity.”
- ✅ “Task metrics (BLEU/ROUGE) + relevance/faithfulness,” “human feedback,” “benchmarks (MMLU, HellaSwag).”
- Eliminate if: misuses metrics (e.g., BLEU for classification) or ignores feedback.
- Likely correct if: combines technical and product metrics (relevance, satisfaction).

## Security/Guardrails
- ❌ “Trust user instructions” or “allow PII/sensitive output.”
- ✅ “Input/output guardrails (Llama Guard, PII checks, toxicity),” “prompt safety,” “rate limiting,” “auditing.”
- Eliminate if: no input/output filtering or disabling controls.
- Likely correct if: integrates safety classification before and after the LLM.

## Retrieval (RAG)
- ❌ “Increase context with no chunking/filters,” “mix different embedding models for docs/queries.”
- ✅ “Chunking with overlap,” “metadata filters,” “same model for docs and queries,” “reranking when needed.”
- Eliminate if: dumps full documents into the prompt or skips metadata filters.
- Likely correct if: Vector Search + filters + rerank.

## Embeddings/Vector Search
- ❌ “Choose embeddings ignoring language/domain.”
- ✅ “Multilingual/domain‑fit embeddings,” “Z‑Ordering/Data Skipping,” “Vector Search with filters.”
- Eliminate if: uses different embedding models across index and query.
- Likely correct if: keeps the same vector space and optimizes storage/query.

## Model Choice
- ❌ “Always proprietary external” or “always open source.”
- ✅ Decide by privacy, quality, cost, latency.
- Eliminate if: ignores compliance or time‑to‑market.
- Likely correct if: aligns model type to case (internal privacy vs speed/quality).

## Governance (Unity Catalog)
- ❌ “Ad‑hoc permissions in each system.”
- ✅ “Central ACLs in UC,” “lineage/auditing,” “Registry with aliases (@champion/@challenger).”
- Eliminate if: no centralized control or traceability.
- Likely correct if: uses UC for permissions and end‑to‑end lineage.

## Evaluation
- ❌ “No reference datasets nor clear metrics.”
- ✅ “Offline + online,” “LLM‑as‑a‑judge when references lack,” “A/B and canary.”
- Eliminate if: decides without evidence or with a single isolated metric.
- Likely correct if: triangulates benchmarks, feedback, and controlled experiments.

## Privacy & Compliance
- Eliminate if: sends sensitive data to external APIs without guarantees or ignores GDPR/CCPA.
- Likely correct if: proposes self‑hosted/UC‑secured models, anonymization/pseudonymization, and UC access control.

## Prompt Engineering (patterns)
- Eliminate if: vague prompt without role/instructions/format.
- Likely correct if: includes role, context, clear input and format; uses delimiters; few‑shot for structured output.

## Chunking vs Reranking
- Eliminate if: only “larger chunks” to improve retrieval.
- Likely correct if: chunking with overlap + filters; and reranking when top‑K has noise.

## Vector DB vs Plugins
- Eliminate if: chooses a plugin with no governance/scale needs.
- Likely correct if: dedicated vector DB (Mosaic AI Vector Search) for CRUD, filters, ACLs, scalability.

## MLflow & Registry
- Eliminate if: deploys without versioning or aliases.
- Likely correct if: Registry in UC, aliases (@champion/@challenger), lineage for auditability.

## Traffic/A‑B/Canary
- Eliminate if: sends 50% to an unknown model initially.
- Likely correct if: starts with 5–10% canary and scales by metrics.

## Caching & Retries
- Eliminate if: re‑queries identical prompts without cache or uses infinite retries.
- Likely correct if: cache for frequent prompts, retries with backoff and limits.

## Decoding (temperature/top_p)
- Eliminate if: raises temperature to “fix” factual accuracy.
- Likely correct if: lower temperature for factual tasks; adjust top_p/top_k thoughtfully.

## Context Window / Lost in the Middle
- Eliminate if: dumps full documents into the prompt.
- Likely correct if: RAG + chunking + relevant passage selection.

## Multilingual
- Eliminate if: embeddings/model don’t support the target language.
- Likely correct if: embeddings and LLM aligned to language/domain.

## Monitoring – Thresholds
- Eliminate if: monitoring without thresholds/alerts.
- Likely correct if: defines p95/p99 latency, drift thresholds and alerts; logs to Inference Tables.
