# Prompt para Claude Code — Instalar un arnés "Harness Engineering" (cualquier stack web)

> **Cómo usarlo:** abrí Claude Code en VS Code, en la **raíz** del repositorio donde querés
> instalar el arnés (sirve igual si está vacío o si ya tiene código), y pegá *todo* el bloque
> de abajo (entre las líneas `INICIO`/`FIN`) como primer mensaje.
>
> El prompt está hecho para ser **agnóstico**: no asume lenguaje, framework, gestor de paquetes
> ni patrón de diseño. Lo primero que hace el agente es *detectar* (o preguntarte) cómo se prueba,
> se lintea y se construye tu proyecto, y recién entonces arma el arnés adaptado a eso.

---

## ───────────────────────── INICIO DEL PROMPT ─────────────────────────

Tu tarea es **instalar un arnés de "Harness Engineering"** en este repositorio: una capa de
archivos y reglas que convierte el repo en un sistema sobre el cual vos (y futuros agentes) pueden
trabajar de forma **autónoma, una feature a la vez, y verificable**. El arnés debe funcionar para
**cualquier desarrollo web**, sin importar lenguaje, framework ni patrones. No instalás una app:
instalás la *estructura* alrededor de la app.

No empecés a crear archivos hasta terminar la FASE 0.

### Principios que el arnés DEBE encarnar (no negociables)

1. **El repositorio ES el sistema.** Todo el estado (alcance, specs, progreso, criterios de
   calidad) vive en archivos versionados, no en el chat.
2. **Divulgación progresiva.** `AGENTS.md` es un *mapa*, no una biblia: el agente lee bajo demanda,
   no todo de golpe.
3. **Una feature a la vez.** Como mucho una feature `in_progress`. Se valida por script, no por buena fe.
4. **Spec Driven Development (SDD) con puerta humana.** Toda feature marcada con SDD pasa por
   `requirements → design → tasks` (requirements en notación **EARS**) y **se detiene** para que un
   humano apruebe antes de tocar código.
5. **Estado en disco, no en chat.** `specs/`, `progress/current.md` e `history.md` sobreviven a
   reinicios y a ventanas de contexto reventadas.
6. **Verificación ejecutable.** El agente no dice "funciona": lo demuestra corriendo `./init.sh`,
   que ejecuta los tests/lint/build reales del proyecto.
7. **Trazabilidad obligatoria.** Cada requisito `R<n>` se mapea a al menos un test concreto; el
   revisor rechaza si falta cobertura.
8. **Patrón Leader → Spec → Implementer → Reviewer + regla anti-teléfono-descompuesto.** El leader
   orquesta y no codifica; el spec_author especifica y no codifica; el implementer codifica una
   feature; el reviewer aprueba/rechaza y no edita. Los subagentes **escriben sus resultados en
   archivos** y solo devuelven una referencia ligera, nunca el contenido por chat.

### FASE 0 — Detectar el stack (antes de crear NADA)

1. Inspeccioná la raíz y detectá el tipo de proyecto leyendo los manifiestos presentes:
   `package.json`, `pnpm-lock.yaml`/`yarn.lock`/`bun.lockb`, `requirements.txt`/`pyproject.toml`,
   `go.mod`, `Cargo.toml`, `composer.json`, `Gemfile`, `pom.xml`/`build.gradle`, `deno.json`, etc.
2. Determiná, **para este proyecto concreto**, estos cinco comandos (dejá vacío el que no aplique):
   - `TEST_CMD` — cómo se corren los tests (p. ej. `npm test`, `pnpm test`, `bun test`,
     `pytest -q`, `go test ./...`, `cargo test`, `php artisan test`, `deno test`).
   - `LINT_CMD` — linter/formateo en modo verificación (p. ej. `npm run lint`, `ruff check .`,
     `golangci-lint run`, `cargo clippy`). Vacío si no hay.
   - `TYPECHECK_CMD` — chequeo de tipos si aplica (p. ej. `tsc --noEmit`, `mypy .`). Vacío si no hay.
   - `BUILD_CMD` — build de verificación (p. ej. `npm run build`, `go build ./...`). Vacío si no hay.
   - `RUN_CMD` — cómo se levanta la app en local (informativo).
