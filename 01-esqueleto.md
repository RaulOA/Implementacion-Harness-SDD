# Etapa 1 / 4 — Esqueleto del arnés

> **Contrato común (todas las etapas).** Arnés **estándar**: misma estructura en todo proyecto (🔒),
> contenido derivado de ESTE proyecto (🔎). Cero suposiciones; estado **en archivos versionados del
> repo** (viaja solo entre máquinas); una feature a la vez; SDD con puerta humana; verificación
> ejecutable; trazabilidad `R<n> → test`; patrón leader/spec_author/implementer/reviewer con
> anti-teléfono. **No avances de etapa sin la confirmación del final.** Leé `HARNESS-INSTALL.md`: de
> ahí salen el stack, los 5 comandos y las rutas 🔎.

## Tu único trabajo en esta etapa

Crear **todos los archivos estándar del arnés**. **NO migres ni quites nada de lo viejo todavía** (eso
es la Etapa 2). El contenido 🔎 sale de `HARNESS-INSTALL.md`; la estructura 🔒 es idéntica en todo
proyecto. Creá exactamente esta lista y marcá cada uno al terminar.

### Raíz
- [ ] `CLAUDE.md` — fuerza el rol **`leader`**: el modelo siempre orquesta y **nunca edita código de
  la app ni tests** directamente; para tareas de código lanza el subagente apropiado; **nunca marca
  `done` solo**; **nunca salta la puerta humana**. Excepción: lectura/exploración y cambios fuera de
  código (docs, configuración, archivos de `progress/`) los hace el propio leader.
- [ ] `AGENTS.md` — mapa de navegación (divulgación progresiva): pasos al empezar (correr `init.ps1`,
  leer `progress/current.md` y el tablero), tabla "qué es cada cosa y cuándo leerla", reglas duras,
  flujo SDD, ritual de cierre.
- [ ] `feature_list.json` — tablero vivo: `project`, `description`, `rules` (`one_feature_at_a_time`,
  `require_tests_to_close`, `require_approved_spec_to_implement`,
  `valid_status: [pending, spec_ready, in_progress, done, blocked]`, `sdd_required_when`), y
  `features[]` (`id`, `name`, `title`, `description`, `acceptance[]`, opcional `"sdd": true`, `status`,
  y los campos de aprobación **`approved_by`/`approved_at`** que escribe **solo** el humano). **Al pasar
  a `done`**, mové la entrada a `progress/history.md` y **podala del JSON**. Dejá 1–2 features de
  ejemplo en `pending` con `"sdd": true`.
- [ ] `CHECKPOINTS.md` 🔒 — criterios objetivos de "estado final correcto": arnés completo, estado
  coherente (≤1 `in_progress`, y toda `in_progress` con `approved_by`/`approved_at`), código respeta la
  arquitectura, verificación real (`init.ps1` verde, tests por módulo), sesión cerrada bien, bloque SDD
  (toda feature `sdd` no-`pending` con sus 3 specs, requirements EARS, tasks `[x]`, cada `R<n>` con
  test), y un ítem de **constitución** (existe y el cambio no viola principios). Sin nada atado a un
  lenguaje.
- [ ] `init.ps1` 🔒 *forma* / 🔎 *comandos* — PowerShell, con bloque de configuración arriba
  (`$TestCmd`, `$LintCmd`, `$TypecheckCmd`, `$BuildCmd` desde `HARNESS-INSTALL.md`). En orden
  **fail-fast** (lo barato primero, corta al primer fallo) con `[OK]`/`[WARN]`/`[FAIL]` y exit code:
  (1) toolchain disponible; (2) archivos base del arnés existen; (3) invariantes del tablero
  (`feature_list.json` parseable con `ConvertFrom-Json`, estados válidos, **≤1 `in_progress`**, **toda
  `in_progress` con `approved_by`/`approved_at`**, y specs presentes para features `sdd` no-`pending`);
  (4) corre `Typecheck`/`Lint`/`Build`/`Test` no vacíos (lo caro al final); (5) resumen. Sin chequeos
  de base de datos ni de red.

