# Guía del arnés — cómo trabajar con él sin ser el eslabón débil

> Guardá este archivo en `docs/` de tu proyecto para tenerlo a mano (también viaja con el repo).
> Es para **consultar**: leela entera una vez, y después volvé a la sección que necesités.
>
> Los comandos `/` que se muestran son del adaptador de **Claude Code**; la **disciplina** que enseña
> (capturar ≠ ejecutar, revisar el spec, exigir evidencia) sirve con cualquier agente.

---

## 0. Antes que nada: tu trabajo cambió

Hasta ahora vos escribías el código. Con el arnés, **el agente escribe el código; vos dirigís, decidís
y revisás.** Es un cambio de oficio, no solo de herramienta. Y eso mueve de lugar tu palanca… y tus
puntos de falla.

El agente es rápido y disciplinado: no se cansa, no se salta pasos, no se olvida del plan. En este
equipo, **el factor variable sos vos.** Eso no es un insulto: es la buena noticia. Significa que hay
**unos pocos momentos concretos** donde podés hundir o levantar todo el proceso, y esta guía es para que
seas fuerte exactamente en esos momentos. Hacerlos bien es, literalmente, dejar de ser el eslabón débil.

Spoiler de cuáles son: **pedir bien, revisar el spec antes de que se escriba código, elegir el peso
correcto de cada tarea, y exigir evidencia.** Todo lo demás el arnés ya lo sostiene solo.

---

## 1. La idea de fondo: capturar ≠ ejecutar

Tu forma de trabajar no es lineal: estás en una cosa y se te ocurre otra, orgánicamente. Eso es una
fortaleza, pero **mata el foco** si lo dejás suelto. La solución es separar dos modos que no deben
mezclarse:

- **Capturar** es instantáneo y desordenado. Se te ocurre algo → lo anotás en **una línea** → seguís en
  lo tuyo. **Nunca** frenás la tarea actual para "ver" la idea nueva.
- **Ejecutar** es disciplinado. **Una sola cosa a la vez**, por su flujo. Esto el arnés ya te lo fuerza.

Pensalo como una libreta al lado del teclado: cuando se te cruza una idea, la escribís en la libreta y
no soltás lo que tenías entre manos. Después, en frío, decidís qué de la libreta se vuelve trabajo de
verdad y qué se descarta.

Tu cronograma es la **columna vertebral** (lo planificado). Las ideas orgánicas caen en el **backlog**
(la libreta). Y de vez en cuando, en frío, reconciliás backlog contra cronograma. Así nunca perdés una
idea **ni** te descarrila.

---

## 2. El loop de tu día (con los comandos)

El arnés te da **comandos `/`**: tu rutina convertida en botones. No tenés que acordarte del flujo; solo
invocás el del momento. El día se ve así:

1. **Abrís un bloque de trabajo** → `/retomar`. El agente te dice en qué quedaste, qué hay en curso y
   qué sigue. (Volvés después de tres días y en una línea sabés dónde estabas.)
2. **Se te ocurre algo a mitad de camino** → `/idea "lo que sea"`. Queda guardado y seguís. No descarrila.
3. **Elegís UNA cosa** (del cronograma o del backlog):
   - algo nuevo → `/nueva "..."`
   - un arreglo → `/arreglar "..."`
   El agente decide solo si necesita el flujo pesado (spec + tu aprobación) o va directo.
4. **Cerrás el bloque** → `/cerrar`. Limpieza automática: lo terminado se resume, lo pesado se archiva,
   el escritorio queda limpio.
5. **Cada tanto** (no todos los días) → `/planificar`. Procesás el backlog contra tu cronograma: qué
   promovés, qué dejás para "algún día", qué tirás.

### Un martes cualquiera (ejemplo)

> `/retomar` → *"Quedaste a mitad del export de usuarios a CSV; está en curso. Pendiente en el cronograma:
> el filtro de búsqueda en la grilla. init.ps1 verde."*
>
> Arrancás con el export. A mitad pensás "el login debería recordar el último idioma". No frenás:
> `/idea "recordar último idioma elegido en el login"`. Seguís.
>
> Terminás el export → `/cerrar`. El agente corre los tests, resume el export en una línea de historial,
> deja el escritorio limpio.
>
> Siguiente bloque: `/nueva "filtro de búsqueda por nombre y rol en la grilla de usuarios"`. El agente ve
> que toca UI + endpoint → lo trata como feature con spec, lo escribe y **para** para tu aprobación.

Eso es el modelo. No hay que memorizarlo: está en los comandos.

---

## 3. Las cuatro destrezas que te hacen fuerte

Acá está el músculo de la guía. Para cada una: qué es, **por qué los humanos fallan ahí** (= cómo te
volvés el eslabón débil), y cómo hacerlo bien.

### Destreza 1 — Pedir bien (el pedido es la semilla)