3. Detectá también **dónde vive el código** y **dónde viven los tests** (p. ej. `src/`, `app/`,
   `components/`, `lib/`, `pages/`; tests en `__tests__/`, `test/`, `tests/`, `e2e/`, o
   co-ubicados `*.test.*`/`*.spec.*`). Vas a registrar estas rutas en `docs/architecture.md` y todos
   los agentes las usarán **en vez de** rutas fijas.
4. **Mostrame** lo que detectaste (stack, los cinco comandos, rutas de código y tests) y **pedime
   confirmación o corrección**. Si el repo está vacío o algo es ambiguo, hacé preguntas cortas en vez
   de inventar. No avances a la FASE 1 sin mi visto bueno.

### FASE 1 — Crear el arnés

Creá exactamente estos archivos. Donde digo "copiá la idea", reproducí la intención del arnés de
referencia pero **adaptada al stack confirmado** y **sin ninguna suposición de lenguaje**.

**Raíz**

- `AGENTS.md` — Mapa de navegación (divulgación progresiva). Incluí: (a) pasos obligatorios al
  empezar (correr `./init.sh`, leer `progress/current.md` y `feature_list.json`); (b) una **tabla**
  que diga qué contiene cada archivo/carpeta y *cuándo* leerlo; (c) las reglas duras (una feature a
  la vez, no declarar `done` sin verificación verde, no saltar spec ni la puerta humana, documentar
  en `progress/current.md` mientras trabajás); (d) el flujo SDD; (e) el ritual de cierre de sesión.
- `CLAUDE.md` — Carga automática que **fuerza el rol `leader`**: en este repo el modelo siempre
  orquesta y nunca edita código de la app ni tests directamente; para cualquier tarea de código
  lanza el subagente apropiado (`spec_author`, `implementer`, `reviewer`); nunca marca `done` solo;
  nunca salta la puerta de aprobación humana. Incluí la excepción: preguntas de lectura/exploración
  y cambios fuera de código (docs, `progress/`, configuración) los puede hacer el propio leader.
- `feature_list.json` — El alcance. Estructura: `project`, `description`, un objeto `rules`
  (`one_feature_at_a_time: true`, `require_tests_to_close: true`,
  `require_approved_spec_to_implement: true`, `valid_status`, `sdd_required_when`), y un arreglo
  `features` donde cada feature tiene `id`, `name`, `title`, `description`, `acceptance` (lista de
  criterios verificables), opcional `"sdd": true`, y `status` (uno de
  `pending | spec_ready | in_progress | done | blocked`). Dejá 1–2 features de ejemplo en `pending`
  con `"sdd": true` para que yo pueda probar el flujo.
- `CHECKPOINTS.md` — Criterios objetivos de "estado final correcto" que un revisor (humano o IA)
  recorre marcando `[x]`/`[ ]`. Generalizá los del arnés de referencia y **quitá todo lo
  específico de Python**: el arnés está completo (existen los archivos base), el estado es coherente
  (≤1 `in_progress`, toda `done` con tests que pasan, `progress/current.md` limpio), el código
  respeta la arquitectura declarada, la verificación es real (hay tests por módulo, `./init.sh`
  termina verde), la sesión cerró bien, y el bloque SDD (toda feature `sdd` no-`pending` tiene su
  carpeta `specs/<name>/` con los 3 archivos, requirements en EARS, todas las tasks `[x]`, cada
  `R<n>` cubierto por un test).