### `docs/`
- [ ] `docs/specs.md` 🔒 — proceso SDD **casi literal**: estructura `specs/<feature>/{requirements,
  design,tasks}.md`; tabla de estados; **puerta humana** (`pending → [spec_author] → spec_ready → ⏸
  HUMANO → in_progress → [implementer → reviewer] → done`); **EARS estricto** con sus cinco patrones:

  | Patrón | Plantilla |
  |---|---|
  | Ubicuo | `El sistema DEBE <acción>.` |
  | Evento | `CUANDO <disparador>, el sistema DEBE <acción>.` |
  | Estado | `MIENTRAS <estado>, el sistema DEBE <acción>.` |
  | Opcional | `DONDE <feature opcional>, el sistema DEBE <acción>.` |
  | No deseado | `SI <evento no deseado> ENTONCES el sistema DEBE <acción>.` |

  Reglas EARS: id estable `R1`, `R2`…; un solo `DEBE` por requisito; nada de verbos blandos, solo
  `DEBE`/`NO DEBE`; cada `R<n>` verificable por un test; trazabilidad `R<n> ↔ test` obligatoria.
  **Compuerta de complejidad:** solo features multi-archivo, riesgosas o que tocan la interfaz llevan
  `"sdd": true`; un cambio trivial o de un archivo va **sin** SDD (implementer directo).
- [ ] `docs/architecture.md` 🔎 — "qué es hacer un buen trabajo" en este proyecto, leído del código
  existente (o de lo declarado si es greenfield): capas/carpetas reales y responsabilidades, **rutas de
  código y tests**, política de dependencias, manejo de errores, y un "qué NO hacer". Los agentes
  referencian este archivo en vez de rutas fijas.
- [ ] `docs/conventions.md` 🔎 — estilo, nombres y estructura **derivados del código ya presente** (o de
  preferencias). No copies reglas de un lenguaje que el proyecto no usa.
- [ ] `docs/verification.md` 🔎 — niveles adaptados al stack: unitarios (obligatorio), integración/E2E
  para UI (obligatorio si toca interfaz), smoke manual (opcional), trazabilidad `R<n> → test`
  (obligatorio para `sdd`). Comandos reales + anti-patrones.
- [ ] `docs/constitution.md` 🔒 *estructura* / 🔎 *sección del proyecto* — **principios inmutables**
  (estilo Spec Kit), corto a propósito. Encabezado con `Versión: 1.0.0` + fecha. **Núcleo invariante**
  (`DEBE`/`NO DEBE` + 1 línea): (1) ninguna feature es `done` sin `init.ps1` verde y trazabilidad
  completa; (2) la spec es el contrato — sin spec aprobado por humano no hay código de feature `sdd`;
  (3) una feature a la vez, estado en archivos; (4) ante fallo de herramienta el agente NO improvisa
  workarounds: para, marca `blocked`, documenta. **Sección del proyecto** 🔎: 3–6 principios propios
  del stack/dominio (calidad, testing, seguridad/datos sensibles, compatibilidad, accesibilidad,
  rendimiento). **Regla de enmienda**: cambiarla requiere acción humana + bump semver; el agente nunca
  la enmienda solo. *(Para la sección del proyecto, presentame el borrador y esperá mi visto bueno —
  es contrato.)*

### `.claude/agents/` — los cuatro subagentes 🔒 *contratos* / 🔎 *comandos y rutas*
Markdown con frontmatter (`name`, `description`, `tools`, **`model`**); cuerpo = system prompt. Hablá de
"código de la aplicación" y "tests" y de las rutas de `docs/architecture.md`, **sin** referencias a un
lenguaje. **Ruteo de modelo:** Opus en leader y spec_author; Sonnet en implementer y reviewer.
- [ ] `leader.md` (`Read, Glob, Grep, Bash, Agent` · `model: opus`) — orquesta, no codifica. Arranque:
  lee `AGENTS.md`/tablero/`progress/current.md`, corre `init.ps1`. Flujo SDD por casos: `pending` →
  lanza `spec_author` y **para** pidiendo aprobación; `spec_ready` con aprobación → escribe
  `approved_by`/`approved_at` (solo por acción humana), pasa a `in_progress`, lanza `implementer` y
  luego `reviewer`; `spec_ready` sin aprobación → recuerda; `in_progress` → pregunta reanudar/abortar.
  Anti-teléfono. Prohibido: editar código, marcar `done`, saltar la puerta humana.
