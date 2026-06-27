# Analizador de Oraciones — instrucciones del proyecto

> Léeme entero al empezar cualquier sesión. Este archivo es la fuente de verdad del
> proyecto y es **autosuficiente**: aunque la memoria automática no cargue, aquí está
> todo lo necesario para trabajar sin improvisar.

## 1. Qué es este proyecto

Aplicación web educativa (Lengua Castellana, ESO/Bachillerato, LOMLOE · EBAU Murcia)
para que el alumno **construya** el análisis sintáctico de **oraciones simples**:
selecciona palabras → las agrupa en sintagmas → les pone tipo y función → comprueba y
recibe feedback razonado. Inspiración: GeoGebra/Desmos (laboratorio, no test).

**No** es un test de opción múltiple. **No** es el Taller de Sintaxis (ver §6).

Documentos de diseño (ya escritos, NO los reescribas sin que el usuario lo pida):
- `Claude/GDD_Analizador_Oraciones_v1.md` — Game Design Document v1.0 (el "qué").
- `Claude/justificacion_cambio_metodologico.md` — el "porqué" pedagógico/normativo.
- `marco_teorico_didactico.md` — fundamentación teórica detallada.
- `articulos reflexión metalinguistica .../` — PDFs de investigación de respaldo.
- `PLAN_DE_TRABAJO.md` — **el plan por fases. Síguelo. No improvises fases nuevas.**

## 2. Estado actual

**Fase 0 — Decisión. SIN CÓDIGO todavía.** El plan está cerrado; el siguiente paso
real es la **Fase 1: maqueta de UNA sola oración** (ver `PLAN_DE_TRABAJO.md`).
Repo git local inicializado; sin remoto de GitHub aún (ver §7).

## 3. Cómo trabajar con el usuario (reglas que él fijó — respétalas)

El usuario es **profesor de Lengua, no programador, aprendiendo solo**. Por tanto:

1. **Decide tú lo técnico cuando haya margen** y explícalo en una línea. No le
   sobrecargues con preguntas de fontanería (`¿main o dev?`). Para decisiones de
   verdad (alcance, qué hacer con un bug, granularidad) SÍ pregunta con
   `AskUserQuestion` y marca la recomendada con "(Recommended)".
2. **Una cosa a la vez.** No mezcles "crear X" con "refactorizar Y". Si encuentras un
   bug mientras haces otra cosa, anótalo y sigue; no lo arregles en el mismo commit.
3. **Espera su confirmación entre pasos.** Si el paso tiene efecto visual, pídele que
   lo verifique en el navegador. Si es invisible, dile que no hay nada que verificar.
4. **Prefiere fidelidad y bajo riesgo** sobre elegancia. Su frase: "lo que sea más
   fácil para mí".
5. **Sin TypeScript, React, Vue ni bundlers.** HTML + CSS + JS vanilla puro. Para
   `onclick=""` usa `window.X`.
6. **Un commit por paso**, mensaje claro, y **`git push` justo después de cada commit**
   sin pedir confirmación (cuando haya remoto). Si el push falla, avisa pero no
   bloquees. Cierra los mensajes de commit con la línea Co-Authored-By de Claude.
7. Responde a menudo con comandos cortos ("sigue", "paso 2", "ok"). Tradúcelos al
   contexto.

## 4. Las 3 correcciones al GDD (acordadas — aplícalas siempre)

El GDD es bueno pero tiene tres puntos que se corrigen en la implementación:

1. **Paleta de colores: usa la del TALLER, no la del GDD.** El alumno aprende UN solo
   código de color en el curso; dos paletas distintas entre las dos apps se lo rompen.
   Paleta real del Taller (verificada en su `css/theme/new-ui.css`):

   | Tipo | Fondo | Borde | Texto |
   |------|-------|-------|-------|
   | SN (nominal)       | `#DBEAFE` | `#60A5FA` | `#1E3A8A` (azul)         |
   | SV (verbal)        | `#D1FAE5` | `#34D399` | `#065F46` (verde)        |
   | SP (preposicional) | `#EDE9FE` | `#A78BFA` | `#4C1D95` (morado)       |
   | SAdj (adjetival)   | `#FCE7F3` | `#F472B6` | `#9D174D` (rosa/magenta) |
   | SAdv (adverbial)   | `#FFEDD5` | `#FB923C` | `#9A3412` (naranja)      |

   (El GDD §7.4 proponía SP naranja, SAdj morado, SAdv teal — IGNÓRALO. SN azul y SV
   verde sí coinciden.)

2. **NO prometas "entrada libre de cualquier oración" en v1.** Cada oración cuesta
   ~30 min de redacción (solución canónica + alternativas + feedback). v1 arranca con
   **1 oración** escrita a mano; luego 3-5. La entrada libre es objetivo de v4+.

3. **La validación de segmentación libre es EL grueso del trabajo**, no un detalle.
   El GDD §8.3 lo despacha en una frase ("compara el estado en memoria con la solución
   canónica"); en realidad es el 70% de la ingeniería. Modelo de validación v1 en
   `PLAN_DE_TRABAJO.md` §Motor.

## 5. Decisiones de alcance v1 (cerradas)

- Solo **oraciones simples de predicado verbal**. Fuera: copulativas/atributo,
  impersonales, pasivas, formas no personales (GDD §4.1).
- Mecánica: **selección por clic**, no drag&drop (más fiable en táctil Chromebook).
- Feedback **solo al final**, nunca durante la construcción. **Sin penalización
  numérica** — el Analizador es FORMATIVO. No portes aquí la curva dura del Taller.
- v1 = **un único archivo HTML** (HTML+CSS+JS+oraciones embebidas en JSON). Backend GAS
  para registrar resultados es **opcional** y posterior.

## 6. Relación con el Taller de Sintaxis (proyecto hermano)

Carpeta hermana: `../proyecto_taller-sintaxis` (app en producción). **NO mezclar las dos
en la misma sesión.** Del Taller se **reutiliza copiando** (nunca enlaces ni
dependencias): el sistema visual/CSS (tipografía, responsive, trabajo táctil de
Chromebook ya resuelto) y el **molde** de backend GAS (`doGet()`, `saveResult()`,
`getColMap_`). **NO se reutiliza** el motor de validación (el del Taller valida
constituyentes ya segmentados; aquí hay que validar agrupaciones libres) ni la UI de
drag&drop. Despliegue GAS **separado** del Taller.

## 7. Git / despliegue

- Repo git **local** ya inicializado en esta carpeta.
- **Remoto de GitHub: aún no creado.** Cuando el usuario quiera, crear repo **privado**
  nuevo (no reutilizar el del Taller) y a partir de ahí aplicar la regla de push
  automático (§3.6). Sugerencia: `gh repo create proyecto_analizador-sintaxis --private --source=. --remote=origin`.
- Servir en local para probar: `npx -p http-server http-server -p 8767 -c-1`
  (8767 para no chocar con el Taller, que usa 8765/8766).

## 8. Terminología (consistente con el Taller)

**sintagma** (nunca "grupo"); **para** introduce CI (no usar "CI" como cajón de
sastre); tipos: SN / SV / SP / SAdj / SAdv. Mantén estos nombres tal cual en código.
