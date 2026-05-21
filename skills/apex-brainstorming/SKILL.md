---
name: apex-brainstorming
description: "Equivalente de /brainstorming (Superpowers) para APEX OC. Explora intent, requisitos y diseño de una feature o workstream antes de implementar. Hard-gate: no avanza a la siguiente fase de APEX sin que el PM apruebe la spec generada. Usado principalmente en Fase 01."
---

# APEX Brainstorming

Skill de diseño pre-implementación. APEX lo invoca como hard-gate en Fase 01 — el PM debe aprobar el output antes de continuar.

## Flujo

1. **Entender el intent** — Preguntar: ¿qué se quiere lograr? ¿por qué ahora? ¿qué problema resuelve?
2. **Explorar requisitos** — Preguntar: ¿quién lo usa? ¿cuáles son los casos de uso principales? ¿qué no debe hacer?
3. **Diseñar la solución** — Proponer enfoque, alternativas consideradas, trade-offs, decisiones tomadas
4. **Generar spec aprobada** — Documento estructurado con: contexto, decisiones, tasks de implementación

## Output esperado

Spec estructurada con:
- Contexto y problema
- Decisiones de diseño con rationale
- Lista de tasks (suficientemente atómica para ejecutar)
- Criterios de aceptación

## Gate

El PM debe aprobar la spec explícitamente antes de que APEX continúe.
Sin aprobación → no avanzar a la siguiente fase ni a `/apex-writing-plans`.
