  
**ANALIZADOR DE ORACIONES**

Laboratorio interactivo de análisis sintáctico

**GAME DESIGN DOCUMENT**

v1.0 · Fase de exploración

| Autor | José Luis Asensio |
| :---- | :---- |
| **Materia** | Lengua Castellana y Literatura · ESO y Bachillerato |
| **Marco curricular** | LOMLOE · EBAU Murcia |
| **Plataforma prevista** | Google Apps Script WebApp / HTML puro |
| **Estado** | Exploración conceptual · Sin código |
| **Fecha** | Junio 2026 |

# **1\. Visión del proyecto**

## **1.1 Qué es**

El Analizador de Oraciones es una aplicación web educativa pensada para que los alumnos de ESO y Bachillerato construyan activamente el análisis sintáctico de oraciones simples. No es un cuestionario de opción múltiple ni un test de rellenar huecos. Es un entorno de trabajo donde el alumno toma decisiones lingüísticas reales: qué palabras forman un sintagma, qué función tiene, cuál es su núcleo.

La herramienta se inspira en cómo funcionan GeoGebra o Desmos: no dan la respuesta ni guían con pistas directas, sino que permiten construir y explorar. Aquí, el objeto que se construye es un análisis sintáctico.

## **1.2 Por qué existe**

El análisis sintáctico tiene un problema didáctico conocido: los alumnos memorizan etiquetas sin comprender relaciones. Pueden escribir «Sujeto» y «Predicado» sin haber procesado por qué ese grupo de palabras tiene esa función. Las fichas tradicionales y los test de opción múltiple no rompen ese patrón porque permiten copiar, adivinar o responder por exclusión.

Este proyecto parte de una hipótesis pedagógica concreta, alineada con los principios de reflexión metalingüística que el profesor está aplicando en el aula:

* La función sintáctica se comprende construyendo la estructura, no nombrándola.

* El error es información: el alumno que agrupa mal aprende más que el que marca la respuesta correcta sin haber razonado.

* La interfaz debe eliminar ambigüedad: cada gesto del alumno es una decisión lingüística registrada.

## **1.3 Diferencia respecto al Taller de Sintaxis**

El Taller de Sintaxis (aplicación ya en producción) trabaja la identificación de constituyentes mediante arrastrar y soltar en oraciones del banco de ejercicios de la app. El Analizador de Oraciones hace algo diferente:

| Dimensión | Taller de Sintaxis | Analizador de Oraciones |
| :---- | :---- | :---- |
| Oraciones | Banco predefinido de la app | Cualquier oración que el profesor introduce |
| Mecánica principal | Drag & drop de constituyentes ya segmentados | Selección libre \+ etiquetado desde cero |
| Foco lingüístico | Identificación de sintagmas | Identificación \+ función \+ análisis interno |
| Modelo de datos | GAS \+ Sheets (backend completo) | JSON local o mínimo GAS |
| Público | Todos los grupos del profesor | Puede empezar como herramienta de clase presencial |

No son aplicaciones competidoras. Son herramientas complementarias que trabajan distintas fases del mismo proceso de aprendizaje.

# **2\. Objetivos de aprendizaje**

## **2.1 Competencias lingüísticas trabajadas**

La aplicación cubre competencias explícitamente recogidas en el currículo LOMLOE para Lengua Castellana y Literatura en ESO y Bachillerato:

| Competencia | Qué hace la app para trabajarla |
| :---- | :---- |
| Reflexión metalingüística | El alumno debe justificar cada agrupación; no puede etiquetar sin haber seleccionado |
| Reconocimiento de sintagmas | La selección de palabras fuerza la segmentación real, no la intuitiva |
| Identificación de funciones sintácticas | El etiquetado de función va siempre ligado a la forma del sintagma |
| Análisis interno de sintagmas | El nivel de detalle opcional permite trabajar núcleo, determinante, adyacente, etc. |
| Producción de argumentación gramatical | El feedback del sistema explica por qué algo es incorrecto, modelando el razonamiento lingüístico |

## **2.2 Lo que el alumno debe poder hacer al terminar**

1. Segmentar una oración en sintagmas sin ayudas visuales previas.

2. Identificar el tipo de sintagma de cada grupo.

