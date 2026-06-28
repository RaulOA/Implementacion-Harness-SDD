# Prompt v2 — Instalar arnés "Harness Engineering" estándar

> **Cómo usarlo:** abrí Claude Code en VS Code (o en la terminal integrada de Visual Studio), en la
> **raíz** del repositorio, y pegá *todo* el bloque entre `INICIO`/`FIN` como primer mensaje.
>
> **Principio rector — entrada variable, salida estándar.** El prompt NO asume nada de tu proyecto:
> el agente **investiga primero** lo que tenés enfrente y **después** moldea el mismo esqueleto de
> arnés en todos lados. La **estructura es invariante**; el **contenido se deriva** del proyecto.
>
> Leyenda usada abajo: 🔒 = invariante (idéntico en todo proyecto) · 🔎 = derivado de la investigación.

---

## ───────────────────────── INICIO DEL PROMPT ─────────────────────────

### Rol y contrato

Tu tarea es **instalar un arnés de "Harness Engineering" estándar** en este repositorio. Producís
**la misma estructura en todos los proyectos** (mismos nombres de archivo, mismos contratos de
agente, mismo proceso, mismo esquema de base de datos), pero el **contenido** lo derivás
investigando *este* proyecto concreto.

Reglas de oro:

- **Cero suposiciones.** No asumas lenguaje, framework, gestor de paquetes, layout de carpetas ni
  herramientas. Ningún proyecto es igual a otro. **Investigá lo que hay antes de crear nada.**
- **Invariante vs derivado.** Lo marcado 🔒 se reproduce idéntico en cualquier proyecto. Lo marcado
  🔎 sale de tu investigación de este repo.
- **No empecés la FASE 1 hasta cerrar la FASE 0 y tener mi confirmación.**

### Principios que el arnés DEBE encarnar 🔒

1. **El repositorio ES el sistema.** Todo el estado (alcance, specs, progreso, calidad) vive fuera
   del chat: parte en archivos versionados, parte en la base de datos del arnés.
2. **Divulgación progresiva.** `AGENTS.md` es un mapa, no una biblia: el agente lee bajo demanda.
3. **Una feature a la vez.** Como mucho una `in_progress`, validado por script.
4. **Spec Driven Development con puerta humana.** Feature marcada con SDD pasa por
   `requirements (EARS) → design → tasks` y **se detiene** a esperar aprobación humana antes de código.
5. **Estado persistente, no en chat.** Sobrevive a reinicios y a ventanas de contexto reventadas.
6. **Verificación ejecutable.** El agente no dice "funciona": lo demuestra corriendo el script de
   verificación, que ejecuta los tests/lint/build reales del proyecto.
7. **Trazabilidad obligatoria.** Cada `R<n>` se mapea a ≥1 test; el revisor rechaza si falta.
8. **Leader → Spec → Implementer → Reviewer + anti-teléfono-descompuesto.** El leader orquesta y no
   codifica; el spec_author especifica y no codifica; el implementer codifica una feature; el
   reviewer aprueba/rechaza y no edita. Los subagentes **persisten su resultado** (en disco o DB) y
   solo devuelven una referencia ligera, nunca el contenido por chat.

### FASE 0 — INVESTIGAR (abierta, sin supuestos, sobre ESTE repo) 🔎

No hay checklist de archivos. La investigación la hacés vos, sobre el proyecto real, con tu criterio.

**0.1 — Stack, layout y tooling.** Explorá el repositorio y determiná, *para este proyecto*:
- Lenguaje(s), framework(s) y gestor de paquetes reales (mirá los manifiestos y el código que
  efectivamente hay, no los que "deberían" estar).
- **Dónde vive el código** y **dónde viven los tests** (las rutas reales, sean cuales sean).
- Los comandos reales de: `TEST`, `LINT`, `TYPECHECK`, `BUILD` y `RUN` (dejá vacío el que no exista).

**0.2 — Inventario de context-engineering previo, por intención (no por nombre).** Este repositorio
puede tener —o no— estructuras de ingeniería de contexto aplicadas por agentes o herramientas
anteriores. **Varían muchísimo de proyecto a proyecto y pueden usar nombres y formas que no podés
predecir**, así que investigá este repo *en sus propios términos*. Buscá cualquier archivo o carpeta
cuya **función** sea alguna de estas, sin importar cómo se llame:
- instruir a una IA o agente sobre cómo trabajar en el repo,
- codificar convenciones, estándares o estilo de código,
- registrar decisiones de arquitectura o diseño,
- preservar memoria, contexto o progreso entre sesiones,
- definir roles, personas, prompts o modos de un agente.

