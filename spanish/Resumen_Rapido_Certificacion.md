# 🚀 Resumen Rápido - Certificación Databricks GenAI Engineer Associate

## 📌 Uso: Consulta Rápida Antes del Examen

---

## 1️⃣ FUNDAMENTOS (25% del examen)

### Conceptos Clave
- **IA Generativa** = Crear contenido nuevo (texto, imagen, código, audio, video)
- **LLMs** = Modelos grandes entrenados con billones de tokens
- **Pre-entrenamiento** = Conocimiento general | **Fine-tuning** = Especialización

### LLMs Importantes
| Modelo | Tipo | Nota |
|--------|------|------|
| **DBRX** | Databricks OSS | Mixture-of-Experts |
| **GPT-4** | OpenAI Propietario | Mejor calidad general |
| **LLaMA 3** | Meta OSS | 8B, 70B params |
| **Dolly** | Databricks | Primer OSS empresarial |

### Stack Databricks
```
Unity Catalog → Gobernanza centralizada, ACLs, Lineage, Auditoría
Delta Lake → ACID Transactions, Time Travel, Auto-Optimization
Vector Search → Búsqueda semántica, Auto-sync con Delta Tables
MLflow → Tracking, Registry, Evaluation, AI Gateway
Model Serving → Producción (APIs), Auto-scaling, Inference Tables
Mosaic AI → Foundation Models, Agent Framework, AI Playground
Delta Live Tables → ETL con Quality Expectations
Feature Serving → Features en tiempo real
```

### Unity Catalog (Gobernanza)
- **ACLs por Nivel**: Data (SELECT, INSERT), AI/ML (EXECUTE, MODIFY), Infra (CREATE, MANAGE)
- **Lineage Automático**: Rastrea origen y uso de datos/modelos
- **Auditoría**: Registra todos los accesos (quién, cuándo, qué)
- **Compliance**: GDPR, HIPAA, SOC2 automático

### Delta Lake (Storage)
- **ACID**: Atomicity, Consistency, Isolation, Durability
- **Time Travel**: `VERSION AS OF` o `TIMESTAMP AS OF`
- **Auto Optimize**: Compacta archivos, Z-Ordering, Data Skipping
- **Vacuum**: Limpia versiones antiguas
- **Para GenAI**: Guarda embeddings, inference logs, chunks

### Riesgos
- **Hallucination**: Inventar datos falsos
- **Bias**: Sesgos en datos/respuestas
- **Prompt Injection**: Manipular LLM
- **Privacy**: Exponer datos sensibles

---

## 2️⃣ DESARROLLO DE SOLUCIONES (30% del examen)

### Prompt Engineering

**Técnicas**:
- **Zero-shot**: Sin ejemplos
- **Few-shot**: Con ejemplos
- **Chain-of-Thought**: Pensar paso a paso
- **Prompt Chaining**: Encadenar prompts

**Componentes de Prompt**:
1. Instrucciones
2. Contexto
3. Input/Pregunta
4. Formato output

### RAG (Retrieval-Augmented Generation)

**Flujo**: Query → Retrieve docs → Augment prompt → Generate

**Ventajas**: Conocimiento actualizado, reduce hallucinations

**Arquitectura**:
```
Documents → Delta Live Tables → Chunking → Embeddings → 
Vector Search → Retrieval → LLM → Response
```

### Chunking

| Tipo | Descripción | Pros/Contras |
|------|-------------|--------------|
| **Fixed-size** | Tamaño fijo | Simple pero puede cortar contexto |
| **Context-aware** | Por sección/párrafo | Mejor contexto, más complejo |
| **Overlap** | Superposición | Mantiene info entre chunks |

**Tip**: Usar overlap de 10-20% entre chunks

### Embeddings

- **Qué son**: Representación vectorial de texto
- **Tip Crítico**: Usar el MISMO modelo para docs y queries
- **Modelos**: BGE-Large, E5, OpenAI Ada, Cohere Embed

