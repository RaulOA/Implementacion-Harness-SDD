# Etapa 0 / 4 — Investigación y plan

> **Contrato común (todas las etapas).** Instalás un arnés "Harness Engineering" **estándar**: misma
> estructura en todo proyecto (🔒), contenido derivado de ESTE proyecto (🔎). Reglas: **cero
> suposiciones** — investigá antes de asumir; el estado vive **fuera del chat, en archivos versionados
> del repo** (así viaja solo entre máquinas vía git); una feature a la vez; SDD con **puerta humana**;
> verificación ejecutable; trazabilidad `R<n> → test`; patrón **leader / spec_author / implementer /
> reviewer** con anti-teléfono (los subagentes escriben su resultado en archivos y devuelven solo una
> referencia). **No avances de etapa sin la confirmación que se pide al final.**

> **Frontera de decisiones (clasificá tus dudas antes de cualquier pausa).** Lo que el estándar ya
> define —todo lo marcado 🔒 y la forma del esqueleto: ubicación y nombres de archivos/carpetas, los
> cuatro roles, dónde van las specs (`specs/<feature>/`), los archivos de memoria, los hooks y los
> comandos `/`— es **invariante**: lo aplicás y, si acaso, lo **informás** ("así lo apliqué"); **nunca
> lo consultás**. Solo me consultás lo **propio de este proyecto** (🔎: stack y comandos reales, fuente
> de los `R<n>`, qué módulos entran en alcance, inconsistencias o hallazgos del repo). Prueba rápida:
> si una duda se resuelve mirando el estándar, no es pregunta —resolvela; si la respuesta cambiaría de
> un proyecto a otro, esa sí me la pasás.

## Tu único trabajo en esta etapa

**Investigar este repositorio y proponer un plan. NO crees el arnés todavía.** El único archivo que
escribís es `HARNESS-INSTALL.md` (el traspaso entre etapas). Todo lo demás es lectura.

No hay checklist de archivos a buscar: la investigación la hacés vos, sobre el proyecto real, con tu
criterio.

### 1 — Stack, layout y tooling 🔎
Explorá el repo y determiná, *para este proyecto*:
- Lenguaje(s), framework(s) y gestor de paquetes **reales** (mirá los manifiestos y el código que
  efectivamente hay).
- **Dónde vive el código** y **dónde viven los tests** (las rutas reales, sean cuales sean).
- Los comandos reales de `TEST`, `LINT`, `TYPECHECK`, `BUILD` y `RUN` (vacío el que no exista).

### 2 — Inventario de context-engineering previo, **por intención (no por nombre)** 🔎
El repo puede tener —o no— estructuras de ingeniería de contexto aplicadas por agentes o herramientas
anteriores. **Varían muchísimo y pueden usar nombres y formas que no podés predecir.** Investigá este
repo *en sus propios términos*. Buscá cualquier archivo o carpeta cuya **función** sea alguna de
estas, sin importar cómo se llame:
- instruir a una IA o agente sobre cómo trabajar en el repo,
- codificar convenciones, estándares o estilo,
- registrar decisiones de arquitectura o diseño,
- preservar memoria, contexto o progreso entre sesiones,
- definir roles, personas, prompts o modos de un agente.

Reglas: **no asumas que existe ningún artefacto en particular**; **no fuerces** nada a encajar; la
**ausencia total es válida y normal**; prestá atención también a estructuras caseras o idiosincráticas
que no se parezcan a ningún patrón conocido.

**Solo buscás context-engineering, NO código.** El **código de dominio o de la app** —incluido SQL,
scripts y migraciones, **aunque esté en carpetas `legacy`/`deprecated`/`_old`**— NO es artefacto del
arnés: el arnés **nunca lo reubica**, se deja donde está. Si dudás si algo es context-engineering o
código de dominio, **flagueálo al humano**; no lo marques archivable por las dudas.

### 3 — Clasificar y proponer mapeo 🔎
Para cada cosa encontrada: **reutilizable** (qué archivo del arnés estándar la absorberá) o
**histórico** (se preserva verbatim en `archive/legacy/`, no se reutiliza pero no se pierde).

Además, marcá aparte las **colisiones**: archivos que ya existen **en rutas que el estándar va a crear o
sobrescribir** (p. ej. un `CLAUDE.md`, `AGENTS.md`, `.claude/settings.json`, una config de MCP o algo en
`docs/` previos). La Etapa 1 los va a tocar, así que hay que preservarlos antes; anotalos en el traspaso
distinguiendo **dos tipos**: archivos de **configuración** (settings.json, MCP) — que se **fusionan**,
conservando lo que el proyecto ya tenía (reglas propias, `env`, hooks, servidores MCP) — y archivos de
**instrucción/prosa** (CLAUDE.md, AGENTS.md, docs) — que se reescriben destilando lo útil del original.

### 4 — Escribir el traspaso
Creá `HARNESS-INSTALL.md` en la raíz con: stack + los 5 comandos + rutas de código/tests; el inventario
con su clasificación y mapeo propuesto. Dejá una sección "Progreso de etapas" con casillas:
`[ ] Etapa 0` … `[ ] Etapa 3`, y marcá la 0 al terminar.

Incluí además la **configuración del chat que el proyecto necesita** (🔎, para que la Etapa 1 la deje
fija de una vez y no haya que tocarla nunca más). La idea es **que fluya, frenando solo lo crítico**: qué
comandos **permitir** (generoso — los de rutina: build, test, lint, lecturas de git, el toolchain — para
no inundar de solicitudes), qué **negar** (lo prohibido: rutas de secretos/credenciales que encontraste
—cadenas de conexión, `.env`, `appsettings` sensibles— y comandos destructivos), y qué dejar tras
**preguntar** (solo lo de impacto real: escrituras por MCP a sistemas externos, deploys, operaciones
destructivas sobre datos reales — nada menor); si el proyecto **realmente depende** de algún MCP server o
plugin (nombralo; si no, ninguno); y el **idioma/tono** de trabajo. El *set* es estándar; los valores los
derivás del proyecto.

## PAUSA — pará acá

Mostrame en el chat un **resumen** de: (a) stack + 5 comandos + rutas; (b) el inventario clasificado y
el mapeo; (c) **dos listas separadas** — lo que resolvés **por estándar** (me lo informás, no me lo
preguntás: ubicación, set de roles, ubicación de specs, formato de los archivos del arnés, hooks,
comandos) y lo que necesita **mi criterio por ser propio del proyecto** (esto sí me lo consultás).
Luego **pará y esperá mi OK**
(o mis correcciones). **No empieces la Etapa 1.** Si el repo está vacío o algo es ambiguo, preguntame
en vez de inventar.