3. Asignar la función sintáctica correcta a cada sintagma.

4. Identificar el núcleo dentro de al menos el SN sujeto y el SV.

5. Corregir sus propios errores a partir del feedback del sistema.

6. Distinguir casos donde hay más de una lectura sintáctica válida.

## **2.3 Lo que NO es objetivo (al menos en v1)**

* Trabajar oraciones compuestas o subordinadas.

* Analizar textos completos.

* Producir diagramas de árbol formales (tipo Chomsky o sintaxis de constituyentes con notación X-barra).

* Sustituir la explicación del profesor: la app es un laboratorio de práctica, no de instrucción.

# **3\. Mecánica de uso**

## **3.1 Flujo principal de una sesión**

Una sesión con la aplicación sigue siempre el mismo flujo de cinco pasos. El alumno no puede saltarse pasos ni avanzar sin haber completado el anterior.

| PASO 1 · Lectura de la oración |
| :---- |

La oración aparece en pantalla con sus palabras como bloques visuales independientes. El alumno la lee. No hay instrucciones visibles salvo el enunciado de la tarea. El profesor habrá explicado la mecánica en clase antes de la primera sesión.

| PASO 2 · Segmentación en sintagmas |
| :---- |

El alumno selecciona palabras (clic o toque) y las agrupa. Cuando ha seleccionado un grupo, elige de un menú compacto qué tipo de sintagma forma: SN, SV, SP, SAdj o SAdv. El bloque se colorea según el tipo. Los colores son fijos y conocidos por el alumno desde el inicio del curso.

| PASO 3 · Etiquetado de funciones |
| :---- |

Una vez agrupado un sintagma, el alumno elige su función sintáctica en la oración: Sujeto, CD, CI, CC (con especificación de tipo), Atributo, Complemento predicativo, Complemento del agente o CRV. El etiquetado es obligatorio antes de pasar al paso siguiente.

| PASO 4 · Análisis interno (opcional) |
| :---- |

El alumno puede abrir cualquier sintagma ya etiquetado para trabajar su estructura interna: identificar núcleo, determinante, adyacente, enlace, término. Este paso es optativo en v1 y puede activarlo o desactivarlo el profesor según el nivel del grupo.

| PASO 5 · Comprobación y feedback |
| :---- |

El alumno pulsa «Comprobar». El sistema revisa su análisis contra la solución de referencia y las alternativas válidas almacenadas en el JSON de la oración. Cada sintagma recibe uno de tres estados: correcto, alternativa aceptable o incorrecto con explicación.

## **3.2 Interacción con la oración**

Todas las interacciones se diseñan para funcionar en Chromebook (ratón y táctil):

* Un clic sobre una palabra la selecciona (resaltado visual).

* Clics adicionales amplían la selección a palabras contiguas.

* Un clic sobre una palabra ya seleccionada la deselecciona.

* Mantener pulsado sobre un sintagma ya creado permite deshacerlo y volver a segmentar.

* No hay drag & drop en v1: la selección por clic es más fiable en táctil y reduce la carga cognitiva.

## **3.3 Restricciones de diseño pedagógico**

Estas restricciones son intencionales y no deben eliminarse aunque sea técnicamente posible:

* El alumno no puede etiquetar un sintagma sin haberlo segmentado primero (la función emerge de la forma, no al revés).

* El sistema no sugiere agrupaciones posibles: el alumno construye desde cero.

* El feedback completo solo aparece al final, no en cada acción. Esto evita que el alumno use el feedback como guía en tiempo real, lo que convertiría la tarea en un test guiado.

* No hay límite de tiempo en v1. La reflexión tiene el tiempo que necesite.

# **4\. Modelo lingüístico**

## **4.1 Alcance sintáctico de v1**

La versión 1 trabaja exclusivamente con oraciones simples de predicado verbal. Este es el tipo de oración que el alumno analiza con más frecuencia en ESO y en los ejercicios de preparación para la EBAU Murcia.

Quedan fuera de v1:

* Oraciones copulativas con predicado nominal (Atributo con ser/estar/parecer) — se incluirán en v2 si el proyecto avanza.

* Oraciones impersonales con se o con verbos meteorológicos.

* Oraciones pasivas reflejas o pasivas perifrásticas.

