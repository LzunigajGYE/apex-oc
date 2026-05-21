---
name: apex-phase-04
description: "APEX Fase 04 — Ejecución. Traduce la estrategia en trabajo concreto. Bifurca por tipo: software/mixed usa $apex-writing-plans + subagentes Codex; strategy/marketing usa agile-product-owner + project-manager. Genera SPRINT.md. Gate: sprint completado y PM aprueba output."
---

Ejecutar esta fase siguiendo exactamente el archivo `core/phases/04-ejecucion.md`.

Inputs requeridos:
- `STRATEGY.md` aprobado
- Respuestas de entrevista Grupos A y B

Bifurcación por tipo de proyecto:

**strategy / marketing:**
- Usar `$apex-writing-plans` para generar plan de trabajo
- Despachar agentes: `project-manager`, `workflow-orchestrator`

**software / mixed:**
- Usar `$apex-writing-plans` para plan técnico
- Usar handoffs del Codex Agents SDK para orquestación multi-agente
- Stack detection: leer `PROJECT.md` para determinar agente senior apropiado
- Subagentes disponibles vía Codex Agents SDK

Outputs esperados:
- `SPRINT.md` con backlog, objetivos y criterios de cierre

Gate: sprint completado, criterios de cierre cumplidos, PM aprueba.
