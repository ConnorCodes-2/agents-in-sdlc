---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente trabaja de forma asíncrona y autónoma, es importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas con solicitudes de incorporación de cambios abiertas, revisar las operaciones y dirigir una sesión en curso.

En esta lección, harás lo siguiente:

- explorar la página de agentes para supervisar las tareas del agente en la nube.
- dirigir una sesión en curso y solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaban modificando los estilos del sitio y añadiendo controles, es lógico incluir también esta función. Dirigirás el trabajo del agente para que implemente ambos modos.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado de las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel lateral. Deberías ver tareas como:
   - actualizar la documentación del código.
   - generar API para modificar los juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - ejecutar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque utilizado.

4. Observa el panel de solicitudes de incorporación de cambios, situado a la derecha. Permite consultar la PR y los archivos modificados.

## Dirigir el agente en la nube

Ahora que has revisado las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la implementación del modo de alto contraste. El título exacto dependerá del estado y del nombre elegido por Copilot.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que termine la configuración y empiece el trabajo. Lo sabrás cuando empiecen a aparecer mensajes de actividad.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo y los usuarios deben poder seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido la indicación y la incorpora al trabajo.

## Dejar que Copilot trabaje

Copilot continuará la tarea actualizada. Incorporará la nueva petición cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar indicaciones.

En esta lección:

- has utilizado la página de agentes para supervisar tareas del agente en la nube.
- has redirigido una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
