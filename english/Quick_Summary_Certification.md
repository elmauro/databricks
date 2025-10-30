# 🚀 Quick Summary – Databricks GenAI Engineer Associate Certification

## 📌 Use: Fast Reference Before the Exam

---

## 1️⃣ FUNDAMENTALS (25% of exam)

### Key Concepts
- **Generative AI** = Create new content (text, image, code, audio, video)
- **LLMs** = Large models trained on billions of tokens
- **Pre-training** = General knowledge | **Fine-tuning** = Specialization

### Important LLMs
| Model | Type | Note |
|------|------|------|
| **DBRX** | Databricks OSS | Mixture-of-Experts |
| **GPT-4** | OpenAI Proprietary | Best overall quality |
| **LLaMA 3** | Meta OSS | 8B, 70B params |
| **Dolly** | Databricks | First enterprise OSS |

### Databricks Stack
```
Unity Catalog → Centralized governance, ACLs, Lineage, Audit
Delta Lake → ACID Transactions, Time Travel, Auto-Optimization
Vector Search → Semantic search, Auto-sync with Delta Tables
MLflow → Tracking, Registry, Evaluation, AI Gateway
Model Serving → Production (APIs), Auto-scaling, Inference Tables
Mosaic AI → Foundation Models, Agent Framework, AI Playground
Delta Live Tables → ETL with Quality Expectations
Feature Serving → Real-time features
```

### Unity Catalog (Governance)
- **Layered ACLs**: Data (SELECT, INSERT), AI/ML (EXECUTE, MODIFY), Infra (CREATE, MANAGE)
- **Automatic Lineage**: Tracks origin and usage of data/models
- **Audit**: Logs all accesses (who, when, what)
- **Compliance**: GDPR, HIPAA, SOC2

### Delta Lake (Storage)
- **ACID** properties
- **Time Travel**: `VERSION AS OF` or `TIMESTAMP AS OF`
- **Auto Optimize**: Compaction, Z-Ordering, Data Skipping
- **Vacuum**: Clean old versions
- **For GenAI**: Store embeddings, inference logs, chunks

### Risks
- **Hallucination**
- **Bias**
- **Prompt Injection**
- **Privacy leaks**

---

## 2️⃣ SOLUTION DEVELOPMENT (30%)

### Prompt Engineering
**Techniques**: Zero-shot, Few-shot, Chain-of-Thought, Prompt Chaining

**Prompt Components**:
1. Instructions
2. Context
3. Input/Question
4. Output format

### RAG (Retrieval-Augmented Generation)
**Flow**: Query → Retrieve docs → Augment prompt → Generate

**Pros**: Up-to-date knowledge, reduces hallucinations

**Architecture**:
```
Documents → DLT → Chunking → Embeddings → 
Vector Search → Retrieval → LLM → Response
```

### Chunking
| Type | Description | Pros/Cons |
|------|-------------|-----------|
| **Fixed-size** | Fixed length | Simple, may cut context |
| **Context-aware** | By section/paragraph | Better context, more complex |
| **Overlap** | Overlaps chunks | Keeps continuity |

**Tip**: Use 10–20% overlap

### Embeddings
- Use the **SAME** model for docs and queries
- Common models: BGE-Large, E5, OpenAI Ada, Cohere Embed

### Vector Search (Databricks)
**Setup**:
1. Create Vector Search Endpoint
2. Create Model Serving Endpoint (embeddings)
3. Create Vector Search Index from Delta table

**Auto-sync with Delta Tables** ✅

### Reranking
- Reorders top-K for better precision
- Use when precision is critical (bge-reranker, Cohere Rerank, etc.)

### RAG Metrics
| Metric | What it Measures |
|--------|------------------|
| **Context Precision** | Are retrieved docs relevant? |
| **Context Recall** | Did we retrieve all relevant docs? |
| **Faithfulness** | Is the answer faithful to context? |
| **Answer Relevancy** | Does it answer the question? |
| **Answer Correctness** | Is it factually correct? |

---

## 3️⃣ APPLICATION DEVELOPMENT (20%)

### Compound AI Systems
- Multiple interacting components vs single-step systems
- Example: Query → Classifier → Tool 1 → Tool 2 → Aggregator → Response

### Frameworks
| Framework | Best For |
|-----------|----------|
| **LangChain** | General apps |
| **LlamaIndex** | Advanced RAG |
| **Haystack** | Document processing |
| **DSPy** | Automatic optimization |

### Agents
- Decide dynamically what to do
- **ReAct Pattern**: Thought → Act → Observe → ...
- Multi-Agent = specialized agents collaborating

### Multi-Modal AI
- Text → Image (DALL·E)
- Image → Text (GPT-4 Vision)
- Text + Image → Text (Claude Vision)
- **CLIP**: shared embedding space for text + image

---

## 4️⃣ DEPLOYMENT & MONITORING (15%)

