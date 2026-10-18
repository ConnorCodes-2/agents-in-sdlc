---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, como un agente de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas seguras o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen con archivos Markdown en `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialidad y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente con Markdown e indican a una IA cómo realizar tareas. La forma más sencilla de distinguirlos es que un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad. Como parte de su trabajo, podría invocar una habilidad para gestionar una solicitud de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Prueba e itera hasta encontrar lo que mejor se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de su capacidad visual o preferencias. Algunos comentarios recientes indican que cuesta leer el tema oscuro actual debido al poco contraste entre el texto y el fondo. El equipo de diseño ha solicitado implementar un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, quieres implementar esta función cuanto antes y utilizarás un agente personalizado.

## Revisar el agente personalizado de accesibilidad

Ya hay un agente de accesibilidad preparado. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve al codespace y abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - problemas y errores frecuentes.

## Crear y asignar una incidencia

Mission control es el lugar central para trabajar con agentes. Permite asignar tareas al agente de GitHub Copilot en la nube, supervisarlas y redirigirlas o proporcionar indicaciones adicionales. Asígnale la tarea de implementar el modo de alto contraste.

1. Abre el repositorio en github.com.
2. Selecciona **Issues** y después **New issue**.
3. Selecciona **Blank issue**.
4. Como **Title**, escribe `Add high contrast mode to website`.
5. Como descripción, escribe:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el panel derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Asignación del agente en la nube con el agente de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos, y has asignado una tarea a uno de ellos. Los agentes permiten convertir la experiencia y los estándares del equipo en instrucciones reutilizables.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