- `init.sh` — Script de verificación e inicialización. Hacelo **parametrizable**: arriba, un bloque
  de configuración con los comandos detectados:
  ```bash
  # === Configuración del arnés (rellenada en la instalación) ===
  TEST_CMD="..."        # obligatorio
  LINT_CMD="..."        # opcional (vacío = se omite)
  TYPECHECK_CMD="..."   # opcional
  BUILD_CMD="..."       # opcional
  ```
  El script debe, en orden y con salidas claras `[OK]`/`[WARN]`/`[FAIL]` y exit code correcto:
  (1) verificar que el toolchain del proyecto está disponible; (2) verificar que existen los
  archivos base del arnés (`AGENTS.md`, `feature_list.json`, `progress/current.md`, los `docs/`,
  `CHECKPOINTS.md`); (3) **validar los invariantes de `feature_list.json`** (JSON válido, estados
  válidos, **máximo un** `in_progress`, y que toda feature `sdd` en estado `spec_ready`/`in_progress`/
  `done` tenga `specs/<name>/{requirements,design,tasks}.md`); (4) correr `TYPECHECK_CMD`, `LINT_CMD`,
  `TEST_CMD` y `BUILD_CMD` que no estén vacíos, fallando si alguno falla; (5) un resumen final.
  Para la validación del paso (3), **escribí la lógica en el runtime que el proyecto ya tenga**
  (Node si es JS/TS, Python si es Python, o `jq` si está disponible) — **no asumás un runtime que el
  stack no use**. Marcalo ejecutable (`chmod +x init.sh`).

**`docs/`**

- `docs/specs.md` — El proceso SDD. Esto es **universal**, reproducilo casi tal cual: la estructura
  `specs/<feature>/{requirements.md, design.md, tasks.md}`; la tabla de estados; la **puerta de
  aprobación humana** (`pending → [spec_author] → spec_ready → ⏸ HUMANO → in_progress →
  [implementer → reviewer] → done`); y la especificación de **EARS estricto** con sus cinco patrones:

  | Patrón | Plantilla |
  |---|---|
  | Ubicuo | `El sistema DEBE <acción>.` |
  | Evento | `CUANDO <disparador>, el sistema DEBE <acción>.` |
  | Estado | `MIENTRAS <estado>, el sistema DEBE <acción>.` |
  | Opcional | `DONDE <feature opcional>, el sistema DEBE <acción>.` |
  | No deseado | `SI <evento no deseado> ENTONCES el sistema DEBE <acción>.` |

  Reglas EARS: cada requisito con id estable `R1`, `R2`, …; un solo `DEBE` por requisito (si hay más,
  partilo); nada de verbos blandos ("podría", "soporta"), solo `DEBE`/`NO DEBE`; cada `R<n>` debe ser
  verificable por un test concreto. Definí también la regla de trazabilidad (cada test mapea a un
  `R<n>` y viceversa) y aclarando que SDD solo aplica a features con `"sdd": true`.
- `docs/architecture.md` — "Qué significa hacer un buen trabajo" en *este* proyecto. Generalo
  **leyendo el código existente** (si lo hay) o desde lo que yo declare (si es greenfield):
  registrá las **capas/carpetas reales** y sus responsabilidades, las **rutas de código y tests**
  detectadas en la FASE 0, la política de dependencias, el manejo de errores esperado, y una sección
  "qué NO hacer". Todos los agentes referencian este archivo en vez de rutas fijas.
- `docs/conventions.md` — Estilo, nombres y estructura del proyecto. **Derivá las reglas del código
  ya presente** (formato, convención de nombres, organización de imports, manejo de errores, política
  de comentarios) o de mis preferencias si no hay código. No copies reglas de Python si el proyecto
  no es Python. Principio guía: homogeneidad extrema (la IA predice mejor cuando el repo se parece a
  sí mismo en todas partes).
