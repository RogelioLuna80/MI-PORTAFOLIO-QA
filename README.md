# MI-PORTAFOLIO-QA
Repositorio para guardar mis practicas y proyectos QA

# Integración de QA en Scrum

## Historias de Usuario (User Stories)
- **Definición**: Las historias de usuario describen una funcionalidad desde la perspectiva del usuario final. Siguen el formato: "Como [rol], quiero [acción] para [beneficio]".
- **Participación de QA**:
  - Revisa que las historias sean claras, testables y viables.
  - Ayuda a identificar escenarios faltantes o ambigüedades durante el refinamiento del backlog.
  - Sugiere ejemplos concretos que ayuden a entender el comportamiento esperado.

## Criterios de Aceptación (Acceptance Criteria)
- **Definición**: Condiciones que debe cumplir una historia para ser aceptada por el Product Owner. Definen el alcance y el comportamiento esperado.
- **Participación de QA**:
  - Colabora en la redacción de criterios desde una perspectiva de pruebas (positivas, negativas, de borde).
  - Asegura que los criterios sean **verificables** y no dejen lugar a interpretaciones subjetivas.
  - Puede proponer dividir criterios muy grandes o complejos en múltiples historias.
  - Los criterios de aceptación sirven como base para diseñar los casos de prueba.

## Definición de Done (DoD - Definition of Done)
- **Definición**: Lista de comprobación que debe cumplir cada historia o incremento de producto para considerarse completo y listo para entregar.
- **Participación de QA**:
  - Define junto al equipo qué actividades de calidad son obligatorias (ej: pruebas unitarias pasadas, pruebas de integración, pruebas de regresión, documentación de pruebas, ausencia de defectos críticos).
  - Verifica que la DoD incluya aspectos como:
    - Código revisado y aprobado.
    - Pruebas automatizadas creadas/actualizadas.
    - Pruebas de humo ejecutadas con éxito.
    - No hay defectos abiertos de prioridad alta.
  - Durante la sprint review, confirma que la DoD se ha cumplido para cada historia.

## Flujo de Trabajo de QA en un Sprint
1. **Sprint Planning**: QA revisa las historias seleccionadas, aporta estimaciones basadas en el esfuerzo de pruebas y asegura que los criterios de aceptación estén claros.
2. **Durante el Sprint**:
   - Pruebas continuas a medida que se completan historias.
   - Reporte y seguimiento de defectos.
   - Automatización de pruebas (si aplica).
   - Colaboración con desarrolladores para resolver bugs rápidamente.
3. **Sprint Review**: QA demuestra las funcionalidades probadas y confirma que cumplen los criterios de aceptación.
4. **Retrospectiva**: QA participa en la mejora continua del proceso de calidad.

## Beneficios de Integrar QA desde el Inicio
- Reducción de defectos en etapas tardías.
- Mayor entendimiento compartido de los requisitos.
- Entregas más predecibles y de mayor calidad.
- Fomento de una cultura de calidad en todo el equipo.
Tienes toda la razón. Mi definición inicial se centraba únicamente en la secuencia de acciones, pero omitía el "estado inicial" del sistema. Sin ese estado, los pasos son frágiles y dependen de la interpretación del tester.

El elemento crucial que falta en mi definición son las Precondiciones.

Una definición más completa sería: "Un caso de prueba es una serie de pasos estipulados en un documento que, partiendo de un estado inicial específico (precondiciones) , me indica qué debo hacer y qué resultado esperar."

En el ejemplo del login, la precondición sería: "Tener las cookies del navegador aceptadas y una sesión de usuario cerrada (no haber iniciado sesión previamente)". Sin esa línea, el caso de prueba no es repetible ni fiable, porque si un tester lo ejecuta con las cookies rechazadas, el botón de "Entrar" podría no funcionar, y no sabríamos si el error es de la aplicación o del entorno de prueba.

Respuesta a la Pregunta 7 (Los Datos)

