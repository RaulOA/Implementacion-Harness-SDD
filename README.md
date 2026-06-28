# Arnés de Harness Engineering para programar con agentes de IA

> Un arnés **portable** (agnóstico de agente) para que una IA trabaje de forma **autónoma, verificable y
> estandarizada** sobre **cualquier** proyecto web — y para que el programador **no sea el eslabón
> débil**. Incluye el instalador de referencia para **Claude Code** y una guía pedagógica.

Todo el material está en **español de Costa Rica**.

---

## ¿Qué es esto?

Un **arnés** (en inglés *harness*) no es una aplicación: es la **estructura de archivos y reglas que
rodea al código** para que un agente de IA pueda trabajar solo, sin perderse y sin que tengás que
confiar a ciegas en lo que produce.

El arnés en sí es **solo archivos y una disciplina**, así que lo lee cualquier agente —o incluso una
persona—. Este repositorio contiene:

1. **Una secuencia de prompts** que instala el arnés sobre cualquier repositorio, sin importar el
   lenguaje, el framework ni los patrones (detecta lo que hay y se adapta). Es el **instalador para
   Claude Code** — una de varias adaptaciones posibles.
2. **Una guía** para el humano que va a operar el arnés en el día a día.

Está afinado para **Claude Code**, pero el núcleo es agnóstico de agente (ver "Cómo está organizado" más
abajo para usarlo con otro).

---

## El problema que resuelve

Programar "a feeling" con una IA (el *vibe coding*) funciona para un demo y se cae a pedazos en un
proyecto real: el agente **se desvía** del objetivo, **olvida** decisiones cuando se llena su memoria,
**inventa** APIs que no existen, y las features nuevas contradicen a las viejas. Y como la IA es la
parte rápida y disciplinada del equipo, **el humano termina siendo el cuello de botella**.

La respuesta, que en 2026 se volvió práctica estándar, es el **Spec-Driven Development (SDD)** combinado
con **harness engineering**: el código deja de ser la fuente de verdad y pasa a ser un *resultado* de una
especificación clara; y alrededor del agente se construye una estructura que lo obliga a trabajar de a
una cosa, a especificar antes de codificar, a pedir aprobación humana, y a **demostrar** que funciona en
vez de afirmarlo.

Este repo lleva eso a algo **reutilizable y estandarizado**: el mismo arnés, igual en todos tus
proyectos y en todas tus máquinas.

---

## ¿Qué hay adentro?

```
.
├── arnes-prompts/              ← La secuencia de instalación (versión actual, recomendada)
│   ├── README-secuencia.md     ·  Índice: cómo correr las 4 etapas
│   ├── 00-investigacion.md     ·  Etapa 0 — investigar el proyecto y proponer un plan
│   ├── 01-esqueleto.md         ·  Etapa 1 — crear el arnés estándar + el modelo de trabajo
│   ├── 02-migracion.md         ·  Etapa 2 — aprovechar/archivar lo viejo sin perder nada
│   └── 03-verificacion.md      ·  Etapa 3 — verificar y dejar todo listo
│
└── guia-del-arnes.md           ← Guía pedagógica para el operador humano
```

> Usás la secuencia por etapas en **`arnes-prompts/`** junto con **`guia-del-arnes.md`**. (La evolución
> del diseño —incluido un experimento con base de datos que se reemplazó por archivos— queda en el
> historial de git.)

---

## Cómo usarlo

1. Abrí **Claude Code** en la raíz del proyecto donde querés instalar el arnés.
2. Pegá las etapas de `arnes-prompts/` **una por vez, en orden** (0 → 1 → 2 → 3). Cada etapa hace un
   solo trabajo, se verifica, y **para** a esperar tu confirmación antes de seguir.
3. Cuando termine, leé **`guia-del-arnes.md`** para aprender a trabajar con el arnés en el día a día.

Como todo queda en **archivos del repositorio**, al abrir el proyecto en otra computadora basta con
`git pull`: el arnés viaja completo, con su memoria, sin nada que instalar ni configurar. La
investigación inicial también deja fija la **configuración del chat** (permisos, modelo), y el agente
puede correr **hands-off**: avanza solo y solo te frena en lo de impacto real.

---

## Cómo está organizado: núcleo portable + adaptador

El arnés tiene **dos capas**, y separarlas es lo que lo hace servir más allá de Claude:

1. **El núcleo, agnóstico de agente.** La estructura (`specs/`, `progress/`, `archive/`, `docs/`,
   `feature_list.json`), la compuerta de verificación (`init.*`), las puertas humanas, la trazabilidad
   `R<n> → test`, y **`AGENTS.md` como fuente de verdad** (reglas, flujo SDD, roles y navegación). Son
   solo archivos y una disciplina; los lee cualquier agente o modelo — o una persona.
2. **El adaptador a Claude Code.** `CLAUDE.md` (un complemento fino que apunta a `AGENTS.md` + lo propio
   de Claude Code), `.claude/agents/` y `.claude/commands/` (implementan los roles y los comandos),
   `.claude/settings.json` (permisos, hooks, modelo), más los modos de permisos, `ultrathink` y el ruteo
   Opus/Sonnet. Es lo que hace cómodo el núcleo *dentro de* Claude Code.

