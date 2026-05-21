# APEX OC

Sistema de gestión de proyectos con IA para **Codex** (Desktop + CLI). Adapter del APEX Framework para el ecosistema OpenAI.

Integra APEX Framework (governance y fases), skills propios de brainstorming y planning, y subagentes Codex para guiar cualquier tipo de proyecto a través de fases estructuradas con trazabilidad completa.

---

## Instalación

```bash
git clone --recurse-submodules https://github.com/LzunigajGYE/apex-oc ~/.codex/skills/apex-oc
```

Registrar los skills en tu configuración de Codex (`~/.codex/config.toml`):

```toml
[skills]
paths = ["~/.codex/skills/apex-oc/skills"]
```

---

## Uso

Desde cualquier directorio de proyecto en Codex:

```
$apex
```

APEX OC detecta automáticamente si el proyecto es **nuevo** (no hay `apex.config.json`) o **existente** (retoma la fase activa).

---

## Proyecto nuevo

Al invocar `$apex` por primera vez, APEX hace una entrevista en 3 grupos:

1. **Identidad** — nombre, tipo, industria
2. **Equipo** — PM, roles, cliente
3. **Alcance** — objetivo, restricciones, definición de éxito

Luego genera:
- `apex.config.json` — configuración y estado del proyecto
- `PROJECT.md` — visión, objetivo, OKRs
- `TEAM.md` — equipo y stakeholders
- `LOG.md` — trazabilidad desde el primer día

Y arranca **Fase 01 — Inicio**.

---

## Retomar un proyecto

Si `apex.config.json` existe, APEX lee la fase activa y resume:

```
Retomando Fase [N] — [nombre].
Último checkpoint: [item de inProgress]
¿Continuamos desde aquí?
```

---

## Fases

| Fase | Nombre | Skills usados | Output |
|------|--------|--------------|--------|
| 01 | Inicio | `$apex-phase-01`, `$apex-brainstorming` | `PROJECT.md` |
| 02 | Investigación | `$apex-phase-02` | `RESEARCH.md` |
| 03 | Estrategia | `$apex-phase-03` | `STRATEGY.md` |
| 04 | Ejecución | `$apex-phase-04`, `$apex-writing-plans` | `SPRINT.md` |
| 04b | Auditoría | `$apex-phase-04b` | entrada en `LOG.md` |
| 05 | Cierre | `$apex-phase-05` | `CLOSE.md` + entregable |

---

## Skills disponibles

| Skill | Invocación | Descripción |
|-------|-----------|-------------|
| Orquestación | `$apex` | Punto de entrada principal |
| Fase 01 | `$apex-phase-01` | Inicio del proyecto |
| Fase 02 | `$apex-phase-02` | Investigación de mercado |
| Fase 03 | `$apex-phase-03` | Estrategia y roadmap |
| Fase 04 | `$apex-phase-04` | Ejecución (bifurca por tipo) |
| Fase 04b | `$apex-phase-04b` | Auditoría pre-entrega |
| Fase 05 | `$apex-phase-05` | Cierre y entregable final |
| Brainstorming | `$apex-brainstorming` | Diseño pre-implementación |
| Writing plans | `$apex-writing-plans` | Plan de ejecución detallado |

---

## Memoria

APEX OC aprende de cada proyecto:

- `~/.codex/apex/pm-profile.md` — perfil del PM (velocidad, detalle, preferencias)
- `~/.codex/apex/patterns.md` — patrones cross-proyecto (agentes útiles, decisiones frecuentes)

---

## Tipos de proyecto soportados

| Tipo | Descripción |
|------|-------------|
| `software` | Apps, APIs, pipelines, sistemas técnicos |
| `strategy` | Estrategia de negocio, consultoría, planificación |
| `marketing` | Campañas, lanzamientos, posicionamiento |
| `mixed` | Combinación de tecnología y estrategia |

---

## Estructura del repo

```
apex-oc/
├── AGENTS.md                    ← orquestación principal
├── README.md                    ← esta guía
├── core/                        ← submodule: LzunigajGYE/apex-core
│   ├── phases/                  ← fases 01-05 + 04b
│   ├── templates/               ← apex.config.json, PROJECT.md, TEAM.md, LOG.md
│   ├── memory/                  ← pm-profile.md, patterns.md (templates)
│   └── Docs/apex-skill-design.md
└── skills/
    ├── apex/                    ← $apex — orquestador
    ├── apex-phase-01/           ← $apex-phase-01
    ├── apex-phase-02/           ← $apex-phase-02
    ├── apex-phase-03/           ← $apex-phase-03
    ├── apex-phase-04/           ← $apex-phase-04
    ├── apex-phase-04b/          ← $apex-phase-04b
    ├── apex-phase-05/           ← $apex-phase-05
    ├── apex-brainstorming/      ← $apex-brainstorming
    └── apex-writing-plans/      ← $apex-writing-plans

---

## Créditos

| Framework | Autor |
|-----------|-------|
| APEX Framework | Luis Zúñiga |
