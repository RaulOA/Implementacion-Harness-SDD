# Etapa 2 / 4 — Migración y archivado de lo viejo

> **Contrato común (todas las etapas).** Arnés **estándar** (🔒) con contenido derivado (🔎). Cero
> suposiciones; estado en archivos versionados del repo; una feature a la vez; SDD con puerta humana;
> verificación ejecutable; trazabilidad `R<n> → test`; anti-teléfono. **No avances sin la confirmación
> del final.** Leé `HARNESS-INSTALL.md`: ahí está el inventario clasificado de la Etapa 0. Los archivos
> del arnés (Etapa 1) ya existen.

> **Frontera de decisiones (clasificá tus dudas antes de cualquier pausa).** Lo que el estándar ya
> define —todo lo marcado 🔒 y la forma del esqueleto: ubicación y nombres de archivos/carpetas, los
> cuatro roles, dónde van las specs (`specs/<feature>/`), los archivos de memoria, los hooks y los
> comandos `/`— es **invariante**: lo aplicás y, si acaso, lo **informás** ("así lo apliqué"); **nunca
> lo consultás**. Solo me consultás lo **propio de este proyecto** (🔎: stack y comandos reales, fuente
> de los `R<n>`, qué módulos entran en alcance, inconsistencias o hallazgos del repo). Prueba rápida:
> si una duda se resuelve mirando el estándar, no es pregunta —resolvela; si la respuesta cambiaría de
> un proyecto a otro, esa sí me la pasás.

## Tu único trabajo en esta etapa

Trasladar lo viejo al arnés nuevo **sin perder nada**. Esta es la única etapa que **quita** archivos,
así que el orden es estricto: **aprovechar → archivar → recién entonces quitar**.

> Si en la Etapa 0 el inventario quedó vacío (no había context-engineering previo), no hay nada que
> migrar: confirmámelo y pasamos directo a la pausa.

### 1 — Aprovechar lo reutilizable 🔎
Para cada artefacto marcado **reutilizable** en `HARNESS-INSTALL.md`, integrá su **contenido** al
archivo del arnés que le corresponde según el mapeo (convenciones → `docs/conventions.md`, arquitectura
→ `docs/architecture.md`, instrucciones de agente → `CLAUDE.md`/`AGENTS.md`, roles → `.claude/agents/*`,
memoria/progreso → `progress/history.md`). Integrá el contenido, no el archivo viejo; resolvé duplicados
a favor de lo nuevo.

**Specs/diseños viejos:** los de trabajo **ya `done`** se tratan como **histórico** — se archivan
verbatim como registro (paso 2), **no se reconstruyen** al formato `specs/<feature>/{requirements,
design,tasks}`. Ese formato es **solo hacia adelante**: reformatear trabajo terminado es make-work (y
medio ficticio, porque sería reversear el spec desde el código ya escrito). Si alguna spec vieja **no**
está `done` (sigue activa o planeada), esa sí traela al formato nuevo.

### 2 — Archivar TODO lo viejo (verbatim) en `archive/legacy/`
Antes de quitar nada, **mové** cada artefacto viejo que vayas a sacar del lugar activo —reutilizable o
histórico— a `archive/legacy/`, **conservando su ruta relativa original** y su contenido **intacto** (no
lo transcribas ni lo edites: movelo tal cual). Creá `archive/legacy/INDEX.md` con una fila por
artefacto: ruta original, clasificación (`reusable`/`historical`) y, si aplica, a qué archivo del arnés
se integró su contenido.

### 3 — Verificar el archivado
Confirmá que **toda** ruta que vas a dejar fuera del lugar activo está representada en
`archive/legacy/INDEX.md` y que su archivo está físicamente en `archive/legacy/`. Si falta uno, **no
quites nada** y reportá.

### 4 — Recién ahora, dejar limpio el lugar activo
Ya que cada artefacto viejo está movido a `archive/legacy/` (no borrado: movido) y registrado en el
INDEX, el lugar activo queda limpio. Doble respaldo: queda en `archive/legacy/` **y** en el historial de
git.

**Regla dura:** nada se pierde. Lo reutilizable quedó integrado al arnés **y** su original preservado en
`archive/legacy/`. Ante la mínima duda sobre un artefacto, dejalo donde está y reportámelo.

## PAUSA — pará acá

Confirmame: cuántos artefactos se aprovecharon y a qué archivos; cuántos quedaron en `archive/legacy/`
(con su INDEX); y que el lugar activo quedó limpio. Demostrá que **nada se perdió**. El **destino** de
cada artefacto lo fija el estándar (no se consulta); pasame solo los casos donde el **contenido** es
ambiguo sobre a qué archivo del arnés mapea. Marcá `[x] Etapa 2`
en `HARNESS-INSTALL.md`. **Pará y esperá mi OK. No empieces la Etapa 3.**