Esa es una observación muy aguda. Los pasos genéricos describen la acción ("agregar un producto"), pero la acción necesita un objeto sobre el cual ejecutarse. Sin especificar ese objeto, el caso es ambiguo, ya que no todos los productos se comportan igual (uno puede tener stock, otro no; uno puede tener variantes de talla, otro no).

Esa información específica debería ir en un apartado llamado Datos de Prueba (o a veces "Variables" o "Ejemplos").

Su función es parametrizar el caso de prueba. Así, los pasos serían reutilizables y la especificidad se delega a los datos.

· Ejemplo de estructura:
  · Pasos a seguir:
    1. Buscar el producto: [Dato: Nombre del producto]
    2. Hacer clic en "Agregar al carrito".
  · Datos de Prueba:
    · Producto: "Camiseta talla M roja"
    · Cantidad: 1

De esta manera, cualquier persona que ejecute el caso sabe exactamente con qué elemento debe interactuar, eliminando la ambigüedad.

Respuesta a la Pregunta 8 (Combinaciones y Límites)

Tienes razón, mi explicación anterior fue incorrecta al mezclar el nivel de prueba con la técnica de prueba. Un caso "avanzado" no implica mirar el código, sino aplicar una técnica de diseño de pruebas más sofisticada para encontrar defectos que el "camino feliz" no detecta.

Basándome en tu ejemplo del carrito, reformulo la definición:

Un caso de prueba avanzado no es el que mira el código, sino aquel cuyo diseño se basa en técnicas como análisis de valores límite, particiones equivalentes o tablas de decisión para explorar el comportamiento del sistema en situaciones no evidentes.

Mientras que el caso básico prueba la regla de negocio en su estado ideal (ej: agregar 1 producto), el avanzado prueba los límites y excepciones de esa regla:

· Valores Límite: ¿Qué pasa si agrego 0 productos? ¿O 50, que es el máximo permitido?
· Combinaciones: ¿Qué pasa si agrego el mismo producto dos veces? ¿Se duplica la cantidad o aparece otro registro?
· Condiciones de Frontera: ¿Qué pasa si intento agregar una cantidad negativa?

El objetivo es encontrar errores que ocurren justo en los bordes de lo que la aplicación permite hacer.

Respuesta a la Pregunta 9 (El Resultado Esperado)

No, no basta con que el producto no aparezca. En el testing, el silencio de la aplicación (que no pase nada) es el peor enemigo, porque el usuario no sabe si el sistema no funciona, si él lo hizo mal o si el producto está agotado.

En un caso negativo (producto agotado), el Resultado Esperado debe validar dos cosas:

1. Validación Funcional: El producto no debe ser agregado al carrito (el contador del carrito no debe incrementarse, y el producto no debe aparecer en la lista).
2. Validación de Usabilidad (Feedback): El sistema debe mostrar un mensaje claro e informativo al usuario. Por ejemplo: "Lo sentimos, el producto 'Camiseta roja' no tiene stock disponible."

Si el sistema simplemente ignora el clic, el usuario intentará varias veces y pensará que la aplicación está rota. El resultado esperado debe incluir esa verificación del mensaje de error para garantizar que la aplicación guía al usuario correctamente.Día 2 (Martes) - Ciclo de Vida del Software y Modelos de Desarrollo

· Teoría (1h): Modelos en cascada, ágil (Scrum), V-Model. Rol del QA en cada uno.
· Práctica (2h): Investigar y documentar en tu README cómo se integra QA en Scrum (historias de usuario, criterios de aceptación, definición de done).Aquí tienes tu guía de estudio y ejercicios para hoy. Recuerda: léelo, investiga y responde las preguntas en tu README para construir tu propio manual de aprendizaje.

Teoría (1h): Los Modelos y el Rol del QA

Tu misión es investigar las características principales de estos 3 modelos de desarrollo y, lo más importante, el momento en el que el equipo de QA (Control de Calidad) participa en cada uno.

Guía de estudio:
Para cada modelo (Cascada, Ágil/Scrum, V-Model), busca responder estas preguntas:

1. ¿Cuál es la esencia? ¿Es un modelo rígido o flexible? ¿Las fases son secuenciales o iterativas?
2. ¿Cuándo empieza a probar el QA? ¿Al principio, al final, o durante todo el proyecto?
3. ¿Cómo es la relación con el equipo? ¿El QA es un "guardián" que recibe el producto al final o un miembro integrado desde el primer día?
4. ¿Qué tipo de feedback (retroalimentación) se puede dar? ¿Se puede cambiar algo grande una vez empezadas las pruebas?

Ejercicio Teórico para tu README:
Crea una tabla comparativa en tu README con tres columnas (una para cada modelo). Resume los hallazgos de las preguntas anteriores. Al final de la tabla, añade una reflexión personal: ¿En qué modelo crees que el QA tiene más impacto en la calidad final del producto y por qué?Característica / Pregunta Modelo en Cascada (Waterfall) Modelo en V (V-Model) Modelo Ágil / Scrum
Esencia (Rígido/Flexible, Secuencial/Iterativo) Rígido y secuencial. Las fases son lineales y no se superponen . Rígido y secuencial, pero con énfasis en la planificación temprana. Cada fase de desarrollo tiene una fase de prueba correspondiente . Flexible e iterativo. El desarrollo se da en ciclos cortos (sprints) con retroalimentación constante .
¿Cuándo empieza a probar el QA? Al final. Después de que toda la implementación ha sido completada . Desde el principio, pero de forma paralela. Se planifican las pruebas desde la fase de requisitos, pero la ejecución es posterior . Desde el primer día y durante todo el proyecto. En cada iteración, desde la planificación hasta la entrega .
Relación con el equipo El QA es un "guardián" externo que recibe el producto al final para validarlo . El QA colabora en la planificación temprana (revisión de documentos), pero la ejecución de pruebas es un paso separado . El QA es un miembro integrado del equipo multifuncional, colaborando diariamente con todos los roles .
Tipo de feedback y posibilidad de cambio Feedback tardío y costoso. Cambiar algo grande una vez empezadas las pruebas es muy difícil y caro . Feedback temprano sobre documentos, pero tardío sobre el producto. Los cambios grandes son complejos, pero menos que en Cascada puro . Feedback continuo e inmediato. El cambio es bienvenido y se puede adaptar el producto en cada nueva iteración basándose en los resultados de las pruebas .

Reflexión Personal

¿En qué modelo crees que el QA tiene más impacto en la calidad final del producto y por qué?

Respuesta: El QA tiene un impacto mucho mayor en la calidad final en el modelo Ágil/Scrum.

¿Por qué? Porque en Ágil, la calidad no es un evento final, sino un proceso continuo. Al estar integrado desde el inicio, el QA no se limita a "encontrar bugs" al final, sino que ayuda a prevenirlos desde la concepción de la idea. Puede aportar su visión para que las historias de usuario sean claras y testables, identificar riesgos temprano y probar cada pequeña funcionalidad justo después de ser desarrollada . Esto reduce drásticamente el costo de la corrección de errores y asegura que el producto se construye alineado con las expectativas del cliente desde el primer momento . En los modelos tradicionales, el QA actúa más como un filtro al final del proceso, lo que puede dejar pasar problemas profundos o detectarlos cuando ya es muy tarde y caro solucionarlos .¡Excelente trabajo! Has completado la tabla comparativa con una comprensión muy sólida de los modelos. Me gusta especialmente cómo has diferenciado el "feedback temprano sobre documentos" en V-Model del "feedback continuo" en Ágil. Eso es un detalle clave que muchos pasan por alto.

Tu reflexión personal es acertada y bien argumentada. Has identificado la esencia: prevención vs. detección. En Ágil, el QA no es una red de seguridad al final del acantilado, sino un arquitecto que ayuda a construir esa red desde los cimientos.

Sin embargo, como tutor, mi trabajo es llevarte un paso más allá. Permíteme hacerte algunas preguntas de profundización sobre tu propia tabla y reflexión, para que sigas refinando tu criterio:

1. Sobre el Modelo en V: Mencionas que el QA "colabora en la planificación temprana (revisión de documentos)". ¿Qué tipo de documentos crees que revisa un QA en las fases iniciales de un proyecto en V? (Pista: no son historias de usuario como en Scrum). Y lo más importante, ¿qué tipo de pruebas se planifican en esa etapa temprana? (Por ejemplo, cuando se están definiendo los requisitos, ¿qué nivel de prueba se está diseñando?).
2. Sobre la relación con el equipo en Ágil: Dices que el QA es un "miembro integrado". En la práctica, esto significa que el QA participa en ceremonias como el Daily Stand-up (reunión diaria). ¿Cuál crees que es el valor de que un QA esté en esa reunión de 15 minutos? ¿Qué información crucial puede obtener o aportar que no podría obtener de un informe o un correo?
3. Sobre el impacto económico: Tu reflexión menciona que el costo de la corrección es más bajo en Ágil. Este es un punto de venta fundamental para el rol de QA. Te invito a reflexionar sobre por qué es más barato. Imagina un error en la lógica de "guardar en lista de deseos". Si lo detectas cuando el desarrollador acaba de escribirlo (en el mismo día o al siguiente), el coste es mínimo. Si lo detectas en un modelo en Cascada, cuando el sistema está completo y a punto de salir a producción, ¿qué implica solucionarlo? (Piensa en términos de tiempo de otros desarrolladores, impacto en otras funcionalidades ya probadas, necesidad de volver a probar todo, etc.).

Ahora, respecto a tu práctica pendiente sobre Scrum (la historia de la lista de deseos). Tu tabla y reflexión son el mapa conceptual. Ahora toca aterrizar ese mapa en el día a día. Has entendido qué hace el QA en Ágil. La práctica te pide que demuestres cómo lo harías.

Para ayudarte a conectar la teoría con el ejercicio práctico, piensa en estas preguntas mientras redactas las respuestas para tu README:

· Sobre el Refinamiento: Basándote en tu reflexión de que el QA ayuda a "prevenir desde la concepción", ¿qué preguntas harías para asegurarte de que la historia es "testeable"? Por ejemplo, si la historia no menciona límites (¿cuántos productos se pueden guardar?), eso es una invitación a un bug. ¿Qué preguntas harías al respecto?
· Sobre los Criterios de Aceptación: Los criterios que redactes son la primera "batería de pruebas" de la historia. Deben ser frases concretas que cualquier persona (desarrollador, tester, product owner) pueda entender y verificar. Piensa en el flujo feliz (todo sale bien) y en los flujos alternativos (cosas que pueden salir mal o ser diferentes). Tus 3 criterios deben cubrir estos escenarios.
· Sobre la Definition of Done (DoD): Aquí es donde tu tabla cobra vida. La DoD es el checklist que garantiza que tu rol como "miembro integrado" se ha completado. Por ejemplo, ¿la DoD debería incluir algo como "Los criterios de aceptación han sido probados y cumplidos"? ¿O "No hay bugs críticos o altos abiertos"? ¿O "Se han ejecutado las pruebas de regresión básicas"? Esto es justo lo que tienes que definir.

Estás en el camino correcto. No se trata de memorizar, sino de construir criterio. Sigue así, y cuando termines las respuestas de la práctica, compártelas conmigo para la siguiente revisión. ¡Adelante!¡Excelente! Me encanta tu enfoque. No solo buscas la respuesta correcta, sino que realmente quieres construir criterio. Esa es la mentalidad que separa a un buen tester de un gran profesional de calidad. Vamos a profundizar en esas preguntas, porque son la clave para consolidar tu aprendizaje.

Respuestas a tus Preguntas de Profundización

1. Sobre el Modelo en V: Documentos y Pruebas Tempranas