Reglas para este inventario: **no asumas que existe ningún artefacto en particular**; **no fuerces**
nada a encajar; la **ausencia total es un resultado válido y normal**; y prestá atención también a
estructuras idiosincráticas o caseras que no se parezcan a ningún patrón conocido.

**0.3 — Clasificar y proponer mapeo.** Para cada cosa encontrada, clasificá:
- **Reutilizable** → qué pieza del arnés estándar la va a absorber (su contenido se migra).
- **Histórico** → se preserva tal cual como archivo histórico (no se reutiliza pero no se pierde).

**0.4 — Confirmar.** Mostrame: (a) stack + los cinco comandos + rutas de código/tests detectados;
(b) el inventario de context-engineering previo con su clasificación y mapeo propuesto; (c) la cadena
de conexión / acceso a la base de datos que voy a usar. **Pedime confirmación o corrección.** Si algo
es ambiguo o el repo está vacío, hacé preguntas cortas. No avances sin mi visto bueno.

### FASE 1 — APLICAR EL ARNÉS ESTÁNDAR

Reproducí **siempre la misma estructura**. Lo 🔒 es idéntico en todo proyecto; rellená lo 🔎 con la
FASE 0.

#### Regla de almacenamiento 🔒

- **Archivos** = sistema de registro de la **definición del arnés** y del **contrato vigente** (se
  lee mucho, es estable, va en git y se revisa en el PR).
- **Base de datos** (del arnés) = sistema de registro de lo **histórico y los logs** (se escribe
  mucho, se relee poco, crece sin tope). Esto reduce cantidad de archivos, tamaño del repo **y**
  tokens (el agente consulta filas acotadas en vez de releer archivos que crecen).

#### Acceso a la base de datos 🔒

- Usá una base **dedicada y aislada** para el arnés (p. ej. `ClaudeHarness`). **NO uses la cuenta
  `sa` ni un login con rol de servidor.** Pedí/usá un login con `db_owner` **solo sobre esa base**:
  el agente es dueño total de su propio almacenamiento, sin poder tocar nada más de la instancia.
  Si solo tengo `sa`, creá ese login acotado vos mismo y usá ese de ahí en adelante.
- **Conexión:** preferí un **MCP server de MSSQL** (herramientas tipadas, con confirmación por
  permiso antes de escribir). Si no hay MCP configurado, **caé automáticamente a `sqlcmd`** por la
  shell. Documentá cuál quedó activo en `docs/architecture.md`.
- **Esquema:** usá **exactamente** el DDL de la sección «DDL de la base de datos» de abajo. No lo
  interpretes ni lo improvises; es idéntico en todo proyecto.

#### DDL de la base de datos 🔒

Ejecutá este script tal cual contra la base del arnés (SSMS / Azure Data Studio / `sqlcmd`; si tu MCP
no soporta `GO`, quitá los `GO` y corré cada `CREATE` por separado). Es **idempotente** e **idéntico
en todos los proyectos**.

