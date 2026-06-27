# Plan de trabajo — Analizador de Oraciones

> Plan por fases con **puertas**: cada fase se prueba en aula antes de pasar a la
> siguiente. Si una fase no convence, se para ahí habiendo gastado poco. El objetivo es
> que el proyecto **fluya sin improvisar**: cada sesión sabe qué toca y qué NO toca.
>
> Reglas de ejecución en `CLAUDE.md` §3. Correcciones al GDD en `CLAUDE.md` §4.
> Alcance v1 en `CLAUDE.md` §5.

---

## Principio rector

**Validar la mecánica con UNA oración real en clase antes de construir nada más.**
Todo lo que no sirva a ese objetivo es prescindible en v1 (GDD §10.1).

El riesgo real del proyecto no es la UI: es el **motor de validación** de segmentación
libre. Por eso la Fase 1 lo incluye desde el principio, aunque sea con una sola oración.

---

## Fase 0 — Decisión · SIN CÓDIGO ✅ (hecha)

- [x] Leer GDD, justificación metodológica y marco teórico.
- [x] Verificar la paleta real del Taller y decidir reutilizarla.
- [x] Separar el proyecto (carpeta propia `proyecto_analizador-sintaxis`, git local).
- [x] Fijar alcance v1: **1 oración, sin backend, un archivo HTML**.
- [x] Andamiaje Claude: `CLAUDE.md`, memoria semilla, este plan.

**Puerta:** el usuario aprueba arrancar la Fase 1. ← estamos aquí.

---

## Fase 1 — Maqueta de UNA oración (el corazón del proyecto)

Un solo `index.html` con la mecánica COMPLETA sobre **una única oración**. Si la
mecánica no convence en clase, se para aquí.

Oración de referencia del GDD (sirve para la maqueta):
*"Ayer los dos hermanos pequeños de María entregaron rápidamente a su profesora de
Lengua un precioso ramo de flores del jardín."*

### Pasos (uno por sesión, un commit por paso)

1. **Esqueleto + tokens.** HTML único; render de la oración como bloques de palabra
   independientes (GDD §3.1 paso 1). Tipografía serif para la oración (≥20px), paleta
   de fondo "sin etiquetar" gris claro. CSS reutilizado/adaptado del Taller.
2. **Selección por clic.** Clic selecciona/deselecciona palabra; selección múltiple
   (contigua y no contigua). Estado en un objeto JS en memoria. SIN drag&drop.
3. **Etiquetado de tipo de sintagma.** Panel inferior contextual: aparece al haber
   selección; botones SN/SV/SP/SAdj/SAdv (uno activo); al confirmar, los bloques se
   colorean con la **paleta del Taller** (`CLAUDE.md` §4). Botón Cancelar.
4. **Etiquetado de función.** Tras elegir tipo, aparecen las funciones **filtradas por
   tipo** (un SAdv no puede ser Sujeto; un SN no puede ser CC) — GDD §4.2 y §7.3.
   Incluye botón **"Sujeto elíptico"** con selección de persona gramatical (GDD §4.4).
5. **Motor de validación + feedback al final** (ver abajo). Botón "Comprobar".
6. **Núcleo (análisis interno mínimo).** Solo marcar el núcleo del SN sujeto y del SV
   (GDD §10.1: "solo núcleo en v1"). Resto de análisis interno → v2.
7. **Pulido táctil/Chromebook** y prueba real en un dispositivo de aula.

### Motor de validación v1 (el punto difícil — modelo concreto)

El análisis del alumno es un conjunto de **sintagmas**, cada uno = un **conjunto de
ids de token** + tipo + función (+ núcleo opcional). Reglas que simplifican el problema
sin perder valor pedagógico:

- **Cada token pertenece como mucho a un sintagma** (sin solapamientos). Esto elimina la
  parte combinatoria más dura.
- **El orden no importa; los sintagmas discontinuos se permiten** porque se comparan
  *conjuntos* de ids, no rangos.
- Validación de cada sintagma del alumno:
  1. ¿Su conjunto de tokens **coincide exactamente** con un sintagma de la
     `solucion_canonica`? → comparar tipo / función / núcleo → ✔ o ✘ con explicación.
  2. ¿Coincide con una **alternativa** declarada? → ◎ "alternativa válida" + nota.
  3. ¿No coincide con ninguno? → ✘ **error de segmentación** (la agrupación en sí está
     mal): mensaje guiado por `feedback.errores_comunes` si hay patrón que case.
- Tres estados visuales (GDD §6.2): ✔ correcto, ◎ alternativa, ✘ error con explicación
  lingüística (nunca solo "incorrecto"). Feedback **solo al pulsar Comprobar**.
- Botón "Ver solución" **explícito** y **registrado** (que conste que lo pidió).

> Este modelo de "conjuntos de tokens" es la decisión de ingeniería clave de v1.
> Mantenerlo simple aquí es lo que hace el proyecto viable.

**Puerta Fase 1:** ¿un alumno de 3.º ESO completa el análisis sin instrucciones en
pantalla y entiende el feedback solo? (criterios de éxito GDD §10.2). Si sí → Fase 2.

---

## Fase 2 — Pocas oraciones (solo si la Fase 1 funcionó)

- 3-5 oraciones escritas a mano (esquema JSON del GDD §5.1).
- Sujeto elíptico real en alguna.
- Alternativas válidas reales (casos de doble lectura, GDD §4.3).
- Selección de oración por parámetro de URL o aleatoria.

**Puerta:** el formato JSON aguanta varias oraciones sin tocar el motor.

---

## Fase 3 — Registro opcional (backend GAS mínimo)

- `Code.gs` con `doGet()` (sirve el HTML) y `saveResult()` (escribe en un Sheet NUEVO,
  separado del Taller). Molde copiado del Taller (`getColMap_`, append seguro).
- Envío con `google.script.run` al finalizar: alumno, id de oración, análisis
  serializado, timestamp, % de sintagmas correctos (GDD §8.4).
- **Despliegue GAS independiente del Taller.**

**Puerta:** el registro aporta algo útil al profesor → se mantiene; si no, se quita.

---

## Fase 4 — Escala (solo si todo lo anterior se validó)

- Convertidor del banco existente `Oraciones_Banco` del Taller → esquema del Analizador
  (mayor activo oculto: decenas de oraciones ya analizadas, GDD reutilizable como
  fuente). Deja de escribir oraciones a mano.
- Análisis interno completo (determinante, adyacente, complemento del nombre) — GDD v2.
- Panel docente / selección por nivel — GDD v2/v3.

---

## Lo que NO se hace (para no dispersarse)

Fuera de v1 por decisión, no por olvido (GDD §10.1, §2.3): oraciones compuestas,
predicado nominal, pasivas/impersonales, diagramas de árbol, gamificación, modo oscuro,
entrada libre de oraciones, panel docente. Si surge la tentación, anótalo y sigue.