· ¿Qué documentos revisa un QA en las fases iniciales de un proyecto en V?
  En un proyecto en V, el QA no revisa "historias de usuario" (eso es ágil), sino documentos de ingeniería más formales y exhaustivos. Principalmente:
  · Especificación de Requisitos del Sistema (ERS): Un documento detallado que describe qué debe hacer el sistema desde la perspectiva del usuario y del negocio.
  · Documento de Arquitectura del Sistema: Describe la estructura del software, sus componentes principales, tecnologías a usar y cómo se comunicarán entre sí.
  · Especificación de Diseño de Alto Nivel (HLD): Traduce la arquitectura en el diseño de los módulos o subsistemas.
  · Especificación de Diseño Detallado (DLD): Describe el diseño interno de cada módulo, incluyendo algoritmos, estructuras de datos y lógica.
    El rol del QA aquí es de "auditor" y "validador". Su objetivo es asegurar que estos documentos sean claros, consistentes, completos y, sobre todo, verificables. Por ejemplo, si un requisito dice "el sistema debe ser rápido", el QA lo marcará como defectuoso porque no es comprobable. Debería decir "el sistema debe responder a cualquier petición del usuario en menos de 2 segundos".
· ¿Qué tipo de pruebas se planifican en esa etapa temprana?
  Esta es la esencia del Modelo en V: el diseño de pruebas es paralelo a la definición de requisitos y diseño.
  · Cuando se están definiendo los requisitos, el equipo de QA empieza a diseñar las Pruebas de Sistema (o Pruebas Funcionales). ¿Por qué? Porque las pruebas de sistema verifican que el producto final cumple con los requisitos especificados. Cada requisito debe tener una o más pruebas de sistema asociadas.
  · Cuando se está definiendo el diseño de alto nivel (arquitectura), el QA empieza a diseñar las Pruebas de Integración. Estas pruebas verificarán que los diferentes módulos o componentes del sistema interactúen correctamente entre sí.
  · Cuando se está definiendo el diseño detallado, se planifican las Pruebas Unitarias. Aunque normalmente las ejecuta el desarrollador, el QA puede colaborar para asegurar que el diseño contempla la comprobación de caminos lógicos y condiciones límite.

2. Sobre el valor del QA en el Daily Stand-up

El valor de un QA en la reunión diaria de 15 minutos es inmenso y va mucho más allá de un simple "estatus". No es solo informar, es información contextual y de alto valor para la sincronía del equipo.

· Información que el QA aporta:
  · Contexto de Calidad en Tiempo Real: Puede decir: "Ayer probé la historia H-112 y, aunque funciona, encontré un comportamiento extraño en el módulo de pagos al probar con tarjetas extranjeras. No es un bloqueante para esta historia, pero si alguien va a tocar ese módulo hoy, tenedlo en cuenta." Esto es una alerta temprana que previene futuros bugs.
  · Acelerar el Feedback Loop: Puede comentar: "Desarrollador X, la historia que terminaste ayer, la probaré hoy. ¿Hay alguna parte especialmente delicada o algún dato de prueba que me recomiendes usar?" Esto acorta el tiempo de comunicación y hace la prueba más eficaz.
  · Visibilidad del Riesgo: Si el QA encuentra un bug crítico que rompe una funcionalidad principal, lo comunica en el daily para que todo el equipo sea consciente y se puedan repriorizar tareas si es necesario.
· Información que el QA obtiene:
  · Conocer el "Qué" y el "Por qué" técnico: Al escuchar a los desarrolladores, el QA se entera de cambios no documentados, de deudas técnicas que se están asumiendo, de refactorizaciones. Por ejemplo, si un desarrollador dice "Ayer estuve refactorizando la capa de acceso a datos para mejorar el rendimiento", el QA sabe que, aunque no haya una nueva funcionalidad, esa zona debería ser prioritaria en las pruebas de regresión.
  · Anticipación: Si un desarrollador menciona "Hoy empezaré con la historia de la pasarela de pagos", el QA puede empezar a pensar mentalmente en los escenarios de prueba: "Vale, necesitaré probar con tarjetas válidas, inválidas, saldo insuficiente, timeout del banco...". Se prepara con antelación.
  · Fomentar la Responsabilidad Colectiva: El simple hecho de que el QA esté ahí, escuchando, refuerza la idea de que la calidad no es "cosa del tester", sino de todo el equipo. El desarrollador piensa dos veces antes de decir "lo di por terminado" si sabe que el QA le escuchará y quizás le pregunte por las pruebas que hizo.