```sql
/* === Setup único (como admin): base aislada + login acotado (db_owner SOLO aquí, sin rol de servidor) === */
IF DB_ID(N'ClaudeHarness') IS NULL
    CREATE DATABASE ClaudeHarness;
GO
USE ClaudeHarness;
GO
IF SUSER_ID(N'claude_harness') IS NULL
    CREATE LOGIN claude_harness WITH PASSWORD = N'<contraseña-fuerte-aquí>';
GO
IF DATABASE_PRINCIPAL_ID(N'claude_harness') IS NULL
    CREATE USER claude_harness FOR LOGIN claude_harness;
GO
ALTER ROLE db_owner ADD MEMBER claude_harness;
GO

/* === Tablas (las corre el login del arnés) === */

IF OBJECT_ID(N'dbo.harness_sessions', N'U') IS NULL
CREATE TABLE dbo.harness_sessions (
    session_id   BIGINT        IDENTITY(1,1) NOT NULL CONSTRAINT PK_harness_sessions PRIMARY KEY,
    feature_name NVARCHAR(200) NULL,
    plan         NVARCHAR(MAX) NULL,
    status       VARCHAR(20)   NOT NULL CONSTRAINT DF_harness_sessions_status DEFAULT ('active'),
    notes        NVARCHAR(MAX) NULL,
    started_at   DATETIME2(3)  NOT NULL CONSTRAINT DF_harness_sessions_started DEFAULT (SYSUTCDATETIME()),
    ended_at     DATETIME2(3)  NULL,
    CONSTRAINT CK_harness_sessions_status CHECK (status IN ('active','closed','blocked')),
    INDEX IX_harness_sessions_status  NONCLUSTERED (status),
    INDEX IX_harness_sessions_feature NONCLUSTERED (feature_name)
);
GO

IF OBJECT_ID(N'dbo.harness_agent_reports', N'U') IS NULL
CREATE TABLE dbo.harness_agent_reports (
    report_id    BIGINT        IDENTITY(1,1) NOT NULL CONSTRAINT PK_harness_agent_reports PRIMARY KEY,
    session_id   BIGINT        NULL,
    feature_name NVARCHAR(200) NOT NULL,
    report_type  VARCHAR(20)   NOT NULL,
    verdict      VARCHAR(20)   NULL,
    content      NVARCHAR(MAX) NOT NULL,
    created_at   DATETIME2(3)  NOT NULL CONSTRAINT DF_harness_reports_created DEFAULT (SYSUTCDATETIME()),
    CONSTRAINT CK_harness_reports_type    CHECK (report_type IN ('spec','implementation','review')),
    CONSTRAINT CK_harness_reports_verdict CHECK (verdict IS NULL OR verdict IN ('approved','changes_requested')),
    CONSTRAINT FK_harness_reports_session FOREIGN KEY (session_id) REFERENCES dbo.harness_sessions (session_id),
    INDEX IX_harness_reports_feature NONCLUSTERED (feature_name, report_type),
    INDEX IX_harness_reports_session NONCLUSTERED (session_id)
);
GO

IF OBJECT_ID(N'dbo.harness_feature_history', N'U') IS NULL
CREATE TABLE dbo.harness_feature_history (
    feature_id       BIGINT        IDENTITY(1,1) NOT NULL CONSTRAINT PK_harness_feature_history PRIMARY KEY,
    feature_name     NVARCHAR(200) NOT NULL CONSTRAINT UQ_harness_feature_history_name UNIQUE,
    feature_snapshot NVARCHAR(MAX) NULL,
    status           VARCHAR(20)   NOT NULL,
    approved_by      NVARCHAR(200) NULL,
    approved_at      DATETIME2(3)  NULL,
    created_at       DATETIME2(3)  NOT NULL CONSTRAINT DF_harness_feathist_created DEFAULT (SYSUTCDATETIME()),
    closed_at        DATETIME2(3)  NULL,
    CONSTRAINT CK_harness_feathist_status CHECK (status IN ('approved','in_progress','done','blocked')),
    INDEX IX_harness_feathist_status NONCLUSTERED (status)
);
GO

IF OBJECT_ID(N'dbo.harness_spec_archive', N'U') IS NULL
CREATE TABLE dbo.harness_spec_archive (
    spec_id         BIGINT        IDENTITY(1,1) NOT NULL CONSTRAINT PK_harness_spec_archive PRIMARY KEY,
    feature_name    NVARCHAR(200) NOT NULL,
    requirements_md NVARCHAR(MAX) NULL,
    design_md       NVARCHAR(MAX) NULL,
    tasks_md        NVARCHAR(MAX) NULL,
    archived_at     DATETIME2(3)  NOT NULL CONSTRAINT DF_harness_specarch_archived DEFAULT (SYSUTCDATETIME()),
    INDEX IX_harness_specarch_feature NONCLUSTERED (feature_name)
);
GO

IF OBJECT_ID(N'dbo.harness_legacy_context', N'U') IS NULL
CREATE TABLE dbo.harness_legacy_context (
    legacy_id      BIGINT        IDENTITY(1,1) NOT NULL CONSTRAINT PK_harness_legacy_context PRIMARY KEY,
    original_path  NVARCHAR(400) NOT NULL,
    classification VARCHAR(20)   NOT NULL,
    migrated_to    NVARCHAR(400) NULL,
    content        NVARCHAR(MAX) NOT NULL,
    archived_at    DATETIME2(3)  NOT NULL CONSTRAINT DF_harness_legacy_archived DEFAULT (SYSUTCDATETIME()),
    CONSTRAINT CK_harness_legacy_class CHECK (classification IN ('reusable','historical')),
    INDEX IX_harness_legacy_path NONCLUSTERED (original_path)
);
GO
```

