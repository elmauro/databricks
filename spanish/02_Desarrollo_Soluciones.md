# 2️⃣ Desarrollo de Soluciones de IA Generativa

## 📚 Tabla de Contenidos
1. [Prompt Engineering](#prompt-engineering)
2. [¿Qué es RAG?](#qué-es-rag)
3. [Preparación de Datos para RAG](#preparación-de-datos-para-rag)
4. [Chunking de Documentos](#chunking-de-documentos)
5. [Embeddings](#embeddings)
6. [Vector Search](#vector-search)
7. [Reranking](#reranking)
8. [MLflow para RAG](#mlflow-para-rag)
9. [Evaluación de RAG](#evaluación-de-rag)

---

## Prompt Engineering

### ¿Qué es un Prompt?

Un **prompt** es la instrucción o pregunta que le das a un LLM.

**Ejemplo**:
```
Prompt: "Resume este artículo en 3 puntos"
LLM: [genera el resumen]
```

### ¿Qué es Prompt Engineering?

**Definición**: El arte y práctica de **diseñar y refinar prompts** para obtener mejores resultados del LLM.

Es como aprender a hacer las preguntas correctas para obtener las respuestas que necesitas.

---

### Componentes de un Buen Prompt

```
┌─────────────────────────────────────┐
│   1. INSTRUCCIONES                  │
│   "Actúa como experto en Python"   │
├─────────────────────────────────────┤
│   2. CONTEXTO                       │
│   "Para una aplicación web"        │
├─────────────────────────────────────┤
│   3. INPUT/PREGUNTA                 │
│   "¿Cómo optimizo esta función?"   │
├─────────────────────────────────────┤
│   4. FORMATO DE OUTPUT              │
│   "Responde en formato JSON"       │
└─────────────────────────────────────┘
```

### Ejemplo Completo

❌ **Prompt Malo**:
```
"Dame código"
```

✅ **Prompt Bueno**:
```
INSTRUCCIONES: Eres un experto en Python para ciencia de datos.

CONTEXTO: Necesito analizar un dataset de ventas con pandas.

INPUT: Tengo un CSV con columnas: fecha, producto, cantidad, precio.
Quiero calcular ventas totales por mes.

FORMATO: Dame código Python con comentarios explicativos.
```

---

### Técnicas de Prompt Engineering

#### 1. Zero-Shot Prompting

**Definición**: Pedir al LLM que haga algo **sin darle ejemplos**.

**Cuándo usar**: Tareas simples y directas

**Ejemplo**:
```
Prompt: "Traduce 'Hello world' al español"
LLM: "Hola mundo"
```

#### 2. Few-Shot Prompting

**Definición**: Dar **algunos ejemplos** para guiar al LLM.

**Cuándo usar**: Tareas con formato específico o lógica compleja

**Ejemplo**:
```
Prompt:
Clasifica el sentimiento: positivo, negativo, neutral

Ejemplo 1:
Texto: "Me encantó este producto"
Sentimiento: positivo

Ejemplo 2:
Texto: "Terrible experiencia"
Sentimiento: negativo

Ahora clasifica:
Texto: "El servicio es aceptable"
Sentimiento: ?

LLM: "neutral"
```

#### 3. Chain-of-Thought (CoT) Prompting

**Definición**: Pedir al LLM que **piense paso a paso** como un humano.

**Cuándo usar**: Problemas que requieren razonamiento

**Ejemplo**:
```
Prompt:
Resuelve paso a paso:
Si una pizza cuesta $15 y tengo un descuento del 20%, ¿cuánto pago?

Piensa paso a paso:

LLM:
Paso 1: Calcular el 20% de $15
20% de $15 = $15 × 0.20 = $3

Paso 2: Restar el descuento del precio original
$15 - $3 = $12

Respuesta: Pagas $12
```

#### 4. Prompt Chaining

**Definición**: **Encadenar múltiples prompts** donde el output de uno es input del siguiente.

**Cuándo usar**: Tareas complejas que se dividen en pasos

**Ejemplo**:
```
Prompt 1: "Resume este artículo de 1000 palabras"
Output 1: [resumen de 200 palabras]

Prompt 2: "Traduce este resumen al francés: [Output 1]"
Output 2: [resumen en francés]

Prompt 3: "Extrae los 3 puntos clave de: [Output 2]"
Output 3: [3 puntos en francés]
```

**Diagrama**:
```
Artículo → [Resume] → Resumen → [Traduce] → Traducción → [Extrae] → Puntos clave
```

---

### Tips y Trucos de Prompt Engineering

#### 1. Usar Delimitadores

**Por qué**: Separar claramente instrucciones de datos

**Delimitadores comunes**: `###`, ` ``` `, `{}`, `[]`, `---`

**Ejemplo**:
```
Analiza el siguiente texto:
---
[Tu texto aquí]
---

Devuelve el análisis en este formato:
```json
{
  "tema": "...",
  "sentimiento": "..."
}
```
```

#### 2. Especificar Formato de Output

**Ejemplos**:
- "Responde en formato JSON"
- "Genera una tabla markdown"
- "Usa bullets points"
- "Dame código Python con comentarios"

#### 3. Dar un Ejemplo Correcto

```
Prompt:
Necesito que formatees direcciones así:

Ejemplo correcto:
Input: "calle 123 ciudad xyz"
Output: "Calle: 123, Ciudad: XYZ"

Ahora formatea: "avenida 456 pueblo abc"
```

#### 4. Guiar para Mejores Respuestas

✅ **Pide que NO alucine**:
```
"Si no sabes la respuesta, di 'No tengo información suficiente'.
No inventes datos."
```

✅ **Pide que NO asuma información sensible**:
```
"No asumas información personal del usuario.
Si necesitas datos, pregunta explícitamente."
```

✅ **Pide que piense más (CoT)**:
```
"No te apresures a una solución.
Piensa paso a paso y muestra tu razonamiento."
```

---

### Beneficios y Limitaciones

#### ✅ Beneficios
- **Simple y Eficiente**: No requiere reentrenamiento
- **Resultados Predecibles**: Con buenos prompts, outputs consistentes
- **Salidas Personalizadas**: Control sobre formato y estilo

#### ❌ Limitaciones
- **Depende del modelo**: Un prompt que funciona en GPT-4 puede fallar en LLaMA
- **Limitado por conocimiento del modelo**: Si el modelo no sabe algo, prompt engineering no ayuda
- **Necesitas RAG para conocimiento externo**: Datos actualizados o privados requieren RAG

---

## ¿Qué es RAG?

### Definición Simple

**RAG** = **Retrieval-Augmented Generation**

Es una técnica que **combina búsqueda de información + generación de respuestas**.

### ¿Cómo funciona?

```
┌──────────────────────────────────────────────┐
│  1. Usuario pregunta algo                    │
│     "¿Cuál es la política de vacaciones?"   │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  2. RETRIEVAL (Búsqueda)                     │
│     Busca en documentos relevantes           │
│     Encuentra: manual_empleados.pdf          │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  3. AUGMENTATION (Aumentar el prompt)        │
│     Prompt: "Basado en este contexto:        │
│     [contenido del manual]                   │
│     Responde: ¿política de vacaciones?"      │
└─────────────┬────────────────────────────────┘
              ↓
┌──────────────────────────────────────────────┐
│  4. GENERATION (Generar respuesta)           │
│     LLM lee el contexto y genera respuesta   │
│     "Según el manual, tienes 15 días..."     │
└──────────────────────────────────────────────┘
```

### ¿Por qué es importante RAG?

#### Problema sin RAG:
```
Pregunta: "¿Cuál es nuestro nuevo producto de marzo 2024?"
LLM: "No tengo información actualizada posterior a 2023" ❌
```

#### Solución con RAG:
```
Pregunta: "¿Cuál es nuestro nuevo producto de marzo 2024?"

RAG:
1. Busca en documentos → encuentra "Lanzamiento_Marzo2024.pdf"
2. Extrae contexto → "Producto X lanzado el 15 de marzo"
3. LLM genera → "El 15 de marzo 2024 lanzamos el Producto X..." ✅
```

### Casos de Uso de RAG

| Caso de Uso | Descripción | Ejemplo |
|-------------|-------------|---------|
| **Q&A Chatbot** | Responde preguntas sobre documentos | Chatbot de soporte técnico |
| **Search Augmentation** | Mejora resultados de búsqueda | Google + resumen AI |
| **Content Creation** | Genera contenido basado en fuentes | Resumen de múltiples papers |
| **Summarization** | Resume documentos largos | Executive summary de reportes |

### Arquitectura RAG en Databricks

```
┌─────────────────────────────────────────────────────┐
│  FASE 1: DOCUMENT EMBEDDING (Preparación)          │
├─────────────────────────────────────────────────────┤
│                                                     │
│  External Sources (PDFs, Docs, etc.)               │
│         ↓                                           │
│  Delta Live Tables (Ingestion batch/streaming)     │
│         ↓                                           │
│  Files & Metadata (Delta Tables)                   │
│         ↓                                           │
│  Document Processing (Chunking, cleaning)          │
│         ↓                                           │
│  Mosaic AI Model Serving (Embeddings)              │
│         ↓                                           │
│  Mosaic AI Vector Search (Storage)                 │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  FASE 2: QUERY & RETRIEVAL (Uso)                   │
├─────────────────────────────────────────────────────┤
│                                                     │
│  User Query                                         │
│         ↓                                           │
│  Mosaic AI Model Serving (Embed query)             │
│         ↓                                           │
│  Mosaic AI Vector Search (Find similar docs)       │
│         ↓                                           │
│  Retrieved Context                                  │
│         ↓                                           │
│  Prompt Augmentation (Add context to prompt)       │
│         ↓                                           │
│  Mosaic AI Model Serving (LLM Generation)          │
│         ↓                                           │
│  Response                                           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## Preparación de Datos para RAG

### Problemas Potenciales

| Problema | Descripción | Impacto |
|----------|-------------|---------|
| **Poor Quality Output** | Datos sucios/incorrectos | Respuestas incorrectas |
| **Lost in the Middle** | LLM ignora docs del medio | Información importante perdida |
| **Inefficient Retrieval** | Datos mal preparados | Búsqueda lenta/inexacta |
| **Exposing Data** | Mala gobernanza | Violaciones de seguridad |
| **Wrong Embedding Model** | Modelo inadecuado | Baja calidad de búsqueda |

### Proceso de Preparación de Datos

```
1. INGESTION & PRE-PROCESSING
   • Cargar documentos (PDF, Word, HTML, etc.)
   • Limpiar y normalizar
   • Extraer texto

2. DATA STORAGE & GOVERNANCE
   • Guardar en Delta Lake
   • Configurar Unity Catalog (permisos)

3. CHUNKING
   • Dividir documentos en chunks
   • Fixed-size o Context-aware

4. EMBEDDING
   • Convertir chunks a vectores
   • Usar modelo de embeddings apropiado

5. VECTOR STORE
   • Guardar en Mosaic AI Vector Search
   • Indexar para búsqueda rápida
```

---

## Chunking de Documentos

### ¿Qué es Chunking?

**Definición**: Dividir documentos largos en **pedazos (chunks)** más pequeños.

**¿Por qué?**:
- LLMs tienen límite de tokens (ej. 8k, 32k, 128k)
- Chunks más pequeños = búsqueda más precisa
- Mejor retrieval de información relevante

### Visualización del Problema

```
❌ Sin Chunking:
[Documento completo de 10,000 palabras]
↓
LLM (límite: 4,000 tokens)
↓
ERROR: Excede límite

✅ Con Chunking:
[Chunk 1: 500 palabras]
[Chunk 2: 500 palabras]
[Chunk 3: 500 palabras]
...
↓
Solo envías los chunks relevantes al LLM
↓
✅ Funciona perfectamente
```

---

### Estrategias de Chunking

#### 1. Fixed-Size Chunking (Tamaño Fijo)

**Cómo funciona**: Divide cada X tokens/caracteres

**Ejemplo**:
```
Documento: "Este es un documento largo sobre Python. Python es un lenguaje 
            de programación. Se usa mucho en ciencia de datos..."

Chunk 1 (50 chars): "Este es un documento largo sobre Python. Python"
Chunk 2 (50 chars): " es un lenguaje de programación. Se usa mucho "
Chunk 3 (50 chars): "en ciencia de datos..."
```

**Pros**:
- ✅ Simple
- ✅ Rápido
- ✅ Barato computacionalmente

**Contras**:
- ❌ Puede cortar en medio de una oración
- ❌ Pierde contexto
- ❌ No respeta estructura del documento

#### 2. Context-Aware Chunking (Consciente del Contexto)

**Cómo funciona**: Divide respetando estructura lógica

**Opciones**:
- Por oración
- Por párrafo
- Por sección (H1, H2, H3 en HTML/Markdown)
- Por puntuación especial (`.`, `\n`)

**Ejemplo**:
```
Documento markdown:
# Sección 1: Introducción
Este es el párrafo de introducción.

## Subsección 1.1
Contenido de la subsección.

# Sección 2: Desarrollo
...

Chunks generados:
Chunk 1: "# Sección 1: Introducción\nEste es el párrafo de introducción."
Chunk 2: "## Subsección 1.1\nContenido de la subsección."
Chunk 3: "# Sección 2: Desarrollo\n..."
```

**Pros**:
- ✅ Mantiene contexto
- ✅ Respeta estructura
- ✅ Más semánticamente coherente

**Contras**:
- ❌ Más complejo
- ❌ Chunks de tamaño variable

#### 3. Chunking con Overlap (Superposición)

**Problema**: Información importante en la frontera entre chunks

**Solución**: Overlap (superposición) entre chunks consecutivos

**Visualización**:
```
Sin Overlap:
[Chunk 1: AAAA] [Chunk 2: BBBB] [Chunk 3: CCCC]

Con Overlap:
[Chunk 1: AAAA][AA]
            [AA][Chunk 2: BBBB][BB]
                            [BB][Chunk 3: CCCC]
                            
Overlap de 2 caracteres entre chunks
```

**Ejemplo Práctico**:
```
Texto original: "Python es fácil. Además, Python es poderoso."

Sin overlap:
Chunk 1: "Python es fácil."
Chunk 2: "Además, Python es poderoso."
→ Se pierde la conexión entre chunks

Con overlap (5 palabras):
Chunk 1: "Python es fácil. Además,"
Chunk 2: "Además, Python es poderoso."
→ Mantiene contexto
```

#### 4. Windowed Summarization (Resumen con Ventana)

**Cómo funciona**: Cada chunk incluye un resumen de chunks anteriores

**Ejemplo**:
```
Chunk 1: 
"Python es un lenguaje de programación."

Chunk 2:
[Resumen anterior: "Introducción a Python"]
"Python se usa en ciencia de datos..."

Chunk 3:
[Resumen anterior: "Python en ciencia de datos"]
"Las librerías principales son NumPy, Pandas..."
```

---

### Herramientas de Chunking

- **ChunkViz**: Herramienta visual para probar estrategias
- **LangChain**: Text splitters (RecursiveCharacterTextSplitter)
- **LlamaIndex**: NodeParser
- **Unstructured**: Auto-chunking inteligente

---

### Desafíos con Documentos Complejos

#### Tipos de Contenido Complejo

```
┌─────────────────────────────────────┐
│  Documento PDF Complejo             │
├─────────────────────────────────────┤
│  • Texto mezclado con imágenes      │
│  • Tablas con datos                 │
│  • Diagramas e infográficos         │
│  • Múltiples columnas               │
│  • Texto con colores (jerarquía)    │
│  • Headers y footers                │
│  • Watermarks                       │
└─────────────────────────────────────┘
```

#### Soluciones

**1. Librerías Tradicionales**:
- **PyMuPDF**: Extracción básica de texto
- **PyPDF**: Similar, simple
- **pdfplumber**: Mejor con tablas

**Limitación**: No entienden layout complejo

**2. Layout Models** (Modelos de Diseño):
- **Donut**: OCR-free document understanding
- **doctr**: Document text recognition
- **Unstructured.io**: Multi-format parsing
- **Hugging Face Layout Models**: LayoutLM, LayoutLMv3

**Ventaja**: Entienden estructura visual

**3. Multi-Modal Models** (Modelos Multi-Modales):
- **GPT-4 Vision**: Procesa imagen + texto
- **Claude Vision**: Similar
- **Open Source**: LLaVA, Fuyu

**Ventaja**: Procesn imagen completa, extraen texto e interpretan diagramas

---

## Embeddings

### ¿Qué es un Embedding?

Una **representación numérica** (vector) de contenido textual.

**Ejemplo Visual**:
```
Texto: "perro"
Embedding: [0.2, 0.8, 0.1, 0.5, ...] (vector de 384 dimensiones)

Texto: "gato"
Embedding: [0.3, 0.7, 0.2, 0.4, ...] (similar a "perro" porque son animales)

Texto: "automóvil"
Embedding: [0.8, 0.1, 0.9, 0.2, ...] (muy diferente)
```

### ¿Por qué son importantes?

Los embeddings capturan **significado semántico**:

```
"perro" vs "can" (sinónimos)
→ Embeddings muy similares

"banco" (asiento) vs "banco" (financiero)
→ Embeddings diferentes (contexto importa)
```

### Cómo se usan en RAG

```
1. INDEXING (Una vez)
   Documentos → Embedding Model → Vectores → Vector DB

2. QUERY (Cada búsqueda)
   Query → Embedding Model → Vector query → 
   Vector DB busca vectores similares → Documentos relevantes
```

---

### Elegir el Modelo de Embeddings Correcto

#### Consideraciones

**1. Propiedades del Texto**:
- **Vocabulario**: ¿Español, inglés, técnico?
- **Dominio**: ¿Médico, legal, general?
- **Longitud**: ¿Tweets cortos o documentos largos?

**2. Capacidades del Modelo**:
- **Multi-idioma**: ¿Necesitas soporte para varios idiomas?
- **Dimensiones**: Más dimensiones = más precisión, pero más costo

**3. Consideraciones Prácticas**:
- **Context window**: ¿Cuántos tokens acepta?
- **Privacidad**: ¿API externa o local?
- **Costo**: ¿Gratis, open-source, o pago?
- **Benchmark**: Probar varios y comparar

#### Modelos Populares

| Modelo | Dimensiones | Idiomas | Contexto | Tipo |
|--------|-------------|---------|----------|------|
| **BGE-Large** | 1024 | Multi | 512 tokens | Open Source |
| **E5** | 768 | Multi | 512 tokens | Open Source |
| **OpenAI Ada** | 1536 | Multi | 8191 tokens | Propietario |
| **Cohere Embed** | 4096 | Multi | Variable | Propietario |

---

### Tip 1: Elige Tu Modelo Sabiamente

**Problema**: Modelo de embeddings entrenado en inglés técnico falla con español coloquial

**Solución**: El modelo debe:
- ✅ Estar entrenado en datos similares a los tuyos
- ✅ Soportar tu(s) idioma(s)
- ✅ Entender tu dominio (médico, legal, etc.)

### Tip 2: Mismo Espacio de Embeddings

**Problema**: Queries y documentos con modelos diferentes

**Ejemplo ERROR**:
```
Documentos embedded con: modelo_A
Queries embedded con: modelo_B
→ Vectores incomparables ❌
```

**Solución**:
```
✅ Usa el MISMO modelo para docs y queries
✅ O entrena con datos similares
```

---

## Vector Search

### ¿Qué es una Base de Datos Vectorial?

**Definición**: Base de datos optimizada para **almacenar y buscar vectores** (embeddings).

**Diferencia con DB tradicional**:

```
DB Tradicional (SQL):
SELECT * FROM products WHERE name = 'iPhone'
→ Búsqueda exacta por palabra

Vector DB:
query = "teléfono inteligente"
→ Encuentra: iPhone, Samsung Galaxy, etc.
→ Búsqueda por SIGNIFICADO
```

### Propiedades de Vector DB

- **CRUD**: Create, Read, Update, Delete
- **Indexing**: Organiza vectores para búsqueda rápida
- **ANN**: Approximate Nearest Neighbor (búsqueda rápida aprox)
- **Filtering**: Soporta filtros metadata (WHERE clauses)
- **Scalability**: Maneja millones/billones de vectores

---

### Métricas de Similitud

#### 1. Euclidean Distance (Distancia Euclidiana)

**Qué mide**: Distancia "en línea recta" entre dos puntos

**Fórmula**: √((x₁-x₂)² + (y₁-y₂)² + ...)

**Cuándo usar**: Cuando la magnitud importa

**Ejemplo**:
```
Vector A: [1, 2]
Vector B: [4, 6]

Distancia = √((1-4)² + (2-6)²) = √(9 + 16) = √25 = 5

Interpretación: Menor distancia = más similar
```

#### 2. Cosine Similarity (Similitud Coseno)

**Qué mide**: Ángulo entre vectores (ignora magnitud)

**Rango**: -1 (opuestos) a 1 (idénticos)

**Cuándo usar**: Textos (la frecuencia absoluta no importa, solo la proporción)

**Ejemplo Visual**:
```
Vector A: [10, 0]
Vector B: [5, 0]

Distancia Euclidiana: Grande (5)
Cosine Similarity: 1.0 (misma dirección)

→ Son similares semánticamente aunque diferentes en magnitud
```

---

### Algoritmos de Vector Search

#### K-Nearest Neighbors (KNN)

**Qué hace**: Encuentra los K vecinos más cercanos

**Problema**: Lento con millones de vectores (compara con todos)

**Solución**: Approximate Nearest Neighbor (ANN)

#### Algoritmos ANN Populares

| Algoritmo | Creador | Características |
|-----------|---------|-----------------|
| **ANNOY** | Spotify | Usa árboles binarios, rápido |
| **HNSW** | - | Hierarchical NSW, muy preciso |
| **FAISS** | Facebook | Altamente optimizado, GPU |
| **ScaNN** | Google | State-of-the-art, scalable |

---

### Vector DB vs Vector Libraries/Plugins

#### Vector Libraries (ej. FAISS, ANNOY)

**Pros**:
- ✅ Simple
- ✅ Rápido para datasets pequeños

**Contras**:
- ❌ No soportan CRUD completo
- ❌ Todo en memoria (RAM)
- ❌ No soportan queries complejas (WHERE)
- ❌ No replicación

**Cuándo usar**: Prototipos, datos estáticos pequeños

#### Vector Plugins (ej. pgvector, elasticsearch)

**Pros**:
- ✅ Se integran con DB existente

**Contras**:
- ❌ Funcionalidad limitada
- ❌ Menos optimizado que vector DB dedicada

**Cuándo usar**: Ya tienes esa DB y no quieres añadir otra

#### Vector DB Dedicada (ej. Mosaic AI Vector Search)

**Pros**:
- ✅ Full CRUD
- ✅ Queries complejas (filtros metadata)
- ✅ Escalabilidad
- ✅ Replicación y alta disponibilidad
- ✅ Gobernanza (Unity Catalog)

**Cuándo usar**: Producción, datos grandes, requirements de gobernanza

---

### Mosaic AI Vector Search (Databricks)

#### Características

```
┌─────────────────────────────────────────────┐
│  MOSAIC AI VECTOR SEARCH                    │
├─────────────────────────────────────────────┤
│  ✅ Integrado con Lakehouse                 │
│  ✅ Auto-sync con Delta Tables              │
│  ✅ Unity Catalog (ACLs, governance)        │
│  ✅ Scalable y low-latency                  │
│  ✅ REST API + Python SDK                   │
│  ✅ Soporta filtros metadata                │
│  ✅ Managed (zero ops overhead)             │
└─────────────────────────────────────────────┘
```

#### Arquitectura

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
│  • Indexer (crea índices)                        │
│  • Vector DB (almacena vectores)                 │
│  • Query Engine (procesa queries)                │
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

#### Setup de Vector Search

```python
# Paso 1: Crear Vector Search Endpoint
from databricks.vector_search.client import VectorSearchClient

vsc = VectorSearchClient()

vsc.create_endpoint(
    name="mi_endpoint",
    endpoint_type="STANDARD"
)

# Paso 2: Crear índice desde Delta Table
vsc.create_delta_sync_index(
    endpoint_name="mi_endpoint",
    index_name="mi_catalogo.mi_schema.mi_indice",
    source_table_name="mi_catalogo.mi_schema.mi_tabla_docs",
    pipeline_type="TRIGGERED",  # o "CONTINUOUS"
    primary_key="id",
    embedding_source_column="text",  # columna con texto
    embedding_model_endpoint_name="mi_embedding_endpoint"
)

# Paso 3: Buscar documentos similares
results = vsc.get_index(
    "mi_catalogo.mi_schema.mi_indice"
).similarity_search(
    query_text="¿Cómo usar Databricks?",
    columns=["id", "text", "metadata"],
    num_results=5
)
```

---

## Reranking

### ¿Qué es Reranking?

**Problema**: Vector search devuelve top-K resultados, pero no todos son igualmente relevantes

**Solución**: **Reranking** = reordenar resultados por relevancia usando un modelo más sofisticado

### Proceso

```
1. INITIAL RETRIEVAL (Rápido)
   Query → Vector Search → Top 100 documentos

2. RERANKING (Preciso)
   Query + Top 100 → Reranker Model → Top 10 más relevantes reordenados

3. GENERATION
   Query + Top 10 reranked → LLM → Respuesta
```

### Ejemplo

**Query**: "¿Cómo hacer reset de contraseña?"

**Sin Reranking**:
```
Resultados Vector Search (top 5):
1. "Reset de contraseña de Facebook" (score: 0.85)
2. "Tutorial general de seguridad" (score: 0.83)
3. "Reset de contraseña de nuestro sistema" (score: 0.82) ← El mejor
4. "Historia de la contraseña" (score: 0.81)
5. "Contraseñas seguras tips" (score: 0.80)
```

**Con Reranking**:
```
Resultados Reranked (top 5):
1. "Reset de contraseña de nuestro sistema" (score: 0.95) ✅
2. "Reset de contraseña de Facebook" (score: 0.70)
3. "Tutorial general de seguridad" (score: 0.60)
4. "Contraseñas seguras tips" (score: 0.55)
5. "Historia de la contraseña" (score: 0.40)
```

### Rerankers Populares

#### Open Source
- **Cross-Encoders**: BERT fine-tuned para reranking
- **bge-reranker-base**: BGE model específico
- **FlashRank**: Ultra-rápido y ligero

#### Propietarios
- **Cohere Rerank**: API de Cohere
- **Jina Reranker**: Jina AI

### Beneficios vs Desafíos

#### ✅ Beneficios
- Mejora precisión de retrieval
- Reduce hallucinations (LLM recibe mejor contexto)
- Mayor relevancia de respuestas

#### ❌ Desafíos
- Añade latencia (llamada extra)
- Aumenta costo (otro modelo)
- Más complejidad en pipeline

---

## MLflow para RAG

### ¿Qué es MLflow?

**Definición**: Plataforma open-source para gestionar el **ciclo de vida completo** de modelos ML y GenAI.

**Co-desarrollado por**: Databricks

### MLflow Tracking

**Para qué**: Registrar experimentos y comparar

**Qué registra**:
- Parámetros del LLM (temperatura, max_tokens, etc.)
- Métricas (accuracy, latency, cost, etc.)
- Artifacts (modelos, prompts, chains, outputs)
- Source code del experimento

**Ejemplo**:
```python
import mlflow

with mlflow.start_run():
    # Registrar parámetros
    mlflow.log_param("model", "gpt-4")
    mlflow.log_param("temperature", 0.7)
    
    # Tu código RAG aquí
    response = my_rag_chain(query)
    
    # Registrar métricas
    mlflow.log_metric("latency", 1.5)  # segundos
    mlflow.log_metric("relevance_score", 0.95)
    
    # Registrar artifacts
    mlflow.log_artifact("prompt_template.txt")
```

---

### MLflow Model (Flavors)

**¿Qué es un "flavor"?**: Formato estándar para empaquetar modelos

**Estructura**:
```
mi_modelo/
  ├── MLmodel  (archivo metadata)
  ├── conda.yaml  (dependencias)
  ├── requirements.txt
  └── model/  (archivos del modelo)
```

**Flavors soportados**:
- `mlflow.pyfunc` (Python function - genérico)
- `mlflow.langchain` (Chains de LangChain)
- `mlflow.openai` (Modelos OpenAI)
- `mlflow.transformers` (Hugging Face)
- `mlflow.pytorch`, `mlflow.tensorflow`, etc.

---

### MLflow Model Registry (en Unity Catalog)

**Para qué**: Organizar, versionar y desplegar modelos

**Características**:
- ✅ Versioning automático
- ✅ Aliases (`@champion`, `@challenger`)
- ✅ Lifecycle management (dev → staging → prod)
- ✅ Collaboration y ACLs (Unity Catalog)
- ✅ Full lineage (qué datos, código, params usó)
- ✅ Tagging y anotaciones

**Ejemplo**:
```python
# Registrar modelo en Unity Catalog
mlflow.set_registry_uri("databricks-uc")

model_uri = f"runs:/{run_id}/model"
registered_model = mlflow.register_model(
    model_uri,
    "mi_catalogo.mi_schema.mi_rag_chatbot"
)

# Asignar alias
from mlflow import MlflowClient
client = MlflowClient()

client.set_registered_model_alias(
    "mi_catalogo.mi_schema.mi_rag_chatbot",
    "champion",
    version=3
)
```

---

## Evaluación de RAG

### ¿Qué evaluar en RAG?

```
┌────────────────────────────────────────┐
│  RAG PIPELINE                          │
├────────────────────────────────────────┤
│  1. CHUNKING                           │
│     ↓                                  │
│     Evaluar: Chunk size, overlap, etc. │
├────────────────────────────────────────┤
│  2. RETRIEVAL                          │
│     ↓                                  │
│     Evaluar: Precision, Recall, etc.   │
├────────────────────────────────────────┤
│  3. GENERATION                         │
│     ↓                                  │
│     Evaluar: Relevance, Faithfulness   │
└────────────────────────────────────────┘
```

---

### Métricas Clave de Evaluación

#### 1. Context Precision

**Qué mide**: ¿Los documentos retrieved son relevantes?

**Fórmula**: (Docs relevantes retrieved) / (Total docs retrieved)

**Ejemplo**:
```
Query: "¿Cómo resetear contraseña?"
Retrieved: 5 docs
Relevantes: 3 docs

Context Precision = 3/5 = 0.6
```

#### 2. Context Recall

**Qué mide**: ¿Recuperamos TODOS los documentos relevantes?

**Fórmula**: (Docs relevantes retrieved) / (Total docs relevantes en DB)

**Ejemplo**:
```
Docs relevantes en DB: 10
Retrieved: 5 docs (todos relevantes)

Context Recall = 5/10 = 0.5
```

**Interpretación**: Recall bajo = nos estamos perdiendo info importante

#### 3. Context Relevance

**Qué mide**: ¿El contexto retrieved es pertinente a la query?

**Medición**: LLM-as-a-judge evalúa cada doc

**Ejemplo**:
```
Query: "Política de vacaciones"

Doc 1: "Empleados tienen 15 días de vacaciones" → Alta relevancia
Doc 2: "Historia de la empresa fundada en 1990" → Baja relevancia
```

#### 4. Faithfulness (Fidelidad)

**Qué mide**: ¿La respuesta es fiel al contexto? (No alucina)

**Fórmula**: (Claims en respuesta soportados por contexto) / (Total claims)

**Ejemplo**:
```
Contexto: "Empleados tienen 15 días de vacaciones"

Respuesta LLM: "Tienes 15 días de vacaciones pagadas"
→ "15 días" ✅ (en contexto)
→ "pagadas" ❌ (no mencionado)

Faithfulness = 1/2 = 0.5 (baja fidelidad)
```

#### 5. Answer Relevancy (Relevancia de Respuesta)

**Qué mide**: ¿La respuesta realmente contesta la pregunta?

**Ejemplo**:
```
Query: "¿Cuántos días de vacaciones tengo?"

Respuesta 1: "Tienes 15 días de vacaciones al año"
→ Alta relevancia ✅

Respuesta 2: "Las vacaciones son importantes para el bienestar"
→ Baja relevancia ❌ (no responde)
```

#### 6. Answer Correctness (Corrección)

**Qué mide**: ¿La respuesta es factualmente correcta?

**Requiere**: Ground truth (respuesta correcta conocida)

**Ejemplo**:
```
Query: "¿Capital de Francia?"

Ground Truth: "París"
Respuesta LLM: "París"
→ Correctness = 1.0 ✅

Respuesta LLM: "Lyon"
→ Correctness = 0.0 ❌
```

---

### MLflow LLM Evaluation

**Características**:
- ✅ Batch evaluation (muchas queries a la vez)
- ✅ Comparación de múltiples modelos/prompts
- ✅ Métricas automáticas (toxicity, perplexity, etc.)
- ✅ LLM-as-a-judge integrado
- ✅ Cost-effective

**Ejemplo**:
```python
import mlflow

# Dataset de evaluación
eval_data = pd.DataFrame({
    "query": ["¿Cómo resetear contraseña?", "¿Política de vacaciones?"],
    "ground_truth": ["Ir a Settings > Reset", "15 días al año"]
})

# Evaluar modelo
results = mlflow.evaluate(
    model="models:/mi_rag_chatbot@champion",
    data=eval_data,
    targets="ground_truth",
    model_type="question-answering",
    evaluators=["default"]
)

# Ver resultados
print(results.metrics)
```

---

## 🎯 Preguntas de Práctica

### Pregunta 1
**¿Qué técnica de prompting darías ejemplos al LLM?**

A) Zero-shot  
B) Few-shot ✅  
C) Chain-of-Thought  
D) Prompt Chaining

**Respuesta**: B - Few-shot = dar ejemplos

---

### Pregunta 2
**¿Qué soluciona RAG?**

A) Problemas de latencia  
B) Knowledge gap del LLM ✅  
C) Costo del modelo  
D) Sesgo en datos

**Respuesta**: B - RAG añade conocimiento externo actualizado

---

### Pregunta 3
**¿Qué estrategia de chunking respeta la estructura del documento?**

A) Fixed-size  
B) Context-aware ✅  
C) Random chunking  
D) No chunking