* Oraciones de predicado con verbo en forma no personal.

Esta restricción no es una limitación técnica: es una decisión didáctica. Introducir demasiadas estructuras en la v1 diluye el foco y complica el motor de validación sin añadir valor pedagógico proporcional.

## **4.2 Inventario de categorías**

| Tipos de sintagma reconocidos |
| :---- |

| Etiqueta | Color asignado | Descripción |
| :---- | :---- | :---- |
| SN | Azul | Sintagma nominal: núcleo sustantivo o pronominal |
| SV | Verde | Sintagma verbal: núcleo verbo conjugado |
| SP | Naranja | Sintagma preposicional: preposición \+ término |
| SAdj | Morado | Sintagma adjetival: núcleo adjetivo |
| SAdv | Teal | Sintagma adverbial: núcleo adverbio |

| Funciones sintácticas reconocidas |
| :---- |

| Etiqueta | Forma esperada | Puede ser ambigua |
| :---- | :---- | :---- |
| Sujeto | SN (explícito o elíptico) | Sí: algunos SP pueden ser sujeto de pasiva |
| CD | SN o SP con 'a' (CD persona) | Sí: distinción CD/CRV a veces controvertida |
| CI | SP con 'a' o 'para' | No en v1 |
| CC Lugar | SAdv o SP | Sí: puede solaparse con CRV en algunos verbos |
| CC Tiempo | SAdv o SP | No |
| CC Modo | SAdv o SP | No |
| CC Causa | SP o cláusula | No en v1 (sin subordinadas) |
| Atributo | SN, SAdj, SP, SAdv | Sí: distinción atributo/CPred en algunos casos |
| CPred | SAdj o SN | Sí: orientación al sujeto o al CD |
| CRV | SP con preposición lexicalizada | Sí: el más problemático para los alumnos |
| C. Agente | SP con 'por' | Solo en pasiva perifrástica (no en v1) |

## **4.3 El problema de las soluciones múltiples**

La sintaxis del español admite en muchos casos más de una lectura correcta. Esto es una realidad lingüística que la app no puede ni debe ignorar. El modelo de validación trabaja así:

* Cada oración tiene una «solución canónica» definida por el profesor al introducir la oración.

* Junto a la solución canónica, el profesor puede definir «alternativas aceptables» para cada sintagma o para la oración completa.

* El sistema valida primero contra la solución canónica. Si no hay coincidencia, valida contra las alternativas.

* Si el análisis del alumno coincide con una alternativa, el feedback lo indica explícitamente: «Correcto. Hay otra lectura posible que también es válida.»

Esto evita que el sistema penalice análisis lingüísticamente legítimos y modela ante el alumno que la gramática no siempre tiene una sola respuesta.

## **4.4 Tratamiento del sujeto elíptico**

El español admite sujeto no expreso (sujeto elíptico o desinencial). Esto plantea un problema de interfaz: ¿cómo señala el alumno un sujeto que no aparece en la cadena de palabras?

La solución propuesta para v1:

* Un botón específico «Sujeto elíptico» en el panel de etiquetado.

* Al pulsarlo, el alumno debe escribir o seleccionar de un desplegable la persona gramatical del sujeto (1ª sg, 2ª pl, etc.).

* El sistema valida tanto la identificación del sujeto como la persona indicada.

Esta solución obliga al alumno a razonar sobre la desinencia verbal, que es precisamente la prueba lingüística del sujeto elíptico. Tiene valor pedagógico directo.

# **5\. Modelo de datos**

## **5.1 Estructura JSON de una oración**

Cada oración que el profesor introduce en la aplicación se almacena como un objeto JSON. Este JSON contiene la oración, sus tokens, la solución canónica, las alternativas válidas y los mensajes de feedback. El profesor edita este JSON directamente (o con una interfaz de administración en versiones futuras).

A continuación se muestra la estructura completa con la oración de ejemplo:

«Ayer los dos hermanos pequeños de María entregaron rápidamente a su profesora de Lengua un precioso ramo de flores del jardín.»