**Cómo las usa el agente** 🔒:
- `harness_sessions` — abre una fila `active` al empezar y la cierra (`closed`/`blocked` + `ended_at`)
  al terminar; el campo `plan` reemplaza al viejo `progress/current.md`.
- `harness_agent_reports` — cada subagente **persiste aquí** su salida (spec / implementación con el
  mapa `R<n> → test` / review con `verdict`) y solo devuelve una referencia al leader (anti-teléfono).
- `harness_feature_history` — **una fila por feature**; el leader la crea al aprobar (`approved_by`/
  `approved_at` los escribe **solo** por acción humana), la actualiza en su ciclo y guarda
  `feature_snapshot` al cerrar. **Compuerta de aprobación** que consulta `init.ps1`/leader:
  `SELECT approved_at FROM dbo.harness_feature_history WHERE feature_name = @f` → si es `NULL`, no se
  permite `in_progress`.
- `harness_spec_archive` — al cerrar una feature `sdd` se archivan aquí sus 3 specs y se borran de `specs/`.
- `harness_legacy_context` — destino verbatim de lo viejo no reutilizable (con `classification` y,
  si aplica, `migrated_to`).

#### Archivos a crear (esqueleto estándar)

**Raíz** 🔒 *nombres* / 🔎 *contenido*
- `AGENTS.md` — Mapa de navegación (divulgación progresiva): pasos al empezar (correr el script de
  verificación, leer el tablero y el estado de sesión), tabla de "qué es cada cosa y cuándo leerla",
  reglas duras, flujo SDD, ritual de cierre. Documentá que el progreso e historial viven en la DB.
- `CLAUDE.md` — Carga automática que **fuerza el rol `leader`**: el modelo siempre orquesta y nunca
  edita código de la app ni tests directamente; para tareas de código lanza el subagente apropiado;
  nunca marca `done` solo; nunca salta la puerta humana. Excepción: lectura/exploración y cambios
  fuera de código (docs, configuración, registros en DB) los hace el propio leader.
- `feature_list.json` — Tablero vivo: `project`, `description`, `rules`
  (`one_feature_at_a_time`, `require_tests_to_close`, `require_approved_spec_to_implement`,
  `valid_status: [pending, spec_ready, in_progress, done, blocked]`, `sdd_required_when`), y
  `features[]` (`id`, `name`, `title`, `description`, `acceptance[]`, opcional `"sdd": true`,
  `status`). **Al pasar a `done`, archivá la fila a `harness_feature_history` y podala del JSON** para
  que el tablero quede siempre liviano. Dejá 1–2 features de ejemplo en `pending` con `"sdd": true`.
- `CHECKPOINTS.md` 🔒 — Criterios objetivos de "estado final correcto" que el revisor recorre marcando
  `[x]`/`[ ]`: arnés completo, estado coherente (≤1 `in_progress`), código respeta la arquitectura
  declarada, verificación real (script verde, tests por módulo), sesión cerrada bien, y bloque SDD
  (toda feature `sdd` no-`pending` con sus 3 specs, requirements en EARS, tasks `[x]`, cada `R<n>`
  cubierto por un test). Sin nada atado a un lenguaje concreto.
- `init.ps1` 🔒 *forma* / 🔎 *comandos* — Script de verificación e inicialización en **PowerShell**.
  Arriba, bloque de configuración con los comandos detectados:
  ```powershell
  # === Configuración del arnés (rellenada en la instalación) ===
  $TestCmd = "..."        # obligatorio (p. ej. dotnet test)
  $LintCmd = "..."        # opcional (vacío = se omite)
  $TypecheckCmd = "..."   # opcional (p. ej. tsc --noEmit / ng build)
  $BuildCmd = "..."       # opcional (p. ej. dotnet build)
  ```
  El script, **en orden fail-fast (corre primero lo barato y corta al primer fallo)** con salidas
  `[OK]`/`[WARN]`/`[FAIL]` y exit code correcto:
  (1) verifica el toolchain disponible; (2) verifica que existen los archivos base del arnés;
  (3) verifica conectividad con la DB del arnés; (4) valida los invariantes del tablero (JSON válido,
  estados válidos, **máximo un** `in_progress`, y que toda feature `sdd` en `spec_ready`/`in_progress`/
  `done` tenga sus 3 specs); **(5) compuerta de aprobación**: rechaza pasar una feature `sdd` a
  `in_progress` si no tiene `approved_by`/`approved_at` en la DB; (6) corre `TypecheckCmd`, `LintCmd`,
  `BuildCmd` y `TestCmd` no vacíos (lo caro al final); (7) resumen.

