---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad con modos de alto contraste y claro. Explora los cambios de código y, si hace falta, proporciona comentarios a Copilot para mejorar su trabajo.

### Escenario

Como hemos recalcado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisa las sugerencias de GitHub Copilot para añadir documentación, juegos relacionados y funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para que el proceso siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, que crea un entorno separado y efímero para su trabajo.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por explorar la primera solicitud de cambios (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que al examinar la PR veas un aviso de que el firewall ha bloqueado a GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud de cambios con un título parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido hasta ahora.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que muestra que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. En la sección de comprobaciones de la solicitud, deberían aparecer los flujos en cola. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes etiquetarlo en comentarios de la PR, como harías con otras personas del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son probabilísticos, no podemos sugerir un texto exacto. Por ejemplo, puedes pedir a Copilot que:

- Añada al principio de cada archivo de código encabezados de comentarios que describan brevemente su contenido.
- Añada comentarios TSDoc a archivos TypeScript y Astro.
- Cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario con una solicitud de cambio a la documentación generada y etiqueta a **@copilot** como etiquetarías a cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia de documentación que quieras incluir en el código.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para efectuar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés satisfecho. Cuando termines, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marca la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot ha generado para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que muestra que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. En la sección de comprobaciones de la solicitud, deberían aparecer los flujos en cola. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre un terminal en el Codespace y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando con el agente en la nube para solicitar cambios, como ajustar el número de juegos relacionados o añadir encabezados de comentarios y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Para ello, añade otro comentario en la pestaña **Conversation** y Copilot iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad creadas con el agente personalizado. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la solicitud de cambios tendrá la marca **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz para activar los distintos modos.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS o estilo para los modos claro y de alto contraste.
   - Los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que muestra que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. En la sección de comprobaciones de la solicitud, deberían aparecer los flujos en cola. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre un terminal en el Codespace y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos claro y de alto contraste en el navegador. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha guiado a Copilot para que implemente las funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o mejora, puedes etiquetar a **@copilot** en un comentario y solicitar cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para indicar interés en una campaña en la página de detalles del juego.
- Implementar paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el entorno de agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas** incluidos en el repositorio para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para accesibilidad, con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de cambios del agente en la nube**, etiquetando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el entorno del agente en la nube. Para seguir explorando, puedes probar los otros entornos:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configuración del firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
