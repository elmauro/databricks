# 4️⃣ GenAI Application Deployment and Monitoring

## 📚 Table of Contents
1. [GenAI Systems Lifecycle](#genai-systems-lifecycle)
2. [Model Packaging](#genai-model-packaging)
3. [MLflow for Deployment](#mlflow-for-deployment)
4. [Deployment Methods](#deployment-methods)
5. [Databricks Model Serving](#databricks-model-serving)
6. [AI Systems Monitoring](#ai-systems-monitoring)
7. [MLOps and LLMOps](#mlops-and-llmops)

---

## GenAI Systems Lifecycle

### Phase 1: System Development

**Characteristics**: Works with **static data**

```
┌────────────────────────────────────────┐
│  1. DEFINE PROBLEM                     │
│     What should the system solve?      │
│                                        │
│     Example:                           │
│     "Chatbot that answers questions    │
│      about products"                   │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  2. SET METRICS                        │
│     How will success be measured?      │
│                                        │
│     Example:                           │
│     • Accuracy > 85%                   │
│     • Latency < 2 seconds              │
│     • User satisfaction > 4/5          │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  3. COLLECT DATA                       │
│     Obtain relevant data               │
│                                        │
│     Example:                           │
│     • FAQs                              │
│     • Support emails                    │
│     • Product documentation             │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  4. PROCESS DATA                       │
│     Clean and structure                 │
│                                        │
│     • Remove duplicates                 │
│     • Normalize text                    │
│     • Convert formats                   │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  5. BUILD SYSTEM                       │
│     Implement the solution              │
│                                        │
│     • RAG (retrieval + generation)     │
│     • Chains (structured flows)        │
│     • Agents (if needed)               │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  6. EVALUATE                           │
│     Test against metrics               │
│                                        │
│     • Manual tests                     │
│     • Automated metrics                │
│     • Pilot user feedback              │
└────────────────────────────────────────┘
```

---

### Phase 2: Deployment & Production

**Characteristics**: **New and changing** data in real time

```
┌────────────────────────────────────────┐
│  1. DEPLOYMENT                         │
│     Move to production                 │
│                                        │
│     • REST API                         │
│     • Web chatbot                      │
│     • Mobile app                       │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  2. MONITORING                         │
│     Watch behavior                     │
│                                        │
│     • Detect degradation               │
│     • Identify errors                  │
│     • Measure data drift               │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  3. ITERATION (if issues)              │
│                                        │
│     ┌──> Collect new data              │
│     ├──> Retrain system                │
│     ├──> Reprocess data                │
│     └──> Re‑deployment                 │
│                                        │
└────────────────────────────────────────┘
```

### Practical Analogy

**Think of it like developing a mobile app**:

**Phase 1 (Development)**:
- You code the app on your computer
- You test with sample data
- You fix bugs
- It works perfectly in your environment

**Phase 2 (Production)**:
- You publish to the App Store
- Thousands of users download it
- You discover new bugs in real scenarios
- Users use the app in unexpected ways
- You need to update regularly

---

## GenAI Model Packaging

### Ways to Package GenAI Logic

With LLMs, the "ML logic" is packaged in new ways:

#### 1. Engineered Prompt

**What it is**: Carefully designed prompt saved as a template

**Example**:
```python
PROMPT_TEMPLATE = """
You are an expert medical assistant.

Patient context:
{patient_history}

Question:
{question}

Instructions:
- Use precise medical terminology
- Cite sources when possible
- If you’re not sure, say "Consult a doctor"

Answer:
"""
```

**How it’s packaged**:
- Save as `.txt` or `.yaml`
- Version in Git
- Load dynamically at runtime

---

#### 2. Chain (Structured Sequence)

**What it is**: A sequence of steps from frameworks like LangChain or LlamaIndex

**Example**:
```python
from langchain.chains import RetrievalQA
from langchain.vectorstores import FAISS
from langchain.llms import OpenAI

# Define chain
chain = RetrievalQA.from_chain_type(
    llm=OpenAI(),
    chain_type="stuff",
    retriever=vectorstore.as_retriever()
)

# Use
answer = chain.run("What is RAG?")
```

**How it’s packaged**:
- Serialize with `mlflow.langchain.log_model(chain)`
- Includes all dependencies
- Reproducible in any environment

---

#### 3. API Call (Lightweight)

**What it is**: Simple call to an LLM via API

**Types**:

##### a) External Proprietary API
```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Hello"}]
)
```

**Advantages**:
- ✅ Easy to use
- ✅ No infrastructure

**Disadvantages**:
- ❌ External dependency
- ❌ Usage cost
- ❌ Data leaves your infrastructure

##### b) Internal API (Self‑Hosted)
```python
import requests

response = requests.post(
    "https://my-databricks.cloud/serving-endpoints/my-llm",
    json={"prompt": "Hello"}
)
```

**Advantages**:
- ✅ Full control
- ✅ Private data
- ✅ Customizable (fine‑tuned)

**Internal model types**:
- **Fine‑tuned**: Adjusted to your domain
- **Pre‑trained**: Unmodified

---

#### 4. Local Invocation with GPU

**What it is**: Run the model locally (not via API)

**Example**:
```python
from transformers import pipeline

generator = pipeline(
    "text-generation",
    model="meta-llama/Llama-2-7b-hf",
    device=0  # GPU 0
)

output = generator("Hello, how are you?")
```

**When to use**:
- Edge deployments (IoT devices)
- No internet connection
- Ultra‑low latency required

**Disadvantages**:
- ❌ Requires GPU/powerful hardware
- ❌ Infrastructure maintenance

---

### Options Summary

| Method | Complexity | Control | Cost | When to Use |
|-------|------------|---------|------|-------------|
| **Prompt** | Low | Low | Variable | Rapid prototype |
| **Chain** | Medium | Medium | Variable | Structured apps |
| **External API** | Low | Low | High | MVP, testing |
| **Internal API** | High | High | Medium | Enterprise production |
| **Local GPU** | High | Total | Low (if HW available) | Edge, offline |

---

## MLflow for Deployment

### What is MLflow?

**Reminder**: Open‑source platform to manage the **full lifecycle** of ML/GenAI

### MLflow Model (Standard Format)

**Structure**:
```
my_model/
├── MLmodel              # Metadata (flavors, signature)
├── conda.yaml           # Dependencies (conda)
├── requirements.txt     # Dependencies (pip)
├── python_env.yaml      # Python version
├── model/               # Model files
│   ├── model.pkl
│   └── tokenizer/
└── input_example.json   # Input example
```

#### MLmodel File
```yaml
artifact_path: model
flavors:
  langchain:
    langchain_version: 0.0.200
    model_data: model
  python_function:
    env: conda.yaml
    loader_module: mlflow.langchain
    python_version: 3.10.12
signature:
  inputs: '[{"name": "query", "type": "string"}]'
  outputs: '[{"name": "answer", "type": "string"}]'
```

---

### mlflow.pyfunc (Python Function Flavor)

**What it is**: **Generic interface** for any Python model

**Why it matters**:
- Any MLflow model can be loaded as a Python function
- Unified interface for deployment
- Compatible with Model Serving

**Key Functions**:
```python
import mlflow

# Save model
mlflow.pyfunc.log_model(
    artifact_path="model",
    python_model=my_model,
    signature=signature,
    input_example=input_example
)

# Load model
loaded_model = mlflow.pyfunc.load_model("models:/my_model/1")

# Predict
result = loaded_model.predict({"query": "What is RAG?"})
```

---

### MLflow Registry in Unity Catalog

**Features**:
```
┌─────────────────────────────────────────┐
│  UNITY CATALOG MODEL REGISTRY           │
├─────────────────────────────────────────┤
│  ✅ Automatic versioning                │
│     • Version 1, 2, 3, ...             │
│                                         │
│  ✅ Aliases                             │
│     • @champion (best model)            │
│     • @challenger (candidate)           │
│     • @staging                          │
│                                         │
│  ✅ Lifecycle Management                │
│     • Development → Staging → Prod      │
│                                         │
│  ✅ Collaboration & ACLs                │
│     • Who can read/write                │
│     • Unity Catalog governance          │
│                                         │
│  ✅ Full Lineage                        │
│     • What data was used                │
│     • What code                         │
│     • What parameters                   │
│                                         │
│  ✅ Tagging & Annotations               │
│     • Custom metadata                   │
└─────────────────────────────────────────┘
```

**Usage Example**:
```python
import mlflow

# Register in Unity Catalog
mlflow.set_registry_uri("databricks-uc")

with mlflow.start_run():
    # Train/build model
    model = build_my_rag_chain()
    
    # Log
    mlflow.langchain.log_model(
        model,
        "model",
        registered_model_name="my_catalog.my_schema.chatbot_v1"
    )

# Set alias
from mlflow import MlflowClient
client = MlflowClient()

client.set_registered_model_alias(
    "my_catalog.my_schema.chatbot_v1",
    "champion",
    version=3
)

# Load model by alias
champion_model = mlflow.langchain.load_model(
    "models:/my_catalog.my_schema.chatbot_v1@champion"
)
```

---

### MLflow and Development Cycle

```
┌─────────────────────────────────────────┐
│  1. MODEL/CHAIN BUILDING                │
│     • Build RAG/Chain/Agent             │
│     • Experiment with prompts           │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. MLFLOW TRACKING                     │
│     • Log experiments                   │
│     • Compare metrics                   │
│     • Select best version               │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. MLFLOW EVALUATION                   │
│     • Evaluate with datasets            │
│     • Automated metrics                 │
│     • LLM-as-a-judge                    │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. MLFLOW REGISTRY                     │
│     • Register model                    │
│     • Versioning                        │
│     • Assign alias (@champion)          │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  5. MODEL DEPLOYMENT                    │
│     • Databricks Model Serving          │
│     • REST API                          │
│     • Monitoring                        │
└─────────────────────────────────────────┘
```

---

## Deployment Methods

### 1. Batch Inference

**What it is**: Process **large amounts of data at once** on a schedule

**When to use**:
- Daily/weekly reports
- ETL jobs
- No immediate response needed

**Example**:
```
Task: "Summarize all quarterly financial reports"

Execution:
- Every Sunday at 2 AM
- Processes 1000 documents
- Generates summaries
- Saves them to a Delta Table

Users:
- See results Monday morning
```

#### ✅ Advantages
- **Cheaper**: Efficient hardware utilization
- **High volume**: Processes millions of records
- **Efficient**: Parallelizes well

#### ❌ Limitations
- **High latency**: Wait hours/days
- **Stale data**: Not real time
- **Not for streaming**: Data must be static

#### Implementation with Spark
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Load data
df = spark.table("my_catalog.my_schema.documents")

# UDF to process with LLM
def summarize_with_llm(text):
    # Call LLM
    return summary

from pyspark.sql.functions import udf
summarize_udf = udf(summarize_with_llm)

# Apply to all
df_summarized = df.withColumn("summary", summarize_udf("text"))

# Save
df_summarized.write.saveAsTable("my_catalog.my_schema.summarized")
```

---

### 2. Streaming Inference

**What it is**: Process **data as it arrives** (stream)

**When to use**:
- Real‑time event processing
- Log analysis
- IoT data

**Example**:
```
Application: "Personalize marketing messages in real time"

Flow:
User clicks → Event captured → 
Stream processor → LLM personalizes message → 
Message sent (< 5 seconds)
```

#### Structured Streaming (Spark)
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Read stream from Kafka
stream_df = spark.readStream \
    .format("kafka") \
    .option("subscribe", "user_events") \
    .load()

# Process with LLM
processed_df = stream_df.withColumn(
    "personalized_message",
    personalize_udf("event_data")
)

# Write results
query = processed_df.writeStream \
    .format("delta") \
    .outputMode("append") \
    .option("checkpointLocation", "/checkpoints") \
    .start("/output")
```

---

### 3. Real‑Time Inference

**What it is**: Generate predictions **instantly** when requested

**When to use**:
- Chatbots
- Interactive APIs
- Applications with waiting users

**Example**:
```
User: "What’s the status of my order?"
↓ (< 2 seconds)
API → Model Serving → LLM → Response
↓
Chatbot: "Your order #12345 is on the way, arriving tomorrow"
```

#### ✅ Advantages
- **Low latency**: Responses in seconds
- **Interactive**: Smooth UX
- **Fresh data**: Uses up‑to‑date information

#### ❌ Challenges
- **Complex infrastructure**: Auto‑scaling, load balancing
- **Cost**: 24/7 servers
- **Requires expertise**: DevOps, SRE

#### Databricks Model Serving
```python
# Already registered in MLflow Registry
```

---

### 4. Embedded/Edge Inference

**What it is**: Run the model **on the device** (no cloud)

**When to use**:
- IoT devices
- Offline mobile apps
- Ultra‑low latency (milliseconds)

**Example**:
```
Autonomous car:
Camera → Local processing (on‑car GPU) → Decision → Action
(Cannot depend on internet)
```

**Example**: Change AC temperature via voice

---

### Method Comparison

| Method | Latency | Cost | Complexity | Best For |
|--------|---------|------|------------|----------|
| **Batch** | High (hours) | Low | Low | Reports, ETL |
| **Streaming** | Medium (seconds) | Medium | Medium | Events, IoT |
| **Real‑Time** | Low (< 2s) | High | High | Chatbots, APIs |
| **Edge** | Very low (ms) | Variable | High | Offline, critical IoT |

---

## 📦 APPLICATIONS in Databricks

These three components represent the **production and operations** stage of your models:

- **MLflow AI Gateway**: How they are exposed via API
- **Model Serving**: How they are served efficiently
- **Lakehouse Monitoring**: How they are supervised in real time

---

### 🧩 1. MLflow AI Gateway

The **unified entry point** (API Gateway) to access AI models —both internal and external— securely and in a standardized way.

**What does it do?**:
- ✅ Provides a **single interface** to interact with any LLM:
  - **Internal** (registered in MLflow or Mosaic AI)
  - **External** (OpenAI, Anthropic, Hugging Face, Azure AI, AWS Bedrock, etc.)
- ✅ Controls **costs, authentication, and traceability** of calls
- ✅ Lets teams use LLMs **without exposing individual API keys**

**Problem it solves**:
```
❌ Without AI Gateway:
Team A → uses OpenAI directly (hardcoded API key)
Team B → uses Anthropic (another key)
Team C → uses internal model (different config)
→ Credential chaos
→ No centralized tracking
→ Uncontrolled costs

✅ With MLflow AI Gateway:
All teams → AI Gateway → Routes to appropriate model
→ Centralized, secure keys
→ Automatic logging
→ Cost control
```

**Architecture**:
```
┌──────────────────────────────────────────┐
│  APPLICATION / USER                      │
└─────────────┬────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│  MLFLOW AI GATEWAY                       │
│  • Authentication                        │
│  • Rate limiting                         │
│  • Cost tracking                         │
│  • Logging                               │
└─────────────┬────────────────────────────┘
              ↓
      ┌───────┴───────┬─────────────┐
      ↓               ↓             ↓
┌──────────┐  ┌──────────┐  ┌──────────┐
│ OpenAI   │  │ Anthropic│  │ DBRX     │
│ GPT‑4    │  │ Claude   │  │ Internal │
└──────────┘  └──────────┘  └──────────┘
```

**Usage Example**:
```python
from mlflow.deployments import get_deploy_client

# Gateway client
client = get_deploy_client("databricks")

# Call GPT‑4 (without exposing API key)
response = client.predict(
    endpoint="chat-gpt-4",
    inputs={
        "messages": [
            {"role": "user", "content": "Explain RAG"}
        ]
    }
)

# Call Claude (same interface)
response_claude = client.predict(
    endpoint="chat-claude",
    inputs={
        "messages": [
            {"role": "user", "content": "Explain RAG"}
        ]
    }
)

# All calls are logged automatically
```

**Advantages**:
- 🔐 **Security**: Centralized keys
- 💰 **Cost control**: Unified tracking
- 📊 **Observability**: Automatic logging
- 🔄 **Flexibility**: Switch providers without changing code

---

### ⚙️ 2. Databricks Model Serving

Service that **deploys models** (ML or LLMs) as **REST endpoints in production**, optimized for low latency and high availability.

**Supported Model Types**:
```
┌─────────────────────────────────────────┐
│  DATABRICKS MODEL SERVING               │
├─────────────────────────────────────────┤
│                                         │
│  1. CUSTOM MODELS (MLflow)              │
│     • Your model registered in UC       │
│     • Custom chains, RAGs, Agents       │
│     • Python, PyTorch, TensorFlow       │
│                                         │
│  2. FOUNDATION MODELS                   │
│     • DBRX Instruct                     │
│     • LLaMA 3 (8B, 70B)                 │
│     • Mixtral 8x7B                      │
│     • BGE‑Large (embeddings)            │
│                                         │
│  3. EXTERNAL MODELS                     │
│     • OpenAI (GPT‑4, GPT‑3.5)           │
│     • Anthropic (Claude)                │
│     • Cohere                            │
│     • AWS Bedrock models                │
│                                         │
└─────────────────────────────────────────┘
```

**Key Features**:

| Function | Description | Advantage |
|----------|-------------|-----------|
| **Real Time** | Instant responses | Chatbots, interactive APIs |
| **Batch Inference** | Massive processing | Periodic reports |
| **MLflow Integration** | Deploy directly from Registry | No manual config |
| **Security UC** | Access control via Unity Catalog | Enterprise governance |
| **Inference Tables** | Save every prediction | Audit and debugging |
| **Auto‑scaling** | Scales by demand | Cost optimization |
| **Scale‑to‑zero** | Turns off when idle | Saves costs |

---

### Inference Tables

**What they are**: Each request‑response is automatically saved in a **Delta Table**

**What they’re for**:
- ✅ Post monitoring
- ✅ Debugging
- ✅ Usage analysis
- ✅ Re‑training (feedback loop)

**Structure**:
```
Inference Table:
├── request_id
├── timestamp
├── input (user query)
├── output (LLM response)
├── latency
├── token_count
└── metadata
```

**Enable it**:
```python
w.serving_endpoints.create(
    name="my-endpoint",
    config={
        "served_models": [{ ... }],
        "traffic_config": { ... },
        "auto_capture_config": {
            "catalog_name": "my_catalog",
            "schema_name": "my_schema",
            "table_name_prefix": "inference_"
        }
    }
)
```

**Result**: It’s automatically created:
```
my_catalog.my_schema.inference_my-endpoint
```

---

### A/B Testing and Canary Deployments

#### Traffic Splitting

**Example**: Test a new model with 10% of traffic
```python
w.serving_endpoints.update_config(
    name="my-endpoint",
    config={
        "served_models": [
            {
                "model_name": "my_catalog.my_schema.chatbot",
                "model_version": "3",  # Old version
                "workload_size": "Small"
            },
            {
                "model_name": "my_catalog.my_schema.chatbot",
                "model_version": "4",  # New version
                "workload_size": "Small"
            }
        ],
        "traffic_config": {
            "routes": [
                {"served_model_name": "chatbot-3", "traffic_percentage": 90},
                {"served_model_name": "chatbot-4", "traffic_percentage": 10}
            ]
        }
    }
)
```

**Flow**:
```
100 requests
├─> 90 requests → Model v3
└─> 10 requests → Model v4

Analyze v4 metrics:
- If better: gradually increase %
- If worse: revert to 100% v3
```

---

### Unity Catalog Integration
```
┌─────────────────────────────────────────┐
│  UNITY CATALOG                          │
├─────────────────────────────────────────┤
│                                         │
│  • UC Volume                            │
│    └─> Files (PDFs, CSVs, etc.)         │
│                                         │
│  • Raw/Processed Text Tables            │
│    └─> Processed documents              │
│                                         │
│  • Embeddings/Index                     │
│    └─> Vector Search index              │
│                                         │
│  • Model/Chain                          │
│    └─> Registered model                 │
│                                         │
│  • Inference Table                      │
│    └─> Request logs                     │
│                                         │
│  • Processed Payloads Table             │
│    └─> Processed data                   │
│                                         │
│  • Metric Tables                        │
│    └─> Lakehouse Monitoring             │
│                                         │
└─────────────────────────────────────────┘
```

**Everything in Unity Catalog** = Centralized governance

---

## AI Systems Monitoring

### Why Monitor?

**Goal**: Diagnose problems **before** they become severe or costly

**Without monitoring**:
```
User: "The chatbot gives weird answers"
→ When did it start?
→ How many users affected?
→ Which inputs cause the issue?
→ ❓ We don’t know
```

**With monitoring**:
```
Automatic alert: "30% increase in low‑quality answers since 2 PM"
→ We know exactly when and how much
→ We see problematic inputs
→ We can act quickly
```

---

### What to Monitor?

#### 1. Input Data
- Query distribution
- Input length
- Languages used
- Detection of malicious inputs (prompt injection)

#### 2. Data in Vector Databases
- Document quality
- Topic coverage
- Information freshness

#### 3. Human Feedback
- 👍👎 reactions
- User comments
- Ratings (1–5 stars)

#### 4. Prompts/Queries and Responses
- Store for analysis
- ⚠️ **Legality**: Can you store user data?
- Privacy compliance (GDPR, CCPA)

---

### AI Assets to Monitor

#### 1. Mid‑Training Checkpoints
- If fine‑tuning, periodic checkpoints
- Loss curve analysis

#### 2. Component Evaluation Metrics
- Retrieval quality
- Embedding quality
- Reranking effectiveness

#### 3. System Evaluation Metrics
- End‑to‑end latency
- Token usage (cost)
- Answer quality

#### 4. Performance & Cost
- Requests per second (RPS)
- P50, P95, P99 latency
- GPU utilization
- Cost per request

---

### 📊 3. Lakehouse Monitoring

A **fully managed** solution to monitor behavior, performance, and quality of **production models** (ML and GenAI).

**What does it monitor?**:
- ✅ **Input and output data** → Detect data drift or quality degradation
- ✅ **Model performance** → Latency, costs, errors
- ✅ **Custom metrics** → Accuracy, faithfulness, relevance
- ✅ **Trends over time** → Automatic dashboards in Databricks SQL

**Internal Architecture**:
```
┌──────────────────────────────────────────┐
│  INFERENCE TABLE (Delta Table)           │
│  • request_id                            │
│  • timestamp                             │
│  • input (query)                         │
│  • output (response)                     │
│  • latency, tokens, metadata             │
└─────────────┬────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│  LAKEHOUSE MONITORING                    │
│  • Periodically analyzes                 │
│  • Generates automatic metrics           │
│  • Detects drift, anomalies              │
└─────────────┬────────────────────────────┘
              ↓
      ┌───────┴───────┬─────────────┐
      ↓               ↓             ↓
┌──────────┐  ┌──────────┐  ┌──────────┐
│ Profile  │  │  Drift   │  │ Custom   │
│ Metrics  │  │ Metrics  │  │ Metrics  │
└──────────┘  └──────────┘  └──────────┘
      │               │             │
      └───────┬───────┴─────────────┘
              ↓
┌──────────────────────────────────────────┐
│  DATABRICKS SQL DASHBOARD                │
│  • Automatic visualizations              │
│  • Configurable alerts                   │
│  • Historical metrics                    │
└──────────────────────────────────────────┘
```

**Key Features**:
```
┌─────────────────────────────────────────┐
│  LAKEHOUSE MONITORING                   │
├─────────────────────────────────────────┤
│  ✅ Fully Managed                       │
│     • Zero ops overhead                 │
│     • No infrastructure to maintain     │
│                                         │
│  ✅ Frictionless                        │
│     • Setup in minutes                  │
│     • Simple configuration              │
│                                         │
│  ✅ Unified                             │
│     • Data + Models in one place        │
│     • All in Unity Catalog              │
│                                         │
│  ✅ Built on Unity Catalog              │
│     • Integrated ACLs                   │
│     • Automatic governance              │
│                                         │
│  📊 Auto‑generates DBSQL Dashboard      │
│     • Ready visualizations              │
│     • Interactive charts                │
│                                         │
└─────────────────────────────────────────┘
```

#### Metric Types

**1. Profile Metrics**:
- Data statistics (min, max, mean, std)
- Distributions
- Nulls, missings

**2. Drift Metrics**:
- Did data change vs baseline?
- Distributional drift
- Statistical tests (KS, χ²)

**3. Custom Metrics**:
- Define your own
- Example: "% of answers > 500 words"

---

#### Lakehouse Monitoring Setup
```python
from databricks import lakehouse_monitoring as lm

# Monitor inference table
lm.create_monitor(
    table_name="my_catalog.my_schema.inference_chatbot",
    profile_type=lm.InferenceLog(
        timestamp_col="timestamp",
        model_id_col="model_version",
        prediction_col="output",
        problem_type="text-generation",
        label_col=None  # No ground truth in real time
    ),
    output_schema_name="my_catalog.my_schema",
    schedule=lm.MonitorCronSchedule(
        quartz_cron_expression="0 0 * * * ?"  # Every hour
    )
)
```

**Automatic result**:
1. **Profile Table**: Statistics per time window
2. **Drift Table**: Drift metrics
3. **Dashboard**: Visualization in Databricks SQL

---

### Monitoring Workflow
```
┌─────────────────────────────────────────┐
│  DEVELOPMENT                            │
│  • Create monitoring tables for all     │
│    components                           │
│  • Define key metrics                   │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  TESTING                                │
│  • Validate monitoring works            │
│  • Adjust alert thresholds              │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  PRODUCTION                             │
│  • Regular table refresh                │
│  • Alerts configured                    │
│  • Dashboards monitored                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  ACTION                                  │
│  • If issues:                            │
│    - Investigate root cause              │
│    - Collect new data                    │
│    - Retrain/adjust                      │
│    - Re‑deployment                       │
└─────────────────────────────────────────┘
```

**Tips**:
- ✅ **Model cost vs performance**: Is a more expensive model worth it?
- ✅ **Regular refresh**: Hourly, daily (based on criticality)
- ✅ **Key alerts**: Not all metrics, only critical ones
- ✅ **Retrain triggers**: Automate if drift > threshold

---

## MLOps and LLMOps

### What is MLOps?

**Definition**: Set of **processes and automation** to manage data, code, and models, improving performance, stability, and efficiency of ML systems.

**Formula**:
```
MLOps = DataOps + DevOps + ModelOps
```

**Components**:
- **DataOps**: Data management (quality, pipelines, versioning)
- **DevOps**: CI/CD, infrastructure as code, deployment
- **ModelOps**: Model management (training, evaluation, deployment, monitoring)

---

### Why does MLOps matter?

| Benefit | Description |
|---------|-------------|
| **Data quality** | Clean, trustworthy data |
| **Optimized processes** | Less manual work, more automation |
| **Cost/performance monitoring** | Optimize ROI |
| **Time to value** | Faster to production |
| **Less manual oversight** | Automate checks |

---

### Multi‑Environment Semantics

**Standard environments**:
```
1. DEVELOPMENT (Dev)
   • Experimentation
   • Breaking changes allowed
   • Synthetic/sampled data

2. STAGING (Stage)
   • Pre‑production
   • Rigorous testing
   • Data similar to prod

3. PRODUCTION (Prod)
   • Real users
   • High availability
   • 24/7 monitoring
```

---

### Environment Separation

#### Option 1: Direct Separation

**Strategy**: **Completely separate Databricks workspaces**
```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  Dev Workspace   │  │ Stage Workspace  │  │  Prod Workspace  │
│                  │  │                  │  │                  │
│  • Experiments   │  │  • Pre‑prod      │  │  • Users         │
│  • Sample data   │  │  • Testing       │  │  • High avail.   │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

**Pros**:
- ✅ Complete isolation
- ✅ No risk to affect prod
- ✅ Scales to multiple projects

**Cons**:
- ❌ More infrastructure
- ❌ More permission complexity

---

#### Option 2: Indirect Separation

**Strategy**: **Single workspace with logical separation** (catalogs, schemas, prefixes)
```
┌─────────────────────────────────────────┐
│  Databricks Workspace                   │
├─────────────────────────────────────────┤
│  Catalog: dev_catalog                   │
│    └─> Schema: chatbot                  │
│                                         │
│  Catalog: staging_catalog               │
│    └─> Schema: chatbot                  │
│                                         │
│  Catalog: prod_catalog                  │
│    └─> Schema: chatbot                  │
└─────────────────────────────────────────┘
```

**Pros**:
- ✅ Less infrastructure
- ✅ Simpler permissions

**Cons**:
- ❌ Risk of mixing environments
- ❌ Doesn’t scale well to many projects

---

### Recommended Architecture (MLOps)

**Deploy‑Code Architecture**:
```
┌────────────────────────────────────────────┐
│  SOURCE CONTROL (Git)                      │
│  • Code                                    │
│  • Configurations                          │
│  • Infrastructure as Code                  │
└─────────────┬──────────────────────────────┘
              │
              ├─────> Dev Workspace
              ├─────> Staging Workspace
              └─────> Prod Workspace
              
CI/CD Pipeline:
  Git Push → Tests → Build → 
  Deploy to Dev → Tests → 
  Deploy to Staging → Tests → 
  Manual Approval → Deploy to Prod
```

---

### LLMOps: Differences vs Traditional MLOps

| Aspect | Traditional MLOps | LLMOps |
|--------|-------------------|--------|
| **Dev Patterns** | Code + data | **+ Text templates (prompts)** |
| **Packaging** | Serialized model | **Complete applications (chains)** |
| **Serving** | Model endpoint | **+ Vector DB, GPU infra, UI** |
| **API Governance** | Less critical | **Critical** (endpoint access) |
| **Cost** | Fixed (infra) | **Variable** (usage, API calls) |
| **Human Feedback** | Optional | **Essential** (improve prompts) |

---

### LLMOps: Specific Aspects

#### 1. Incremental Development
- Iterate prompts
- Adjust chains without retraining

#### 2. Text Templates
- Version prompts as code
- A/B testing of prompts

#### 3. Entire Applications
- Not just the model, the whole app (retriever + LLM + UI)

#### 4. Additional Components
- Vector databases
- Embedding services
- Rerankers

#### 5. GPU Infrastructure
- Requires powerful GPUs
- Optimize batch size, quantization

#### 6. Cost Management
- API‑based models: pay‑per‑token
- Techniques to reduce cost:
  - Use smaller models when possible
  - Cache common responses
  - Prompt compression

#### 7. Human Feedback Loop
- Collect feedback (thumbs up/down)
- Use it to improve prompts
- Fine‑tuning with feedback

---

### Recommended LLMOps Architecture
```
┌────────────────────────────────────────────┐
│  DEVELOPMENT                               │
├────────────────────────────────────────────┤
│  • Experiment with prompts                 │
│  • Build chains/RAGs                       │
│  • Unit test components                    │
│  • MLflow tracking                         │
│                                            │
│  Dev Workspace                             │
│    dev_catalog.chatbot.models              │
│    dev_vector_search_index                 │
└─────────────┬──────────────────────────────┘
              ↓
┌────────────────────────────────────────────┐
│  STAGING                                   │
├────────────────────────────────────────────┤
│  • Integration testing                     │
│  • Performance testing                     │
│  • Canary deployment                       │
│  • Evaluation with test set                │
│                                            │
│  Staging Workspace                         │
│    staging_catalog.chatbot.models          │
│    staging_vector_search_index             │
└─────────────┬──────────────────────────────┘
              ↓
┌────────────────────────────────────────────┐
│  PRODUCTION                                │
├────────────────────────────────────────────┤
│  • Model Serving (auto‑scaling)            │
│  • Lakehouse Monitoring                    │
│  • Human feedback collection               │
│  • Inference tables logging                │
│                                            │
│  Prod Workspace                            │
│    prod_catalog.chatbot.models@champion    │
│    prod_vector_search_index                │
│    prod_inference_logs                     │
└────────────────────────────────────────────┘
```

---

## 🎯 Practice Questions

### Question 1
**Which deployment method is best for an interactive chatbot?**

A) Batch  
B) Streaming  

### Question 2
**What do Inference Tables automatically store?**

A) Only inputs  
B) Only outputs  
C) Full requests and responses ✅  
D) Only aggregated metrics

**Answer**: C – Each request‑response is logged for monitoring

---

### Question 3
**In A/B testing, what percentage would you initially give a new model?**

A) 50%  
B) 90%  
C) 10–20% ✅  
D) 100%

**Answer**: C – Start with low traffic to minimize risk

---

### Question 4
**Which MLflow component manages model versions and aliases?**

A) MLflow Tracking  
B) MLflow Registry ✅  
C) MLflow Evaluation  
D) MLflow Projects

**Answer**: B – Registry = versioning, aliases, lifecycle

---

### Question 5
**What differentiates LLMOps from traditional MLOps?**

A) LLMOps uses Python  
B) LLMOps includes text templates and human feedback ✅  
C) LLMOps is simpler  
D) No difference

**Answer**: B – LLMOps adds prompts, chains, feedback loop

---

## 📝 Executive Summary

### What you MUST know:

✅ **Lifecycle**: Development (static data) → Production (new data)  
✅ **Packaging**: Prompt, Chain, API call (external/internal), Local GPU  
✅ **MLflow pyfunc**: Generic interface for deployment  
✅ **Unity Catalog Registry**: Versioning, aliases (@champion), lineage, ACLs  
✅ **Deployment Methods**:
   - Batch = high volume, low urgency
   - Streaming = real‑time events
   - Real‑Time = chatbots, APIs (< 2s)
   - Edge = offline, IoT

✅ **Model Serving**: Custom, Foundation, External models  
✅ **Inference Tables**: Auto‑logging of requests for monitoring  
✅ **A/B Testing**: Traffic splitting to test versions  
✅ **Lakehouse Monitoring**: Profile, Drift, Custom metrics  
✅ **MLOps** = DataOps + DevOps + ModelOps  
✅ **LLMOps** = MLOps + prompts + chains + human feedback + cost management

---

## 🔗 Next Topic

➡️ **Continue with**: `05_Evaluation_Governance.md` (Evaluation, Security, Guardrails, Metrics)