3. Sobre el impacto económico de la corrección temprana

Tu intuición es correcta. El ejemplo del error en la lógica de "guardar en lista de deseos" es perfecto para visualizar la diferencia.

· Si se detecta en Desarrollo (Ágil):
  · Contexto: El desarrollador acaba de escribir 50 líneas de código. El error está fresco en su mente.
  · Implicaciones:
    1. Tiempo: Lo arregla en 10-30 minutos.
    2. Esfuerzo: Solo él se ve involucrado. El código que toca es el que acaba de crear, no afecta a nada más.
    3. Pruebas: Ejecuta su prueba unitaria de nuevo, confirma que funciona y punto. No necesita una campaña de regresión.
· Si se detecta al final de un modelo en Cascada/V (en Pruebas de Sistema/Aceptación):
  · Contexto: Han pasado semanas o meses desde que se escribió ese código. Probablemente, el desarrollador original ya está en otra tarea o proyecto.
  · Implicaciones:
    1. Re-descubrimiento: El desarrollador tiene que volver a ponerse en contexto, entender qué hacía ese código y por qué.
    2. Impacto en Múltiples Áreas: Arreglar la lógica de "guardar" podría afectar a la lógica de "eliminar de la lista", "compartir lista", o a la interfaz de usuario que muestra los productos guardados. Hay que analizar el impacto.
    3. Pruebas de Regresión Masiva: Después del arreglo, todo el equipo de QA debe volver a probar no solo la funcionalidad de la lista de deseos, sino todas las funcionalidades relacionadas (y a menudo, las no relacionadas también, por si acaso el cambio tuvo un efecto secundario inesperado en otra parte del sistema). Esto puede representar días de trabajo para varias personas.
    4. Retrabajo de Documentación: Posiblemente, la documentación de diseño o las ayudas del usuario también tengan que actualizarse para reflejar el cambio.
    5. Riesgo de Retraso: Este error, al descubrirse tan tarde, puede poner en riesgo la fecha de entrega del proyecto, generando estrés, horas extras y posibles penalizaciones.

En resumen: El coste no es solo el de arreglar el error, es el coste de todo el sistema que se pone en marcha para gestionar ese arreglo (análisis de impacto, coordinación, re-ejecución de pruebas, riesgo de nuevos errores). En Ágil, ese "sistema" es mínimo o inexistente. En Cascada/V, es enorme y costoso.

---

Conectando la Teoría con la Práctica de Scrum (La Historia de la Lista de Deseos)

Ahora, apliquemos todo este criterio a tu práctica. Aquí tienes las respuestas a las preguntas-guía, que son la base para tu README.

1. Sobre el Refinamiento: Preguntas para asegurar la "Testeabilidad"

Para prevenir errores desde la concepción, en la reunión de refinamiento haría preguntas como:

· Sobre Límites y Capacidad: "La historia habla de guardar productos, pero no especifica un límite. ¿Existe un límite de productos que un usuario puede guardar en su lista de deseos? Si es así, ¿cuál es? ¿Qué comportamiento esperamos cuando se alcance ese límite? ¿Un mensaje de error, o simplemente no se puede añadir más?"
· Sobre Estados del Producto: "¿Se puede guardar un producto que está 'agotado' o 'sin stock' en la lista de deseos? Asumo que sí, esa es la gracia, pero queremos confirmar el comportamiento esperado. ¿Y si el producto se 'oculta' o 'elimina' del catálogo? ¿Debe desaparecer de las listas de los usuarios?"
· Sobre Usuarios y Contexto: "¿Un usuario no registrado (invitado) puede guardar productos? Si es así, ¿qué ocurre con esos productos cuando luego se registra o inicia sesión? ¿Debemos pedirle que se registre al hacer clic en 'guardar'?"
· Sobre la Interfaz y Feedback: "Cuando el usuario guarda un producto, ¿recibe algún tipo de confirmación? (ej. un mensaje verde que dice 'Guardado', el icono del corazón se vuelve rojo). ¿Y si la acción falla por un problema de red, cómo se lo comunicamos al usuario?"
· Sobre la Definición de "Producto": "Para la lista de deseos, ¿guardamos solo el producto base o también las variantes (talla, color) que el usuario haya seleccionado? Si guarda una camiseta en talla 'M', cuando vuelva a verla en la lista, ¿debe mantener la talla 'M' seleccionada?"