El agente construye a partir de lo que le pedís. **Pedido vago → construye lo que no era.** No es que el
agente sea tonto; es que rellenó los huecos que vos dejaste, con suposiciones suyas.

Un buen pedido tiene tres cosas: **qué** querés, **por qué** (el objetivo), y **cómo se ve "listo"**.

❌ *"Mejorá la pantalla de usuarios."* → ¿mejorar qué? ¿para quién? ¿cuándo está bien?
✅ *"En la grilla de usuarios quiero un filtro por nombre y por rol, porque hoy hay que scrollear cientos
de filas para encontrar a alguien. Listo = puedo escribir parte del nombre y/o elegir un rol y la grilla
se reduce al instante."*

El "por qué" es el más valioso: le da al agente el criterio para resolver los detalles que vos no
anticipaste. No tenés que diseñar la solución — eso es trabajo del spec. Tenés que dejar **clarísimo el
problema y cómo sabés que se resolvió.**

### Destreza 2 — Revisar el spec en la compuerta (TU momento más importante)

Cuando una tarea es pesada, el agente escribe un **spec** (tres archivos: requisitos, diseño, tareas) y
**para** a esperar tu "aprobado". **Sos vos solo el que abre esa compuerta.** Y acá está la trampa más
cara del arnés:

> El spec es el lugar **más barato** para estar equivocado. Si aprobás un plan torcido, el agente lo
> construye fielmente, y descubrís el error **después**, cuando el código ya existe y arreglarlo cuesta
> diez veces más. Aprobar sin leer ("dale, está bien") es la forma número uno de volverte el eslabón
> débil.

Leé los tres archivos (te toma minutos) y pasalos por estas **cinco preguntas**:

1. **¿Los requisitos dicen lo que yo quería?** Si hay un requisito que no pediste o falta uno que sí,
   pará acá.
2. **¿Cada requisito es verificable?** Tiene que poder comprobarse con un test, no ser una intención
   difusa ("debe ser rápido" no sirve; "debe responder en menos de X" sí).
3. **¿El diseño toca lo que esperabas, y nada de más?** Si para un filtro te quiere reescribir media
   capa de datos, es señal de alarma.
4. **¿Por qué descartó la alternativa?** El diseño debe nombrar al menos una opción que descartó y por
   qué. Si la razón no te cierra, preguntá.
5. **¿Hay algún supuesto oculto?** Buscá lo que el spec da por sentado. Eso es lo que después explota.

Si algo no cuadra, **no apruebes: pedí cambios.** Cambiar un spec son keystrokes; cambiar código son
refactors. Esta es tu palanca más grande en todo el arnés — invertir cinco minutos acá te ahorra horas.

### Destreza 3 — Decidir el peso (flujo pesado vs directo)

No todo merece el ritual completo. El arnés ya separa esto, pero vos podés ayudarlo a decidir bien:

- **Pesado (con spec + aprobación):** multi-archivo, riesgoso, toca la interfaz, o algo que si sale mal
  se nota. Ej.: el filtro de la grilla, un endpoint nuevo, cambiar cómo se guardan los datos.
- **Directo (sin spec):** trivial, un solo archivo, sin riesgo. Ej.: corregir un texto, un typo, ajustar
  un color, renombrar una variable.

La regla mental: *"¿si esto sale mal, me entero enseguida y lo arreglo en un minuto?"* Sí → directo. No →
pesado. Usar el flujo pesado para un typo es un mazo para una nuez; saltárselo en algo riesgoso es jugar
con fuego. **Vos sos el que conoce el riesgo real** — decilo.

### Destreza 4 — Exigir evidencia, no promesas

"Ya quedó listo" no es evidencia. **La evidencia es la salida de los tests y `init.ps1` en verde.** El
arnés está hecho para *demostrar* que funciona, no para *afirmarlo* — pero solo si vos lo exigís.

Cuando el agente diga que terminó, mirá que muestre: el test que cubre lo que pediste pasando, y la
verificación verde. Revisar esa evidencia es más rápido que volver a probar vos a mano, y funciona
incluso para sesiones que no miraste de cerca. Aceptar el "ya está" sin ver nada es cómo se cuelan los
bugs — y cómo te volvés el eslabón débil por confianza.

---

## 4. Los errores que te convierten en el eslabón débil (y el antídoto)

Lista rápida para volver cuando sientas que algo no fluye. Cada uno es una forma concreta de ser el
cuello de botella:

| El error | Por qué te hunde | El antídoto |
|---|---|---|
| Pedir vago | El agente construye lo que no era | Qué + por qué + cómo se ve listo |
| Aprobar el spec sin leer | Construye fiel un plan torcido; lo ves tarde | Las 5 preguntas, antes del código |
| Hacer cinco cosas a la vez | Estado roto, features a medias | Una en curso; el resto a `/idea` |
| Saltarte el cierre | El escritorio se pudre, todo se acumula | `/cerrar` termina **cada** sesión |
| Creer el "ya está" sin ver | Se cuelan bugs | Exigí tests y `init.ps1` verde |
| Mazo para una nuez (o al revés) | Esfuerzo perdido, o cambio inseguro | La regla del riesgo (Destreza 3) |
| Dejar que las ideas te interrumpan | Perdés el hilo de lo actual | `/idea` y seguí |
| Re-explicar lo que el arnés ya sabe | Gastás tokens y generás inconsistencia | Confiá en la memoria; `/retomar` |
| Editar código a mano sin avisar | El arnés pierde el rastro de la realidad | Pasá el trabajo por el flujo; si tocaste algo a mano, **decíselo** para que reconcilie |

Si te ves haciendo varios de estos, no es falta de disciplina tuya: es que todavía no internalizaste el
loop. Se arregla en una semana (sección 7).

---

## 5. Sobre lo viejo y lo archivado (una aclaración que importa)

Cuidado con una confusión natural: **el código nunca se archiva.** Vive siempre en el repo, completo y
funcionando. Lo que el arnés archiva son sus **notas** sobre features viejas: su spec, su informe
detallado.

Entonces, arreglar algo creado hace mucho es una **corrección normal** (`/arreglar`). Si hace falta saber
*por qué* se hizo así en su momento, el índice apunta a dónde quedó esa nota y el agente la trae sola.
Archivar **nunca** vuelve más difícil tocar código viejo — solo guarda el "porqué" en un cajón del que se
saca cuando se necesita. No tengas miedo de tocar lo viejo: para el código, no hay viejo ni nuevo, todo
está ahí.

---

## 6. Cuándo algo sale mal (recuperación)

Va a pasar, y está bien. El arnés tiene redes:

- **El agente nunca borra código a ciegas** ni inventa workarounds: si una herramienta falla, **para**,
  marca `blocked` y te lo dice. Si ves "blocked", es el sistema funcionando, no rompiéndose.
- **Todo está en git.** Si algo quedó mal, revertís. No hay cambio irreversible.
- **Si el agente arrancó por el camino equivocado**, frenalo y corregí el rumbo — mejor temprano. Casi
  siempre es porque el pedido o el spec tenían un hueco (volvé a Destrezas 1 y 2).
- **Si `init.ps1` está en rojo**, nada se da por terminado. Es el semáforo: arreglás lo que marca antes
  de seguir.

La actitud: el arnés está diseñado para que los errores sean baratos y reversibles. No tenés que tenerle
miedo; tenés que leer las señales.

---

## 7. Tu primera semana (cómo agarrar el hábito)

No intentes ser perfecto de entrada. El loop se vuelve automático con repetición. Plan suave:

- **Día 1–2:** practicá con cosas **chicas** (correcciones, ajustes). Forzate a usar `/retomar` al
  empezar y `/cerrar` al terminar, aunque parezca innecesario. El objetivo es el hábito, no el tamaño.
- **Día 3–4:** la próxima vez que se te cruce una idea, en vez de perseguirla, `/idea` y seguí. Sentí lo
  que se siente no descarrilarse. Hacé tu primera feature pesada y **tomate los cinco minutos** de las
  cinco preguntas en el spec.
- **Día 5+:** corré `/planificar` una vez y mirá cómo tu backlog se ordena contra el cronograma. A esta
  altura el loop ya no lo pensás.

Una sola regla para esta semana, si tenés que quedarte con una: **leé el spec antes de aprobar.** Es el
80% del beneficio.

---

## 8. Tarjeta de referencia rápida

**El loop:** `/retomar` → trabajar (capturando con `/idea`) → `/nueva` o `/arreglar` (una a la vez) →
`/cerrar` → cada tanto `/planificar`.

**Los comandos:**

| Comando | Para qué |
|---|---|
| `/retomar` | Ponerme al día: en qué quedé, qué sigue |
| `/idea "..."` | Guardar una idea en una línea y seguir |
| `/nueva "..."` | Trabajo nuevo (decide solo si pesado o directo) |
| `/arreglar "..."` | Corregir algo (reciente o viejo/archivado) |
| `/planificar` | Procesar el backlog contra el cronograma |
| `/cerrar` | Cerrar la sesión: resume, archiva, deja limpio |
| `/estado` | Vistazo rápido de una pantalla |

**Las 5 preguntas para aprobar un spec:** ¿dice lo que pedí? · ¿cada requisito es verificable? · ¿el
diseño toca solo lo esperado? · ¿la alternativa descartada me cierra? · ¿hay supuestos ocultos?

**Pesado o directo:** *"¿si sale mal me entero y lo arreglo en un minuto?"* Sí → directo. No → con spec.

**Evidencia, no promesas:** "ya está" no vale; tests pasando + `init.ps1` verde, sí.

**La regla de oro:** *capturar ≠ ejecutar.* Anotá las ideas, ejecutá de a una, **y leé el spec antes de
aprobar.**
