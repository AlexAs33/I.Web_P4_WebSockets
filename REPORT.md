# Lab 4 WebSocket -- Project Report

## Description of Changes
Se completó el fichero `ElizaServerTest.kt` rellenando únicamente los comentarios numerados que indicaban qué se debía implementar o explicar. El resto del código (clases de test, clientes WebSocket, estructura general) ya estaba implementado previamente.

Las modificaciones realizadas fueron:

1. **Comentario 1 - Explicación de `size = list.size`**: Se explicó que es necesario capturar el tamaño de la lista en una variable local antes de las aserciones para tener un valor estable, ya que el tamaño puede variar entre 4 y 5 debido a condiciones de carrera.

2. **Comentario 2 - Implementación de aserción con rango**: Se reemplazó el `assertEquals` por `assertTrue(size in 4..5)` para verificar que el tamaño de la lista esté dentro del rango esperado, en lugar de forzar un valor exacto.

3. **Comentario 3 - Explicación sobre el uso de rangos**: Se explicó que `assertEquals` no puede usarse porque el tamaño puede variar entre 4 y 5 dependiendo del dispositivo y timing de ejecución, y que forzar uno de los dos valores haría el test frágil.

4. **Comentario 4 - Completar aserción**: Se completó la línea con `assertEquals("Please don't apologize.", list[3])`, verificando que la respuesta de DOCTOR al mensaje "sorry" sea la esperada, ya que es una respuesta única y determinista.

## Technical Decisions

**Uso de rangos para aserciones no deterministas**: La decisión más importante fue utilizar `assertTrue` con un rango de valores (4-5) en lugar de `assertEquals` con un valor fijo. Esto se debe a que la comunicación WebSocket es asíncrona y el timing puede variar según el sistema, haciendo que el número de mensajes recibidos fluctúe ligeramente.

**Captura anticipada del tamaño**: Capturar `size = list.size` antes de realizar las aserciones evita que el valor cambie durante las verificaciones si llegan mensajes adicionales de forma asíncrona. Esta es una práctica común en testing concurrente.

**Selección de "sorry" como mensaje de prueba**: Se eligió "sorry" porque, tras investigación con IA, se confirmó que es una de las pocas frases con respuesta determinista en ELIZA ("Please don't apologize."), lo que permite una verificación fiable del comportamiento conversacional del servidor.

## Learning Outcomes

Durante el desarrollo de esta práctica he aprendido:

1. **Testing de sistemas asíncronos**: Comprendí la importancia de usar mecanismos de sincronización como `CountDownLatch` y por qué las aserciones deben adaptarse a la naturaleza no determinista de sistemas concurrentes.

2. **Aserciones robustas**: Aprendí que no todos los tests pueden usar `assertEquals` con valores exactos. Cuando hay factores de timing involucrados, es preferible verificar rangos de valores aceptables para hacer los tests más robustos y menos propensos a fallos intermitentes.

3. **Condiciones de carrera en tests**: Entendí cómo las condiciones de carrera pueden afectar los resultados de un test y la importancia de capturar valores en variables locales antes de hacer múltiples verificaciones sobre ellos.

4. **Arquitectura de WebSocket**: Profundicé en cómo funciona la comunicación bidireccional mediante WebSocket, el ciclo de vida de las conexiones y cómo los clientes anotados con `@ClientEndpoint` gestionan mensajes.

5. **Comportamiento de ELIZA**: Conocí el funcionamiento del chatbot ELIZA y la importancia de identificar respuestas deterministas para poder realizar tests efectivos.

## AI Disclosure

### AI Tools Used
- Claude

### AI-Assisted Work
Para el desarrollo de esta práctica únicamente se usó la inteligencia artificial para discernir si había alguna frase a la que DOCTOR respondiese siempre de la misma manera. Como se ve en el desarrollo de esta práctica, su respuesta fue que a `sorry` DOCTOR siempre responde `Please don't apologize.`

Esta información fue crucial para completar el comentario 4, permitiendo implementar una verificación determinista del comportamiento del servidor.

### Original Work
Todo el trabajo de implementación fue realizado por mí. La IA solo proporcionó la información sobre qué mensaje tenía respuesta determinista en ELIZA. Las explicaciones en los comentarios 1, 2 y 3, así como la implementación de las aserciones, son trabajo original mío basado en mi comprensión del código y los conceptos de testing concurrente.