**`docs/`** 🔎 *contenido salvo specs.md*
- `docs/specs.md` 🔒 — El proceso SDD, **universal, casi literal**: estructura
  `specs/<feature>/{requirements,design,tasks}.md`; tabla de estados; la **puerta de aprobación
  humana** (`pending → [spec_author] → spec_ready → ⏸ HUMANO → in_progress → [implementer → reviewer]
  → done`); y **EARS estricto** con sus cinco patrones:

  | Patrón | Plantilla |
  |---|---|
  | Ubicuo | `El sistema DEBE <acción>.` |
  | Evento | `CUANDO <disparador>, el sistema DEBE <acción>.` |
  | Estado | `MIENTRAS <estado>, el sistema DEBE <acción>.` |
  | Opcional | `DONDE <feature opcional>, el sistema DEBE <acción>.` |
  | No deseado | `SI <evento no deseado> ENTONCES el sistema DEBE <acción>.` |

  Reglas EARS: id estable `R1`, `R2`…; un solo `DEBE` por requisito; nada de verbos blandos, solo
  `DEBE`/`NO DEBE`; cada `R<n>` verificable por un test concreto; trazabilidad `R<n> ↔ test`
  obligatoria. **Compuerta de complejidad:** solo features multi-archivo, riesgosas o que tocan la
  interfaz llevan `"sdd": true`; un cambio trivial o de un archivo va **sin** SDD (implementer directo)
  para no usar un mazo en una nuez.
- `docs/architecture.md` 🔎 — "Qué es hacer un buen trabajo" en *este* proyecto. Generalo **leyendo
  el código existente** (o desde lo que yo declare si es greenfield): capas/carpetas reales y sus
  responsabilidades, las **rutas de código y tests** de la FASE 0, política de dependencias, manejo de
  errores esperado, el medio de acceso a la DB que quedó activo, y un "qué NO hacer". Todos los
  agentes referencian este archivo en vez de rutas fijas.
- `docs/conventions.md` 🔎 — Estilo, nombres y estructura **derivados del código ya presente** (o de
  mis preferencias). No copies reglas de un lenguaje que el proyecto no usa. Guía: homogeneidad
  extrema (la IA predice mejor cuando el repo se parece a sí mismo).
- `docs/verification.md` 🔎 — Cómo demostrar que funciona, con niveles adaptados al stack: unitarios
  (obligatorio), integración/E2E para features de UI (obligatorio si tocan interfaz), smoke manual
  (opcional), trazabilidad `R<n> → test` (obligatorio para `sdd`). Listá los comandos reales del
  proyecto y los anti-patrones.

#### BLOQUE OPCIONAL — `docs/constitution.md` (principios inmutables, estilo Spec Kit) 🔒 *estructura* / 🔎 *sección del proyecto*

Incluido por defecto (para que todo proyecto quede igual); decime si NO lo querés en alguno. Es el
**contrato de principios inmutables** que gobierna TODO cambio, por encima de la conveniencia. Es
**corto a propósito** — una constitución larga es un anti-patrón.

Estructura (igual en todo proyecto):
- **Encabezado** con `Versión: 1.0.0` y fecha.
- **Núcleo invariante** 🔒 — principios que aplican a cualquier proyecto del arnés, como `DEBE`/`NO
  DEBE` + una línea de justificación. Mínimo:
  1. Ninguna feature es `done` sin verificación verde (`init.ps1`) y trazabilidad `R<n> → test` completa.
  2. La spec es el contrato: no se escribe código de una feature `sdd` sin spec aprobado por un humano.
  3. Una feature a la vez; el estado vive fuera del chat (archivos + DB), nunca solo en la conversación.
  4. Ante un fallo de herramienta el agente NO improvisa workarounds: para, marca `blocked` y documenta.
