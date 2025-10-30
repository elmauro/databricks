# 3️⃣ Generative AI Application Development

## 📚 Table of Contents
1. [Compound AI Systems](#compound-ai-systems)
2. [Multi-Stage Reasoning Chains](#multi-stage-reasoning-chains)
3. [Composition Frameworks](#composition-frameworks)
4. [Databricks Products](#databricks-products-for-compound-systems)
5. [Agents](#agents-agentes)
6. [Multi-Modal AI](#multi-modal-ai)

---

## Compound AI Systems

### What is a Compound AI System?

A **compound AI system** has **multiple components** interacting with each other.

**Key difference**:
```
❌ SIMPLE AI System:
Query → LLM → Response
(One step, one model)

✅ COMPOUND AI System:
Query → [Search] → [Filtering] → [LLM 1] → [Validation] → [LLM 2] → Response
(Multiple steps, multiple components)
```

### Practical Example

#### Simple RAG (Simple AI System)
```
User: "Vacation policy?"
↓
[1 step] Vector Search + LLM
↓
Answer: "15 days per year"
```

#### Compound RAG (Compound AI System)
```
User: "Summarize the 2024 policy changes and compare them with 2023"
↓
[Step 1] Classify intent: "Summary + Comparison"
↓
[Step 2] Search 2024 docs
↓
[Step 3] Search 2023 docs
↓
[Step 4] LLM summarizes each year (in parallel)
↓
[Step 5] LLM compares both summaries
↓
Complete answer
```

---

### Problem: Real-World Prompts Have Multiple Intents

**Example**:
```
Query: "Translate this document to French and then summarize the 5 key points"

Intents:
1. Translation (English → French)
2. Summarization

Tasks:
Task 1: Translate document
Task 2: Extract 5 key points from the translated text
```

**Dependencies**:
```
Task 1 (Translate) → MUST be completed first
↓
Task 2 (Summarize) → Depends on Task 1 output
```

---

### Task Types in Compound Systems

| Type | Description | Example |
|------|-------------|---------|
| **Single LLM Interaction** | One call to the LLM | Generate an email |
| **Tool Call** | LLM uses an external tool | Query SQL database |
| **Chain** | Sequence of interactions | Translate → Summarize → Analyze |
| **Agent** | System that decides dynamically | Research agent |

---

### Use Case: Multi-Article Sentiment Analysis

**Goal**: Get sentiment from many articles on a topic

#### ❌ Naive Solution (Fails)
```
Prompt: "Here are 100 full articles [paste all].
         Give me the overall sentiment."

Problem: 
- Exceeds token limit (context window)
- LLM loses information in the middle ("lost in the middle")
```

#### ✅ Compound Solution (Works)
```
PHASE 1: Summarization (Parallelized)
   Article 1 → LLM → Summary 1
   Article 2 → LLM → Summary 2
   ...
   Article 100 → LLM → Summary 100

PHASE 2: Sentiment Analysis
   [Summary 1, Summary 2, ..., Summary 100] → LLM → Overall sentiment

Advantages:
✅ Does not exceed context window
✅ Processes all articles
✅ More accurate
```

---

## Designing Compound AI Systems

### Design Process
```
┌─────────────────────────────────────────┐
│  1. ANALYSIS                            │
│  • Identify the problem                 │
│  • Define objectives                    │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. DESIGN                              │
│  • Identify intents                     │
│  • Decompose into tasks                 │
│  • Identify necessary tools             │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. DEVELOPMENT                         │
│  • Build each component                 │
│  • Integrate components                 │
│  • Test the flow                        │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. PRODUCTION                          │
│  • Deploy                               │
│  • Configure monitoring                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  5. MONITORING                          │
│  • Metrics tracking                     │
│  • Iteration and improvement            │
└─────────────────────────────────────────┘
```

---

### Step 1: Intent Classification

**What to do?**:
1. **Identify Intents**: What does the user want?
2. **Identify Dependencies**: What must happen first?
3. **Identify Tools**: Which tools are needed?

**Practical Example**:

**Query**: "Research NVIDIA’s current stock price, analyze the latest news, and give me an investment recommendation"

**Identified Intents**:
- Intent 1: Get current price
- Intent 2: Analyze news
- Intent 3: Generate recommendation

**Dependencies**:
```
[Intent 1: Price] ──┐
                    ├──> [Intent 3: Recommendation]
[Intent 2: News] ──┘

Intent 1 and 2 can run in parallel
Intent 3 depends on 1 and 2
```

**Required Tools**:
- Financial API (stock price)
- Web Search (news)
- LLM (analysis and recommendation)

---

### Step 2: Design Architecture

**Example**: Compound RAG System
```
┌────────────────────────────────────────────────────┐
│           CompoundRAGApp Class                     │
├────────────────────────────────────────────────────┤
│                                                    │
│  run_search()                                      │
│    ↓                                               │
│    Databricks Vector Search                        │
│    → Retrieve relevant documents                   │
│                                                    │
│  run_augmented_summary()                           │
│    ↓                                               │
│    Summary LLM (Model Serving)                     │
│    → Summarize each document                       │
│                                                    │
│  run_get_context()                                 │
│    ↓                                               │
│    Combine summaries into context                  │
│                                                    │
│  run_qa()                                          │
│    ↓                                               │
│    QA LLM (Model Serving)                          │
│    → Generate final answer                         │
│                                                    │
└────────────────────────────────────────────────────┘
```

**Flow**:
```python
class CompoundRAGApp:
    def answer_question(self, query):
        # 1. Search for relevant documents
        docs = self.run_search(query)
        
        # 2. Summarize each document (parallelized)
        summaries = [self.run_augmented_summary(doc) for doc in docs]
        
        # 3. Combine summaries into context
        context = self.run_get_context(summaries)
        
        # 4. Generate final answer
        answer = self.run_qa(query, context)
        
        return answer
```

---

## Multi-Stage Reasoning Chains

### Concept

**Definition**: Systems that go through multiple reasoning stages before generating the final answer.

**Analogy**: Like solving a math problem step by step instead of jumping to the answer.

### Concept Mapping

| RAG Concept | Framework Concept |
|-------------|-------------------|
| Retrieval | Tool |
| Tasks | Order of tools |
| Question + Answer | Intent + Route |
| Metadata | Parameters for Route |
| Generation | Reasoning |

---

## Composition Frameworks

### Why use frameworks?

**Problem**: Building compound AI systems from scratch is complex

**Solution**: Frameworks that abstract complexity

---

### 1. LangChain 🦜🔗

**What it is**: The most popular framework for GenAI applications

**Key Concepts**:

#### Prompt
```python
from langchain import PromptTemplate

template = """
You are an expert in {topic}.
Question: {question}
Answer:
"""

prompt = PromptTemplate(
    input_variables=["topic", "question"],
    template=template
)
```

#### Chain
```python
from langchain.chains import LLMChain

chain = LLMChain(
    llm=my_llm,
    prompt=prompt
)

result = chain.run(topic="Python", question="What is a list?")
```

#### Retriever
```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings

vectorstore = FAISS.from_documents(docs, OpenAIEmbeddings())
retriever = vectorstore.as_retriever()

docs = retriever.get_relevant_documents("my query")
```

#### Tool
```python
from langchain.tools import Tool

def web_search(query):
    # Search logic
    return results

tool = Tool(
    name="WebSearch",
    func=web_search,
    description="Searches information on the web"
)
```

---

### 2. LlamaIndex 🦙

**What it is**: Framework focused on **data indexing and retrieval**

**Key Concepts**:

#### Models
```python
from llama_index.llms import OpenAI

llm = OpenAI(model="gpt-4")
```

#### Indexing
```python
from llama_index import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader('data').load_data()
index = VectorStoreIndex.from_documents(documents)
```

#### Querying
```python
query_engine = index.as_query_engine()
response = query_engine.query("What is Python?")
```

#### Agents
```python
from llama_index.agent import OpenAIAgent

agent = OpenAIAgent.from_tools(tools, llm=llm)
response = agent.chat("Search for information about NVIDIA")
```

---

### 3. Haystack 🌾

**What it is**: Open-source framework focused on **document retrieval, generation, summarization**

**Key Concepts**:

#### Pipelines
```python
from haystack import Pipeline
from haystack.nodes import Retriever, Generator

pipeline = Pipeline()
pipeline.add_node(component=retriever, name="Retriever", inputs=["Query"])
pipeline.add_node(component=generator, name="Generator", inputs=["Retriever"])

result = pipeline.run(query="What is AI?")
```

---

### 4. DSPy 🎯

**What it is**: Framework for **programming with LLMs** in a more structured way

**Approach**: Instead of prompts, it uses "signatures" and automatic compilation

**Key Concepts**:

#### Signatures
```python
class QA(dspy.Signature):
    """Answer questions based on context"""
    context = dspy.InputField()
    question = dspy.InputField()
    answer = dspy.OutputField()
```

#### Teleprompters
Automatic prompt optimizers
```python
optimizer = BootstrapFewShot()
optimized_module = optimizer.compile(module, trainset=trainset)
```

---

### Framework Comparison

| Framework | Strength | Best For | Learning Curve |
|-----------|----------|----------|----------------|
| **LangChain** | Versatility | General applications | Medium |
| **LlamaIndex** | Indexing | Advanced RAG | Medium-Low |
| **Haystack** | Pipelines | Document processing | Medium |
| **DSPy** | Optimization | Complex systems | High |

---

### How to Choose a Framework

**Key Questions**:

1. **Features**: Does it have what I need?
2. **Performance**: Is it fast enough?
3. **Scalability**: Does it scale with my data?
4. **Stability**: Is it mature and maintained?
5. **Complexity**: Can I learn it in time?

**General Recommendation**:
- 🆕 Getting started: **LangChain** (more resources, community)
- 🔍 Advanced RAG: **LlamaIndex**
- 📄 Document processing: **Haystack**
- 🎓 Research: **DSPy**

---

## Databricks Products for Compound Systems

### 1. Foundation Model API

**What it is**: Unified API to access LLMs

**Features**:
- ✅ **Instant Access**: No complex setup
- ✅ **Pay-per-token**: Pay only for what you use
- ✅ **External Models**: Integrates Azure OpenAI, AWS Bedrock
- ✅ **Unified Interface**: Same API for all models

**Supported Models**:

| Model | Type | Task | Characteristics |
|-------|------|------|-----------------|
| **DBRX Instruct** | Databricks OSS | Chat | Mixture-of-Experts, enterprise-optimized |
| **Meta LLaMA 3** | Meta OSS | Chat | 8B and 70B parameters |
| **LLaMA 2 70B** | Meta OSS | Chat | Previous model |
| **Mixtral-8x7B** | Mistral OSS | Chat | Mixture-of-Experts |
| **BGE Large** | BAAI OSS | Embeddings | Multilingual |

**Usage**:
```python
from databricks_genai_inference import ChatSession

chat = ChatSession(
    model="databricks-dbrx-instruct",
    system_message="You are a helpful assistant"
)

response = chat.reply("What is Databricks?")
print(response.message)
```

---

### 2. Mosaic AI Vector Search

**Already covered earlier**, but quick reminder:

- ✅ Vector DB integrated with the Lakehouse
- ✅ Unity Catalog (ACLs, governance)
- ✅ Auto-sync with Delta Tables
- ✅ REST API + Python SDK
- ✅ Scalable and low-latency

---

### 3. MLflow Tracking

**For Compound Systems**: Track the entire chain/pipeline
```python
import mlflow

with mlflow.start_run(run_name="compound_rag_experiment"):
    # Log configuration
    mlflow.log_param("retriever_model", "bge-large")
    mlflow.log_param("llm_model", "dbrx-instruct")
    mlflow.log_param("num_docs_retrieved", 5)
    
    # Your compound system
    result = my_compound_system.run(query)
    
    # Log metrics
    mlflow.log_metric("total_latency", result.latency)
    mlflow.log_metric("retrieval_time", result.retrieval_time)
    mlflow.log_metric("generation_time", result.generation_time)
    mlflow.log_metric("relevance_score", result.relevance)
    
    # Log the full chain
    mlflow.langchain.log_model(my_compound_system, "model")
```

---

### 4. Model Serving

**For Compound Systems**: Serve entire chains/pipelines
```python
# Register chain in Unity Catalog
mlflow.langchain.log_model(
    my_chain,
    "model",
    registered_model_name="my_catalog.my_schema.my_chain"
)

# Create serving endpoint
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

w.serving_endpoints.create(
    name="my_compound_rag_endpoint",
    config={
        "served_models": [{
            "model_name": "my_catalog.my_schema.my_chain",
            "model_version": "1",
            "workload_size": "Small",
            "scale_to_zero_enabled": True
        }]
    }
)
```

---

### 5. MLflow Evaluation

**For Compound Systems**: End-to-end evaluation
```python
import mlflow

eval_data = pd.DataFrame({
    "query": ["What is RAG?", "How to use Vector Search?"],
    "expected_response": ["RAG is...", "Vector Search is used..."]
})

with mlflow.start_run():
    results = mlflow.evaluate(
        model="models:/my_chain@champion",
        data=eval_data,
        targets="expected_response",
        model_type="question-answering",
        evaluators=["default", "toxicity", "faithfulness"]
    )
    
    print(results.metrics)
```

---

### 6. Lakehouse Monitoring

**For Compound Systems**: Production monitoring

- Monitor each component of the pipeline
- Detect data drift
- Track latency, cost, quality

---

## Agents (Agentes)

### What is an Agent?

An application that can **dynamically decide** which actions to take to complete a complex task.

**Key Difference**:
```
❌ Chain (Fixed sequence):
Query → [Step 1] → [Step 2] → [Step 3] → Response
(Always the same steps in the same order)

✅ Agent (Dynamic decision):
Query → Agent decides → [Steps needed based on the query] → Response
(Steps vary by task)
```

---

### Agent Components
```
┌─────────────────────────────────────────┐
│  AGENT                                  │
├─────────────────────────────────────────┤
│  1. TASK                                │
│     • User request (prompt)             │
├─────────────────────────────────────────┤
│  2. LLM (Brain)                         │
│     • Coordinates logic                 │
│     • Decides what to do                │
├─────────────────────────────────────────┤
│  3. TOOLS                               │
│     • External resources                │
│     • APIs, databases, search, etc.     │
├─────────────────────────────────────────┤
│  4. MEMORY & PLANNING                   │
│     • Remembers prior actions           │
│     • Plans next steps                  │
└─────────────────────────────────────────┘
```

---

### Agent Example: Stock Investment Advisor

**Task**:
```
"Is it a good time to invest in NVIDIA stock?"
```

**Agent Workflow**:
```
┌─────────────────────────────────────────┐
│  1. PROCESS TASK                        │
│     Agent analyzes the question         │
│     Identifies sub‑tasks:               │
│       - Current price                   │
│       - Recent financials               │
│       - News/announcements              │
│       - Sentiment score                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. COLLECT DATA                        │
│     • Tool 1: Financial API             │
│       → Current price: $850             │
│     • Tool 2: Company API               │
│       → Q4 Revenue: +50%                │
│     • Tool 3: News Search               │
│       → "NVIDIA launches new AI chip"   │
│     • Tool 4: Sentiment API             │
│       → Sentiment: 0.85 (positive)      │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. DATA ANALYSIS                       │
│     LLM analyzes all data               │
│     Identifies trends                   │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. OUTPUT GENERATION                   │
│     LLM synthesizes full report:        │
│     "Based on the current price..."     │
│     "50% growth..."                     │
│     "Positive market sentiment..."      │
│     "Recommendation: Moderate Buy"      │
└─────────────────────────────────────────┘
```

**The important part**: The agent **dynamically decided**:
- Which tools to use
- In what order
- How many times to call each tool
- How to combine information

---

### Agent Reasoning Patterns

#### 1. ReAct (Reasoning + Acting)

**What it is**: Pattern where the agent generates **reasoning traces** (thoughts) and **actions**.

**Loop States**:
```
1. THOUGHT
   "I need the current NVIDIA price"
   ↓
2. ACT
   Tool: FinancialAPI(symbol="NVDA")
   ↓
3. OBSERVE
   Result: "$850"
   ↓
4. THOUGHT (Next)
   "Now I need recent news"
   ↓
5. ACT
   Tool: NewsSearch(query="NVIDIA news")
   ↓
6. OBSERVE
   Result: [articles]
   ↓
7. THOUGHT
   "I have enough information to answer"
   ↓
8. FINAL ANSWER
```

**Code Example (LangChain)**:
```python
from langchain.agents import create_react_agent

agent = create_react_agent(
    llm=llm,
    tools=[financial_tool, news_tool, sentiment_tool],
    prompt=react_prompt
)

result = agent.invoke({"input": "Invest in NVIDIA?"})
```

---

#### 2. Tool Use

**What it is**: Agent interacts with **external tools/APIs** for specific tasks.

**Tool Types**:

| Tool Type | Example | Use |
|-----------|---------|-----|
| **Research/Search** | Google, Wikipedia | Search information |
| **Image** | DALL‑E, Stable Diffusion | Generate images |
| **Document Retrieval** | Vector Search | RAG |
| **Coding** | Code interpreter | Execute code |
| **APIs** | Weather API, Stock API | External data |
| **Databases** | SQL query | Query data |

**Example**:
```python
from langchain.tools import Tool

def search_web(query: str) -> str:
    # Search logic
    return results

def execute_sql(query: str) -> str:
    # Execute SQL
    return results

tools = [
    Tool(name="WebSearch", func=search_web, description="Search the web"),
    Tool(name="SQLQuery", func=execute_sql, description="Query DB")
]

agent = create_agent(llm=llm, tools=tools)
```

---

#### 3. Planning

**What it is**: Agent breaks a complex task into **sub‑tasks** and orchestrates them.

**Task Types**:
```
SINGLE TASK:
Task → Execute → Done

SEQUENTIAL TASKS:
Task A → Task B → Task C → Done
(B depends on A, C depends on B)

GRAPH TASKS (Parallel):
       ┌─ Task B ─┐
Task A ┤          ├─> Task D → Done
       └─ Task C ─┘
(B and C run in parallel)
```

**Example**:
```
Task: "Research NVIDIA competitors, compare prices, generate report"

Agent Plan:
1. [Parallel] Research NVIDIA → price, products
2. [Parallel] Research AMD → price, products
3. [Parallel] Research Intel → price, products
4. [Sequential] Compare collected data
5. [Sequential] Generate final report
```

---

#### 4. Multi‑Agent Collaboration

**What it is**: Several agents working together, each specialized.

**Example**:
```
┌─────────────────────────────────────────┐
│  COORDINATOR AGENT                      │
│  (Orchestrates the flow)                │
└─────────────┬───────────────────────────┘
              │
    ┌─────────┼─────────┬─────────────┐
    ↓         ↓         ↓             ↓
┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
│Research│ │ Data   │ │Writing │ │Review  │
│Agent   │ │Analysis│ │Agent   │ │Agent   │
│        │ │Agent   │ │        │ │        │
└────────┘ └────────┘ └────────┘ └────────┘
```

**Flow**:
1. Coordinator assigns task to Research Agent
2. Research Agent searches for information
3. Data Analysis Agent analyzes data
4. Writing Agent generates report
5. Review Agent reviews and gives feedback
6. If corrections are needed, returns to Writing Agent
7. Coordinator delivers final result

**Advantages**:
- ✅ Specialization (each agent is expert in their area)
- ✅ Scalability (add more agents)
- ✅ Flexibility (different LLMs for different agents)

---

### Tools for Building Agents

| Tool | Type | Description |
|------|------|-------------|
| **LangChain Agents** | Framework | Agents with tools |
| **AutoGPT** | Framework | Autonomous agents |
| **OpenAI Function Calling** | API | LLM calls defined functions |
| **AutoGen** | Framework | Multi‑agent collaboration |
| **Transformers Agents** | Library | Agents with Hugging Face models |

---

### Code Example: Agent with LangChain
```python
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI

# Define tools
def search_web(query):
    # Implementation
    return "Search results..."

def get_stock_price(symbol):
    # Implementation
    return "$850"

tools = [
    Tool(
        name="WebSearch",
        func=search_web,
        description="Searches information on the web"
    ),
    Tool(
        name="StockPrice",
        func=get_stock_price,
        description="Gets a stock price. Input: ticker symbol"
    )
]

# Create agent
llm = OpenAI(temperature=0)
agent = initialize_agent(
    tools,
    llm,
    agent="zero-shot-react-description",  # ReAct pattern
    verbose=True
)

# Use agent
response = agent.run("What is NVIDIA’s current price and what does the news say?")
```

**Example output**:
```
> Entering new AgentExecutor chain...
Thought: I need to get the price and search for news
Action: StockPrice
Action Input: NVDA
Observation: $850
Thought: Now I search for news
Action: WebSearch
Action Input: NVIDIA recent news
Observation: [articles about new chip]
Thought: I have all the necessary information
Final Answer: NVIDIA’s current price is $850. Recent news
mentions the launch of a new AI chip...
```

---

## Multi‑Modal AI

### What is Multi‑Modal AI?

**Definition**: AI applications with **inputs or outputs** that include data types **beyond just text**.

**Common Data Types**:
- 📝 Text
- 🖼️ Image
- 🎵 Audio
- 🎬 Video

---

### Types of Multi‑Modal Applications

#### 1. Multi‑Modal Input

**Example**: Image → Text (Captioning)
```
Input: [Image of a cat]
Model: GPT‑4 Vision
Output: "An orange cat sleeping on a sofa"
```

**Usage**:
```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "What’s in this image?"},
            {"type": "image_url", "image_url": {"url": "https://..."}}
        ]
    }]
)
```

#### 2. Multi‑Modal Output

**Example**: Text → Image (Generation)
```
Input: "A dog astronaut in space"
Model: DALL‑E 3
Output: [Generated image]
```

#### 3. Multi‑Modal Input + Output

**Example**: Image + Text → Text + Image (Editing)
```
Input: [Image] + "Add a hat to the cat"
Model: DALL‑E Edit
Output: [Edited image] + "I added a red hat"
```

---

### Multi‑Modal Architectures

#### Multi‑Modal Retrieval (RAG with Images)
```
Query: "Show me logos similar to this"
[Logo image]
↓
Embedding Model (CLIP)
↓
Vector Search (find similar images)
↓
[Top 5 similar logos]
```

**Models for Multi‑Modal Embeddings**:
- **CLIP** (OpenAI): Text + Image embeddings in the same space
- **BLIP** (Salesforce): Image → text, text → image
- **ImageBind** (Meta): 6 modalities (text, image, audio, depth, thermal, IMU)

#### Multi‑Modal Generator

**Example**: Generate a story with images
```
Input: "Create a short story about a space trip"
↓
Multi‑Modal LLM (GPT‑4V)
↓
Output:
  Text: "An astronaut named Alex traveled to Mars..."
  Image 1: [Astronaut in a spaceship]
  Text: "Upon arrival, he discovered..."
  Image 2: [Martian landscape]
```

---

### Popular Multi‑Modal Models

| Model | Type | Capabilities |
|------|------|--------------|
| **GPT‑4 Vision** | Proprietary | Text → Text, Image+Text → Text |
| **Claude 3** | Proprietary | Similar to GPT‑4V |
| **Gemini** | Proprietary | Text, Image, Audio, Video |
| **LLaVA** | Open Source | Image + Text → Text |
| **Fuyu‑8B** | Open Source | Multi‑modal understanding |

---

### Use Case: Document Understanding

**Problem**: Complex PDF with tables, diagrams, multiple columns

**Multi‑Modal Solution**:
```
PDF Page → Image
↓
GPT‑4 Vision
↓
Prompt: "Extract the content of this page in markdown,
        including tables and diagram descriptions"
↓
Output:
  ## Title
  
  Paragraph text...
  
  | Col1 | Col2 | Col3 |
  |------|------|------|
  | A    | B    | C    |
  
  **Diagram**: Process flow showing...
```

**Advantages over traditional OCR**:
- ✅ Understands complex layout
- ✅ Interprets diagrams
- ✅ Maintains context
- ✅ Not just text extraction; it understands content

---

## 🎯 Practice Questions

### Question 1
**What differentiates a Compound AI System from a Simple AI System?**

A) Compound uses more data  
B) Compound has multiple interacting components ✅  
C) Compound is faster  
D) No difference

**Answer**: B – Compound = multiple components/steps

---

### Question 2
**Which agent pattern uses Thought → Act → Observe?**

A) Planning  
B) Tool Use  
C) ReAct ✅  
D) Multi‑Agent