**Respuesta**: B - Context-aware divide por secciones/párrafos

---

### Pregunta 4
**¿Qué métrica mide si la respuesta es fiel al contexto?**

A) Context Precision  
B) Faithfulness ✅  
C) Answer Relevancy  
D) Perplexity

**Respuesta**: B - Faithfulness = no alucinar

---

### Pregunta 5
**Para queries y documentos en vector search, ¿qué es importante?**

A) Usar modelos diferentes  
B) Usar el mismo modelo de embeddings ✅  
C) Embeddings de diferentes dimensiones  
D) No importa

**Respuesta**: B - Mismo modelo = mismo espacio vectorial

---

## 📝 Resumen Ejecutivo

### Lo que DEBES saber:

✅ **Prompt Engineering**: Zero-shot, Few-shot, Chain-of-Thought, Prompt Chaining  
✅ **RAG** = Retrieval + Augmentation + Generation (soluciona knowledge gap)  
✅ **Chunking**: Fixed-size (simple) vs Context-aware (mejor), usar overlap  
✅ **Embeddings**: Representaciones vectoriales, mismo modelo para queries y docs  
✅ **Vector Search**: Busca por significado (semántica), no por palabra exacta  
✅ **Mosaic AI Vector Search**: Integrado con Lakehouse, Unity Catalog, auto-sync  
✅ **Reranking**: Mejora precisión reordenando resultados  
✅ **MLflow**: Tracking, Registry (Unity Catalog), Evaluation  
✅ **Métricas RAG**: Context Precision/Recall, Faithfulness, Answer Relevancy/Correctness

---

## 🔗 Próximo Tema

➡️ **Continúa con**: `03_Desarrollo_Aplicaciones.md` (Compound AI Systems, Agents, Multi-modal)

