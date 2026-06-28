# Arnés "Harness Engineering" — instalación por etapas (todo en archivos)

El arnés guarda **toda su memoria en archivos dentro del proyecto**. No usa base de datos. Eso
significa que, cuando abrís el proyecto en otra computadora (trabajo, personal, una VM), **git trae
todo** y el arnés funciona igual sin instalar ni configurar nada por máquina.

El prompt se partió en **4 etapas con una pausa entre cada una**, para que el proceso sea confiable:
cada etapa hace **un solo trabajo**, se verifica, y **para** a esperar tu OK antes de seguir.

## Cómo correrlas

En Claude Code (VS Code o terminal de Visual Studio), en la **raíz** del repo, pegá una etapa por vez,
en orden. Esperá la confirmación que pide al final, revisá, y recién entonces pegá la siguiente.

| # | Archivo | Qué hace | ¿Escribe? | Pausa al final |
|---|---------|----------|-----------|----------------|
| 0 | `00-investigacion.md` | Detecta stack/layout/tooling e inventaria lo viejo que ya tengas | Solo `HARNESS-INSTALL.md` (traspaso) | Te muestra hallazgos + plan; **espera tu OK** |
| 1 | `01-esqueleto.md` | Crea todos los archivos estándar del arnés (+ constitución) | Archivos | Autoauditoría de archivos creados |
| 2 | `02-migracion.md` | Aprovecha lo reutilizable, archiva el resto, y **recién ahí** quita lo viejo | Archivos | Confirma que nada se perdió |
| 3 | `03-verificacion.md` | Corre `init.ps1` en verde, resume y explica el flujo | — | Arnés listo, vacío de trabajo |

**Entre etapas, el estado vive en `HARNESS-INSTALL.md`** (lo crea la Etapa 0, las demás lo leen y lo
marcan, la Etapa 3 lo borra). Si una sesión se reinicia, la siguiente etapa retoma desde ahí.

## Dónde guarda su memoria el arnés (todo en archivos)

| Necesidad | Archivo |
|---|---------|
| Tablero de tareas (qué hacer, estados, aprobaciones) | `feature_list.json` |
| Specs de la feature en curso (lo que vos revisás) | `specs/<feature>/` |
| Estado de la sesión activa (se sobrescribe) | `progress/current.md` |
| Bitácora compacta de features terminadas (append-only) | `progress/history.md` |
| Informes detallados de implementación/revisión (append-only) | `progress/reports.md` |
| Context-engineering viejo, preservado verbatim | `archive/legacy/` |

Son **archivos consolidados y podables** (no un archivo por feature), así no se llena de cientos de
archivos como pasaba antes.

## Principio rector (vale para todas)

**Entrada variable, salida estándar.** El agente investiga ESTE proyecto y luego moldea el **mismo
esqueleto** en todos lados. 🔒 = invariante (igual en todo proyecto) · 🔎 = derivado de la
investigación.

## Multi-máquina (no hay que hacer nada)

Como todo son archivos del repo, no hay base de datos que crear ni cadena de conexión que configurar.
En cualquier computadora: `git pull` y listo. El arnés viaja completo, con su memoria.

## Decisiones por defecto (volteables — decíselo al agente)

`init.ps1` como único script estándar (PowerShell, porque tu stack es .NET/Windows); **podar**
features `done` del tablero a `progress/history.md`; **constitución incluida**.
