# 3️⃣ Desarrollo de Aplicaciones de IA Generativa

## 📚 Tabla de Contenidos
1. [Compound AI Systems](#compound-ai-systems)
2. [Multi-Stage Reasoning Chains](#multi-stage-reasoning-chains)
3. [Frameworks de Composición](#frameworks-de-composición)
4. [Productos Databricks](#productos-databricks-para-compound-systems)
5. [Agents (Agentes)](#agents-agentes)
6. [Multi-Modal AI](#multi-modal-ai)

---

## Compound AI Systems

### ¿Qué es un Compound AI System?

Un **sistema de IA compuesto** que tiene **múltiples componentes** interactuando entre sí.

**Diferencia clave**:

```
❌ SIMPLE AI System:
Query → LLM → Response
(Un solo paso, un solo modelo)

✅ COMPOUND AI System:
Query → [Búsqueda] → [Filtrado] → [LLM 1] → [Validación] → [LLM 2] → Response
(Múltiples pasos, múltiples componentes)
```

### Ejemplo Práctico

#### Simple RAG (Simple AI System)
```
Usuario: "¿Política de vacaciones?"
↓
[1 paso] Vector Search + LLM
↓
Respuesta: "15 días al año"
```

#### Compound RAG (Compound AI System)
```
Usuario: "Resume los cambios en políticas de 2024 y compáralos con 2023"
↓
[Paso 1] Clasificar intención: "Resumen + Comparación"
↓
[Paso 2] Buscar docs 2024
↓
[Paso 3] Buscar docs 2023
↓
[Paso 4] LLM resume cada año (paralelamente)
↓
[Paso 5] LLM compara ambos resúmenes
↓
Respuesta completa
```

---

### Problema: Prompts del Mundo Real Tienen Múltiples Intents

**Ejemplo**:
```
Query: "Traduce este documento al francés y luego resume los 5 puntos clave"

Intents:
1. Traducción (inglés → francés)
2. Summarization (resumen)

Tasks:
Task 1: Traducir documento
Task 2: Extraer 5 puntos clave del traducido
```

**Dependencias**:
```
Task 1 (Traducir) → DEBE completarse primero
↓
Task 2 (Resumir) → Depende del output de Task 1
```

---

### Tipos de Tasks en Compound Systems

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **Single LLM Interaction** | Una llamada al LLM | Generar un email |
| **Tool Call** | LLM usa herramienta externa | Query SQL database |
| **Chain** | Secuencia de interactions | Traducir → Resumir → Analizar |
| **Agent** | Sistema que decide dinámicamente | Agente investigador |

---

### Caso de Uso: Análisis de Sentimiento Multi-Artículo

**Objetivo**: Obtener el sentimiento de muchos artículos sobre un tema

#### ❌ Solución Ingenua (Falla)
```
Prompt: "Aquí están 100 artículos completos [pega todo].
         Dame el sentimiento general."

Problema: 
- Excede límite de tokens (context window)
- LLM pierde información del medio ("lost in the middle")
```

#### ✅ Solución Compound (Funciona)
```
FASE 1: Summarization (Paralelizada)
   Artículo 1 → LLM → Resumen 1
   Artículo 2 → LLM → Resumen 2
   ...
   Artículo 100 → LLM → Resumen 100

FASE 2: Sentiment Analysis
   [Resumen 1, Resumen 2, ..., Resumen 100] → LLM → Sentimiento general

Ventajas:
✅ No excede context window
✅ Procesa todos los artículos
✅ Más preciso
```

---

## Diseñando Compound AI Systems

### Proceso de Diseño

```
┌─────────────────────────────────────────┐
│  1. ANÁLISIS                            │
│  • Identificar el problema              │
│  • Definir objetivos                    │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. DISEÑO                              │
│  • Identificar intents                  │
│  • Descomponer en tasks                 │
│  • Identificar herramientas necesarias  │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. DESARROLLO                          │
│  • Construir cada componente            │
│  • Integrar componentes                 │
│  • Probar el flujo                      │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. PRODUCCIÓN                          │
│  • Desplegar                            │
│  • Configurar monitoreo                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  5. MONITOREO                           │
│  • Tracking de métricas                 │
│  • Iteración y mejora                   │
└─────────────────────────────────────────┘
```

---

### Paso 1: Intent Classification

**¿Qué hacer?**:
1. **Identificar Intents**: ¿Qué quiere el usuario?
2. **Identificar Dependencias**: ¿Qué debe pasar primero?
3. **Identificar Tools**: ¿Qué herramientas necesito?

**Ejemplo Práctico**:

**Query**: "Investiga el precio actual de acciones de NVIDIA, analiza las últimas noticias y dame una recomendación de inversión"

**Intents Identificados**:
- Intent 1: Obtener precio actual
- Intent 2: Analizar noticias
- Intent 3: Generar recomendación

**Dependencies**:
```
[Intent 1: Precio] ──┐
                     ├──> [Intent 3: Recomendación]
[Intent 2: Noticias] ─┘

Intent 1 y 2 pueden ser paralelos
Intent 3 depende de 1 y 2
```

**Tools Necesarias**:
- Financial API (precio de acciones)
- Web Search (noticias)
- LLM (análisis y recomendación)

---

### Paso 2: Design Architecture

**Ejemplo**: Compound RAG System

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

**Flujo**:
```python
class CompoundRAGApp:
    def answer_question(self, query):
        # 1. Buscar documentos relevantes
        docs = self.run_search(query)
        
        # 2. Resumir cada documento (paralelizado)
        summaries = [self.run_augmented_summary(doc) for doc in docs]
        
        # 3. Combinar summaries en contexto
        context = self.run_get_context(summaries)
        
        # 4. Generar respuesta final
        answer = self.run_qa(query, context)
        
        return answer
```

---

## Multi-Stage Reasoning Chains

### Concepto

**Definición**: Sistemas que pasan por múltiples etapas de razonamiento antes de generar respuesta final.

**Analogía**: Como resolver un problema de matemáticas paso a paso en lugar de saltar a la respuesta.

### Mapeo de Conceptos

| Concepto RAG | Concepto en Framework |
|--------------|----------------------|
| Retrieval | Tool |
| Tasks | Order of tools |
| Question + Answer | Intent + Route |
| Metadata | Parameters for Route |
| Generation | Reasoning |

---

## Frameworks de Composición

### ¿Por qué usar frameworks?

**Problema**: Construir compound AI systems desde cero es complejo

**Solución**: Frameworks que abstraen complejidad

---

### 1. LangChain 🦜🔗

**Qué es**: Framework más popular para aplicaciones GenAI

**Conceptos Clave**:

#### Prompt
```python
from langchain import PromptTemplate

template = """
Eres un experto en {topic}.
Pregunta: {question}
Respuesta:
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

result = chain.run(topic="Python", question="¿Qué es una lista?")
```

#### Retriever
```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings

vectorstore = FAISS.from_documents(docs, OpenAIEmbeddings())
retriever = vectorstore.as_retriever()

docs = retriever.get_relevant_documents("mi query")
```

#### Tool
```python
from langchain.tools import Tool

def buscar_web(query):
    # Lógica de búsqueda
    return resultados

tool = Tool(
    name="BuscadorWeb",
    func=buscar_web,
    description="Busca información en la web"
)
```

---

### 2. LlamaIndex 🦙

**Qué es**: Framework enfocado en **indexing y retrieval** de datos

**Conceptos Clave**:

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
response = query_engine.query("¿Qué es Python?")
```

#### Agents
```python
from llama_index.agent import OpenAIAgent

agent = OpenAIAgent.from_tools(tools, llm=llm)
response = agent.chat("Busca información sobre NVIDIA")
```

---

### 3. Haystack 🌾

**Qué es**: Framework open-source enfocado en **document retrieval, generation, summarization**

**Conceptos Clave**:

#### Pipelines
```python
from haystack import Pipeline
from haystack.nodes import Retriever, Generator

pipeline = Pipeline()
pipeline.add_node(component=retriever, name="Retriever", inputs=["Query"])
pipeline.add_node(component=generator, name="Generator", inputs=["Retriever"])

result = pipeline.run(query="¿Qué es IA?")
```

---

### 4. DSPy 🎯

**Qué es**: Framework para **programar con LLMs** de forma más estructurada

**Enfoque**: En lugar de prompts, usa "signatures" y compilación automática

**Conceptos Clave**:

#### Signatures
```python
class QA(dspy.Signature):
    """Responde preguntas basándote en el contexto"""
    context = dspy.InputField()
    question = dspy.InputField()
    answer = dspy.OutputField()
```

#### Teleprompters
Optimizadores automáticos de prompts

```python
optimizer = BootstrapFewShot()
optimized_module = optimizer.compile(module, trainset=trainset)
```

---

### Comparación de Frameworks

| Framework | Fortaleza | Mejor Para | Curva de Aprendizaje |
|-----------|-----------|------------|---------------------|
| **LangChain** | Versatilidad | Aplicaciones generales | Media |
| **LlamaIndex** | Indexing | RAG avanzado | Media-Baja |
| **Haystack** | Pipelines | Document processing | Media |
| **DSPy** | Optimización | Sistemas complejos | Alta |

---

### Cómo Elegir un Framework

**Preguntas Clave**:

1. **Features**: ¿Tiene lo que necesito?
2. **Performance**: ¿Es suficientemente rápido?
3. **Scalability**: ¿Escala con mis datos?
4. **Stability**: ¿Es maduro y mantenido?
5. **Complexity**: ¿Puedo aprenderlo a tiempo?

**Recomendación General**:
- 🆕 Empezando: **LangChain** (más recursos, comunidad)
- 🔍 RAG avanzado: **LlamaIndex**
- 📄 Document processing: **Haystack**
- 🎓 Investigación: **DSPy**

---

## Productos Databricks para Compound Systems

### 1. Foundation Model API

**Qué es**: API unificada para acceder a LLMs

**Características**:
- ✅ **Instant Access**: Sin setup complejo
- ✅ **Pay-per-token**: Paga solo lo que usas
- ✅ **External Models**: Integra Azure OpenAI, AWS Bedrock
- ✅ **Unified Interface**: Misma API para todos los modelos

**Modelos Soportados**:

| Modelo | Tipo | Tarea | Características |
|--------|------|-------|----------------|
| **DBRX Instruct** | Databricks OSS | Chat | Mixture-of-Experts, optimizado empresas |
| **Meta LLaMA 3** | Meta OSS | Chat | 8B y 70B parámetros |
| **LLaMA 2 70B** | Meta OSS | Chat | Modelo anterior |
| **Mixtral-8x7B** | Mistral OSS | Chat | Mixture-of-Experts |
| **BGE Large** | BAAI OSS | Embeddings | Multilingüe |

**Uso**:
```python
from databricks_genai_inference import ChatSession

chat = ChatSession(
    model="databricks-dbrx-instruct",
    system_message="Eres un asistente útil"
)

response = chat.reply("¿Qué es Databricks?")
print(response.message)
```

---

### 2. Mosaic AI Vector Search

**Ya cubierto en sección anterior**, pero recordatorio rápido:

- ✅ Vector DB integrada con Lakehouse
- ✅ Unity Catalog (ACLs, governance)
- ✅ Auto-sync con Delta Tables
- ✅ REST API + Python SDK
- ✅ Scalable y low-latency

---

### 3. MLflow Tracking

**Para Compound Systems**: Trackear toda la chain/pipeline

```python
import mlflow

with mlflow.start_run(run_name="compound_rag_experiment"):
    # Loguear configuración
    mlflow.log_param("retriever_model", "bge-large")
    mlflow.log_param("llm_model", "dbrx-instruct")
    mlflow.log_param("num_docs_retrieved", 5)
    
    # Tu compound system
    result = my_compound_system.run(query)
    
    # Loguear métricas
    mlflow.log_metric("total_latency", result.latency)
    mlflow.log_metric("retrieval_time", result.retrieval_time)
    mlflow.log_metric("generation_time", result.generation_time)
    mlflow.log_metric("relevance_score", result.relevance)
    
    # Loguear la chain completa
    mlflow.langchain.log_model(my_compound_system, "model")
```

---

### 4. Model Serving

**Para Compound Systems**: Servir chains/pipelines completas

```python
# Registrar chain en Unity Catalog
mlflow.langchain.log_model(
    my_chain,
    "model",
    registered_model_name="mi_catalogo.mi_schema.mi_chain"
)

# Crear endpoint de serving
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

w.serving_endpoints.create(
    name="mi_compound_rag_endpoint",
    config={
        "served_models": [{
            "model_name": "mi_catalogo.mi_schema.mi_chain",
            "model_version": "1",
            "workload_size": "Small",
            "scale_to_zero_enabled": True
        }]
    }
)
```

---

### 5. MLflow Evaluation

**Para Compound Systems**: Evaluar end-to-end

```python
import mlflow

eval_data = pd.DataFrame({
    "query": ["¿Qué es RAG?", "¿Cómo usar Vector Search?"],
    "expected_response": ["RAG es...", "Vector Search se usa..."]
})

with mlflow.start_run():
    results = mlflow.evaluate(
        model="models:/mi_chain@champion",
        data=eval_data,
        targets="expected_response",
        model_type="question-answering",
        evaluators=["default", "toxicity", "faithfulness"]
    )
    
    print(results.metrics)
```

---

### 6. Lakehouse Monitoring

**Para Compound Systems**: Monitorear en producción

- Monitorear cada componente del pipeline
- Detectar drift en datos
- Tracking de latencia, costo, calidad
- Alertas automáticas

---

## Agents (Agentes)

### ¿Qué es un Agent?

Una aplicación que puede **decidir dinámicamente** qué acciones tomar para completar una tarea compleja.

**Diferencia Clave**:

```
❌ Chain (Secuencia fija):
Query → [Paso 1] → [Paso 2] → [Paso 3] → Response
(Siempre los mismos pasos en el mismo orden)

✅ Agent (Decisión dinámica):
Query → Agent decide → [Pasos necesarios según la query] → Response
(Pasos varían según la tarea)
```

---

### Componentes de un Agent

```
┌─────────────────────────────────────────┐
│  AGENT                                  │
├─────────────────────────────────────────┤
│  1. TASK                                │
│     • Request del usuario (prompt)      │
├─────────────────────────────────────────┤
│  2. LLM (Brain)                         │
│     • Coordina la lógica                │
│     • Decide qué hacer                  │
├─────────────────────────────────────────┤
│  3. TOOLS                               │
│     • Recursos externos                 │
│     • APIs, databases, search, etc.     │
├─────────────────────────────────────────┤
│  4. MEMORY & PLANNING                   │
│     • Recuerda acciones previas         │
│     • Planifica próximos pasos          │
└─────────────────────────────────────────┘
```

---

### Ejemplo de Agent: Stock Investment Advisor

**Task**:
```
"¿Es buen momento para invertir en acciones de NVIDIA?"
```

**Agent Workflow**:

```
┌─────────────────────────────────────────┐
│  1. PROCESS TASK                        │
│     Agent analiza la pregunta           │
│     Identifica sub-tareas:              │
│       - Precio actual                   │
│       - Finanzas recientes              │
│       - Noticias/announcements          │
│       - Sentiment score                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. COLLECT DATA                        │
│     • Tool 1: Financial API             │
│       → Precio actual: $850             │
│     • Tool 2: Company API               │
│       → Revenue Q4: +50%                │
│     • Tool 3: News Search               │
│       → "NVIDIA lanza nuevo chip AI"    │
│     • Tool 4: Sentiment API             │
│       → Sentiment: 0.85 (positivo)      │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. DATA ANALYSIS                       │
│     LLM analiza todos los datos         │
│     Identifica tendencias               │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. OUTPUT GENERATION                   │
│     LLM sintetiza reporte completo:     │
│     "Basado en el precio actual..."     │
│     "El crecimiento del 50%..."         │
│     "Sentiment positivo del mercado..."  │
│     "Recomendación: Compra moderada"    │
└─────────────────────────────────────────┘
```

**Lo importante**: El agent **decidió dinámicamente**:
- Qué tools usar
- En qué orden
- Cuántas veces llamar cada tool
- Cómo combinar la información

---

### Agent Reasoning Patterns

#### 1. ReAct (Reasoning + Acting)

**Qué es**: Patrón donde el agent genera **reasoning traces** (pasos de pensamiento) y **actions** (acciones).

**Estados del Loop**:

```
1. THOUGHT (Pensamiento)
   "Necesito saber el precio actual de NVIDIA"
   ↓
2. ACT (Acción)
   Tool: FinancialAPI(symbol="NVDA")
   ↓
3. OBSERVE (Observación)
   Result: "$850"
   ↓
4. THOUGHT (Siguiente pensamiento)
   "Ahora necesito las noticias recientes"
   ↓
5. ACT
   Tool: NewsSearch(query="NVIDIA noticias")
   ↓
6. OBSERVE
   Result: [artículos]
   ↓
7. THOUGHT
   "Tengo suficiente información para responder"
   ↓
8. FINAL ANSWER
```

**Ejemplo Código (LangChain)**:
```python
from langchain.agents import create_react_agent

agent = create_react_agent(
    llm=llm,
    tools=[financial_tool, news_tool, sentiment_tool],
    prompt=react_prompt
)

result = agent.invoke({"input": "¿Invertir en NVIDIA?"})
```

---

#### 2. Tool Use (Uso de Herramientas)

**Qué es**: Agent interactúa con **external tools/APIs** para tareas específicas.

**Tipos de Tools**:

| Tool Type | Ejemplo | Uso |
|-----------|---------|-----|
| **Research/Search** | Google, Wikipedia | Buscar información |
| **Image** | DALL-E, Stable Diffusion | Generar imágenes |
| **Document Retrieval** | Vector Search | RAG |
| **Coding** | Code interpreter | Ejecutar código |
| **APIs** | Weather API, Stock API | Datos externos |
| **Databases** | SQL query | Consultar datos |

**Ejemplo**:
```python
from langchain.tools import Tool

def search_web(query: str) -> str:
    # Lógica de búsqueda
    return results

def execute_sql(query: str) -> str:
    # Ejecutar SQL
    return results

tools = [
    Tool(name="WebSearch", func=search_web, description="Busca en web"),
    Tool(name="SQLQuery", func=execute_sql, description="Consulta DB")
]

agent = create_agent(llm=llm, tools=tools)
```

---

#### 3. Planning (Planificación)

**Qué es**: Agent descompone una tarea compleja en **sub-tareas** y las orquesta.

**Tipos de Tareas**:

```
SINGLE TASK:
Task → Execute → Done

SEQUENTIAL TASKS:
Task A → Task B → Task C → Done
(B depende de A, C depende de B)

GRAPH TASKS (Paralelo):
       ┌─ Task B ─┐
Task A ┤          ├─> Task D → Done
       └─ Task C ─┘
(B y C se ejecutan en paralelo)
```

**Ejemplo**:
```
Task: "Investiga competidores de NVIDIA, compara precios, genera reporte"

Plan del Agent:
1. [Parallel] Investigar NVIDIA → precio, productos
2. [Parallel] Investigar AMD → precio, productos
3. [Parallel] Investigar Intel → precio, productos
4. [Sequential] Comparar datos recolectados
5. [Sequential] Generar reporte final
```

---

#### 4. Multi-Agent Collaboration

**Qué es**: Varios agents trabajando juntos, cada uno especializado.

**Ejemplo**:

```
┌─────────────────────────────────────────┐
│  COORDINATOR AGENT                      │
│  (Orquesta el flujo)                    │
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

**Flujo**:
1. Coordinator assign task a Research Agent
2. Research Agent busca información
3. Data Analysis Agent analiza los datos
4. Writing Agent genera reporte
5. Review Agent revisa y da feedback
6. Si hay correcciones, vuelve a Writing Agent
7. Coordinator entrega resultado final

**Ventajas**:
- ✅ Especialización (cada agent es experto en su área)
- ✅ Escalabilidad (añadir más agents)
- ✅ Flexibilidad (diferentes LLMs para diferentes agents)

---

### Herramientas para Construir Agents

| Herramienta | Tipo | Descripción |
|-------------|------|-------------|
| **LangChain Agents** | Framework | Agents con tools |
| **AutoGPT** | Framework | Agents autónomos |
| **OpenAI Function Calling** | API | LLM llama funciones definidas |
| **AutoGen** | Framework | Multi-agent collaboration |
| **Transformers Agents** | Library | Agents con Hugging Face models |

---

### Ejemplo Código: Agent con LangChain

```python
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI

# Definir tools
def search_web(query):
    # Implementación
    return "Resultados de búsqueda..."

def get_stock_price(symbol):
    # Implementación
    return "$850"

tools = [
    Tool(
        name="WebSearch",
        func=search_web,
        description="Busca información en la web"
    ),
    Tool(
        name="StockPrice",
        func=get_stock_price,
        description="Obtiene precio de una acción. Input: símbolo ticker"
    )
]

# Crear agent
llm = OpenAI(temperature=0)
agent = initialize_agent(
    tools,
    llm,
    agent="zero-shot-react-description",  # ReAct pattern
    verbose=True
)

# Usar agent
response = agent.run("¿Cuál es el precio actual de NVIDIA y qué dicen las noticias?")
```

**Output ejemplo**:
```
> Entering new AgentExecutor chain...
Thought: Necesito obtener el precio y buscar noticias
Action: StockPrice
Action Input: NVDA
Observation: $850
Thought: Ahora busco noticias
Action: WebSearch
Action Input: NVIDIA noticias recientes
Observation: [artículos sobre nuevo chip]
Thought: Tengo toda la información necesaria
Final Answer: El precio actual de NVIDIA es $850. Las noticias recientes 
hablan sobre el lanzamiento de un nuevo chip de IA...
```

---

## Multi-Modal AI

### ¿Qué es Multi-Modal AI?

**Definición**: Aplicaciones de IA con **inputs o outputs** que incluyen tipos de datos **más allá de solo texto**.

**Tipos de Datos Comunes**:
- 📝 Text
- 🖼️ Image
- 🎵 Audio
- 🎬 Video

---

### Tipos de Aplicaciones Multi-Modal

#### 1. Multi-Modal Input

**Ejemplo**: Image → Text (Captioning)

```
Input: [Imagen de un gato]
Model: GPT-4 Vision
Output: "Un gato naranja durmiendo en un sofá"
```

**Uso**:
```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "¿Qué hay en esta imagen?"},
            {"type": "image_url", "image_url": {"url": "https://..."}}
        ]
    }]
)
```

#### 2. Multi-Modal Output

**Ejemplo**: Text → Image (Generation)

```
Input: "Un perro astronauta en el espacio"
Model: DALL-E 3
Output: [Imagen generada]
```

#### 3. Multi-Modal Input + Output

**Ejemplo**: Image + Text → Text + Image (Editing)

```
Input: [Imagen] + "Añade un sombrero al gato"
Model: DALL-E Edit
Output: [Imagen editada] + "He añadido un sombrero rojo"
```

---

### Arquitecturas Multi-Modal

#### Multi-Modal Retrieval (RAG con Imágenes)

```
Query: "Muéstrame logos similares a este"
[Imagen de logo]
↓
Embedding Model (CLIP)
↓
Vector Search (busca imágenes similares)
↓
[Top 5 logos similares]
```

**Modelos para Embeddings Multi-Modal**:
- **CLIP** (OpenAI): Text + Image embeddings en mismo espacio
- **BLIP** (Salesforce): Imagen → texto, texto → imagen
- **ImageBind** (Meta): 6 modalidades (text, image, audio, depth, thermal, IMU)

#### Multi-Modal Generator

**Ejemplo**: Generar historia con imágenes

```
Input: "Crea una historia corta sobre un viaje espacial"
↓
LLM Multi-Modal (GPT-4V)
↓
Output:
  Texto: "Un astronauta llamado Alex viajó a Marte..."
  Imagen 1: [Astronauta en nave]
  Texto: "Al llegar, descubrió..."
  Imagen 2: [Paisaje marciano]
```

---

### Modelos Multi-Modal Populares

| Modelo | Tipo | Capacidades |
|--------|------|-------------|
| **GPT-4 Vision** | Propietario | Text → Text, Image+Text → Text |
| **Claude 3** | Propietario | Similar a GPT-4V |
| **Gemini** | Propietario | Text, Image, Audio, Video |
| **LLaVA** | Open Source | Image + Text → Text |
| **Fuyu-8B** | Open Source | Multi-modal understanding |

---

### Caso de Uso: Document Understanding

**Problema**: PDF complejo con tablas, diagramas, múltiples columnas

**Solución Multi-Modal**:

```
PDF Page → Image
↓
GPT-4 Vision
↓
Prompt: "Extrae el contenido de esta página en markdown,
        incluyendo tablas y descripciones de diagramas"
↓
Output:
  ## Título
  
  Texto del párrafo...
  
  | Col1 | Col2 | Col3 |
  |------|------|------|
  | A    | B    | C    |
  
  **Diagrama**: Flujo de proceso mostrando...
```

**Ventajas sobre OCR tradicional**:
- ✅ Entiende layout complejo
- ✅ Interpreta diagramas
- ✅ Mantiene contexto
- ✅ No solo extrae texto, lo comprende

---

## 🎯 Preguntas de Práctica

### Pregunta 1
**¿Qué diferencia un Compound AI System de un Simple AI System?**

A) Compound usa más datos  
B) Compound tiene múltiples componentes interactuando ✅  
C) Compound es más rápido  
D) No hay diferencia

**Respuesta**: B - Compound = múltiples componentes/pasos

---

### Pregunta 2
**¿Qué patrón de agent usa Thought → Act → Observe?**

A) Planning  
B) Tool Use  
C) ReAct ✅  
D) Multi-Agent

