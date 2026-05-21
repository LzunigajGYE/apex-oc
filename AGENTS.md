# APEX OC — Sistema de gestión de proyectos con IA para Codex

Skill universal para Codex (Desktop + CLI). Integra APEX Framework (governance), skills propios de brainstorming y planning, y subagentes Codex para ejecutar proyectos de cualquier tipo a través de fases estructuradas.

Invocar con: `$apex`

---

## Flujo de inicio (cada invocación de $apex)

1. Leer `~/.codex/apex/pm-profile.md` — si existe, cargar perfil del PM
2. Leer `~/.codex/apex/patterns.md` — si existe, cargar patrones cross-proyecto
3. Buscar `apex.config.json` en el directorio actual:
   - **Existe** → **MODO RETOMAR**: leer `currentPhase` → ejecutar fase activa
   - **No existe** → **MODO NUEVO**: ejecutar entrevista → crear docs base → iniciar Fase 01
4. Ejecutar la fase activa (ver `core/phases/0X-*.md`)
5. Al cerrar la sesión → escribir aprendizajes a memoria global

---

## MODO NUEVO — Entrevista de proyecto

Esperar respuesta completa de cada grupo antes de pasar al siguiente.

### Grupo 1 — Identidad
1. ¿Cuál es el nombre del proyecto?
2. ¿Qué tipo de proyecto es? `software` / `strategy` / `marketing` / `mixed`
3. ¿Cuál es la industria y el contexto general?

### Grupo 2 — Equipo y stakeholders
4. ¿Quién es el PM (la persona que aprueba las decisiones)?
5. ¿Con qué equipo se cuenta? (roles, no necesariamente personas)
6. ¿Quién es el cliente o destinatario final del proyecto?

### Grupo 3 — Alcance y éxito
7. ¿Cuál es el objetivo principal en una línea?
8. ¿Qué restricciones existen? (tiempo, presupuesto, tecnología)
9. ¿Cómo se ve el éxito al terminar?

**Al completar los 3 grupos, generar:**
- `apex.config.json` — usar `core/templates/apex.config.json` como base
- `PROJECT.md` — usar `core/templates/PROJECT.md`
- `TEAM.md` — usar `core/templates/TEAM.md`
- `LOG.md` — usar `core/templates/LOG.md`

Luego arrancar directamente **Fase 01** (ver `core/phases/01-inicio.md`).

---

## MODO RETOMAR

1. Leer `apex.config.json` → extraer `currentPhase`, `inProgress`, `completedPhases`, `environmentAuditDone`
2. **[Si `environmentAuditDone: false`] Auditoría de entorno** — solo la primera vez que APEX entra a un proyecto existente:
   - Escanear `AGENTS.md` del proyecto buscando skills de orquestación/PM activos
   - Escanear `.codex/config.toml` local (si existe) buscando hooks que interfieran con el flujo de fases
   - Clasificar cada elemento:
     - **Conflicto** — skills que definen fases propias, gates, memoria de proyecto o flujos de aprobación
     - **Complementario** — skills de ejecución técnica, contenido o análisis
     - **Sin impacto** — hooks y configs que no interactúan con el flujo de APEX
   - Reportar hallazgos al PM con clasificación clara
   - Si hay conflictos → recomendar supresión: agregar nota en `AGENTS.md` del proyecto indicando que APEX es el orquestador activo
   - Si PM aprueba → escribir nota de supresión en `AGENTS.md` del proyecto
   - Marcar `environmentAuditDone: true` en `apex.config.json`
3. Leer `pm-profile.md` → adaptar tono y nivel de detalle
4. Resumir con contexto:

```
Retomando Fase [N] — [nombre].
Completadas: [lista de fases completadas]
En progreso: [último item de inProgress]

¿Continuamos desde aquí?
```

5. Ejecutar la fase activa cargando su archivo `core/phases/0X-*.md`

---

## Adaptación por pm-profile

| Perfil detectado | Comportamiento |
|-----------------|----------------|
| `velocidad: rapido` | Recomendación directa, menos opciones |
| `velocidad: deliberado` | Presenta 3 opciones con trade-offs |
| `acepta_recomendaciones: raramente` | Presenta opciones sin sesgar hacia ninguna |
| `nivel_detalle: alto` | Expande explicaciones y ejemplos |
| `nivel_detalle: bajo` | Solo puntos clave, sin contexto extra |
| `enfoque: datos` | Justificaciones con métricas y evidencia |
| `enfoque: intuicion` | Narrativa y razonamiento cualitativo |

---

## Memoria — cuándo y qué escribir

APEX OC mantiene dos archivos en `~/.codex/apex/`:

### `pm-profile.md`
Actualizar **al final de cada sesión** con observaciones sobre velocidad de decisión, nivel de detalle solicitado, frecuencia de aceptación de recomendaciones y preguntas recurrentes.

### `patterns.md`
Actualizar **al cerrar cada fase** con agentes útiles, documentos más iterados y decisiones comunes por tipo de proyecto.

**Regla de patrón confirmado**: si un comportamiento ocurre 3+ veces en proyectos distintos → registrarlo explícitamente.

### `apex.config.json` del proyecto
Actualizar al cierre de cada fase: mover fase a `completedPhases`, actualizar `currentPhase`, registrar aprobación en `approvals`, actualizar `lastRun`.

---

## Skills disponibles

| Skill | Invocación | Equivalente en apex-cc |
|-------|-----------|------------------------|
| Orquestación principal | `$apex` | `/apex` |
| Fase 01 — Inicio | `$apex-phase-01` | `core/phases/01-inicio.md` |
| Fase 02 — Investigación | `$apex-phase-02` | `core/phases/02-investigacion.md` |
| Fase 03 — Estrategia | `$apex-phase-03` | `core/phases/03-estrategia.md` |
| Fase 04 — Ejecución | `$apex-phase-04` | `core/phases/04-ejecucion.md` |
| Fase 04b — Auditoría | `$apex-phase-04b` | `core/phases/04b-auditoria.md` |
| Fase 05 — Cierre | `$apex-phase-05` | `core/phases/05-cierre.md` |
| Brainstorming | `$apex-brainstorming` | `/brainstorming` (Superpowers) |
| Writing plans | `$apex-writing-plans` | `/writing-plans` (Superpowers) |

---

## Integración Codex Agents SDK

Para proyectos `software`/`mixed` en Fase 04, APEX OC usa handoffs del Agents SDK:

- Verifica existencia de `PROJECT.md` antes de arrancar Fase 02
- Verifica existencia de `RESEARCH.md` antes de Fase 03
- Verifica existencia de `STRATEGY.md` antes de Fase 04
- Gate explícito: el agente PM verifica que todos los outputs existen antes de hacer handoff al siguiente

---

## Créditos

| Framework | Autor | Repo |
|-----------|-------|------|
| APEX Framework | Luis Zúñiga | github.com/LzunigajGYE/apex-oc |
| APEX Core | Luis Zúñiga | github.com/LzunigajGYE/apex-core |