- **Sección del proyecto** 🔎 — 3 a 6 principios propios de ESTE proyecto, que proponés desde la FASE 0
  (calidad de código, estándares de testing, seguridad y manejo de datos sensibles, compatibilidad
  hacia atrás, accesibilidad, rendimiento — lo que aplique al stack y dominio detectados). Cada uno
  `DEBE`/`NO DEBE` + justificación de una línea.
- **Regla de enmienda** 🔒 — cambiar, agregar o quitar un principio requiere acción explícita del
  humano y un bump de versión (semver). El agente NUNCA enmienda la constitución por su cuenta.

Cableado al activarlo:
- **spec_author** e **implementer** leen `docs/constitution.md` antes de trabajar; ninguna decisión de
  diseño o implementación puede violar un principio.
- **reviewer** suma a su protocolo: verificar el cambio contra cada principio; si lo viola, **rechaza**
  (es correctitud, no estilo).
- **CHECKPOINTS.md** suma un ítem: existe `docs/constitution.md` con núcleo + sección de proyecto, y el
  cambio revisado no viola ningún principio.
- **init.ps1** suma la verificación de existencia de `docs/constitution.md`.
- En la **FASE 0.4**, presentame el borrador de la sección de proyecto para que lo confirme antes de
  fijarlo (la constitución es contrato: necesita mi firma).

**`.claude/agents/` — los cuatro subagentes** 🔒 *contratos* / 🔎 *comandos y rutas referenciados*.
Markdown con frontmatter YAML (`name`, `description`, `tools`, **`model`**); el cuerpo es el system
prompt. Hablá de "código de la aplicación" y "tests" y de las rutas declaradas en
`docs/architecture.md`, **sin** referencias a un lenguaje concreto. **Ruteo de modelo (clave en
suscripción):** `model: opus` en `leader` y `spec_author`; `model: sonnet` en `implementer` y
`reviewer`.

- `leader.md` (`tools: Read, Glob, Grep, Bash, Agent` · `model: opus`) — Orquestador. Arranque: lee
  `AGENTS.md`, el tablero, el estado de sesión; corre `init.ps1`. Flujo SDD por casos según el status
  de la primera feature no-`done`/no-`blocked`: `pending` → lanza `spec_author` y **para** pidiendo
  aprobación humana; `spec_ready` con aprobación → registra `approved_by/at`, pasa a `in_progress`,
  lanza `implementer` y luego `reviewer`; `spec_ready` sin aprobación → recuerda al humano que le
  toca; `in_progress` → pregunta si reanuda o aborta. Anti-teléfono: instruye a los subagentes a
  persistir resultados (disco/DB) y solo acepta referencias. Tabla de escalado. Prohibido: editar
  código, marcar `done`, saltar la puerta humana.
- `spec_author.md` (`Read, Write, Edit, Glob, Grep, Bash` · `model: opus`) — Redacta los 3 archivos
  de `specs/<name>/` para **una** feature `pending` con `"sdd": true`: requirements EARS (cada
  `acceptance` cubierto por ≥1 `R<n>`), design (archivos a tocar, firmas, excepciones, **≥1
  alternativa descartada con justificación**), tasks (checklist `[ ]`, cada task cita sus `R<n>`).
  Marca `spec_ready` y **para**. Nunca toca código/tests; si el `acceptance` es insuficiente, marca
  `blocked`. Salida: una línea.
- `implementer.md` (`Read, Write, Edit, Glob, Grep, Bash` · `model: sonnet`) — Implementa **una**
  feature ya en `in_progress` siguiendo su spec aprobado. Ejecuta cada `T<n>` en orden marcando `[x]`,
  escribe el test junto a cada cambio, corre `init.ps1` hasta verde, persiste el mapa `R<n> → test` en
  `harness_agent_reports`. No se autoaprueba. Si una task exige desviarse del spec, para y pide
  cambios al spec. Salida: una línea.
- `reviewer.md` (`Read, Glob, Grep, Bash` · `model: sonnet`) — Aprueba o rechaza; **no edita código**.
  Verifica trazabilidad (cada `R<n>` con test; si falta → rechaza), tasks completas, conformidad con
  `docs/architecture.md` y `docs/conventions.md`, corre `init.ps1` (verde obligatorio), recorre
  `CHECKPOINTS.md`. **Marca solo brechas que afecten correctitud o un requisito; el estilo es
  opcional** (no persigas todo: dispara reciclados y sobre-ingeniería). Persiste el veredicto en
  `harness_agent_reports`. Salida: una línea.

