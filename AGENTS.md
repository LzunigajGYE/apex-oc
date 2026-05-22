# APEX OC — Sistema de gestión de proyectos con IA para Codex

Skill universal para Codex (Desktop + CLI). Integra APEX Framework (governance), skills propios de brainstorming y planning, y subagentes Codex para ejecutar proyectos de cualquier tipo a través de fases estructuradas.

Invocar con: `$apex`

---

## Flujo de inicio (cada invocación de $apex)

1. Verificar si `~/.codex/apex/` existe:
   - Si **no existe** → crear directorio + copiar `core/memory/pm-profile.md` y `core/memory/patterns.md` como base vacía
2. Leer `~/.codex/apex/pm-profile.md` — si existe, cargar perfil del PM
3. Leer `~/.codex/apex/patterns.md` — si existe, cargar patrones cross-proyecto
4. Leer la hora actual: `TZ=[timezone_del_proyecto] date +"%H:%M"` (o `America/Guayaquil` si no hay config aún)
   - Registrar en `apex-time.log` si el proyecto ya tiene uno
5. Buscar `apex.config.json` en el directorio actual:
   - **Existe** → **MODO RETOMAR**: leer `currentPhase` → ejecutar fase activa
   - **No existe pero hay PROJECT.md / RESEARCH.md / STRATEGY.md** → **MODO RECUPERACIÓN** (ver abajo)
   - **No existe y directorio vacío** → **MODO NUEVO**: ejecutar entrevista → crear docs base → iniciar Fase 01
6. Ejecutar la fase activa (ver `core/phases/0X-*.md`)

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

## MODO RECUPERACIÓN

Cuando no existe `apex.config.json` pero sí hay documentos del proyecto:

1. Informar al PM: _"No encontré `apex.config.json`. El proyecto parece existente — intento reconstruir el estado."_
2. Inferir fase actual según documentos presentes:
   - Solo `PROJECT.md` → Fase 02 (investigación pendiente)
   - `PROJECT.md` + `RESEARCH.md` → Fase 03 (estrategia pendiente)
   - `PROJECT.md` + `RESEARCH.md` + `STRATEGY.md` → Fase 04 (ejecución pendiente)
   - `PROJECT.md` + `SPRINT.md` → Fase 04b (auditoría pendiente)
3. Mostrar estado inferido al PM y pedir confirmación
4. Si PM confirma → regenerar `apex.config.json` con la fase inferida y datos extraídos de los docs
5. Si PM rechaza → pedir que indique la fase correcta manualmente

**Si `apex.config.json` existe pero está malformado** (JSON inválido o faltan campos clave):
- Avisar: _"El archivo `apex.config.json` parece dañado. ¿Quieres que intente repararlo o prefieres corregirlo manualmente?"_
- No continuar hasta que el archivo sea válido.

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

APEX OC mantiene dos archivos en `~/.codex/apex/` (fuera del skill — sobrevive reinstalaciones):

### `~/.codex/apex/pm-profile.md`
Actualizar **al cerrar cada fase** con observaciones sobre velocidad de decisión, nivel de detalle solicitado, frecuencia de aceptación de recomendaciones y preguntas recurrentes.

### `~/.codex/apex/patterns.md`
Actualizar **al cerrar cada fase** con agentes útiles, documentos más iterados y decisiones comunes por tipo de proyecto.

**Regla de patrón confirmado**: si un comportamiento ocurre 3+ veces en proyectos distintos → registrarlo explícitamente.

### `apex.config.json` del proyecto
Actualizar al cierre de cada fase:
- Mover fase a `completedPhases`
- Actualizar `currentPhase` a la siguiente
- Registrar `phases.[fase-actual].closedAt` con timestamp
- Registrar aprobación en `approvals`
- Actualizar `lastRun`

---

## Regla de fase

**Una fase por sesión.** Al completar el gate de una fase:
1. Actualizar `apex.config.json` (ver Memoria arriba)
2. Escribir aprendizajes en `~/.codex/apex/pm-profile.md` y `~/.codex/apex/patterns.md`
3. Registrar `PHASE_END` en `apex-time.log`
4. Informar al PM que la fase cerró — **no iniciar la siguiente fase en la misma sesión**

---

## Módulo de tiempo

Activo cuando `timeTracking.enabled: true` en `apex.config.json` (valor por defecto).

### Cómo leer la hora

```bash
TZ=[workCalendar.timezone] date +"%H:%M"
```

### Ventanas de comportamiento

| Hora | Evento | Acción de APEX OC |
|------|--------|------------------|
| 09:00 | DAY_START | Saludar al PM, mostrar fase activa + tareas pendientes. Registrar `DAY_START` en `apex-time.log`. |
| 12:00–13:30 | MIDDAY | Si PM inicia sesión en este rango → mencionar brevemente. Sin bloquear. |
| 16:30 | WIND_DOWN | Avisar: _"Quedan ~90 min de jornada. ¿Qué queremos cerrar hoy?"_ Una sola vez por día. |
| 18:00 | END_OF_HOURS | Si hay gate abierto → recordar al PM antes de cerrar. Registrar `WINDOW_ALERT` en log. |
| 20:00 | NIGHT_START | Registrar `OUT_OF_HOURS` en log **silenciosamente**. Sin alerta al PM. |
| 23:00+ | LATE | Registrar `OUT_OF_HOURS` en log silenciosamente. Sin alerta. |

**Reglas anti-spam:**
- Solo **una alerta por ventana por día** — usar `windowAlertedAt` en `apex.config.json`
- Si `timeTracking.enabled: false` → sin registros, sin alertas

### Deadline de sesión

Si `timeTracking.sessionDeadline` está definido (ISO timestamp):
- **30 min antes**: _"En 30 minutos termina tu sesión programada. ¿Cerramos la fase o dejamos checkpoint?"_
- **15 min antes**: _"Quedan 15 minutos."_
- Solo 2 alertas en total por deadline

Desactivar para una sesión: `$apex time off`

### Radar de decisiones

APEX monitorea decisiones registradas en `apex-time.log` (tipo `DECISION`). Si en la sesión actual hay 3+ decisiones sin revisión:
- **Una sola alerta por sesión**: _"Llevamos [N] decisiones tomadas en esta sesión. ¿Las revisamos antes de continuar?"_
- Usar `timeTracking.radarAlertedAt` para no repetir la alerta
- Escribir silenciosamente en log si el PM responde "no ahora"

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