### Vector Search (Databricks)

**Setup**:
1. Crear Vector Search Endpoint
2. Crear Model Serving Endpoint (embeddings)
3. Crear Vector Search Index (desde Delta Table)

**Auto-sync con Delta Tables** ✅

### Reranking

**Qué hace**: Reordena top-K resultados para mejorar precisión

**Cuándo usar**: Cuando precisión es crítica

**Modelos**: bge-reranker, Cohere Rerank, FlashRank

### Métricas RAG

| Métrica | Qué Mide |
|---------|----------|
| **Context Precision** | ¿Docs retrieved son relevantes? |
| **Context Recall** | ¿Recuperamos todos los docs relevantes? |
| **Faithfulness** | ¿Respuesta fiel al contexto? |
| **Answer Relevancy** | ¿Responde la pregunta? |
| **Answer Correctness** | ¿Es factualmente correcta? |

---

## 3️⃣ DESARROLLO DE APLICACIONES (20% del examen)

### Compound AI Systems

**Definición**: Sistema con múltiples componentes interactuando

**vs Simple System**: Simple = 1 paso | Compound = múltiples pasos

**Ejemplo**: Query → Classifier → Tool 1 → Tool 2 → Aggregator → Response

### Frameworks

| Framework | Mejor Para |
|-----------|------------|
| **LangChain** | Aplicaciones generales |
| **LlamaIndex** | RAG avanzado |
| **Haystack** | Document processing |
| **DSPy** | Optimización automática |

### Agents

**Qué son**: Sistemas que deciden dinámicamente qué hacer

**Componentes**:
1. **Task** (tarea)
2. **LLM** (cerebro)
3. **Tools** (herramientas externas)
4. **Memory & Planning**

**ReAct Pattern**:
```
Thought → Act → Observe → Thought → Act → ...
```

**Multi-Agent**: Varios agents especializados colaborando

### Multi-Modal AI

**Tipos**:
- Text → Image (DALL-E)
- Image → Text (GPT-4 Vision)
- Text + Image → Text (Claude Vision)

**Modelo Clave**: **CLIP** = embeddings text + image en mismo espacio

---

## 4️⃣ DESPLIEGUE Y MONITOREO (15% del examen)

### Métodos de Deployment

| Método | Latencia | Mejor Para |
|--------|----------|------------|
| **Batch** | Horas/días | Reportes, ETL |
| **Streaming** | Segundos | Eventos real-time |
| **Real-Time** | < 2 seg | Chatbots, APIs |
| **Edge** | Milisegundos | IoT, offline |

### MLflow

**Tracking**: Loguear experimentos, parámetros, métricas

**Registry (Unity Catalog)**:
- Versioning automático
- Aliases: `@champion`, `@challenger`, `@staging`
- Lineage completo
- ACLs

**Pyfunc**: Interfaz genérica para deployment

### Databricks Model Serving

**Soporta**:
1. Custom Models (MLflow)
2. Foundation Models (DBRX, LLaMA)
3. External Models (OpenAI, Anthropic)

**Features**:
- Auto-scaling
- A/B testing (traffic splitting)
- Inference Tables (auto-logging)
- Scale-to-zero

### Monitoring

**Databricks Lakehouse Monitoring**:
- Profile metrics (estadísticas)
- Drift metrics (cambios en datos)
- Custom metrics

**Qué monitorear**:
- Input data
- Vector DB data
- Human feedback
- Prompts/responses
- Latency, cost, quality

### MLOps vs LLMOps

**MLOps** = DataOps + DevOps + ModelOps

**LLMOps adiciona**:
- Text templates (prompts)
- Chains completas
- Vector databases
- Human feedback loop
- Cost management (pay-per-token)

---

## 5️⃣ EVALUACIÓN Y GOBERNANZA (10% del examen)

### Issues Críticos

