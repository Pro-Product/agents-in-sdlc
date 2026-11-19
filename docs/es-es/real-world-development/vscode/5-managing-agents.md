---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat para VS Code muestra cada llamada a herramientas y cada edición de archivo. Puedes revisar cada diff en línea, aceptar o rechazar los cambios y guiar la conversación con prompts de seguimiento para refinar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- guiarás al agente durante la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido una función más: un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de empezar una conversación nueva, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas: lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá el diff en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** situado en la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Guía al agente para que añada el modo claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la misma sesión, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el punto en que se quedó, explorará los cambios realizados y propondrá cómo añadir el modo claro.
3. Revisa cada cambio nuevo, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no te convence —por ejemplo, intenta rediseñar los controles de los modos de una forma que no te gusta— puedes detenerlo con el botón **Stop** junto al campo de entrada del chat y enviar un prompt de seguimiento para aclarar lo que quieres.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina los diff. Confirma que la implementación sigue las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en almacenamiento local.
4. Cuando estés satisfecho, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has explorado:

- cómo supervisar el trabajo de un agente en tiempo real.
- cómo aceptar o rechazar ediciones de archivos.
- cómo guiar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás tu trabajo de accesibilidad en una solicitud de cambios y revisarás los cambios que ha producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