**Answer**: C – ReAct = Reasoning + Acting with observational loop

---

### Question 3
**Which framework is best for advanced RAG?**

A) LangChain  
B) LlamaIndex ✅  
C) Haystack  
D) DSPy

**Answer**: B – LlamaIndex specializes in indexing/retrieval

---

### Question 4
**Which model would you use for text + image embeddings in the same space?**

A) GPT‑4  
B) BERT  
C) CLIP ✅  
D) LLaMA

**Answer**: C – CLIP is multi‑modal (text + image)

---

### Question 5
**In multi‑agent collaboration, who orchestrates the flow?**

A) Research Agent  
B) Coordinator Agent ✅  
C) Review Agent  
D) There is no coordinator

**Answer**: B – Coordinator organizes and assigns tasks

---

## 📝 Executive Summary

### What you MUST know:

✅ **Compound AI System** = multiple interacting components (vs simple = 1 step)  
✅ **Intent Classification**: Identify intents, dependencies, needed tools  
✅ **Frameworks**: LangChain (general), LlamaIndex (RAG), Haystack (docs), DSPy (optimization)  
✅ **Databricks Products**: Foundation Model API, Vector Search, MLflow, Model Serving  
✅ **Agent** = dynamically decides what to do (vs chain = fixed sequence)  
✅ **Agent Components**: Task, LLM (brain), Tools, Memory/Planning  
✅ **ReAct Pattern**: Thought → Act → Observe (loop)  
✅ **Multi‑Agent**: Multiple specialized agents collaborating  
✅ **Multi‑Modal**: Input/output beyond text (image, audio, video)  
✅ **CLIP**: Multi‑modal embeddings (text + image)

---

## 🔗 Next Topic

➡️ **Continue with**: `04_Deployment_Monitoring.md` (Deployment, MLflow, Model Serving, Monitoring)