| {   "id": "oracion\_001",   "texto": "Ayer los dos hermanos pequeños de María entregaron...",   "nivel": "3ESO",   "tokens": \[     { "id": 0, "forma": "Ayer", "lema": "ayer", "categoria": "Adv" },     { "id": 1, "forma": "los", "lema": "el", "categoria": "Det" },     ...   \],   "solucion\_canonica": {     "sintagmas": \[       {         "id": "s1",         "tokens": \[0\],         "tipo": "SAdv",         "funcion": "CC\_Tiempo",         "nucleo": 0       },       ...     \]   },   "alternativas": \[     { "sintagma\_id": "s3", "tipo\_alt": "SP", "funcion\_alt": "CC\_Lugar", "nota": "..." }   \],   "feedback": {     "errores\_comunes": \[       { "patron": "SP\_como\_CC\_en\_SN", "mensaje": "Este SP modifica al sustantivo, no al verbo..." }     \]   } } |
| :---- |

## **5.2 Campo por campo**

| Campo | Tipo | Descripción |
| :---- | :---- | :---- |
| id | string | Identificador único de la oración |
| texto | string | Oración completa como cadena de texto |
| nivel | string | Nivel educativo: '2ESO', '3ESO', '4ESO', '1BACH' |
| tokens | array | Lista de palabras con id, forma, lema y categoría morfológica |
| solucion\_canonica.sintagmas | array | Lista de sintagmas con tokens asignados, tipo, función y núcleo |
| alternativas | array | Lecturas alternativas aceptables para sintagmas concretos |
| feedback.errores\_comunes | array | Patrones de error previsibles con mensajes de explicación |

## **5.3 Dónde se almacena**

En v1, los JSONs de oraciones se almacenan directamente en el archivo JavaScript del frontend como un array de objetos. No se necesita backend para leer oraciones. Esto simplifica el despliegue y permite usar la app incluso sin conexión activa al servidor GAS.

Los resultados del alumno, si se desea registrarlos, se envían al backend GAS mediante una llamada a google.script.run al finalizar la sesión. El backend los escribe en una hoja de cálculo. Esta parte es optativa en v1.

# **6\. Sistema de feedback**

## **6.1 Principio de diseño del feedback**

El feedback de esta aplicación sigue el principio de la gramática pedagógica: explicar el error en términos lingüísticos, no solo marcar correcto o incorrecto. Un alumno que ve ✘ sin explicación no aprende nada nuevo. Un alumno que ve ✘ con «Este SP depende del nombre ramo, no del verbo. Fíjate en qué pregunta responde: ¿de dónde? pero en relación con ramo, no con entregaron» tiene una oportunidad real de revisar su análisis.

## **6.2 Tres niveles de resultado**

| Símbolo | Significado | Qué ve el alumno |
| :---- | :---- | :---- |
| ✔ | Correcto | Sintagma coloreado en verde. Ningún mensaje adicional. |
| ◎ | Alternativa válida | Sintagma coloreado en azul claro. Mensaje: «Tu análisis es válido. Hay otra lectura también correcta: \[explicación\].» |
| ✘ | Error | Sintagma coloreado en rojo. Mensaje de explicación \+ indicación de dónde buscar la solución correcta. |

## **6.3 Plantillas de mensajes de feedback**

Los mensajes de feedback no son genéricos. Cada oración tiene sus propios mensajes definidos en el JSON, escritos por el profesor. Las plantillas son el andamiaje que el profesor usa para redactarlos:

| Plantilla para error de función |
| :---- |

«Has clasificado \[grupo de palabras\] como \[función errónea\]. Sin embargo, \[grupo de palabras\] \[prueba sintáctica: qué pregunta responde / si se puede sustituir por pronombre / si se puede pronominalizar\]. Por eso su función es \[función correcta\].»

| Plantilla para error de tipo de sintagma |
| :---- |

«Has etiquetado \[grupo de palabras\] como \[tipo erróneo\]. Fíjate en el núcleo del grupo: \[núcleo\] es un \[categoría gramatical correcta\]. Por eso el sintagma es un \[tipo correcto\].»

| Plantilla para alternativa válida |
| :---- |

«Tu análisis es lingüísticamente aceptable. También es posible interpretar \[grupo de palabras\] como \[función/tipo alternativo\] porque \[razón\]. Ambas lecturas son válidas.»

## **6.4 Lo que el feedback NO hace**

