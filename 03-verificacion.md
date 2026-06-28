# Etapa 3 / 4 — Verificación final y arranque

> **Contrato común (todas las etapas).** Arnés **estándar** (🔒) con contenido derivado (🔎). Cero
> suposiciones; estado en archivos versionados del repo; una feature a la vez; SDD con puerta humana;
> verificación ejecutable; trazabilidad `R<n> → test`; anti-teléfono. Leé `HARNESS-INSTALL.md`: las
> Etapas 0–2 ya están marcadas.

## Tu único trabajo en esta etapa

Cerrar la instalación: verificar que todo está sano, dejar el arnés **vacío de trabajo** y explicarme
cómo arranco. **No implementes ninguna feature ni marqués nada como `done`.**

### 1 — Verificación ejecutable
Corré `init.ps1` y mostrame la salida completa. Debe terminar **verde** (o, si el repo está vacío y aún
no hay tests, con `[WARN]` claros y exit 0 — nunca `[FAIL]` silenciosos). Si está en rojo, **no cierres**:
reportá el bloqueo y pará.

### 2 — Chequeo de coherencia
Confirmá: los archivos estándar del arnés están; la memoria en archivos existe (`progress/current.md`,
`progress/history.md`, `progress/reports.md`); `feature_list.json` tiene ≤1 `in_progress` (idealmente 0)
y al menos una feature `pending` con `"sdd": true`; lo viejo quedó en `archive/legacy/` con su INDEX; no
quedó basura de la instalación.

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

Recordá: como todo son archivos del repo, en otra computadora basta `git pull` — el arnés viaja completo,
sin nada que instalar. Con eso queda listo e idéntico al de cualquier otro proyecto. Fin de la
instalación.
