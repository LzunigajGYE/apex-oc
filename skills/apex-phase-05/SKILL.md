---
name: apex-phase-05
description: "APEX Fase 05 — Cierre. Produce el entregable final, documenta lecciones aprendidas y cierra formalmente el proyecto. Despacha report-generator y research-synthesizer en paralelo. Genera CLOSE.md + entregable en formato acordado con el PM."
---

Ejecutar esta fase siguiendo exactamente el archivo `core/phases/05-cierre.md`.

Inputs requeridos:
- Auditoría aprobada (Fase 04b completada)
- Respuestas de entrevista Grupos A y B (entregable final + retrospectiva)

Agentes a despachar en paralelo:
- `report-generator` — draft del entregable final
- `research-synthesizer` — síntesis de lecciones aprendidas
- `data-analyst` — si hay métricas cuantitativas

Outputs esperados:
- `CLOSE.md` con resumen ejecutivo, resultados vs OKRs, lecciones y pendientes
- Entregable final en formato acordado (doc, presentación, informe)
- `apex.config.json` actualizado: `status → closed`

Gate: PM aprueba CLOSE.md y entregable final.
Al cerrar: actualizar pm-profile.md y patterns.md con aprendizajes del proyecto completo.