* No da la solución directamente antes de que el alumno la pida. El alumno debe pulsar «Ver solución» expresamente, y queda registrado que lo hizo.

* No guía al alumno durante la construcción. El feedback solo aparece al finalizar.

* No penaliza con puntuación negativa. Los errores son oportunidades de aprendizaje, no demérito.

# **7\. Diseño de interfaz**

## **7.1 Principios de diseño UX**

* Una pantalla, una tarea. No hay menús de navegación, no hay instrucciones largas, no hay elementos decorativos. La oración es lo primero que ve el alumno al cargar la app.

* La carga cognitiva se gestiona mediante la revelación progresiva. El panel de etiquetado de funciones solo aparece después de que el alumno ha seleccionado y tipado un sintagma.

* El color es información, no decoración. Cada tipo de sintagma tiene un color fijo que el alumno aprende durante el curso. Los colores se establecen en la primera sesión y no cambian.

* La tipografía es legible a distancia. La aplicación debe poder usarse en un Chromebook escolar en una clase con luz variada. Tamaño mínimo de fuente para el texto de la oración: 20px.

## **7.2 Distribución de la pantalla**

La pantalla principal tiene tres zonas:

| Zona | Contenido | Altura aproximada |
| :---- | :---- | :---- |
| Zona superior | Enunciado breve de la tarea. Nunca más de dos líneas. | 10% de la pantalla |
| Zona central (oración) | Bloques de palabras con colores de sintagma. Es el espacio de trabajo principal. | 50% de la pantalla |
| Zona inferior (panel) | Panel de etiquetado contextual: aparece al seleccionar palabras. Botón «Comprobar». | 40% de la pantalla |

No hay barra lateral. No hay menú superior. La pantalla es la tarea.

## **7.3 Panel de etiquetado**

Cuando el alumno selecciona una o más palabras, aparece el panel de etiquetado en la zona inferior. El panel muestra:

* Las palabras seleccionadas (confirmación visual de lo que ha seleccionado).

* Botones de tipo de sintagma: SN / SV / SP / SAdj / SAdv. Solo uno puede estar activo.

* Botones de función sintáctica: aparecen después de elegir el tipo. El sistema filtra qué funciones son posibles según el tipo de sintagma elegido (un SAdv no puede ser Sujeto; un SN no puede ser CC).

* Botón «Confirmar sintagma». El grupo queda registrado y los bloques de palabras se colorean.

* Botón «Cancelar». Deshace la selección sin registrar nada.

## **7.4 Paleta de colores propuesta**

| Elemento | Color (hex) | Uso |
| :---- | :---- | :---- |
| SN | \#2E6DA4 (azul) | Bloques del sintagma nominal |
| SV | \#2A7A4B (verde) | Bloques del sintagma verbal |
| SP | \#C05A00 (naranja) | Bloques del sintagma preposicional |
| SAdj | \#6B3FA0 (morado) | Bloques del sintagma adjetival |
| SAdv | \#1A8080 (teal) | Bloques del sintagma adverbial |
| Sin etiquetar | \#F0F0F0 (gris claro) | Palabras no agrupadas aún |
| Feedback ✔ | \#D6F0E3 (verde claro) | Sintagma correcto tras comprobar |
| Feedback ◎ | \#D6E4F7 (azul claro) | Alternativa válida tras comprobar |
| Feedback ✘ | \#FDE8E8 (rojo claro) | Error tras comprobar |

## **7.5 Tipografía propuesta**

| Uso | Fuente | Tamaño |
| :---- | :---- | :---- |
| Palabras de la oración | Georgia o Lora (serif) | 22–24px |
| Etiquetas de sintagma | Inter o DM Sans (sans-serif) | 13–14px, bold |
| Panel y botones | Inter o DM Sans | 15–16px |
| Feedback | Inter o DM Sans | 14px |

El contraste entre la tipografía serif de la oración y la sans-serif del panel refuerza visualmente la distinción entre el objeto de estudio (la oración) y las herramientas de análisis (el panel).

# **8\. Arquitectura técnica**

## **8.1 Principio de mínima complejidad técnica**

El Analizador de Oraciones es una aplicación educativa, no una plataforma. La arquitectura técnica debe estar al servicio de la pedagogía, no al revés. La regla de oro de este proyecto es: añadir complejidad técnica solo cuando aporte valor pedagógico demostrable.

