---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones: añadir documentación al código, crear una sección de juegos relacionados para que el equipo de diseño pueda probarla e implementar funciones de accesibilidad con controles de modo de alto contraste y modo claro. Revisa los cambios y, si es necesario, proporciona comentarios a Copilot para perfeccionar el resultado.

### Escenario

Los principios de diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las sugerencias de Copilot para la documentación, la función de juegos relacionados y la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

Como el agente realiza tareas de forma asíncrona y sin supervisión, se aplican medidas de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utiliza para su trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) generada por Copilot: la que añade documentación al código. Para esta tarea utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> En la PR puede aparecer un aviso de que el firewall bloqueó a GitHub Copilot. Es **normal**: el acceso a recursos externos, incluidas las llamadas a servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, abre la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Revisa el código actualizado, incluidos los comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Busca también las convenciones de TypeScript y los encabezados de comentario indicados por las instrucciones personalizadas que revisaste al inicio del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que algunos flujos de trabajo esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la solicitud. Confirma que se superan las comprobaciones del proyecto de la aplicación Astro. Puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la solicitud o en comentarios sobre una línea de código. Copilot los detectará e iniciará otra sesión para responder. Como los resultados son probabilísticos, no hay un prompt único que debas utilizar. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario breves al principio de cada archivo de código.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite cambios en la documentación generada y menciona **@copilot** como mencionarías a otra persona. Puedes usar una de las ideas anteriores o pedir otra mejora.
2. Selecciona **View Session** para ver el trabajo de Copilot y observa que inicia una sesión nueva.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la solicitud.
5. Abre **Files changed** y revisa los cambios. Puedes seguir iterando hasta quedar conforme. Después, convierte la PR de borrador en una solicitud lista para revisión y fusiónala.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que Copilot ha creado para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay una indicación de que algunos flujos de trabajo esperan aprobación.
7. Si la hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Confirma que se superan las comprobaciones del proyecto de la aplicación Astro; puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en el codespace para probar manualmente la función. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes pedirle más cambios mediante comentarios en la pestaña **Conversation**, por ejemplo, ajustar el número de juegos que se muestran o añadir encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debería incluir el modo de alto contraste asignado en el Ejercicio 3 y el modo claro solicitado desde mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud mostrará la marca **[WIP]**. Espera a que termine.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los controles de la interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript o TypeScript que gestiona los modos.
6. Cuando hayas revisado los cambios y estés conforme, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay una indicación de que algunos flujos de trabajo esperan aprobación.
8. Si la hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Confirma que se superan las comprobaciones del proyecto de la aplicación Astro; puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que las preferencias persisten al recargar la página.

El agente personalizado de accesibilidad ha guiado a Copilot para que implemente las funciones siguiendo buenas prácticas. Si observas problemas o posibles mejoras de accesibilidad, menciona a **@copilot** en un comentario para solicitar cambios, como hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle trabajo real del backlog y dedicarte a otras tareas. Crea más incidencias y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para quienes apoyan los juegos en la página de detalles.
- implementar paginación en la página con la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- has revisado los archivos de instrucciones personalizados del repositorio para reconocer su efecto en el trabajo del agente.
- has asignado incidencias al agente de GitHub Copilot en la nube y has visto cómo prepara su entorno, planifica y trabaja de forma asíncrona.
- has creado y utilizado un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- has utilizado la página de agentes de Copilot como centro de control para supervisar y dirigir la sesión.
- has revisado e iterado sobre las PR, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Repaso y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ [Recorrido de VS Code](../../vscode/): explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 [Recorrido de CLI](../../cli/): entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

Prueba estas ideas en tu propio repositorio:

- asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