| Issue | Mitigación |
|-------|------------|
| **Prompt Injection** | Guardrails, Llama Guard |
| **Hallucinations** | RAG, fact-checking |
| **Bias** | Auditar datos, fairness metrics |
| **Privacy** | Unity Catalog ACLs, data anonymization |

### Guardrails

**Tipos**:
1. **Input**: Prevenir inputs maliciosos
2. **Output**: Filtrar respuestas problemáticas
3. **Contextual**: Guiar comportamiento LLM

**Llama Guard**: Clasificador de contenido unsafe (Meta)

### Métricas LLM

**Base**:
- **Loss**: Error de predicción
- **Perplexity**: Sorpresa del modelo (menor = mejor)
- **Toxicity**: Contenido dañino (menor = mejor)

**Task-Specific**:
- **BLEU**: Traducción
- **ROUGE**: Summarization

**Benchmarks**:
- MMLU (conocimiento general)
- HellaSwag (common sense)
- HumanEval (código)

### LLM-as-a-Judge

**Cuándo usar**: No hay métricas automáticas o ground truth

**Cómo**: Usar un LLM (GPT-4) para evaluar respuestas de otro LLM

### Evaluación End-to-End

**Métricas**:
- **Costo**: Cost per request, GPU hours
- **Performance**: Latency (P50, P95, P99), throughput
- **Quality**: CSAT, accuracy, task completion rate
- **Custom**: Deflection rate, time saved

### Offline vs Online

**Offline**:
- Dataset curado
- Reproducible
- Pre-deployment

**Online**:
- Usuarios reales
- A/B testing
- Production monitoring

**Estrategia**: Offline → Canary (10%) → Full production

---

## 🎯 FÓRMULAS Y NÚMEROS CLAVE

### Context Window
- GPT-3.5: 4K tokens
- GPT-4: 8K, 32K, 128K
- Claude 3: 200K tokens

### Embeddings Dimensions
- BGE-Large: 1024
- OpenAI Ada: 1536
- E5: 768

### Latency Targets
- Chatbot: < 2 segundos
- Streaming: < 5 segundos
- Batch: Horas/días OK

### A/B Testing
- Empezar con 5-10% tráfico nuevo modelo
- Incrementar gradualmente si métricas buenas

---

## 📋 COMANDOS Y CÓDIGO ESENCIALES

### Unity Catalog (ACLs)
```sql
-- Dar permisos
GRANT SELECT ON TABLE catalog.schema.table TO `group_name`;
GRANT EXECUTE ON MODEL catalog.schema.model TO `developers`;

-- Revocar permisos
REVOKE INSERT ON TABLE catalog.schema.sales FROM `interns`;

-- Ver permisos
SHOW GRANTS ON TABLE catalog.schema.customer_data;
```

### Delta Lake
```sql
-- Time Travel
SELECT * FROM catalog.schema.sales VERSION AS OF 100;
SELECT * FROM catalog.schema.sales TIMESTAMP AS OF '2024-01-15';

-- Optimización
OPTIMIZE catalog.schema.sales ZORDER BY (date, region);

-- Auto Optimize (activar)
ALTER TABLE catalog.schema.data SET TBLPROPERTIES (
  'delta.autoOptimize.optimizeWrite' = 'true',
  'delta.autoOptimize.autoCompact' = 'true'
);

-- Vacuum (limpiar versiones antiguas)
VACUUM catalog.schema.table RETAIN 168 HOURS;

-- Ver historial
DESCRIBE HISTORY catalog.schema.table;
```

### MLflow Registry
```python
# Registrar modelo
mlflow.langchain.log_model(
    model,
    "model",
    registered_model_name="catalog.schema.model_name"
)

# Cargar por alias
model = mlflow.langchain.load_model(
    "models:/catalog.schema.model_name@champion"
)
```

### Vector Search
```python
# Crear índice
vsc.create_delta_sync_index(
    endpoint_name="endpoint",
    index_name="catalog.schema.index",
    source_table_name="catalog.schema.docs",
    primary_key="id",
    embedding_source_column="text",
    embedding_model_endpoint_name="embed_endpoint"
)

# Buscar
results = index.similarity_search(
    query_text="mi query",
    num_results=5
)
```

