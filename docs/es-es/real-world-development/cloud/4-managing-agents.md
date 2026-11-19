---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargara de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de agente con solicitudes de cambios abiertas, examinar las operaciones realizadas e incluso orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una función para cambiar de modo, parecía lógico incluirlo. Orientarás el trabajo del agente para que añada ambos modos.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado actual de las tareas que has asignado al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las acciones que ha realizado Copilot, que pueden incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar varios pasos para completar la tarea.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de la solicitud de cambios (PR) de la derecha. Permite consultar la PR y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas en curso, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo recibe y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estuviera trabajando al recibir el mensaje.

Como el proceso tardará varios minutos, puedes aprovechar para repasar todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Puedes utilizarla para seguir las tareas activas y completadas, revisar el trabajo e incluso redirigir las sesiones en curso para cambiar su alcance o proporcionar otras indicaciones.

Has explorado estos conceptos:

- has utilizado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