**`.claude/settings.json`** 🔒 *forma* / 🔎 *comandos* — Hooks que **el arnés ejecuta, no el agente**:
`PostToolUse` con matcher `Edit|Write` que corre lo **barato** (`TypecheckCmd` o `LintCmd`, **no** la
suite completa) y muestra solo el resumen; `Stop` que corre `init.ps1` antes de cerrar y, si tu
versión de Claude Code lo soporta, **bloquea el cierre (exit code ≠ 0)** cuando está en rojo (si no,
deja advertencia clara). `permissions.allow` con los comandos del proyecto que el agente puede correr
sin pedir permiso.

### MIGRACIÓN del context-engineering previo (no perder NADA) 🔎

Orden: **descubrir → clasificar → migrar → archivar**, y borrar de disco **solo al final**, con
**triple respaldo**:
1. Lo **reutilizable** se levanta a los archivos estándar correspondientes (según el mapeo de la
   FASE 0).
2. Todo lo **demás** se preserva verbatim en `harness_legacy_context` (ruta original + contenido +
   fecha).
3. Recién entonces se quita del árbol activo (sigue, además, en el historial de git).

Nada de lo que construyeron agentes anteriores se elimina: o se migra, o queda archivado.

### Reglas de estandarización (releelas) 🔒

- **Mismo esqueleto en todo proyecto.** Nombres/ubicación de archivos, contratos de agente, proceso
  SDD, esquema de DB y CHECKPOINTS son idénticos siempre. Solo cambia el contenido derivado (🔎).
- **Cero rutas/nombres fijos en la fase de descubrimiento.** Buscás por intención, no por nombre de
  archivo. No asumas que algo existe.
- **Cero suposiciones de lenguaje** en `init.ps1`, hooks, `conventions.md`, `architecture.md` ni los
  agentes: todo lo ejecutable sale del bloque de configuración; todas las rutas salen de
  `architecture.md`.

### FASE 2 — Autoverificar y cerrar

1. Corré `init.ps1` y mostrame la salida. Debe terminar **verde** (o, si el repo está vacío, con
   `[WARN]` claros y exit 0).
2. Resumime qué creaste, qué migraste, qué archivaste a la DB, y **cómo arranco el flujo**: dejo una
   feature `pending` con `"sdd": true` y te pido *"implementá la siguiente feature pendiente"*; espero
   `spec_ready`, leo los 3 specs, y respondo *"aprobado"* para que sigás.
3. **No marqués ninguna feature como `done`** en esta instalación. El arnés queda listo, vacío de
   trabajo.

### Qué NO hacer

- ❌ No asumir estructura, stack ni artefactos previos: investigá primero.
- ❌ No prescribir nombres de archivo en la búsqueda de context-engineering (buscá por intención).
- ❌ No implementar ninguna feature de la app durante la instalación (solo el arnés).
- ❌ No usar `sa` ni un login de servidor para la DB; usá una base dedicada con login acotado.
- ❌ No borrar nada del context-engineering viejo sin antes migrarlo o archivarlo.
- ❌ No dejar `init.ps1` o los hooks con comandos que el stack no tenga.
- ❌ No avanzar de fase sin la confirmación que se pide.

## ────────────────────────── FIN DEL PROMPT ──────────────────────────

---

### Nota para vos (no para el agente)

- **Decisiones por defecto** que tomé (volteables): conexión por **MCP** con respaldo a `sqlcmd`;
  **podar** features `done` al DB; **`init.ps1`** como único estándar; **constitución incluida** por
  defecto (para que todo proyecto quede igual — decile al agente si no la querés en alguno).
- **Una vez, por máquina:** correr el bloque «Setup único» del DDL (poné una contraseña fuerte en el
  login `claude_harness`), instalar/configurar el MCP de MSSQL apuntando a ese login, y listo: de ahí
  en adelante el prompt corre igual en todos los repos.
- **Degradación con gracia:** si la DB está caída, el arnés sigue *definiéndose* (archivos intactos)
  y el agente avisa; solo pierde la capacidad de loguear/archivar hasta que vuelva.
