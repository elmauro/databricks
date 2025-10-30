# 🧠 Tips de Heurísticas para Exámenes (ES)

Guía rápida para identificar respuestas correctas/incorrectas en preguntas tipo Databricks GenAI.

## Sesgo (Bias)
- ❌ Si una opción dice “remover datos/sesgar/eliminar información del entrenamiento”, es mala práctica.
- ✅ Mejor: “etiquetar (taggear) y filtrar en recuperación”, “balancear dataset”, “auditar sesgos”, “usar fairness metrics”.
- Descarta si: propone borrar información, ignorar métricas de sesgo, o entrenar solo con un subconjunto homogéneo.
- Probable correcta si: sugiere tagging + filtros en Vector Search/metadata, evaluación de sesgo y datasets balanceados.

## Alucinaciones (Hallucinations)
- ❌ “Aumentar temperatura” o “confiar ciegamente en el LLM”.
- ✅ “Usar RAG con fuentes verificadas”, “verificación de hechos”, “LLM-as-a-judge”, “citar fuentes”, “mejorar prompts/estructurar”.
- Descarta si: no usa contexto externo, sugiere ignorar citas, o recomienda solo bajar/ subir temperatura como solución.
- Probable correcta si: añade RAG, verifica con fuentes, cita documentos, o agrega guardrails de fact-checking.

## Costos
- ❌ “Usar siempre el modelo más grande” o “no medir tokens”.
- ✅ “Modelos más pequeños cuando aplique”, “caché de respuestas”, “compresión de prompts”, “monitorizar tokens y costo/request”, “scale-to-zero”.
- Descarta si: no mide costos/tokens, o fija infra 24/7 sin necesidad.
- Probable correcta si: instrumenta métricas de costo, usa auto-escalado y selecciona modelo por costo/beneficio.

## Latencia
- ❌ “Agregar pasos innecesarios sin paralelismo”.
- ✅ “Paralelizar cadenas”, “Vector Search para reducir contexto”, “servir en tiempo real con auto-escalado”, “optimizar batch/quantization (si self-host)”.
- Descarta si: carga todo el corpus en el prompt o propone una sola cola sin escalado.
- Probable correcta si: reduce contexto con recuperación semántica y usa infraestructura autoescalable.

## Disponibilidad (Availability)
- ❌ “Un solo endpoint sin escalado/observabilidad”.
- ✅ “Auto-scaling”, “A/B o canary”, “monitoring + alertas”, “fallbacks controlados”.
- Descarta si: no hay monitoreo ni redundancia.
- Probable correcta si: usa endpoints con auto-escalado, A/B, y capturas en Inference Tables.

## Drift (Datos/Modelo)
- ❌ “Ignorar drift” o “reentrenar sin evidencias”.
- ✅ “Lakehouse Monitoring (profile/drift/custom)”, “triggers de retraining con umbrales”, “versionado y comparación”.
- Descarta si: reentrena arbitrariamente o no compara contra baseline.
- Probable correcta si: define umbrales y plan de retraining basado en métricas.

## Calidad (Quality)
- ❌ “Medir solo una métrica” o “solo loss/perplexity”.
- ✅ “Métricas por tarea (BLEU/ROUGE) + relevance/faithfulness”, “feedback humano”, “benchmarks (MMLU, HellaSwag)”.
- Descarta si: confunde métricas (p. ej., usar BLEU para clasificación) o ignora feedback.
- Probable correcta si: combina métricas técnicas y de producto (relevancia, satisfacción).

## Seguridad/Guardrails
- ❌ “Confiar en instrucciones del usuario” o “permitir PII/salida sensible”.
- ✅ “Input/output guardrails (Llama Guard, PII check, toxicity)”, “prompt safety”, “rate limiting”, “auditoría”.
- Descarta si: no filtra entradas/salidas o desactiva controles.
- Probable correcta si: integra clasificación de seguridad antes y después del LLM.