**Para usarlo con otro agente** (Cursor, Aider, Codex, lo que venga): mantenés el núcleo igual y
reescribís solo el adaptador — apuntás el archivo de instrucciones de ese agente a `AGENTS.md` y
expresás los cuatro roles y la compuerta como ese agente los maneje. El núcleo no cambia. Por eso la
regla de oro es **mantener `AGENTS.md` completo y autosuficiente**: que describa los roles y el flujo en
concepto, no solo "corré tal subagente". Lo mismo con la config: los archivos como `settings.json` se
**fusionan** (se agregan las claves del arnés sin borrar lo que ya tenías), no se sobrescriben.

---

## Las ideas clave

- **Entrada variable, salida estándar.** El agente *investiga* cada proyecto y luego moldea **el mismo
  esqueleto** en todos lados. La estructura es invariante; el contenido se deriva del proyecto.
- **Capturar ≠ ejecutar.** Las ideas se anotan en una línea y se sigue; se ejecuta de a una cosa. Pensado
  para una forma de trabajar no lineal y orgánica.
- **Spec-Driven Development con puerta humana.** Lo importante se especifica (requisitos en notación
  EARS → diseño → tareas) y **se detiene** a esperar tu aprobación antes de tocar código.
- **El estado vive fuera del chat, en archivos.** Sobrevive a reinicios y a ventanas de contexto
  reventadas, y viaja entre máquinas vía git.
- **Verificación ejecutable y trazabilidad.** Nada se da por terminado sin pruebas en verde, y cada
  requisito se mapea a un test concreto.
- **Cuatro roles que se controlan entre sí** (orquestador, autor de specs, implementador, revisor), con
  una regla "anti-teléfono-descompuesto": cada uno deja su trabajo en disco, no en la conversación.
- **El humano como pieza fuerte, no débil.** La guía entrena los pocos momentos donde el operador hace o
  rompe el proceso: pedir bien, revisar el spec, elegir el peso de cada tarea y exigir evidencia.
- **Portable, no atado a Claude.** El núcleo (estructura, specs, verificación, `AGENTS.md`) sirve con
  cualquier agente; lo específico de Claude Code es un adaptador que se puede reescribir.

---

## Cómo se construyó (el trabajo detrás)

Esto no salió de un solo prompt. Nació de una **conversación larga e iterativa**, con varias vueltas de
diseño y decisiones con sus tira y afloja:

1. Se **analizó un arnés existente** ([`betta-tech/harness-sdd`](https://github.com/betta-tech/harness-sdd))
   y se **investigó el panorama** de SDD de 2026 (GitHub Spec Kit, AWS Kiro, OpenSpec, BMAD, entre otros)
   para entender qué prácticas valía la pena adoptar.
2. Se **generalizó** ese arnés —atado a Python— a algo agnóstico de tecnología.
3. Se **partió en etapas con compuertas** para que el proceso fuera confiable, en vez de un único prompt
   denso que el agente podía no cumplir entero.
4. Se evaluó dónde guardar la memoria del arnés: se probó una **base de datos**, y se **descartó a favor
   de archivos** al ver que el trabajo en varias máquinas se resuelve gratis con git.
5. Se sumó un **modelo de trabajo diario** (comandos reutilizables, captura de ideas, reglas de retención
   para no acumular archivos) y una **guía pedagógica** para el operador humano.
6. Se cerró un **agujero de diseño**: el prompt no le marcaba al agente la frontera entre "lo decide el
   estándar" y "lo decide el humano", y eso amenazaba la estandarización entre proyectos.
7. Se **separó el núcleo agnóstico de agente del adaptador a Claude Code**, y se definió que los archivos
   de configuración se **fusionan** en vez de sobrescribirse — para que el arnés sirva con cualquier
   agente y no pise lo que el proyecto ya tenía.

El resultado intenta capturar el consenso actual sobre cómo trabajar con agentes de codificación
—specs, verificación, estado en disco, orquestación, puerta humana— en algo que cualquiera pueda tomar y
adaptar.

---

## Inspiración y contexto

- Inspirado en [`betta-tech/harness-sdd`](https://github.com/betta-tech/harness-sdd), un ejemplo
  didáctico de los principios de Harness Engineering.
- Apoyado en el ecosistema de **Spec-Driven Development** de 2026 (GitHub Spec Kit, AWS Kiro, OpenSpec,
  BMAD) y en la notación **EARS** para requisitos.

---

## Créditos

Este material es fruto de un trabajo **a cuatro manos** entre el autor del repositorio y **Claude
(Anthropic)**. Claude aportó el grueso de la redacción, la investigación del panorama de SDD y la
síntesis. La **dirección**, las **decisiones de diseño**, el **conocimiento del dominio** y las
**correcciones de rumbo** —las que hicieron que esto sirviera y no quedara genérico— son del autor. Una
herramienta que redacta rinde tanto como quien la dirige.

---

## Uso y licencia

Tomalo, adaptalo y mejoralo libremente. Son plantillas y guías: usalas con criterio, ajustándolas a tu
contexto. (Sugerencia: agregá una licencia abierta como **MIT** si querés dejar el permiso explícito.)

> Aviso: el campo de SDD evoluciona rápido y esto refleja prácticas de 2026. Las plantillas son un punto
> de partida, no dogma — la última palabra siempre es tuya.