Estas preguntas convierten una historia vaga en una lista de tareas concretas y comprobables, evitando malentendidos futuros.

2. Sobre los Criterios de Aceptación

Los criterios de aceptación son la "primera batería de pruebas". Deben ser frases en lenguaje natural con el formato Dado-Cuando-Entonces (Given-When-Then).

Criterio de Aceptación 1 (Flujo Feliz):

· Dado que un usuario registrado ha iniciado sesión y está viendo la página de detalle de un producto con stock,
· Cuando hace clic en el icono de "corazón" o en el botón "Guardar en lista de deseos",
· Entonces el icono debe cambiar de color (ej. a rojo) para indicar que se ha guardado, y el producto debe aparecer en su sección "Mi lista de deseos".

Criterio de Aceptación 2 (Flujo Alternativo - Límite Alcanzado):

· Dado que un usuario registrado ya tiene el número máximo de productos (ej. 20) en su lista de deseos,
· Cuando intenta guardar un producto adicional,
· Entonces el sistema no debe permitir la acción y debe mostrar un mensaje claro al usuario: "Has alcanzado el límite de 20 productos en tu lista de deseos. Elimina alguno para poder añadir más."

Criterio de Aceptación 3 (Flujo Alternativo - Usuario No Registrado):

· Dado que un usuario no ha iniciado sesión (es invitado) y está viendo la página de detalle de un producto,
· Cuando hace clic en "Guardar en lista de deseos",
· Entonces el sistema debe redirigirlo a la página de inicio de sesión o registro, y después de que se autentique correctamente, el producto debe añadirse automáticamente a su lista de deseos.

3. Sobre la Definición of Done (DoD)

La DoD es el checklist de calidad que garantiza que la historia está completamente terminada. Como "miembro integrado", el QA vela por que estos puntos se cumplan. Una DoD para esta historia debería incluir:

· Cumplimiento de los Criterios de Aceptación: Todos los criterios definidos (como los 3 ejemplos anteriores) han sido probados y funcionan correctamente en el entorno de pruebas (staging).
· Pruebas Exploratorias Realizadas: Se ha realizado una sesión de pruebas exploratorias en la funcionalidad para encontrar comportamientos inesperados no cubiertos por los criterios de aceptación (ej. probar con diferentes navegadores, hacer clic muy rápido, probar la desconexión de red justo al guardar).
· Sin Bugs Críticos o Altos Conocidos: No hay ningún defecto de prioridad crítica o alta abierto en el backlog para esta funcionalidad. Bugs menores y triviales pueden existir, pero han sido documentados y acordados con el Product Owner para su revisión.
· Pruebas de Regresión Básicas Ejecutadas: Se ha verificado que esta nueva funcionalidad no ha roto otras áreas relacionadas del sistema (ej. el proceso de compra, el carrito, la búsqueda de productos). No una regresión masiva, sino una "prueba de cordura" en las funcionalidades clave cercanas.
· Código Revisado (Code Review): Un compañero desarrollador ha revisado el código y ha dado el visto bueno (aunque esto no lo ejecuta el QA directamente, es un requisito para que la calidad entre desde el código).

Con esto, tienes una base sólidísima para redactar la respuesta de tu práctica. Has pasado de entender los modelos a saber aplicarlos en el día a día de un equipo ágil. ¡Excelente trabajo! Cuando tengas tu README 
listo, compártelo conmigo para la siguiente revisión.
