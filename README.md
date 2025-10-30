# Databricks Generative AI Engineer Associate – Study Workspace

This repo organizes study materials, practice exams, and bilingual notes (ES/EN) to prepare for the Databricks Certified Generative AI Engineer Associate exam.

## What's inside
- Spanish guides (detailed): see `spanish/`
- English guides (mirrored translations): see `english/`
- Practice exams (Markdown format): see `practice_exams/`
- Quick heuristics/tips: `tips_es.md` (ES) and `tips_en.md` (EN)
- Certification overview: `README_CERTIFICACION.md` (ES) and `english/README_CERTIFICATION.md` (EN)

## Spanish guides (principal)
- `spanish/01_Fundamentos_IA_Generativa.md`
- `spanish/02_Desarrollo_Soluciones.md` (RAG, prompts, embeddings, Vector Search)
- `spanish/03_Desarrollo_Aplicaciones.md` (Compound systems, Agents, Multi‑modal)
- `spanish/04_Despliegue_Monitoreo.md` (MLflow, Model Serving, Monitoring)
- `spanish/05_Evaluacion_Gobernanza.md` (Evaluación, Seguridad, Guardrails)
- Resumen y guía completa: `Guia_Certificacion_Databricks_GenAI_COMPLETA.md`, `Resumen_Rapido_Certificacion.md`

## English guides (mirrored)
- `english/01_Generative_AI_Fundamentals.md`
- `english/02_Solution_Development.md`
- `english/03_Application_Development.md`
- `english/04_Deployment_Monitoring.md`
- `english/05_Evaluation_Governance.md`
- Overview: `english/Complete_Guide_Databricks_GenAI_Certification.md`, `english/Quick_Summary_Certification.md`

## Practice exams (Markdown)
- Located in `practice_exams/` (Test1–Test12). Each exam:
  - Keeps original questions/answers
  - Marks correct answers with ✅ and bold
  - Adds concise rationales

## Heuristics/tips (fast elimination)
- `tips_es.md`: heurísticas en español (bias, hallucinations, costo, latencia, disponibilidad, drift, calidad, guardrails, RAG, embeddings, modelo, gobernanza, evaluación)
- `tips_en.md`: mirrored in English

Example (Bias): preferir etiquetar y filtrar en recuperación; NO remover información del entrenamiento.

## Repo hygiene
- `.gitignore` excludes: `*.txt`, `*.pdf`, and `sources/` folder (privacy and size control)
- PDFs and raw sources should remain outside version control

## How to study
1. Read the Spanish or English series (1→5) end‑to‑end
2. Use the quick summary files to reinforce
3. Do all practice exams; aim for >80%
4. Review `tips_es.md` / `tips_en.md` before retakes
5. Practice small RAG/Serving notebooks in Databricks

## License / Notes
- Content is for personal study. Respect any licenses of external materials.