En v1, la app puede existir como un único archivo HTML. No necesita base de datos, no necesita backend activo durante la sesión, no necesita autenticación. Si se quiere registrar resultados, se añade un backend GAS mínimo. Nada más.

## **8.2 Estructura de archivos (GAS WebApp)**

| Archivo | Responsabilidad |
| :---- | :---- |
| Code.gs | doGet(): sirve el HTML. saveResult(): escribe en Sheets si se activa el registro. |
| Index.html | Toda la lógica de la aplicación: HTML \+ CSS \+ JavaScript en un único archivo. |
| oraciones.json (embebido) | Array de objetos JSON con las oraciones y sus soluciones. Incluido en el JS del HTML. |

Esta estructura es deliberadamente minimalista. Un solo archivo HTML es suficiente para v1 y reduce el riesgo de errores de integración en GAS.

## **8.3 Lo que ocurre en el frontend (sin servidor)**

7. Al cargar la página, el JS selecciona una oración del array (aleatoria o según parámetro URL).

8. Renderiza los tokens como elementos HTML interactivos.

9. Gestiona la selección de tokens y el estado de los sintagmas en un objeto JavaScript en memoria.

10. Al pulsar «Comprobar», compara el estado en memoria con la solución canónica del JSON.

11. Muestra el feedback directamente en el DOM sin ninguna llamada al servidor.

12. Opcionalmente, al finalizar, envía el resultado al backend GAS mediante google.script.run.

## **8.4 Lo que ocurre en el backend (opcional en v1)**

Si el profesor activa el registro de resultados, Code.gs expone una función saveResult() que recibe:

* Identificador del alumno (nombre o email de Google, si el acceso es mediante Google Workspace).

* Id de la oración analizada.

* El análisis completo del alumno como JSON serializado.

* Timestamp de la sesión.

* Resultado global: porcentaje de sintagmas correctos.

Esta información se escribe en una hoja de cálculo de Google Sheets. El profesor puede revisarla después de clase.

## **8.5 Compatibilidad**

| Dispositivo | Soporte previsto |
| :---- | :---- |
| Chromebook (táctil) | Diseño principal. Todo debe funcionar con toque sin hover. |
| Chromebook (teclado \+ ratón) | Totalmente compatible. Atajos de teclado en versiones futuras. |
| Tablet Android/iOS | Compatible por diseño táctil desde el principio. |
| Ordenador de aula | Compatible. Pantallas de más de 1024px muestran más contexto. |
| Móvil | No es el objetivo, pero la interfaz debe ser funcional en pantallas de 375px. |

# **9\. Conexión con la didáctica de la gramática contextual**

## **9.1 Reflexión metalingüística en la interfaz**

La reflexión metalingüística no es un método de enseñanza que se añade a una aplicación: es una forma de diseñar las tareas. En este proyecto, los principios de la gramática pedagógica se traducen en decisiones concretas de diseño:

| Principio pedagógico | Decisión de diseño |
| :---- | :---- |
| El alumno debe manipular el lenguaje, no solo observarlo | El análisis se construye mediante selección y etiquetado, no mediante elección entre opciones |
| La función sintáctica emerge de la forma, no al revés | El sistema obliga a etiquetar el tipo de sintagma antes de asignar la función |
| El error es parte del aprendizaje | El feedback explica el error lingüísticamente; no hay penalización numérica |
| La gramática tiene casos límite y debate | Las alternativas válidas están explícitamente marcadas y explicadas |
| La prueba de conmutación es la herramienta del lingüista | Los mensajes de feedback mencionan pruebas: pronominalización, sustitución, pregunta de función |

## **9.2 Alineación con LOMLOE**

La aplicación se alinea con los descriptores operativos de la competencia específica 5 de Lengua Castellana y Literatura en ESO (reflexión sobre el funcionamiento de la lengua) y con los criterios de evaluación relacionados con el análisis sintáctico en 3º y 4º de ESO y en Bachillerato:

* Reconocer y explicar los grupos sintácticos dentro de la oración simple.

* Identificar las funciones sintácticas de los grupos nominales, verbales, preposicionales y adverbiales.

