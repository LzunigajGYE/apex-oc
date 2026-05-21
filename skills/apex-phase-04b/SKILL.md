---
name: apex-phase-04b
description: "APEX Fase 04b — Auditoría. Revisión sistemática antes de cerrar o entregar. Para software/mixed: security-review, code-reviewer, senior-qa. Para strategy/marketing: review de consistencia vs spec. Agrega entrada a LOG.md. Gate: hallazgos críticos resueltos y PM aprueba."
---

Ejecutar esta fase siguiendo exactamente el archivo `core/phases/04b-auditoria.md`.

Inputs requeridos:
- Sprint completado (Fase 04 aprobada)
- Respuestas de entrevista Grupo A (alcance de la auditoría)

APEX OC ejecuta la auditoría directamente — no despacha subagentes en esta fase.

**software / mixed** (en orden):
1. `security-review` — vulnerabilidades, secrets, OWASP
2. `code-reviewer` — calidad, antipatterns, cobertura
3. `senior-qa` — tests, edge cases, regresiones

**strategy / marketing:**
1. `review` — consistencia del entregable vs PROJECT.md y STRATEGY.md

Clasificación: Crítico / Alto / Medio / Bajo

Outputs esperados:
- Entrada en `LOG.md` con hallazgos clasificados y resoluciones

Gate: críticos resueltos, PM aprueba cierre de auditoría.
