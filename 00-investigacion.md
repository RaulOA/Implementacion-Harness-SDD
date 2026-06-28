# Etapa 0 / 4 — Investigación y plan

> **Contrato común (todas las etapas).** Instalás un arnés "Harness Engineering" **estándar**: misma
> estructura en todo proyecto (🔒), contenido derivado de ESTE proyecto (🔎). Reglas: **cero
> suposiciones** — investigá antes de asumir; el estado vive **fuera del chat, en archivos versionados
> del repo** (así viaja solo entre máquinas vía git); una feature a la vez; SDD con **puerta humana**;
> verificación ejecutable; trazabilidad `R<n> → test`; patrón **leader / spec_author / implementer /
> reviewer** con anti-teléfono (los subagentes escriben su resultado en archivos y devuelven solo una
> referencia). **No avances de etapa sin la confirmación que se pide al final.**

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

### 3 — Clasificar y proponer mapeo 🔎
Para cada cosa encontrada: **reutilizable** (qué archivo del arnés estándar la absorberá) o
**histórico** (se preserva verbatim en `archive/legacy/`, no se reutiliza pero no se pierde).

### 4 — Escribir el traspaso
Creá `HARNESS-INSTALL.md` en la raíz con: stack + los 5 comandos + rutas de código/tests; el inventario
con su clasificación y mapeo propuesto. Dejá una sección "Progreso de etapas" con casillas:
`[ ] Etapa 0` … `[ ] Etapa 3`, y marcá la 0 al terminar.

## PAUSA — pará acá

Mostrame en el chat un **resumen** de: (a) stack + 5 comandos + rutas; (b) el inventario clasificado y
el mapeo; (c) cualquier ambigüedad o decisión que necesite mi criterio. Luego **pará y esperá mi OK**
(o mis correcciones). **No empieces la Etapa 1.** Si el repo está vacío o algo es ambiguo, preguntame
en vez de inventar.
