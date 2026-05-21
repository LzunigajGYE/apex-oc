---
name: apex-writing-plans
description: "Equivalente de /writing-plans (Superpowers) para APEX OC. Convierte una spec aprobada (de $apex-brainstorming) en un plan de ejecución detallado con tasks, checkpoints y criterios de completitud. Usado en Fase 04."
---

# APEX Writing Plans

Convierte el diseño aprobado en un plan ejecutable. Usado en Fase 04 después de que `$apex-brainstorming` haya producido una spec aprobada.

## Flujo

1. Leer la spec aprobada por el PM
2. Descomponer en tasks atómicas con:
   - Descripción clara de qué hacer
   - Archivos a tocar (si aplica)
   - Criterio de completitud verificable
   - Dependencias entre tasks
3. Agrupar tasks en checkpoints (bloques de trabajo coherentes)
4. Identificar tasks que pueden ejecutarse en paralelo
5. Generar el plan en formato estructurado

## Output esperado

Plan en markdown con:
- Objetivo del plan
- Tasks agrupadas por checkpoint
- Dependencias marcadas explícitamente
- Estimación de complejidad por task (baja / media / alta)

## Integración con Fase 04

El plan generado se almacena como referencia durante la ejecución.
Cada checkpoint completado se registra en `SPRINT.md`.
