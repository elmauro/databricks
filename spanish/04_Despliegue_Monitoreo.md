# 4️⃣ Despliegue y Monitoreo de Aplicaciones GenAI

## 📚 Tabla de Contenidos
1. [Ciclo de Vida de Sistemas GenAI](#ciclo-de-vida-de-sistemas-genai)
2. [Empaquetado de Modelos](#empaquetado-de-modelos-genai)
3. [MLflow para Deployment](#mlflow-para-deployment)
4. [Métodos de Despliegue](#métodos-de-despliegue)
5. [Databricks Model Serving](#databricks-model-serving)
6. [Monitoreo de Sistemas AI](#monitoreo-de-sistemas-ai)
7. [MLOps y LLMOps](#mlops-y-llmops)

---

## Ciclo de Vida de Sistemas GenAI

### Fase 1: System Development (Desarrollo)

**Características**: Trabaja con **datos estáticos**

```
┌────────────────────────────────────────┐
│  1. DEFINIR PROBLEMA                   │
│     ¿Qué debe resolver el sistema?     │
│                                        │
│     Ejemplo:                           │
│     "Chatbot que responde preguntas    │
│      sobre productos"                  │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  2. ESTABLECER MÉTRICAS                │
│     ¿Cómo medir el éxito?              │
│                                        │
│     Ejemplo:                           │
│     • Precisión > 85%                  │
│     • Latencia < 2 segundos            │
│     • Satisfacción usuario > 4/5       │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  3. RECOPILAR DATOS                    │
│     Obtener datos relevantes           │
│                                        │
│     Ejemplo:                           │
│     • FAQs                             │
│     • Emails de soporte                │
│     • Documentación de productos       │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  4. PROCESAR DATOS                     │
│     Limpiar y estructurar              │
│                                        │
│     • Eliminar duplicados              │
│     • Normalizar texto                 │
│     • Convertir formatos               │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  5. CONSTRUIR SISTEMA                  │
│     Implementar solución               │
│                                        │
│     • RAG (búsqueda + generación)      │
│     • Chains (flujos estructurados)    │
│     • Agents (si necesario)            │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  6. EVALUAR                            │
│     Probar contra métricas             │
│                                        │
│     • Pruebas manuales                 │
│     • Métricas automatizadas           │
│     • Feedback de usuarios piloto      │
└────────────────────────────────────────┘
```

---

### Fase 2: Deployment & Production (Producción)

**Características**: Datos **nuevos y cambiantes** en tiempo real

```
┌────────────────────────────────────────┐
│  1. DEPLOYMENT                         │
│     Llevar a producción                │
│                                        │
│     • API REST                         │
│     • Chatbot web                      │
│     • Aplicación móvil                 │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  2. MONITOREO                          │
│     Vigilar comportamiento             │
│                                        │
│     • Detectar degradación             │
│     • Identificar errores              │
│     • Medir drift de datos             │
└─────────────┬──────────────────────────┘
              ↓
┌────────────────────────────────────────┐
│  3. ITERACIÓN (si hay problemas)       │
│                                        │
│     ┌──> Recolectar nuevos datos      │
│     ├──> Reentrenar sistema           │
│     ├──> Reprocesar datos             │
│     └──> Re-deployment                │
│                                        │
└────────────────────────────────────────┘
```

### Analogía Práctica

**Piénsalo como desarrollar una app móvil**:

**Fase 1 (Development)**: 
- Programas la app en tu computadora
- Pruebas con datos de prueba
- Arreglas bugs
- Funciona perfecto en tu entorno

**Fase 2 (Production)**:
- Publicas en App Store
- Miles de usuarios la descargan
- Descubres nuevos bugs en escenarios reales
- Usuarios usan la app de formas inesperadas
- Tienes que actualizar regularmente

---

## Empaquetado de Modelos GenAI

### Formas de Empaquetar Lógica GenAI

Con LLMs, la "lógica ML" se empaqueta de nuevas formas:

#### 1. Engineered Prompt (Prompt Diseñado)

**Qué es**: Prompt cuidadosamente diseñado que se guarda como plantilla

**Ejemplo**:
```python
PROMPT_TEMPLATE = """
Eres un asistente médico experto.

Contexto del paciente:
{patient_history}

Pregunta:
{question}

Instrucciones:
- Usa terminología médica precisa
- Cita fuentes cuando sea posible
- Si no estás seguro, di "Consulta con un médico"

Respuesta:
"""
```

**Cómo se empaqueta**:
- Guardar como archivo `.txt` o `.yaml`
- Versionar en Git
- Cargar dinámicamente en runtime

---

#### 2. Chain (Secuencia Estructurada)

**Qué es**: Secuencia de pasos desde frameworks como LangChain o LlamaIndex

**Ejemplo**:
```python
from langchain.chains import RetrievalQA
from langchain.vectorstores import FAISS
from langchain.llms import OpenAI

# Definir chain
chain = RetrievalQA.from_chain_type(
    llm=OpenAI(),
    chain_type="stuff",
    retriever=vectorstore.as_retriever()
)

# Usar
answer = chain.run("¿Qué es RAG?")
```

**Cómo se empaqueta**:
- Serializar con `mlflow.langchain.log_model(chain)`
- Incluye todas las dependencias
- Reproducible en cualquier entorno

---

#### 3. API Call (Llamada Ligera a API)

**Qué es**: Llamada simple a un LLM vía API

**Tipos**:

##### a) API Externa Propietaria
```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Hola"}]
)
```

**Ventajas**:
- ✅ Fácil de usar
- ✅ Sin infraestructura

**Desventajas**:
- ❌ Dependencia externa
- ❌ Costo por uso
- ❌ Datos salen de tu infraestructura

##### b) API Interna (Self-Hosted)
```python
import requests

response = requests.post(
    "https://mi-databricks.cloud/serving-endpoints/mi-llm",
    json={"prompt": "Hola"}
)
```

**Ventajas**:
- ✅ Control total
- ✅ Datos privados
- ✅ Customizable (fine-tuned)

**Tipos de modelos internos**:
- **Fine-tuned**: Ajustado a tu dominio
- **Pre-trained**: Sin modificar

---

#### 4. Invocación Local con GPU

**Qué es**: Ejecutar modelo localmente (no vía API)

**Ejemplo**:
```python
from transformers import pipeline

generator = pipeline(
    "text-generation",
    model="meta-llama/Llama-2-7b-hf",
    device=0  # GPU 0
)

output = generator("Hola, ¿cómo estás?")
```

**Cuándo usar**:
- Edge deployments (dispositivos IoT)
- Sin conexión a internet
- Latencia ultra-baja requerida

**Desventajas**:
- ❌ Requiere GPU/hardware potente
- ❌ Mantenimiento de infraestructura

---

### Resumen de Opciones

| Método | Complejidad | Control | Costo | Cuándo Usar |
|--------|-------------|---------|-------|-------------|
| **Prompt** | Baja | Bajo | Variable | Prototipo rápido |
| **Chain** | Media | Medio | Variable | Apps estructuradas |
| **API Externa** | Baja | Bajo | Alto | MVP, pruebas |
| **API Interna** | Alta | Alto | Medio | Producción empresarial |
| **Local GPU** | Alta | Total | Bajo (si ya tienes HW) | Edge, offline |

---

## MLflow para Deployment

### ¿Qué es MLflow?

**Recordatorio**: Plataforma open-source para gestionar **ciclo de vida completo** de ML/GenAI

### MLflow Model (Formato Estándar)

**Estructura**:
```
mi_modelo/
├── MLmodel              # Metadata (flavors, signature)
├── conda.yaml           # Dependencias (conda)
├── requirements.txt     # Dependencias (pip)
├── python_env.yaml      # Versión Python
├── model/               # Archivos del modelo
│   ├── model.pkl
│   └── tokenizer/
└── input_example.json   # Ejemplo de input
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

**Qué es**: **Interfaz genérica** para cualquier modelo Python

**Por qué es importante**:
- Cualquier modelo MLflow puede cargarse como Python function
- Interfaz unificada para deployment
- Compatible con Model Serving

**Funciones Clave**:
```python
import mlflow

# Guardar modelo
mlflow.pyfunc.log_model(
    artifact_path="model",
    python_model=mi_modelo,
    signature=signature,
    input_example=input_example
)

# Cargar modelo
loaded_model = mlflow.pyfunc.load_model("models:/mi_modelo/1")

# Predecir
result = loaded_model.predict({"query": "¿Qué es RAG?"})
```

---

### MLflow Registry en Unity Catalog

**Características**:

```
┌─────────────────────────────────────────┐
│  UNITY CATALOG MODEL REGISTRY           │
├─────────────────────────────────────────┤
│  ✅ Versioning automático               │
│     • Version 1, 2, 3, ...              │
│                                         │
│  ✅ Aliases                             │
│     • @champion (mejor modelo)          │
│     • @challenger (candidato)           │
│     • @staging                          │
│                                         │
│  ✅ Lifecycle Management                │
│     • Development → Staging → Prod      │
│                                         │
│  ✅ Collaboration & ACLs                │
│     • Quién puede leer/escribir         │
│     • Unity Catalog governance          │
│                                         │
│  ✅ Full Lineage                        │
│     • Qué datos usó                     │
│     • Qué código                        │
│     • Qué parámetros                    │
│                                         │
│  ✅ Tagging & Annotations               │
│     • Metadata custom                   │
└─────────────────────────────────────────┘
```

**Ejemplo de Uso**:
```python
import mlflow

# Registrar en Unity Catalog
mlflow.set_registry_uri("databricks-uc")

with mlflow.start_run():
    # Entrenar/construir modelo
    model = build_my_rag_chain()
    
    # Loguear
    mlflow.langchain.log_model(
        model,
        "model",
        registered_model_name="mi_catalogo.mi_schema.chatbot_v1"
    )

# Establecer alias
from mlflow import MlflowClient
client = MlflowClient()

client.set_registered_model_alias(
    "mi_catalogo.mi_schema.chatbot_v1",
    "champion",
    version=3
)

# Cargar modelo por alias
champion_model = mlflow.langchain.load_model(
    "models:/mi_catalogo.mi_schema.chatbot_v1@champion"
)
```

---

### MLflow y Ciclo de Desarrollo

```
┌─────────────────────────────────────────┐
│  1. MODEL/CHAIN BUILDING                │
│     • Construir RAG/Chain/Agent         │
│     • Experimentar con prompts          │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. MLFLOW TRACKING                     │
│     • Registrar experimentos            │
│     • Comparar métricas                 │
│     • Seleccionar mejor versión         │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. MLFLOW EVALUATION                   │
│     • Evaluar con datasets              │
│     • Métricas automatizadas            │
│     • LLM-as-a-judge                    │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. MLFLOW REGISTRY                     │
│     • Registrar modelo                  │
│     • Versionar                         │
│     • Asignar alias (@champion)         │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  5. MODEL DEPLOYMENT                    │
│     • Databricks Model Serving          │
│     • REST API                          │
│     • Monitoreo                         │
└─────────────────────────────────────────┘
```

---

## Métodos de Despliegue

### 1. Batch Inference (Por Lotes)

**Qué es**: Procesar **muchos datos a la vez** en un schedule

**Cuándo usar**:
- Reportes diarios/semanales
- ETL jobs
- No se necesita respuesta inmediata

**Ejemplo**:
```
Tarea: "Resumir todos los reportes financieros del trimestre"

Ejecución:
- Todos los domingos a las 2 AM
- Procesa 1000 documentos
- Genera resúmenes
- Los guarda en Delta Table

Usuarios:
- Lunes por la mañana ven resultados
```

#### ✅ Ventajas
- **Más barato**: Hardware se usa eficientemente
- **Alto volumen**: Procesa millones de registros
- **Eficiente**: Paraleliza bien

#### ❌ Limitaciones
- **Alta latencia**: Esperas horas/días
- **Datos stale**: No es tiempo real
- **No para streaming**: Datos deben ser estáticos

#### Implementación con Spark
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Cargar datos
df = spark.table("mi_catalogo.mi_schema.documentos")

# UDF para procesar con LLM
def summarize_with_llm(text):
    # Llamar a LLM
    return summary

from pyspark.sql.functions import udf
summarize_udf = udf(summarize_with_llm)

# Aplicar a todos
df_summarized = df.withColumn("summary", summarize_udf("text"))

# Guardar
df_summarized.write.saveAsTable("mi_catalogo.mi_schema.summarized")
```

---

### 2. Streaming Inference (Flujo Continuo)

**Qué es**: Procesar **datos conforme llegan** (stream)

**Cuándo usar**:
- Procesamiento de eventos en tiempo real
- Análisis de logs
- IoT data

**Ejemplo**:
```
Aplicación: "Personalizar mensajes de marketing en tiempo real"

Flujo:
Usuario hace clic → Evento capturado → 
Stream processor → LLM personaliza mensaje → 
Mensaje enviado (< 5 segundos)
```

#### Structured Streaming (Spark)
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Leer stream de Kafka
stream_df = spark.readStream \
    .format("kafka") \
    .option("subscribe", "user_events") \
    .load()

# Procesar con LLM
processed_df = stream_df.withColumn(
    "personalized_message",
    personalize_udf("event_data")
)

# Escribir resultados
query = processed_df.writeStream \
    .format("delta") \
    .outputMode("append") \
    .option("checkpointLocation", "/checkpoints") \
    .start("/output")
```

---

### 3. Real-Time Inference (Tiempo Real)

**Qué es**: Generar predicciones **instantáneamente** cuando se solicitan

**Cuándo usar**:
- Chatbots
- APIs interactivas
- Aplicaciones con usuarios esperando

**Ejemplo**:
```
Usuario: "¿Cuál es el estado de mi orden?"
↓ (< 2 segundos)
API → Model Serving → LLM → Response
↓
Chatbot: "Tu orden #12345 está en camino, llegará mañana"
```

#### ✅ Ventajas
- **Baja latencia**: Respuestas en segundos
- **Interactivo**: UX fluida
- **Datos frescos**: Usa info actualizada

#### ❌ Desafíos
- **Infraestructura compleja**: Auto-scaling, load balancing
- **Costo**: Servidores 24/7
- **Requiere expertise**: DevOps, SRE

#### Databricks Model Serving
```python
# Ya registrado en MLflow Registry
# Crear endpoint

from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

w.serving_endpoints.create(
    name="chatbot-endpoint",
    config={
        "served_models": [{
            "model_name": "mi_catalogo.mi_schema.chatbot",
            "model_version": "3",
            "workload_size": "Small",  # Small, Medium, Large
            "scale_to_zero_enabled": True  # Ahorra costos
        }]
    }
)

# Llamar API
import requests

response = requests.post(
    "https://mi-workspace.cloud.databricks.com/serving-endpoints/chatbot-endpoint/invocations",
    headers={"Authorization": f"Bearer {token}"},
    json={"inputs": {"query": "¿Estado de mi orden?"}}
)
```

---

### 4. Embedded/Edge Inference

**Qué es**: Ejecutar modelo **en el dispositivo** (sin cloud)

**Cuándo usar**:
- IoT devices
- Aplicaciones móviles offline
- Latencia ultra-baja (milisegundos)

**Ejemplo**:
```
Auto autónomo:
Cámara → Procesamiento local (GPU en auto) → Decisión → Acción
(No puede depender de internet)
```

**Ejemplo**: Modificar temperatura del aire acondicionado con voz

---

### Comparación de Métodos

| Método | Latencia | Costo | Complejidad | Mejor Para |
|--------|----------|-------|-------------|------------|
| **Batch** | Alta (horas) | Bajo | Baja | Reportes, ETL |
| **Streaming** | Media (segundos) | Medio | Media | Eventos, IoT |
| **Real-Time** | Baja (< 2s) | Alto | Alta | Chatbots, APIs |
| **Edge** | Muy baja (ms) | Variable | Alta | Offline, IoT crítico |

---

## 📦 APPLICATIONS en Databricks

Estos tres componentes representan la **etapa de producción y operación** de tus modelos:

- **MLflow AI Gateway**: Cómo se exponen vía API
- **Model Serving**: Cómo se sirven eficientemente  
- **Lakehouse Monitoring**: Cómo se supervisan en tiempo real

---

### 🧩 1. MLflow AI Gateway

La **puerta de entrada unificada** (API Gateway) para acceder a modelos de IA —tanto internos como externos— de forma segura y estandarizada.

**¿Qué hace?**:
- ✅ Proporciona una **interfaz única** para interactuar con cualquier modelo LLM:
  - **Interno** (registrado en MLflow o Mosaic AI)
  - **Externo** (OpenAI, Anthropic, Hugging Face, Azure AI, AWS Bedrock, etc.)
- ✅ Controla **costos, autenticación y trazabilidad** de las llamadas
- ✅ Permite a los equipos usar LLMs **sin exponer claves API individuales**

**Problema que resuelve**:
```
❌ Sin AI Gateway:
Equipo A → usa OpenAI directamente (API key hardcodeada)
Equipo B → usa Anthropic (otra key)
Equipo C → usa modelo interno (config diferente)
→ Caos de credenciales
→ No hay tracking centralizado
→ Costos incontrolables

✅ Con MLflow AI Gateway:
Todos los equipos → AI Gateway → Enruta a modelo apropiado
→ Keys centralizadas y seguras
→ Logging automático
→ Control de costos
```

**Arquitectura**:
```
┌──────────────────────────────────────────┐
│  APLICACIÓN / USUARIO                    │
└─────────────┬────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│  MLFLOW AI GATEWAY                       │
│  • Autenticación                         │
│  • Rate limiting                         │
│  • Cost tracking                         │
│  • Logging                               │
└─────────────┬────────────────────────────┘
              ↓
      ┌───────┴───────┬─────────────┐
      ↓               ↓             ↓
┌──────────┐  ┌──────────┐  ┌──────────┐
│ OpenAI   │  │ Anthropic│  │ DBRX     │
│ GPT-4    │  │ Claude   │  │ Internal │
└──────────┘  └──────────┘  └──────────┘
```

**Ejemplo de Uso**:
```python
from mlflow.deployments import get_deploy_client

# Cliente del gateway
client = get_deploy_client("databricks")

# Llamar a GPT-4 (sin exponer API key)
response = client.predict(
    endpoint="chat-gpt-4",
    inputs={
        "messages": [
            {"role": "user", "content": "Explica RAG"}
        ]
    }
)

# Llamar a Claude (misma interfaz)
response_claude = client.predict(
    endpoint="chat-claude",
    inputs={
        "messages": [
            {"role": "user", "content": "Explica RAG"}
        ]
    }
)

# Todas las llamadas se loguean automáticamente
```

**Ventajas**:
- 🔐 **Seguridad**: Keys centralizadas
- 💰 **Control de costos**: Tracking unificado
- 📊 **Observabilidad**: Logging automático
- 🔄 **Flexibilidad**: Cambiar de proveedor sin cambiar código

---

### ⚙️ 2. Databricks Model Serving

Servicio que **despliega modelos** de Machine Learning o LLMs como **endpoints REST en producción**, optimizados para baja latencia y alta disponibilidad.

**Tipos de Modelos Soportados**:

```
┌─────────────────────────────────────────┐
│  DATABRICKS MODEL SERVING               │
├─────────────────────────────────────────┤
│                                         │
│  1. CUSTOM MODELS (MLflow)              │
│     • Tu modelo registrado en UC        │
│     • Chains, RAGs, Agents custom       │
│     • Python, PyTorch, TensorFlow       │
│                                         │
│  2. FOUNDATION MODELS                   │
│     • DBRX Instruct                     │
│     • LLaMA 3 (8B, 70B)                 │
│     • Mixtral 8x7B                      │
│     • BGE-Large (embeddings)            │
│                                         │
│  3. EXTERNAL MODELS                     │
│     • OpenAI (GPT-4, GPT-3.5)           │
│     • Anthropic (Claude)                │
│     • Cohere                            │
│     • AWS Bedrock models                │
│                                         │
└─────────────────────────────────────────┘
```

**Características Clave**:

| Función | Descripción | Ventaja |
|---------|-------------|---------|
| **Tiempo Real** | Respuestas instantáneas | Chatbots, APIs interactivas |
| **Batch Inference** | Procesamiento masivo | Reportes periódicos |
| **Integración MLflow** | Deploy directo desde Registry | Sin configuración manual |
| **Security UC** | Control de acceso via Unity Catalog | Gobernanza empresarial |
| **Inference Tables** | Guarda cada predicción | Auditoría y debugging |
| **Auto-scaling** | Escala según demanda | Optimiza costos |
| **Scale-to-zero** | Apaga cuando no se usa | Ahorra costos |

---

### Inference Tables (Tablas de Inferencia)

**Qué son**: Cada request-response se guarda automáticamente en una **Delta Table**

**Para qué sirve**:
- ✅ Monitoreo posterior
- ✅ Debugging
- ✅ Análisis de uso
- ✅ Re-entrenamiento (feedback loop)

**Estructura**:
```
Inference Table:
├── request_id
├── timestamp
├── input (query del usuario)
├── output (respuesta del LLM)
├── latency
├── token_count
└── metadata
```

**Habilitarlo**:
```python
w.serving_endpoints.create(
    name="mi-endpoint",
    config={
        "served_models": [{ ... }],
        "traffic_config": { ... },
        "auto_capture_config": {
            "catalog_name": "mi_catalogo",
            "schema_name": "mi_schema",
            "table_name_prefix": "inference_"
        }
    }
)
```

**Resultado**: Se crea automáticamente:
```
mi_catalogo.mi_schema.inference_mi-endpoint
```

---

### A/B Testing y Canary Deployments

#### Traffic Splitting

**Ejemplo**: Probar nuevo modelo con 10% de tráfico

```python
w.serving_endpoints.update_config(
    name="mi-endpoint",
    config={
        "served_models": [
            {
                "model_name": "mi_catalogo.mi_schema.chatbot",
                "model_version": "3",  # Versión vieja
                "workload_size": "Small"
            },
            {
                "model_name": "mi_catalogo.mi_schema.chatbot",
                "model_version": "4",  # Versión nueva
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

**Flujo**:
```
100 requests
├─> 90 requests → Modelo v3
└─> 10 requests → Modelo v4

Analizar métricas de v4:
- Si mejor: aumentar % gradualmente
- Si peor: volver a 100% v3
```

---

### Integración con Unity Catalog

```
┌─────────────────────────────────────────┐
│  UNITY CATALOG                          │
├─────────────────────────────────────────┤
│                                         │
│  • UC Volume                            │
│    └─> Archivos (PDFs, CSVs, etc.)     │
│                                         │
│  • Raw/Processed Text Tables            │
│    └─> Documentos procesados            │
│                                         │
│  • Embeddings/Index                     │
│    └─> Vector Search index              │
│                                         │
│  • Model/Chain                          │
│    └─> Modelo registrado                │
│                                         │
│  • Inference Table                      │
│    └─> Logs de requests                 │
│                                         │
│  • Processed Payloads Table             │
│    └─> Datos procesados                 │
│                                         │
│  • Metric Tables                        │
│    └─> Lakehouse Monitoring             │
│                                         │
└─────────────────────────────────────────┘
```

**Todo en Unity Catalog** = Gobernanza centralizada

---

## Monitoreo de Sistemas AI

### ¿Por qué Monitorear?

**Objetivo**: Diagnosticar problemas **antes** de que sean severos o costosos

**Sin monitoreo**:
```
Usuario: "El chatbot da respuestas raras"
→ ¿Cuándo empezó?
→ ¿Cuántos usuarios afectados?
→ ¿Qué inputs causan el problema?
→ ❓ No sabemos
```

**Con monitoreo**:
```
Alerta automática: "Aumento del 30% en respuestas de baja calidad desde las 2 PM"
→ Sabemos exactamente cuándo y cuánto
→ Vemos los inputs problemáticos
→ Podemos actuar rápido
```

---

### ¿Qué Monitorear?

#### 1. Input Data
- Distribución de queries
- Longitud de inputs
- Idiomas usados
- Detección de inputs maliciosos (prompt injection)

#### 2. Data en Vector Databases
- Calidad de documentos
- Coverage de temas
- Actualidad de información

#### 3. Human Feedback
- 👍👎 reactions
- Comentarios de usuarios
- Ratings (1-5 estrellas)

#### 4. Prompts/Queries y Responses
- Guardar para análisis
- ⚠️ **Legalidad**: ¿Puedes almacenar datos de usuarios?
- Privacy compliance (GDPR, CCPA)

---

### AI Assets a Monitorear

#### 1. Mid-Training Checkpoints
- Si estás fine-tuning, checkpoints periódicos
- Análisis de loss curves

#### 2. Component Evaluation Metrics
- Retrieval quality
- Embedding quality
- Reranking effectiveness

#### 3. System Evaluation Metrics
- End-to-end latency
- Token usage (costo)
- Answer quality

#### 4. Performance & Cost
- Requests per second (RPS)
- P50, P95, P99 latency
- GPU utilization
- Cost per request

---

### 📊 3. Lakehouse Monitoring

Solución **totalmente gestionada** para monitorear el comportamiento, rendimiento y calidad de **modelos en producción** (ML y GenAI).

**¿Qué monitorea?**:
- ✅ **Datos de entrada y salida** → Detectar data drift o degradación de calidad
- ✅ **Rendimiento del modelo** → Latencia, costos, errores
- ✅ **Métricas personalizadas** → Precisión, fidelidad, relevancia
- ✅ **Tendencias a lo largo del tiempo** → Dashboards automáticos en Databricks SQL

**Arquitectura Interna**:
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
│  • Analiza periódicamente                │
│  • Genera métricas automáticas           │
│  • Detecta drift, anomalías              │
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
│  • Visualizaciones automáticas           │
│  • Alertas configurables                 │
│  • Histórico de métricas                 │
└──────────────────────────────────────────┘
```

**Características Clave**:

```
┌─────────────────────────────────────────┐
│  LAKEHOUSE MONITORING                   │
├─────────────────────────────────────────┤
│  ✅ Fully Managed                       │
│     • Zero ops overhead                 │
│     • No infraestructura que mantener   │
│                                         │
│  ✅ Frictionless                        │
│     • Setup en minutos                  │
│     • Configuración simple              │
│                                         │
│  ✅ Unified                             │
│     • Datos + Modelos en un lugar       │
│     • Todo en Unity Catalog             │
│                                         │
│  ✅ Built on Unity Catalog              │
│     • ACLs integradas                   │
│     • Governance automático             │
│                                         │
│  📊 Auto-generates DBSQL Dashboard      │
│     • Visualizaciones listas            │
│     • Gráficos interactivos             │
│                                         │
└─────────────────────────────────────────┘
```

#### Tipos de Métricas

**1. Profile Metrics**:
- Estadísticas de datos (min, max, mean, std)
- Distribuciones
- Nulls, missings

**2. Drift Metrics**:
- ¿Los datos cambiaron vs baseline?
- Distributional drift
- Statistical tests (KS, χ²)

**3. Custom Metrics**:
- Defines las tuyas
- Ejemplo: "% de respuestas > 500 palabras"

---

#### Setup de Lakehouse Monitoring

```python
from databricks import lakehouse_monitoring as lm

# Monitorear inference table
lm.create_monitor(
    table_name="mi_catalogo.mi_schema.inference_chatbot",
    profile_type=lm.InferenceLog(
        timestamp_col="timestamp",
        model_id_col="model_version",
        prediction_col="output",
        problem_type="text-generation",
        label_col=None  # No tenemos ground truth en tiempo real
    ),
    output_schema_name="mi_catalogo.mi_schema",
    schedule=lm.MonitorCronSchedule(
        quartz_cron_expression="0 0 * * * ?"  # Cada hora
    )
)
```

**Resultado automático**:
1. **Profile Table**: Estadísticas por ventana de tiempo
2. **Drift Table**: Métricas de drift
3. **Dashboard**: Visualización en Databricks SQL

---

### Workflow de Monitoreo

```
┌─────────────────────────────────────────┐
│  DESARROLLO                             │
│  • Crear monitoring tables para todos   │
│    los componentes                      │
│  • Definir métricas clave               │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  TESTING                                │
│  • Validar que monitoring funciona      │
│  • Ajustar thresholds de alertas       │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  PRODUCCIÓN                             │
│  • Refresh regular de tablas            │
│  • Alertas configuradas                 │
│  • Dashboards monitoreados              │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  ACCIÓN                                 │
│  • Si hay problemas:                    │
│    - Investigar causa raíz              │
│    - Recolectar nuevos datos            │
│    - Reentrenar/ajustar                 │
│    - Re-deployment                      │
└─────────────────────────────────────────┘
```

**Tips**:
- ✅ **Modelar costo vs performance**: ¿Vale la pena modelo más caro?
- ✅ **Refresh regular**: Hourly, daily (según criticidad)
- ✅ **Alertas clave**: No todas las métricas, solo las críticas
- ✅ **Retrain triggers**: Automatizar si drift > threshold

---

## MLOps y LLMOps

### ¿Qué es MLOps?

**Definición**: Conjunto de **procesos y automatización** para gestionar datos, código y modelos, mejorando performance, estabilidad y eficiencia de sistemas ML.

**Fórmula**:
```
MLOps = DataOps + DevOps + ModelOps
```

**Componentes**:
- **DataOps**: Gestión de datos (calidad, pipelines, versioning)
- **DevOps**: CI/CD, infrastructure as code, deployment
- **ModelOps**: Gestión de modelos (training, evaluation, deployment, monitoring)

---

### ¿Por qué importa MLOps?

| Beneficio | Descripción |
|-----------|-------------|
| **Calidad de datos** | Datos limpios y confiables |
| **Procesos optimizados** | Menos manual work, más automatización |
| **Monitoreo de costo/performance** | Optimizar ROI |
| **Time to value** | Más rápido a producción |
| **Menos oversight manual** | Automatizar checks |

---

### Multi-Environment Semantics

**Entornos estándar**:
```
1. DEVELOPMENT (Dev)
   • Experimentación
   • Breaking changes permitidos
   • Datos sintéticos/muestreados

2. STAGING (Stage)
   • Pre-producción
   • Testing riguroso
   • Datos similares a prod

3. PRODUCTION (Prod)
   • Usuarios reales
   • Alta disponibilidad
   • Monitoreo 24/7
```

---

### Separación de Entornos

#### Opción 1: Direct Separation

**Estrategia**: **Workspaces de Databricks completamente separados**

```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  Dev Workspace   │  │ Stage Workspace  │  │  Prod Workspace  │
│                  │  │                  │  │                  │
│  • Experimentos  │  │  • Pre-prod      │  │  • Usuarios      │
│  • Datos sample  │  │  • Testing       │  │  • Alta disp.    │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

**Pros**:
- ✅ Aislamiento completo
- ✅ No hay riesgo de afectar prod
- ✅ Escala a múltiples proyectos

**Contras**:
- ❌ Más infraestructura
- ❌ Más complejidad de permisos

---

#### Opción 2: Indirect Separation

**Estrategia**: **Un solo workspace con separación lógica** (catálogos, schemas, prefijos)

```
┌─────────────────────────────────────────┐
│  Databricks Workspace                   │
├─────────────────────────────────────────┤
│  Catálogo: dev_catalog                  │
│    └─> Schema: chatbot                  │
│                                         │
│  Catálogo: staging_catalog              │
│    └─> Schema: chatbot                  │
│                                         │
│  Catálogo: prod_catalog                 │
│    └─> Schema: chatbot                  │
└─────────────────────────────────────────┘
```

**Pros**:
- ✅ Menos infraestructura
- ✅ Permisos más simples

**Contras**:
- ❌ Riesgo de mezclar entornos
- ❌ No escala bien a múltiples proyectos

---

### Arquitectura Recomendada (MLOps)

**Deploy-Code Architecture**:

```
┌────────────────────────────────────────────┐
│  SOURCE CONTROL (Git)                      │
│  • Código                                  │
│  • Configuraciones                         │
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

### LLMOps: Diferencias con MLOps Tradicional

| Aspecto | MLOps Tradicional | LLMOps |
|---------|-------------------|--------|
| **Dev Patterns** | Código + datos | **+ Text templates (prompts)** |
| **Packaging** | Modelo serializado | **Aplicaciones completas (chains)** |
| **Serving** | Model endpoint | **+ Vector DB, GPU infra, UI** |
| **API Governance** | Menos crítico | **Crítico** (acceso a endpoints) |
| **Cost** | Fijo (infra) | **Variable** (uso, API calls) |
| **Human Feedback** | Opcional | **Esencial** (mejorar prompts) |

---

### LLMOps: Aspectos Específicos

#### 1. Incremental Development
- Iterar prompts
- Ajustar chains sin reentrenar

#### 2. Text Templates
- Versionar prompts como código
- A/B testing de prompts

#### 3. Entire Applications
- No solo modelo, toda la app (retriever + LLM + UI)

#### 4. Additional Components
- Vector databases
- Embedding services
- Rerankers

#### 5. GPU Infrastructure
- Requiere GPUs potentes
- Optimización de batch size, quantization

#### 6. Cost Management
- API-based models: pay-per-token
- Técnicas para reducir costo:
  - Usar modelos más pequeños cuando sea posible
  - Caching de respuestas comunes
  - Prompt compression

#### 7. Human Feedback Loop
- Recolectar feedback (thumbs up/down)
- Usar para mejorar prompts
- Fine-tuning con feedback

---

### Arquitectura LLMOps Recomendada

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
│  • Model Serving (auto-scaling)            │
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

## 🎯 Preguntas de Práctica

### Pregunta 1
**¿Qué método de deployment es mejor para un chatbot interactivo?**

A) Batch  
B) Streaming  
C) Real-Time ✅  
D) Edge

**Respuesta**: C - Chatbots requieren respuesta inmediata (< 2 segundos)

---

### Pregunta 2
**¿Qué guardan las Inference Tables automáticamente?**

A) Solo los inputs  
B) Solo los outputs  
C) Requests y responses completos ✅  
D) Solo métricas agregadas

**Respuesta**: C - Cada request-response se loguea para monitoreo

---

### Pregunta 3
**En A/B testing, ¿qué porcentaje darías a un modelo nuevo inicialmente?**

A) 50%  
B) 90%  
C) 10-20% ✅  
D) 100%

**Respuesta**: C - Empiezas con poco tráfico para minimizar riesgo

---

### Pregunta 4
**¿Qué componente de MLflow gestiona versiones y aliases de modelos?**

A) MLflow Tracking  
B) MLflow Registry ✅  
C) MLflow Evaluation  
D) MLflow Projects

**Respuesta**: B - Registry = versioning, aliases, lifecycle

---

### Pregunta 5
**¿Qué diferencia LLMOps de MLOps tradicional?**

A) LLMOps usa Python  
B) LLMOps incluye text templates y human feedback ✅  
C) LLMOps es más simple  
D) No hay diferencia

**Respuesta**: B - LLMOps añade prompts, chains, feedback loop

---

## 📝 Resumen Ejecutivo

### Lo que DEBES saber:

✅ **Ciclo de Vida**: Development (datos estáticos) → Production (datos nuevos)  
✅ **Empaquetado**: Prompt, Chain, API call (externa/interna), Local GPU  
✅ **MLflow pyfunc**: Interfaz genérica para deployment  
✅ **Unity Catalog Registry**: Versioning, aliases (@champion), lineage, ACLs  
✅ **Deployment Methods**:
   - Batch = alto volumen, baja urgencia
   - Streaming = eventos en tiempo real
   - Real-Time = chatbots, APIs (< 2s)
   - Edge = offline, IoT

✅ **Model Serving**: Custom, Foundation, External models  
✅ **Inference Tables**: Auto-logging de requests para monitoreo  
✅ **A/B Testing**: Traffic splitting para probar versiones  
✅ **Lakehouse Monitoring**: Profile, Drift, Custom metrics  
✅ **MLOps** = DataOps + DevOps + ModelOps  
✅ **LLMOps** = MLOps + prompts + chains + human feedback + cost management

---

## 🔗 Próximo Tema

➡️ **Continúa con**: `05_Evaluacion_Gobernanza.md` (Evaluación, Seguridad, Guardrails, Métricas)