**Respuesta**: C - ReAct = Reasoning + Acting con loop observacional

---

### Pregunta 3
**¿Qué framework es mejor para RAG avanzado?**

A) LangChain  
B) LlamaIndex ✅  
C) Haystack  
D) DSPy

**Respuesta**: B - LlamaIndex se especializa en indexing/retrieval

---

### Pregunta 4
**¿Qué modelo usarías para embeddings de texto + imagen en mismo espacio?**

A) GPT-4  
B) BERT  
C) CLIP ✅  
D) LLaMA

**Respuesta**: C - CLIP es multi-modal (text + image)

---

### Pregunta 5
**En multi-agent collaboration, ¿quién orquesta el flujo?**

A) Research Agent  
B) Coordinator Agent ✅  
C) Review Agent  
D) No hay coordinador

**Respuesta**: B - Coordinator organiza y asigna tareas

---

## 📝 Resumen Ejecutivo

### Lo que DEBES saber:

✅ **Compound AI System** = múltiples componentes interactuando (vs simple = 1 paso)  
✅ **Intent Classification**: Identificar intents, dependencies, tools needed  
✅ **Frameworks**: LangChain (general), LlamaIndex (RAG), Haystack (docs), DSPy (optimization)  
✅ **Databricks Products**: Foundation Model API, Vector Search, MLflow, Model Serving  
✅ **Agent** = decide dinámicamente qué hacer (vs chain = secuencia fija)  
✅ **Agent Components**: Task, LLM (brain), Tools, Memory/Planning  
✅ **ReAct Pattern**: Thought → Act → Observe (loop)  
✅ **Multi-Agent**: Varios agents especializados colaborando  
✅ **Multi-Modal**: Input/output más allá de texto (imagen, audio, video)  
✅ **CLIP**: Embeddings multi-modal (text + image)

---

## 🔗 Próximo Tema

➡️ **Continúa con**: `04_Despliegue_Monitoreo.md` (Deployment, MLflow, Model Serving, Monitoring)