- [ ] `spec_author.md` (`Read, Write, Edit, Glob, Grep, Bash` · `model: opus`) — redacta los 3 specs de
  una feature `pending` con `"sdd": true`: requirements EARS (cada `acceptance` cubierto por ≥1 `R<n>`),
  design (archivos, firmas, excepciones, **≥1 alternativa descartada**), tasks (checklist `[ ]`, cada
  una cita sus `R<n>`). Marca `spec_ready` y **para**. Nunca toca código/tests. Salida: una línea
  (`spec_ready -> specs/<name>/`).
- [ ] `implementer.md` (`Read, Write, Edit, Glob, Grep, Bash` · `model: sonnet`) — implementa **una**
  feature `in_progress` según su spec. Anota su plan en `progress/current.md`. Cada `T<n>` en orden
  marcando `[x]`, test junto a cada cambio, corre `init.ps1` hasta verde. **Anti-teléfono:** escribe su
  informe (archivos tocados + mapa `R<n> → test` + salida de tests) **anexándolo a
  `progress/reports.md`** y devuelve solo la referencia. No se autoaprueba. Si una task exige desviarse
  del spec, para y pide cambios. Salida: una línea.
- [ ] `reviewer.md` (`Read, Glob, Grep, Bash` · `model: sonnet`) — aprueba/rechaza, **no edita**.
  Verifica trazabilidad (cada `R<n>` con test), tasks completas, conformidad con `architecture.md`/
  `conventions.md`/**`constitution.md`**, corre `init.ps1` (verde), recorre `CHECKPOINTS.md`. **Marca
  solo brechas de correctitud o requisito; el estilo es opcional.** Anexa su veredicto a
  `progress/reports.md`. Salida: una línea (`APPROVED`/`CHANGES_REQUESTED -> progress/reports.md`).

### `.claude/settings.json` 🔒 *forma* / 🔎 *comandos*
- [ ] Hooks que el arnés ejecuta (no el agente): `PostToolUse` matcher `Edit|Write` que corre lo
  **barato** (`Typecheck` o `Lint`, **no** la suite completa) y muestra solo el resumen; `Stop` que
  corre `init.ps1` antes de cerrar y, si tu Claude Code lo soporta, **bloquea el cierre (exit ≠ 0)** en
  rojo. `permissions.allow` con los comandos del proyecto.

### `progress/` — la memoria del arnés (archivos consolidados, no un archivo por feature)
- [ ] `progress/current.md` — plantilla del estado de la sesión activa (feature en curso, plan, notas,
  bloqueos). Se **sobrescribe** cada sesión.
- [ ] `progress/history.md` — bitácora **compacta append-only** de features terminadas: una entrada
  corta por feature (qué, cuándo, `approved_by`, resumen de 1–2 líneas). Inicia con un encabezado.
- [ ] `progress/reports.md` — informes **detallados append-only** de implementación/revisión (el lugar
  en disco del anti-teléfono). Inicia con un encabezado.

> **Nota de tokens:** los agentes leen `history.md`/`reports.md` de forma **dirigida** (buscando la
> feature o las últimas entradas), no enteros. Son podables si crecen mucho.

## PAUSA — pará acá

**Autoauditoría:** listame los archivos creados, confirmá que `init.ps1` referencia solo comandos del
bloque de configuración (nada de un lenguaje ajeno) y que las rutas salen de `architecture.md`. Mostrame
el borrador de la **sección de proyecto de la constitución** para mi visto bueno. Marcá `[x] Etapa 1` en
`HARNESS-INSTALL.md`. **Pará y esperá mi OK. No empieces la Etapa 2.**
