# 2️⃣ Generative AI Solution Development

## 📚 Table of Contents
1. [Prompt Engineering](#prompt-engineering)
2. [What is RAG?](#what-is-rag)
3. [Data Preparation for RAG](#data-preparation-for-rag)
4. [Document Chunking](#document-chunking)
5. [Embeddings](#embeddings)
6. [Vector Search](#vector-search)
7. [Reranking](#reranking)
8. [MLflow for RAG](#mlflow-for-rag)
9. [RAG Evaluation](#rag-evaluation)

---

## Prompt Engineering

### What is a Prompt?

A **prompt** is the instruction or question you give to an LLM.

**Example**:
```
Prompt: "Summarize this article in 3 bullet points"
LLM: [generates the summary]
```

### What is Prompt Engineering?

**Definition**: The art and practice of **designing and refining prompts** to obtain better results from the LLM.

It’s like learning to ask the right questions to get the answers you need.

---

### Components of a Good Prompt

```
┌─────────────────────────────────────┐
│   1. INSTRUCTIONS                   │
│   "Act as a Python expert"         │
├─────────────────────────────────────┤
│   2. CONTEXT                        │
│   "For a web application"          │
├─────────────────────────────────────┤
│   3. INPUT/QUESTION                 │
│   "How do I optimize this function?"│
├─────────────────────────────────────┤
│   4. OUTPUT FORMAT                  │
│   "Respond in JSON format"          │
└─────────────────────────────────────┘
```

### Full Example

❌ **Bad Prompt**:
```
"Give me code"
```

✅ **Good Prompt**:
```
INSTRUCTIONS: You are a Python expert for data science.

CONTEXT: I need to analyze a sales dataset with pandas.

INPUT: I have a CSV with columns: date, product, quantity, price.
I want to calculate total sales per month.

FORMAT: Give me Python code with explanatory comments.
```

---

### Prompt Engineering Techniques

#### 1. Zero‑Shot Prompting

**Definition**: Ask the LLM to do something **without giving examples**.

**When to use**: Simple and direct tasks

**Example**:
```
Prompt: "Translate 'Hello world' to Spanish"
LLM: "Hola mundo"
```

#### 2. Few‑Shot Prompting

**Definition**: Provide **a few examples** to guide the LLM.

**When to use**: Tasks with specific formatting or complex logic

**Example**:
```
Prompt:
Classify sentiment: positive, negative, neutral

Example 1:
Text: "I loved this product"
Sentiment: positive

Example 2:
Text: "Terrible experience"
Sentiment: negative

Now classify:
Text: "The service is acceptable"
Sentiment: ?

LLM: "neutral"
```

#### 3. Chain‑of‑Thought (CoT) Prompting

**Definition**: Ask the LLM to **think step by step** like a human.

**When to use**: Problems requiring reasoning

**Example**:
```
Prompt:
Solve step by step:
If a pizza costs $15 and I have a 20% discount, how much do I pay?

Think step by step:

LLM:
Step 1: Calculate 20% of $15
20% of $15 = $15 × 0.20 = $3

Step 2: Subtract the discount from the original price
$15 - $3 = $12

Answer: You pay $12
```

#### 4. Prompt Chaining

**Definition**: **Chain multiple prompts** where the output of one is the input of the next.

**When to use**: Complex tasks divided into steps

**Example**:
```
Prompt 1: "Summarize this 1000‑word article"
Output 1: [200‑word summary]

Prompt 2: "Translate this summary to French: [Output 1]"
Output 2: [summary in French]

Prompt 3: "Extract the 3 key points from: [Output 2]"
Output 3: [3 points in French]
```

**Diagram**:
```
Article → [Summarize] → Summary → [Translate] → Translation → [Extract] → Key points
```

---

### Prompt Engineering Tips and Tricks

#### 1. Use Delimiters

**Why**: Clearly separate instructions from data

**Common delimiters**: `###`, ` ``` `, `{}`, `[]`, `---`

**Example**:
```
Analyze the following text:
---
[Your text here]
---

Return the analysis in this format:
```json
{
  "topic": "...",
  "sentiment": "..."
}
```

#### 2. Specify Output Format

**Examples**:
- "Respond in JSON format"
- "Generate a markdown table"
- "Use bullet points"
- "Give me Python code with comments"

#### 3. Provide a Correct Example

```
Prompt:
I need you to format addresses like this:

Correct example:
Input: "street 123 city xyz"
Output: "Street: 123, City: XYZ"

Now format: "avenue 456 town abc"
```

#### 4. Guide Toward Better Answers

✅ **Ask it NOT to hallucinate**:
```
"If you don’t know the answer, say 'I don’t have enough information.'
Do not make up facts."
```

✅ **Ask it NOT to assume sensitive information**:
```
"Do not assume personal information about the user.
If you need data, ask explicitly."
```

✅ **Ask it to think more (CoT)**:
```
"Do not rush to a solution.
Think step by step and show your reasoning."
```

---

### Benefits and Limitations

#### ✅ Benefits
- **Simple and Efficient**: No retraining required
- **Predictable Results**: With good prompts, outputs are consistent
- **Customized Outputs**: Control over format and style

#### ❌ Limitations
- **Model‑dependent**: A prompt that works on GPT‑4 may fail on LLaMA
- **Limited by model knowledge**: If the model doesn’t know something, prompt engineering won’t help
- **You need RAG for external knowledge**: Updated or private data requires RAG

---

## What is RAG?

### Simple Definition

**RAG** = **Retrieval‑Augmented Generation**

It is a technique that **combines information retrieval + answer generation**.

### How does it work?

```
┌──────────────────────────────────────────────┐
│  1. The user asks something                  │
│     "What is the vacation policy?"          │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  2. RETRIEVAL (Search)                       │
│     Searches relevant documents              │
│     Finds: employee_manual.pdf               │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  3. AUGMENTATION (Augment the prompt)        │
│     Prompt: "Based on this context:          │
│     [manual content]                         │
│     Answer: vacation policy?"                │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  4. GENERATION (Generate the answer)         │
│     The LLM reads the context and answers    │
│     "According to the manual, you have 15..."│
└──────────────────────────────────────────────┘
```

### Why is RAG important?

#### Problem without RAG:
```
Question: "What is our new product from March 2024?"
LLM: "I don’t have updated information after 2023" ❌
```

#### Solution with RAG:
```
Question: "What is our new product from March 2024?"

RAG:
1. Searches documents → finds "Launch_March2024.pdf"
2. Extracts context → "Product X launched on March 15"
3. LLM generates → "On March 15, 2024 we launched Product X..." ✅
```

### RAG Use Cases

| Use Case | Description | Example |
|----------|-------------|---------|
| **Q&A Chatbot** | Answers questions about documents | Tech support chatbot |
| **Search Augmentation** | Improves search results | Google + AI summary |
| **Content Creation** | Generates content from sources | Summary of multiple papers |
| **Summarization** | Summarizes long documents | Executive summary of reports |

### RAG Architecture in Databricks

```
┌─────────────────────────────────────────────────────┐
│  PHASE 1: DOCUMENT EMBEDDING (Preparation)          │
├─────────────────────────────────────────────────────┤
│                                                     │
│  External Sources (PDFs, Docs, etc.)               │
│         ↓                                           │
│  Delta Live Tables (Batch/Streaming ingestion)      │
│         ↓                                           │
│  Files & Metadata (Delta Tables)                    │
│         ↓                                           │
│  Document Processing (Chunking, cleaning)           │
│         ↓                                           │
│  Mosaic AI Model Serving (Embeddings)               │
│         ↓                                           │
│  Mosaic AI Vector Search (Storage)                  │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  PHASE 2: QUERY & RETRIEVAL (Usage)                 │
├─────────────────────────────────────────────────────┤
│                                                     │
│  User Query                                         │
│         ↓                                           │
│  Mosaic AI Model Serving (Embed query)              │
│         ↓                                           │
│  Mosaic AI Vector Search (Find similar docs)        │
│         ↓                                           │
│  Retrieved Context                                  │
│         ↓                                           │
│  Prompt Augmentation (Add context to prompt)        │
│         ↓                                           │
│  Mosaic AI Model Serving (LLM Generation)           │
│         ↓                                           │
│  Response                                           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## Data Preparation for RAG

### Potential Problems

| Problem | Description | Impact |
|---------|-------------|--------|
| **Poor Quality Output** | Dirty/incorrect data | Wrong answers |
| **Lost in the Middle** | LLM ignores middle docs | Important info lost |
| **Inefficient Retrieval** | Poorly prepared data | Slow/inaccurate search |
| **Exposing Data** | Poor governance | Security violations |
| **Wrong Embedding Model** | Inadequate model | Low search quality |

### Data Preparation Process

```
1. INGESTION & PRE‑PROCESSING
   • Load documents (PDF, Word, HTML, etc.)
   • Clean and normalize
   • Extract text

2. DATA STORAGE & GOVERNANCE
   • Store in Delta Lake
   • Configure Unity Catalog (permissions)

3. CHUNKING
   • Split documents into chunks
   • Fixed‑size or Context‑aware

4. EMBEDDING
   • Convert chunks to vectors
   • Use an appropriate embedding model

5. VECTOR STORE
   • Store in Mosaic AI Vector Search
   • Index for fast search
```

---

## Document Chunking

### What is Chunking?

**Definition**: Split long documents into smaller **pieces (chunks)**.

**Why?**:
- LLMs have token limits (e.g., 8k, 32k, 128k)
- Smaller chunks = more precise retrieval
- Better retrieval of relevant information

### Problem Visualization

```
❌ Without Chunking:
[10,000‑word full document]
↓
LLM (limit: 4,000 tokens)
↓
ERROR: Exceeds limit

✅ With Chunking:
[Chunk 1: 500 words]
[Chunk 2: 500 words]
[Chunk 3: 500 words]
...
↓
You only send the relevant chunks to the LLM
↓
✅ Works perfectly
```

---

### Chunking Strategies

#### 1. Fixed‑Size Chunking

**How it works**: Split every X tokens/characters

**Example**:
```
Document: "This is a long document about Python. Python is a programming
           language. It is widely used in data science..."

Chunk 1 (50 chars): "This is a long document about Python. Python"
Chunk 2 (50 chars): " is a programming language. It is widely "
Chunk 3 (50 chars): "used in data science..."
```

**Pros**:
- ✅ Simple
- ✅ Fast
- ✅ Computationally cheap

**Cons**:
- ❌ May cut in the middle of a sentence
- ❌ Loses context
- ❌ Doesn’t respect document structure

#### 2. Context‑Aware Chunking

**How it works**: Split while respecting logical structure

**Options**:
- By sentence
- By paragraph
- By section (H1, H2, H3 in HTML/Markdown)
- By special punctuation (`.`, `\n`)

**Example**:
```
Markdown document:
# Section 1: Introduction
This is the introduction paragraph.

## Subsection 1.1
Subsection content.

# Section 2: Development
...

Generated chunks:
Chunk 1: "# Section 1: Introduction\nThis is the introduction paragraph."
Chunk 2: "## Subsection 1.1\nSubsection content."
Chunk 3: "# Section 2: Development\n..."
```

**Pros**:
- ✅ Keeps context
- ✅ Respects structure
- ✅ More semantically coherent

**Cons**:
- ❌ More complex
- ❌ Variable‑size chunks

#### 3. Chunking with Overlap

**Problem**: Important information at the boundary between chunks

**Solution**: Overlap between consecutive chunks

**Visualization**:
```
Without Overlap:
[Chunk 1: AAAA] [Chunk 2: BBBB] [Chunk 3: CCCC]

With Overlap:
[Chunk 1: AAAA][AA]
```

#### 4. Windowed Summarization

**How it works**: Each chunk includes a summary of previous chunks

**Example**:
```
Chunk 1:
"Python is a programming language."

Chunk 2:
[Previous summary: "Introduction to Python"]
"Python is used in data science..."

Chunk 3:
[Previous summary: "Python in data science"]
"The main libraries are NumPy, Pandas..."
```

---

### Chunking Tools

- **ChunkViz**: Visual tool to test strategies
- **LangChain**: Text splitters (RecursiveCharacterTextSplitter)
- **LlamaIndex**: NodeParser
- **Unstructured**: Smart auto‑chunking

---

### Challenges with Complex Documents

#### Types of Complex Content

```
┌─────────────────────────────────────┐
│  Complex PDF Document               │
├─────────────────────────────────────┤
│  • Text mixed with images           │
│  • Tables with data                 │
│  • Diagrams and infographics        │
│  • Multiple columns                 │
│  • Text with colors (hierarchy)     │
│  • Headers and footers              │
│  • Watermarks                       │
└─────────────────────────────────────┘
```

#### Solutions

**1. Traditional Libraries**:
- **PyMuPDF**: Basic text extraction
- **PyPDF**: Similar, simple
- **pdfplumber**: Better with tables

**Limitation**: Do not understand complex layout

**2. Layout Models**:
- **Donut**: OCR‑free document understanding
- **doctr**: Document text recognition
- **Unstructured.io**: Multi‑format parsing
- **Hugging Face Layout Models**: LayoutLM, LayoutLMv3

**Advantage**: Understand visual structure

**3. Multi‑Modal Models**:
- **GPT‑4 Vision**: Processes image + text
- **Claude Vision**: Similar
- **Open Source**: LLaVA, Fuyu

**Advantage**: Process the full image, extract text and interpret diagrams

---

## Embeddings

### What is an Embedding?

A **numerical representation** (vector) of textual content.

**Visual Example**:
```
Text: "dog"
Embedding: [0.2, 0.8, 0.1, 0.5, ...] (384‑dimensional vector)

Text: "cat"
Embedding: [0.3, 0.7, 0.2, 0.4, ...] (similar to "dog" because they’re animals)

Text: "automobile"
Embedding: [0.8, 0.1, 0.9, 0.2, ...] (very different)
```

### Why are they important?

Embeddings capture **semantic meaning**:
```
"dog" vs "hound" (synonyms)
→ Very similar embeddings

"bank" (bench) vs "bank" (financial)
→ Different embeddings (context matters)
```

### How they are used in RAG
```
1. INDEXING (Once)
   Documents → Embedding Model → Vectors → Vector DB

2. QUERY (Each search)
   Query → Embedding Model → Query vector → 
   Vector DB finds similar vectors → Relevant documents
```

---

### Choosing the Right Embedding Model

#### Considerations

**1. Text Properties**:
- **Vocabulary**: Spanish, English, technical?
- **Domain**: Medical, legal, general?
- **Length**: Short tweets or long documents?

**2. Model Capabilities**:
- **Multilingual**: Do you need multiple languages?
- **Dimensions**: More dimensions = more precision, but higher cost

**3. Practical Considerations**:
- **Context window**: How many tokens does it accept?
- **Privacy**: External API or local?
- **Cost**: Free, open source, or paid?
- **Benchmark**: Test several and compare

#### Popular Models

| Model | Dimensions | Languages | Context | Type |
|-------|------------|-----------|---------|------|
| **BGE‑Large** | 1024 | Multi | 512 tokens | Open Source |
| **E5** | 768 | Multi | 512 tokens | Open Source |
| **OpenAI Ada** | 1536 | Multi | 8191 tokens | Proprietary |
| **Cohere Embed** | 4096 | Multi | Variable | Proprietary |

---

### Tip 1: Choose Your Model Wisely

**Problem**: Embedding model trained on technical English fails with colloquial Spanish

**Solution**: The model should:
- ✅ Be trained on data similar to yours
- ✅ Support your language(s)
- ✅ Understand your domain (medical, legal, etc.)

### Tip 2: Same Embedding Space

**Problem**: Queries and documents use different models

**Error Example**:
```
Documents embedded with: model_A
Queries embedded with: model_B
→ Incomparable vectors ❌
```

**Solution**:
```
✅ Use the SAME model for docs and queries
✅ Or train with similar data
```

---

## Vector Search

### What is a Vector Database?

**Definition**: A database optimized to **store and search vectors** (embeddings).

**Difference from a traditional DB**:
```
Traditional DB (SQL):
SELECT * FROM products WHERE name = 'iPhone'
→ Exact keyword search

Vector DB:
query = "smartphone"
→ Finds: iPhone, Samsung Galaxy, etc.
→ Search by MEANING
```

### Vector DB Properties

- **CRUD**: Create, Read, Update, Delete
- **Indexing**: Organizes vectors for fast search
- **ANN**: Approximate Nearest Neighbor (fast approximate search)
- **Filtering**: Supports metadata filters (WHERE clauses)
- **Scalability**: Handles millions/billions of vectors

---

### Similarity Metrics

#### 1. Euclidean Distance

**What it measures**: "Straight‑line" distance between two points

**Formula**: √((x₁−x₂)² + (y₁−y₂)² + ...)

**When to use**: When magnitude matters

**Example**:
```
Vector A: [1, 2]
Vector B: [4, 6]

Distance = √((1−4)² + (2−6)²) = √(9 + 16) = √25 = 5

Interpretation: Smaller distance = more similar
```

#### 2. Cosine Similarity

**What it measures**: Angle between vectors (ignores magnitude)

**Range**: −1 (opposite) to 1 (identical)

**When to use**: Text (absolute frequency doesn’t matter, only proportion)

**Visual Example**:
```
Vector A: [10, 0]
Vector B: [5, 0]

Euclidean Distance: Large (5)
Cosine Similarity: 1.0 (same direction)

→ Semantically similar even with different magnitudes
```

---

### Vector Search Algorithms

#### K‑Nearest Neighbors (KNN)

**What it does**: Finds the K closest neighbors

**Problem**: Slow with millions of vectors (compares with all)

**Solution**: Approximate Nearest Neighbor (ANN)

#### Popular ANN Algorithms

| Algorithm | Creator | Characteristics |
|-----------|---------|-----------------|
| **ANNOY** | Spotify | Binary trees, fast |
| **HNSW** | - | Hierarchical NSW, very accurate |
| **FAISS** | Facebook | Highly optimized, GPU |
| **ScaNN** | Google | State‑of‑the‑art, scalable |

---

### Vector DB vs Vector Libraries/Plugins

#### Vector Libraries (e.g., FAISS, ANNOY)

**Pros**:
- ✅ Simple
- ✅ Fast for small datasets

**Cons**:
- ❌ No full CRUD support
- ❌ Everything in memory (RAM)
- ❌ No complex queries (WHERE)
- ❌ No replication

**When to use**: Prototypes, small static data

#### Vector Plugins (e.g., pgvector, Elasticsearch)

**Pros**:
- ✅ Integrate with existing DB

**Cons**:
- ❌ Limited functionality
- ❌ Less optimized than a dedicated vector DB

**When to use**: You already have that DB and don’t want to add another

#### Dedicated Vector DB (e.g., Mosaic AI Vector Search)

**Pros**:
- ✅ Full CRUD
- ✅ Complex queries (metadata filters)
- ✅ Scalability
- ✅ Replication and high availability
- ✅ Governance (Unity Catalog)

**When to use**: Production, large data, governance requirements

---

### Mosaic AI Vector Search (Databricks)

#### Features
```
┌─────────────────────────────────────────────┐
│  MOSAIC AI VECTOR SEARCH                    │
├─────────────────────────────────────────────┤
│  ✅ Integrated with Lakehouse               │
│  ✅ Auto‑sync with Delta Tables             │
│  ✅ Unity Catalog (ACLs, governance)        │
│  ✅ Scalable and low‑latency                │
│  ✅ REST API + Python SDK                   │
│  ✅ Supports metadata filters               │
│  ✅ Managed (zero ops overhead)             │
└─────────────────────────────────────────────┘
```

#### Architecture
```
┌──────────────────────────────────────────────────┐
│  Source Delta Table                              │
│  (docs, chunks, metadata)                        │
└──────────────┬───────────────────────────────────┘
               │ Auto Sync
               ↓
┌──────────────────────────────────────────────────┐
│  MOSAIC AI VECTOR SEARCH ENGINE                  │
├──────────────────────────────────────────────────┤
│  • Indexer (creates indexes)                     │
│  • Vector DB (stores vectors)                    │
│  • Query Engine (processes queries)              │
└──────────────┬───────────────────────────────────┘
               ↓
┌──────────────────────────────────────────────────┐
│  MOSAIC AI MODEL SERVING                         │
│  (Embedding Models)                              │
│  • Custom Models                                 │
│  • Foundation Models (BGE, E5)                   │
│  • External Models (OpenAI, Cohere)              │
└──────────────────────────────────────────────────┘
```

#### Vector Search Setup
```python
# Step 1: Create Vector Search Endpoint
from databricks.vector_search.client import VectorSearchClient

vsc = VectorSearchClient()

vsc.create_endpoint(
    name="my_endpoint",
    endpoint_type="STANDARD"
)

# Step 2: Create index from Delta Table
vsc.create_delta_sync_index(
    endpoint_name="my_endpoint",
    index_name="my_catalog.my_schema.my_index",
    source_table_name="my_catalog.my_schema.my_docs_table",
    pipeline_type="TRIGGERED",  # or "CONTINUOUS"
    primary_key="id",
    embedding_source_column="text",  # column with text
    embedding_model_endpoint_name="my_embedding_endpoint"
)

# Step 3: Search similar documents
results = vsc.get_index(
    "my_catalog.my_schema.my_index"
).similarity_search(
    query_text="How to use Databricks?",
    columns=["id", "text", "metadata"],
    num_results=5
)
```

---

## Reranking

### What is Reranking?

**Problem**: Vector search returns top‑K results, but not all are equally relevant

**Solution**: **Reranking** = reorder results by relevance using a more sophisticated model

### Process
```
1. INITIAL RETRIEVAL (Fast)
   Query → Vector Search → Top 100 documents

2. RERANKING (Accurate)
   Query + Top 100 → Reranker Model → Top 10 most relevant reordered

3. GENERATION
   Query + Top 10 reranked → LLM → Answer
```

### Example

**Query**: "How to reset a password?"

**Without Reranking**:
```
Vector Search results (top 5):
1. "Facebook password reset" (score: 0.85)
2. "General security tutorial" (score: 0.83)
3. "Our system password reset" (score: 0.82) ← The best
4. "Password history" (score: 0.81)
5. "Secure password tips" (score: 0.80)
```

**With Reranking**:
```
Reranked results (top 5):
1. "Our system password reset" (score: 0.95) ✅
2. "Facebook password reset" (score: 0.70)
3. "General security tutorial" (score: 0.60)
4. "Secure password tips" (score: 0.55)
5. "Password history" (score: 0.40)
```

### Popular Rerankers

#### Open Source
- **Cross‑Encoders**: BERT fine‑tuned for reranking
- **bge‑reranker‑base**: Specific BGE model
- **FlashRank**: Ultra‑fast and lightweight

#### Proprietary
- **Cohere Rerank**: Cohere API
- **Jina Reranker**: Jina AI

### Benefits vs Challenges

#### ✅ Benefits
- Improves retrieval precision
- Reduces hallucinations (LLM receives better context)
- Higher answer relevance

#### ❌ Challenges
- Adds latency (extra call)
- Increases cost (another model)
- More pipeline complexity

---

## MLflow for RAG

### What is MLflow?

**Definition**: Open‑source platform to manage the **full lifecycle** of ML and GenAI models.

**Co‑developed by**: Databricks

### MLflow Tracking

**What for**: Log experiments and compare

**What it logs**:
- LLM parameters (temperature, max_tokens, etc.)
- Metrics (accuracy, latency, cost, etc.)
- Artifacts (models, prompts, chains, outputs)
- Experiment source code

**Example**:
```python
import mlflow

with mlflow.start_run():
    # Log parameters
    mlflow.log_param("model", "gpt-4")
    mlflow.log_param("temperature", 0.7)
    
    # Your RAG code here
    response = my_rag_chain(query)
    
    # Log metrics
    mlflow.log_metric("latency", 1.5)  # seconds
    mlflow.log_metric("relevance_score", 0.95)
    
    # Log artifacts
    mlflow.log_artifact("prompt_template.txt")
```

---

### MLflow Model (Flavors)

**What is a "flavor"?**: Standard format to package models

**Structure**:
```
my_model/
  ├── MLmodel  (metadata file)
  ├── conda.yaml  (dependencies)
  ├── requirements.txt
  └── model/  (model files)
```

**Supported flavors**:
- `mlflow.pyfunc` (Python function – generic)
- `mlflow.langchain` (LangChain chains)
- `mlflow.openai` (OpenAI models)
- `mlflow.transformers` (Hugging Face)
- `mlflow.pytorch`, `mlflow.tensorflow`, etc.

---

### MLflow Model Registry (in Unity Catalog)

**What for**: Organize, version, and deploy models

**Features**:
- ✅ Automatic versioning
- ✅ Aliases (`@champion`, `@challenger`)
- ✅ Lifecycle management (dev → staging → prod)
- ✅ Collaboration and ACLs (Unity Catalog)
- ✅ Full lineage (which data, code, params used)
- ✅ Tagging and annotations

**Example**:
```python
# Register model in Unity Catalog
mlflow.set_registry_uri("databricks-uc")

model_uri = f"runs:/{run_id}/model"
registered_model = mlflow.register_model(
    model_uri,
    "my_catalog.my_schema.my_rag_chatbot"
)

# Assign alias
from mlflow import MlflowClient
client = MlflowClient()

client.set_registered_model_alias(
    "my_catalog.my_schema.my_rag_chatbot",
    "champion",
    version=3
)
```

---

## RAG Evaluation

### What to Evaluate in RAG?
```
┌────────────────────────────────────────┐
│  RAG PIPELINE                          │
├────────────────────────────────────────┤
│  1. CHUNKING                           │
│     ↓                                  │
│     Evaluate: Chunk size, overlap, etc.│
├────────────────────────────────────────┤
│  2. RETRIEVAL                          │
│     ↓                                  │
│     Evaluate: Precision, Recall, etc.  │
├────────────────────────────────────────┤
│  3. GENERATION                         │
│     ↓                                  │
│     Evaluate: Relevance, Faithfulness  │
└────────────────────────────────────────┘
```

---

### Key Evaluation Metrics

#### 1. Context Precision

**What it measures**: Are the retrieved documents relevant?

**Formula**: (Relevant retrieved docs) / (Total retrieved docs)

**Example**:
```
Query: "How to reset a password?"
Retrieved: 5 docs
Relevant: 3 docs

Context Precision = 3/5 = 0.6
```

#### 2. Context Recall

**What it measures**: Did we retrieve ALL relevant documents?

**Formula**: (Relevant retrieved docs) / (Total relevant docs in DB)

**Example**:
```
Relevant docs in DB: 10
Retrieved: 5 docs (all relevant)

Context Recall = 5/10 = 0.5
```

**Interpretation**: Low recall = we’re missing important info

#### 3. Context Relevance

**What it measures**: Is the retrieved context pertinent to the query?

**Measurement**: LLM‑as‑a‑judge evaluates each doc

**Example**:
```
Query: "Vacation policy"

Doc 1: "Employees have 15 days of vacation" → High relevance
Doc 2: "Company history founded in 1990" → Low relevance
```

#### 4. Faithfulness

**What it measures**: Is the answer faithful to the context? (No hallucinations)

**Formula**: (Claims in the answer supported by context) / (Total claims)

**Example**:
```
Context: "Employees have 15 days of vacation"

LLM Answer: "You have 15 days of paid vacation"
→ "15 days" ✅ (in context)
→ "paid" ❌ (not mentioned)

Faithfulness = 1/2 = 0.5 (low fidelity)
```

#### 5. Answer Relevancy

**What it measures**: Does the answer actually address the question?

**Example**:
```
Query: "How many vacation days do I have?"

Answer 1: "You have 15 vacation days per year"
→ High relevance ✅

Answer 2: "Vacations are important for well‑being"
→ Low relevance ❌ (doesn’t answer)
```

#### 6. Answer Correctness

**What it measures**: Is the answer factually correct?

**Requires**: Ground truth (known correct answer)

**Example**:
```
Query: "Capital of France?"

Ground Truth: "Paris"
LLM Answer: "Paris"
→ Correctness = 1.0 ✅

LLM Answer: "Lyon"
→ Correctness = 0.0 ❌
```

---

### MLflow LLM Evaluation

**Features**:
- ✅ Batch evaluation (many queries at once)
- ✅ Comparison of multiple models/prompts
- ✅ Automatic metrics (toxicity, perplexity, etc.)
- ✅ Built‑in LLM‑as‑a‑judge
- ✅ Cost‑effective

**Example**:
```python
import mlflow

# Evaluation dataset
eval_data = pd.DataFrame({
    "query": ["How to reset password?", "Vacation policy?"],
    "ground_truth": ["Go to Settings > Reset", "15 days per year"]
})

# Evaluate model
results = mlflow.evaluate(
    model="models:/my_rag_chatbot@champion",
    data=eval_data,
    targets="ground_truth",
    model_type="question-answering",
    evaluators=["default"]
)

# View results
print(results.metrics)
```

---

## 🎯 Practice Questions

### Question 1
**Which prompting technique provides examples to the LLM?**

A) Zero‑shot  
B) Few‑shot ✅  
C) Chain‑of‑Thought  
D) Prompt Chaining

**Answer**: B – Few‑shot = provide examples

---

### Question 2
**What does RAG solve?**

A) Latency issues  
B) LLM knowledge gap ✅  
C) Model cost  
D) Data bias

**Answer**: B – RAG adds updated external knowledge

---

### Question 3
**Which chunking strategy respects document structure?**

A) Fixed‑size  
B) Context‑aware ✅  
C) Random chunking  
D) No chunking

**Answer**: B – Context‑aware splits by sections/paragraphs

---

### Question 4
**Which metric measures if the answer is faithful to the context?**

A) Context Precision  
B) Faithfulness ✅  
C) Answer Relevancy  
D) Perplexity

**Answer**: B – Faithfulness = no hallucination

---

### Question 5
**For queries and documents in vector search, what is important?**

A) Use different models  
B) Use the same embedding model ✅  
C) Embeddings with different dimensions  
D) It doesn’t matter

**Answer**: B – Same model = same vector space

---

## 📝 Executive Summary

### What you MUST know:

✅ **Prompt Engineering**: Zero‑shot, Few‑shot, Chain‑of‑Thought, Prompt Chaining  
✅ **RAG** = Retrieval + Augmentation + Generation (solves knowledge gap)  
✅ **Chunking**: Fixed‑size (simple) vs Context‑aware (better), use overlap  
✅ **Embeddings**: Vector representations, same model for queries and docs  
✅ **Vector Search**: Searches by meaning (semantics), not exact word  
✅ **Mosaic AI Vector Search**: Integrated with Lakehouse, Unity Catalog, auto‑sync  
✅ **Reranking**: Improves precision by reordering results  
✅ **MLflow**: Tracking, Registry (Unity Catalog), Evaluation  
✅ **RAG Metrics**: Context Precision/Recall, Faithfulness, Answer Relevancy/Correctness

---

## 🔗 Next Topic

➡️ **Continue with**: `03_Application_Development.md` (Compound AI Systems, Agents, Multi‑modal)