## Recuperación (RAG)
- ❌ “Aumentar contexto sin chunking ni filtros”, “mezclar modelos de embeddings distintos”.
- ✅ “Chunking con solapamiento”, “filtros por metadata”, “mismo modelo para docs y queries”, “reranking cuando haga falta”.
- Descarta si: mete documentos completos al prompt o no filtra por metadatos.
- Probable correcta si: usa Vector Search + filtros + rerank.

## Embeddings/Vector Search
- ❌ “Elegir embeddings sin considerar idioma/dominio”.
- ✅ “Embeddings multi-idioma/dominio”, “Z-Ordering/Data Skipping”, “Vector Search con filtros”.
- Descarta si: usa modelos de embeddings distintos entre index y query.
- Probable correcta si: mantiene el mismo espacio vectorial y optimiza almacenamiento/consulta.

## Elección de Modelo
- ❌ “Siempre propietario externo” o “siempre open source”.
- ✅ “Privacidad, calidad, costo, latencia” como criterios.
- Descarta si: ignora compliance o TTM (time-to-market).
- Probable correcta si: alinea el tipo de modelo al caso (privacidad interna vs rapidez/calidad).

## Gobernanza (Unity Catalog)
- ❌ “Permisos ad-hoc en cada sistema”.
- ✅ “ACLs centralizados en UC”, “lineage/auditoría”, “Registry con aliases (@champion/@challenger)”.
- Descarta si: no hay control centralizado ni trazabilidad.
- Probable correcta si: usa UC para permisos y linaje extremo a extremo.

## Evaluación
- ❌ “No usar datasets de referencia ni métricas claras”.
- ✅ “Offline + online”, “LLM-as-a-judge cuando falten referencias”, “A/B y canary”.
- Descarta si: toma decisiones sin evidencia o con una sola métrica aislada.
- Probable correcta si: triangula con benchmarks, feedback y experimentos controlados.

## Privacidad y Cumplimiento
- Descarta si: exporta datos sensibles a APIs externas sin garantías o ignora GDPR/CCPA.
- Probable correcta si: propone modelos self‑host/UC‑secured, anonimización/pseudonimización, y control de acceso en UC.

## Prompt Engineering (patrones)
- Descarta si: prompt vago sin instrucciones/rol/formato.
- Probable correcta si: incluye rol, contexto, entrada clara y formato; usa delimitadores; few‑shot cuando hay formato.

## Chunking vs Reranking
- Descarta si: solo “chunks más grandes” para recuperar mejor.
- Probable correcta si: chunking con solapamiento + filtros; y reranking cuando top‑K contiene ruido.

## Vector DB vs Plugins
- Descarta si: usa plugin sin necesidades de gobernanza/escala.
- Probable correcta si: vector DB dedicada (Mosaic AI Vector Search) para CRUD, filtros, ACLs y escalabilidad.

## MLflow y Registry
- Descarta si: despliega sin versionar ni alias.
- Probable correcta si: usa Registry en UC, aliases (@champion/@challenger), y lineage para auditoría.

## Tráfico/A‑B/Canary
- Descarta si: envía 50% a un modelo desconocido de inicio.
- Probable correcta si: empieza con 5–10% canary y escala según métricas.

## Caching y Retries
- Descarta si: re‑consulta idénticas sin caché o aplica retries infinitos.
- Probable correcta si: caché para prompts frecuentes, retries con backoff y límites.

## Decoding (temperatura/top_p)
- Descarta si: sube temperatura para “arreglar” precisión factual.
- Probable correcta si: baja temperatura para factual; ajusta top_p/top_k con criterio.

## Context Window / Lost in the Middle
- Descarta si: mete documentos completos en el prompt.
- Probable correcta si: RAG + chunking + selección de pasajes relevantes.

## Multilingüe
- Descarta si: embeddings/modelo no soportan el idioma requerido.
- Probable correcta si: embeddings y LLM alineados al idioma/dominio.

## Monitoreo – Umbrales
- Descarta si: monitoreo sin umbrales/alertas.
- Probable correcta si: define p95/p99 latencia, drift thresholds y alertas; captura en Inference Tables.