### Model Serving
```python
# Crear endpoint
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

### Lakehouse Monitoring
```python
from databricks import lakehouse_monitoring as lm

lm.create_monitor(
    table_name="catalog.schema.inference_table",
    profile_type=lm.InferenceLog(...),
    schedule=lm.MonitorCronSchedule(...)
)
```

---

## 🚨 ERRORES COMUNES A EVITAR

❌ Usar modelos diferentes para embeddings de docs y queries  
❌ No usar overlap en chunking  
❌ Ignorar gobernanza (Unity Catalog)  
❌ No monitorear en producción  
❌ No tener guardrails  
❌ Confiar solo en offline evaluation  
❌ No versionar modelos  
❌ No usar inference tables  
❌ Ignorar costos (pay-per-token)  
❌ No recolectar human feedback  

---

## ✅ CHECKLIST ANTES DEL EXAMEN

- [ ] Entiendo la diferencia entre pre-training y fine-tuning
- [ ] Sé qué es RAG y cómo funciona
- [ ] Conozco las técnicas de prompt engineering
- [ ] Entiendo chunking strategies
- [ ] Sé qué son embeddings y por qué el mismo modelo es crucial
- [ ] Conozco Databricks Vector Search
- [ ] Entiendo compound AI systems vs simple systems
- [ ] Sé qué es un agent y el patrón ReAct
- [ ] Conozco los frameworks (LangChain, LlamaIndex)
- [ ] Entiendo los métodos de deployment (batch, streaming, real-time)
- [ ] Conozco MLflow (Tracking, Registry, Evaluation)
- [ ] Sé qué es Model Serving y sus features
- [ ] Entiendo inference tables
- [ ] Conozco Lakehouse Monitoring
- [ ] Sé la diferencia entre MLOps y LLMOps
- [ ] Entiendo guardrails y Llama Guard
- [ ] Conozco las métricas (BLEU, ROUGE, Perplexity)
- [ ] Entiendo LLM-as-a-judge
- [ ] Sé la diferencia entre offline y online evaluation
- [ ] Conozco Unity Catalog y su rol en gobernanza
- [ ] He practicado con los 12 exámenes de SkillCertPro

---

## 💡 TIPS PARA EL EXAMEN

1. **Lee cuidadosamente**: Muchas preguntas tienen trampa en los detalles
2. **Elimina opciones obviamente incorrectas** primero
3. **Keywords importantes**: "NUNCA", "SIEMPRE", "MEJOR", "ÚNICO"
4. **Databricks-specific**: Si hay opción Databricks vs genérica, probablemente es Databricks
5. **Unity Catalog es clave**: Aparece en muchas preguntas de gobernanza
6. **MLflow está en todo**: Tracking, Registry, Evaluation, Deployment
7. **Tiempo**: 90 min para 60 preguntas = 1.5 min/pregunta. Maneja tu tiempo
8. **No te atasques**: Marca y continúa, vuelve después
9. **Revisa al final**: Si tienes tiempo, revisa respuestas marcadas

---

## 📊 DISTRIBUCIÓN DEL EXAMEN

- 15 preguntas: Fundamentos (25%)
- 18 preguntas: Desarrollo de Soluciones (30%)
- 12 preguntas: Desarrollo de Aplicaciones (20%)
- 9 preguntas: Despliegue y Monitoreo (15%)
- 6 preguntas: Evaluación y Gobernanza (10%)

**Total**: 60 preguntas | **Tiempo**: 90 minutos | **Aprobación**: 70%

---

## 🎓 ¡Buena Suerte!

**Último consejo**: Confía en tu preparación. Si has estudiado las guías completas y hecho los exámenes de práctica, estás listo. 🚀

---

**Creado para certificación**: Databricks Certified Generative AI Engineer Associate

