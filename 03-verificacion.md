# Etapa 3 / 4 — Verificación final y arranque

> **Contrato común (todas las etapas).** Arnés **estándar** (🔒) con contenido derivado (🔎). Cero
> suposiciones; estado en archivos versionados del repo; una feature a la vez; SDD con puerta humana;
> verificación ejecutable; trazabilidad `R<n> → test`; anti-teléfono. Leé `HARNESS-INSTALL.md`: las
> Etapas 0–2 ya están marcadas.

> **Frontera de decisiones (clasificá tus dudas antes de cualquier pausa).** Lo que el estándar ya
> define —todo lo marcado 🔒 y la forma del esqueleto: ubicación y nombres de archivos/carpetas, los
> cuatro roles, dónde van las specs (`specs/<feature>/`), los archivos de memoria, los hooks y los
> comandos `/`— es **invariante**: lo aplicás y, si acaso, lo **informás** ("así lo apliqué"); **nunca
> lo consultás**. Solo me consultás lo **propio de este proyecto** (🔎: stack y comandos reales, fuente
> de los `R<n>`, qué módulos entran en alcance, inconsistencias o hallazgos del repo). Prueba rápida:
> si una duda se resuelve mirando el estándar, no es pregunta —resolvela; si la respuesta cambiaría de
> un proyecto a otro, esa sí me la pasás.

## Tu único trabajo en esta etapa

Cerrar la instalación: verificar que todo está sano, dejar el arnés **vacío de trabajo** y explicarme
cómo arranco. **No implementes ninguna feature ni marqués nada como `done`.**

### 1 — Verificación ejecutable
Corré `init.ps1` y mostrame la salida completa. Debe terminar **verde** (o, si el repo está vacío y aún
no hay tests, con `[WARN]` claros y exit 0 — nunca `[FAIL]` silenciosos). Si está en rojo, **no cierres**:
reportá el bloqueo y pará.

### 2 — Chequeo de coherencia
Confirmá: los archivos estándar del arnés están (incluidos los comandos `/` en `.claude/commands/` y la
config en `.claude/settings.json`); la memoria en archivos existe (`progress/current.md`,
`progress/history.md`, `progress/reports.md`, `progress/backlog.md`); `feature_list.json` tiene ≤1
`in_progress` (idealmente 0) y al menos una feature `pending` con `"sdd": true`; lo viejo quedó en
`archive/legacy/` con su INDEX; no quedó basura de la instalación.

### 3 — Limpieza del traspaso
Marcá `[x] Etapa 3` en `HARNESS-INSTALL.md` y luego **borralo** (su contenido ya cumplió su función; lo
permanente quedó en el arnés).

### 4 — Resumen y arranque
Dame un resumen corto de: qué quedó instalado (archivos), qué se aprovechó/archivó, y **cómo arranco el
flujo SDD**:
> Dejo una feature `pending` con `"sdd": true` y te pido *"implementá la siguiente feature pendiente"*.
> Vos lanzás `spec_author`, que escribe los 3 specs y **para** en `spec_ready`. Yo leo
> `specs/<feature>/` y respondo *"aprobado"* (o pido cambios). Recién ahí escribís `approved_by`/
> `approved_at`, pasás a `in_progress`, y corrés `implementer` → `reviewer` hasta `done`.

Para el día a día no hace falta recordar esta ceremonia: usá los **comandos `/`** (`/retomar`, `/idea`,
`/nueva`, `/arreglar`, `/cerrar`…) que son tu rutina hecha botones, y leé **`guia-del-arnes.md`** para
operarlo bien. Si querés que avance sin interrumpirte, corré en modo **hands-off** (`auto`); solo te
frenará en lo de impacto real.

Recordá: como todo son archivos del repo, en otra computadora basta `git pull` — el arnés viaja completo,
sin nada que instalar. Con eso queda listo e idéntico al de cualquier otro proyecto. Fin de la
instalación.