### Deployment Methods
| Method | Latency | Best For |
|--------|---------|----------|
| **Batch** | Hours/days | Reports, ETL |
| **Streaming** | Seconds | Real-time events |
| **Real-Time** | <2s | Chatbots, APIs |
| **Edge** | ms | IoT, offline |

### MLflow
- Tracking: log experiments
- Registry (Unity Catalog): versioning, aliases, lineage, ACLs
- Pyfunc: generic interface for deployment

### Model Serving
- Supports: Custom (MLflow), Foundation Models, External Models
- Features: Auto-scaling, A/B testing, Inference Tables, Scale-to-zero

### Monitoring
- Lakehouse Monitoring: profile metrics, drift, custom metrics
- Monitor: input data, vector DB, human feedback, prompts/responses, latency, cost, quality

### MLOps vs LLMOps
- LLMOps adds prompts, chains, vector DBs, feedback, cost management

---

## 5️⃣ EVALUATION & GOVERNANCE (10%)

### Critical Issues
| Issue | Mitigation |
|------|------------|
| **Prompt Injection** | Guardrails, Llama Guard |
| **Hallucinations** | RAG, fact-checking |
| **Bias** | Audit data, fairness metrics |
| **Privacy** | ACLs, anonymization |

### Guardrails
- Input, Output, Contextual guardrails
- Llama Guard for unsafe content classification

### LLM Metrics
- **Base**: Loss, Perplexity, Toxicity
- **Task**: BLEU (translation), ROUGE (summarization)
- **Benchmarks**: MMLU, HellaSwag, HumanEval

### LLM-as-a-Judge
- Use when no ground truth; have an LLM assess another

### End-to-End Evaluation
- **Cost**: cost/request, GPU hours
- **Performance**: Latency P50/P95/P99, throughput
- **Quality**: CSAT, accuracy, task completion rate
- **Custom**: deflection rate, time saved

### Offline vs Online
- Offline: curated dataset, reproducible, pre-deployment
- Online: real users, A/B testing, production monitoring
- Strategy: Offline → Canary (10%) → Full prod

---

## 🎯 KEY NUMBERS
- Context windows: GPT-3.5 4K; GPT-4 8K/32K/128K; Claude 3 200K
- Embedding dims: BGE-Large 1024, Ada 1536, E5 768
- Latency targets: Chatbots <2s; Streaming <5s

---

## 📋 ESSENTIAL COMMANDS & SNIPPETS

### Unity Catalog (ACLs)
```sql
GRANT SELECT ON TABLE catalog.schema.table TO `group_name`;
GRANT EXECUTE ON MODEL catalog.schema.model TO `developers`;
SHOW GRANTS ON TABLE catalog.schema.customer_data;
```

### Delta Lake
```sql
SELECT * FROM catalog.schema.sales VERSION AS OF 100;
OPTIMIZE catalog.schema.sales ZORDER BY (date, region);
VACUUM catalog.schema.table RETAIN 168 HOURS;
```

### MLflow Registry
```python
mlflow.langchain.log_model(
    model,
    "model",
    registered_model_name="catalog.schema.model_name"
)
```

### Vector Search
```python
vsc.create_delta_sync_index(
    endpoint_name="endpoint",
    index_name="catalog.schema.index",
    source_table_name="catalog.schema.docs",
    primary_key="id",
    embedding_source_column="text",
    embedding_model_endpoint_name="embed_endpoint"
)
```

### Model Serving
```python
w.serving_endpoints.create(
    name="my-endpoint",
    config={
        "served_models": [{
            "model_name": "catalog.schema.model",
            "model_version": "1",
            "workload_size": "Small"
        }]
    }
)
```

---

## 🚨 COMMON MISTAKES TO AVOID
- Using different embedding models for docs vs queries
- No overlap in chunking
- Ignoring governance (Unity Catalog)
- No production monitoring
- No guardrails
- Relying only on offline evaluation
- Not versioning models
- Not using inference tables
- Ignoring token costs
- No human feedback loop

---

## ✅ PRE-EXAM CHECKLIST
- [ ] Pre-training vs fine-tuning
- [ ] What is RAG and how it works
- [ ] Prompt engineering techniques
- [ ] Chunking strategies
- [ ] Same model for embeddings
- [ ] Databricks Vector Search
- [ ] Compound vs simple systems
- [ ] Agents and ReAct pattern
- [ ] Deployment methods (batch, streaming, real-time)
- [ ] MLflow (Tracking, Registry, Evaluation)
- [ ] Model Serving features
- [ ] Inference Tables
- [ ] Lakehouse Monitoring
- [ ] MLOps vs LLMOps
- [ ] Guardrails & Llama Guard
- [ ] Metrics (BLEU, ROUGE, Perplexity)
- [ ] LLM-as-a-judge
- [ ] Offline vs Online evaluation
- [ ] Unity Catalog governance
- [ ] Completed 12 SkillCertPro exams

---

## 🎓 Good Luck!
If you’ve studied the full guides and done the practice exams, you’re ready. 🚀