- `docs/verification.md` — Cómo demostrar que el trabajo funciona, con niveles adaptados al stack:
  Nivel 1 tests unitarios (obligatorio), Nivel 2 test de integración/E2E para features de UI
  (obligatorio para features que tocan la interfaz), Nivel 3 smoke test manual (opcional), Nivel 4
  trazabilidad `R<n> → test` (obligatorio para features `sdd`). Listá los comandos reales del
  proyecto. Incluí anti-patrones ("lo añadí, debería funcionar" = falta test ejecutable; test que
  solo verifica que no lanza excepción; marcar `done` sin `./init.sh` verde).

**`.claude/agents/` — los cuatro subagentes** (Markdown con frontmatter YAML `name`/`description`/
`tools`; el cuerpo es el system prompt). Reproducí estos contratos **sin referencias a un lenguaje
concreto** (hablá de "código de la aplicación" y "tests", y de las rutas declaradas en
`docs/architecture.md`):

- `leader.md` (`tools: Read, Glob, Grep, Bash, Agent`) — Orquestador. Protocolo de arranque (lee
  `AGENTS.md`, `feature_list.json`, `progress/current.md`; corre `./init.sh`). Maneja el flujo SDD
  por casos según el status de la primera feature no-`done`/no-`blocked`: si `pending` → lanza
  `spec_author` y **para** pidiendo aprobación humana; si `spec_ready` con aprobación → pasa a
  `in_progress` y lanza `implementer`, luego `reviewer`; si `spec_ready` sin aprobación → recuerda al
  humano que le toca; si `in_progress` → pregunta si reanuda o aborta. Regla anti-teléfono: instruye
  a los subagentes a escribir en archivos y solo acepta referencias. Tabla de escalado de esfuerzo
  (trivial → 1 spec_author → ⏸ → 1 implementer; medio → +reviewer; refactor → +2-3 exploradores en
  paralelo). Prohibiciones: editar código, marcar `done`, saltar la puerta humana.
- `spec_author.md` (`tools: Read, Write, Edit, Glob, Grep, Bash`) — Redacta los 3 archivos de
  `specs/<name>/` para **una** feature `pending` con `"sdd": true`: requirements en EARS (cada
  criterio de `acceptance` cubierto por ≥1 `R<n>`), design (archivos a tocar, firmas nuevas,
  excepciones, **al menos una alternativa descartada con justificación**) y tasks (checklist `[ ]`,
  cada task referencia los `R<n>` que cubre). Cambia el status a `spec_ready` y **para**. Nunca toca
  código ni tests; si el `acceptance` es insuficiente, marca `blocked` y pide clarificación. Salida:
  una sola línea (`spec_ready -> specs/<name>/` o `blocked -> progress/spec_<name>.md`).
- `implementer.md` (`tools: Read, Write, Edit, Glob, Grep, Bash`) — Implementa **una** feature ya
  en `in_progress` siguiendo su spec aprobado. Pre-condiciones: la feature está `in_progress` y
  existen los 3 archivos del spec (si no, para). Ejecuta cada task `T<n>` en orden marcándola `[x]`,
  escribe el test junto con cada cambio, corre `./init.sh` hasta verde, documenta el mapa
  `R<n> → test` en `progress/impl_<name>.md`. No se autoaprueba ni marca `done`. Si una task exige
  desviarse del spec, para y pide cambios al spec (no inventa requisitos). Salida: una sola línea
  (`done -> progress/impl_<name>.md` o `blocked -> ...`).
- `reviewer.md` (`tools: Read, Glob, Grep, Bash`) — Aprueba o rechaza; **no edita código**. Verifica
  trazabilidad (cada `R<n>` cubierto por un test concreto, si falta → rechaza), tasks completas
  (toda `[x]` salvo justificación documentada), conformidad con `docs/architecture.md` y
  `docs/conventions.md`, corre `./init.sh` (verde obligatorio) y recorre `CHECKPOINTS.md`. Escribe el
  veredicto en `progress/review_<name>.md` (`APPROVED`/`CHANGES_REQUESTED` + detalle citando archivos
  y líneas). Salida: una sola línea.

