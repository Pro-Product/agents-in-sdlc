---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo habitual de desarrollo implica localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a cambiarlo y repetir el proceso.

El modo de agente de Copilot está diseñado para encargarse de este trabajo.

El modo de agente trabaja con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo puedes utilizar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube funciona como un compañero de programación que trabaja de forma asíncrona y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás:

- cómo el modo de agente de Copilot puede explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- cómo puede implementar funcionalidades en la interfaz y en la capa de datos.
- cómo revisar los cambios y las pruebas generadas antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editorial. En el ejercicio anterior ya añadiste un asistente para editoriales; ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay ninguna API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, el comando `npm run dev` inicia un único servidor de desarrollo Astro. Puedes ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá una notificación con la URL, parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace local desde el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, abre una página con más información, incluidos el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía entre ejercicios; se indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos añadir capacidades nuevas. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Al preparar el taller utilizamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias sean distintas a las que se describen en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot podría utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía a Copilot este prompt para consultar las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funciones, que incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad para probarlos y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar las instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios independientes en la aplicación:

- Añadir o mejorar la lógica de filtrado de la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas. Añade la funcionalidad:

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Utiliza este prompt para pedir a Copilot que implemente la funcionalidad descrita en la incidencia correspondiente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos necesarios. Debería encontrar los asistentes de la capa de datos, la interfaz y las pruebas. Después, empezará a modificar archivos y ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar varios minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Al terminar el trabajo, todos los cambios tendrán una opción **Keep** o **Undo**; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Chat con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Es importante revisar siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has visto cómo:

- el modo de agente puede implementar funciones en la interfaz y en la capa de datos.
- el modo de agente puede explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- se pueden revisar los cambios y las pruebas generadas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo que has aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo.
- En el prompt, proporciona a Copilot las indicaciones necesarias para crear la implementación deseada.
- Es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Ese es el flujo habitual de trabajo con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