* Aplicar el conocimiento gramatical para mejorar la producción y comprensión de textos.

La app no sustituye la evaluación formal de estos criterios, pero proporciona práctica deliberada y registro de errores que puede informar la evaluación del profesor.

## **9.3 Uso previsto en el aula**

El Analizador de Oraciones está pensado para tres contextos de uso:

| Contexto | Cómo se usa | Duración típica |
| :---- | :---- | :---- |
| Práctica dirigida en clase | El profesor proyecta la misma oración para todos. Los alumnos trabajan en su Chromebook. Se pone en común al final. | 20–30 minutos |
| Práctica autónoma | El alumno accede desde casa o en tiempo libre. Trabaja oraciones del nivel asignado. | 10–20 minutos por oración |
| Evaluación formativa | El profesor activa el registro de resultados. Revisa los análisis en Sheets después de clase. | Sin tiempo límite |

# **10\. Hoja de ruta del proyecto**

## **10.1 Versión 1 · Lo que se construye ahora**

El objetivo de v1 es validar la mecánica central con una única oración real en clase. Todo lo demás es prescindible en esta fase.

| Funcionalidad | Incluida en v1 | Notas |
| :---- | :---- | :---- |
| Tokenización y render de la oración | Sí | Array de tokens en JS. Sin procesamiento automático. |
| Selección de tokens por clic | Sí | Selección múltiple de palabras contiguas o no contiguas. |
| Etiquetado de tipo de sintagma | Sí | SN, SV, SP, SAdj, SAdv con colores. |
| Etiquetado de función sintáctica | Sí | Lista filtrada según tipo de sintagma. |
| Sujeto elíptico | Sí | Botón específico \+ campo de persona gramatical. |
| Feedback al finalizar | Sí | Comparación contra JSON de referencia. |
| Alternativas válidas | Sí | Definidas en el JSON por el profesor. |
| Análisis interno del sintagma | Parcial | Solo núcleo en v1. Determinante y adyacente en v2. |
| Múltiples oraciones / selección | No | Una oración por sesión en v1. |
| Registro de resultados en Sheets | Opcional | Se activa si el profesor lo necesita. |
| Panel docente | No | Versión futura si el registro de datos se prueba útil. |
| Gamificación | No | Fuera de alcance en exploración. |
| Modo oscuro | No | Fuera de alcance en v1. |

## **10.2 Criterios de éxito de v1**

La v1 se considerará exitosa si:

* Un alumno de 3º ESO puede completar el análisis de una oración sin instrucciones escritas en pantalla, solo con la explicación previa del profesor.

* El feedback es comprensible para el alumno sin ayuda del profesor.

* La app funciona sin errores en un Chromebook escolar en la red del centro.

* El profesor puede introducir una oración nueva en menos de 30 minutos editando el JSON.

## **10.3 Posibles versiones futuras**

Solo si v1 se valida en clase:

| Versión | Novedad principal |
| :---- | :---- |
| v2 | Múltiples oraciones con selección por nivel. Análisis interno completo (determinante, adyacente, complemento del nombre). Registro de resultados activado por defecto. |
| v3 | Panel docente mínimo: tabla de resultados por alumno. Comparación de análisis entre alumnos. Exportación a PDF del análisis del alumno. |
| v4 | Oraciones de predicado nominal (Atributo). Oraciones pasivas. Primer banco de oraciones de EBAU Murcia. |
| v5 (especulativa) | Integración con el Taller de Sintaxis: oraciones compartidas entre ambas apps. Análisis del proceso del alumno (orden de construcción, tiempo por sintagma). |

## **10.4 Lo que este GDD no decide**

Este documento es de exploración. Las siguientes decisiones quedan deliberadamente abiertas para después de la primera prueba en clase:

* Si el registro de resultados se activa o no en v1.

* Si la interfaz de introducción de oraciones (para el profesor) se construye como panel web o como edición directa del JSON.

* Si la app se despliega como GAS WebApp independiente o como módulo embebido en el Taller de Sintaxis.

* Qué oraciones concretas forman el primer banco de v1 (eso lo decide el profesor según lo que esté trabajando en clase).

Analizador de Oraciones · GDD v1.0

Documento de exploración · Junio 2026 · Sin código comprometido