**`.claude/settings.json`** — Hooks que **el arnés ejecuta, no el agente**, así no se pueden saltar.
Configurá: un hook `PostToolUse` con matcher `Edit|Write` que corra `TEST_CMD` (modo silencioso, y
mostrá solo el resumen) tras editar/escribir; y un hook `Stop` que corra `./init.sh` antes de cerrar
sesión y avise si falló. Usá los comandos detectados, no comandos de Python. Agregá una sección
`permissions.allow` con los comandos del proyecto que el agente puede correr sin pedir permiso
(`./init.sh`, `TEST_CMD`, `RUN_CMD`, etc.).

**`progress/`**

- `progress/current.md` — Plantilla vacía del estado de la sesión activa (feature en curso, plan,
  notas, bloqueos). Se llena *mientras* se trabaja, no al final.
- `progress/history.md` — Bitácora append-only; se inicia vacía o con un encabezado.

### Reglas de generalización (críticas — leelas antes de escribir)

- **Cero suposiciones de lenguaje.** Nada en `init.sh`, los hooks, `conventions.md`, `architecture.md`
  ni los agentes debe asumir un lenguaje, framework o herramienta que no detectaste/confirmaste.
- **Rutas por declaración, no fijas.** No hardcodees `src/` ni `tests/`. Las rutas reales de código y
  tests van en `docs/architecture.md` y todos los agentes las referencian desde ahí.
- **Comandos por configuración.** Todo lo ejecutable sale del bloque de configuración de `init.sh`
  (`TEST_CMD`, etc.). Si mañana cambia el test runner, se edita en un solo lugar.
- **EARS y el patrón de 4 agentes son universales:** se reproducen casi idénticos (solo se limpia el
  vocabulario específico de lenguaje).
- **Greenfield vs brownfield.** Si ya hay código, derivá convenciones y arquitectura *de él*. Si está
  vacío, derivalas de lo que yo declare y dejá `architecture.md`/`conventions.md` como punto de
  partida explícito, no como reglas inventadas.

### FASE 2 — Autoverificación y cierre

1. Ejecutá `./init.sh` y mostrame la salida. Debe terminar **verde** (o, si el repo está vacío y aún
   no hay tests, con `[WARN]` claros y exit code 0, nunca con `[FAIL]` silenciosos).
2. Dame un resumen corto de qué creaste y **cómo arranco el flujo**: que deje una feature en
   `pending` con `"sdd": true` y te pida *"implementá la siguiente feature pendiente"*; que esperás a
   `spec_ready`, leo los 3 archivos del spec, y respondo *"aprobado"* para que sigás.
3. No marqués ninguna feature como `done` en esta instalación. El arnés queda listo, vacío de trabajo.

### Qué NO hacer

- ❌ No implementés ninguna feature de la app durante la instalación (solo el arnés).
- ❌ No copies reglas, comandos ni rutas del proyecto de ejemplo en Python.
- ❌ No dejes `init.sh` o los hooks con comandos que el stack no tenga.
- ❌ No avances de fase sin la confirmación que se pide en cada una.

## ────────────────────────── FIN DEL PROMPT ──────────────────────────

---

### Nota práctica (para vos, no para el agente)

- Es **un arnés casero**, no un producto: lo bueno es que es 100% tuyo, transparente y editable; lo
  malo es que vos lo mantenés. Si querés algo "de fábrica" con comunidad detrás, mirá la comparación
  que te pasé (GitHub Spec Kit, Kiro, OpenSpec).
- El patrón multi-agente cuesta tokens (varias veces más que un chat normal). Para cambios chicos
  —un fix de cinco líneas— el arnés completo es un mazo para una nuez: dejá esa feature **sin**
  `"sdd": true` y que el implementer la haga directo, o resolvelo a mano.
- Si trabajás mucho en VS Code, el hook `Stop` que corre `./init.sh` antes de cerrar es lo que más
  te va a salvar de cerrar con algo roto.
