---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot te permiten crear asistentes de IA especializados y adaptados a tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio que le ayuden a realizar determinados tipos de trabajo con más eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la especialidad y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Hay cierta superposición entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown y explican a la IA cómo realizar tareas. La distinción más sencilla es que un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para orquestar habilidades e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad; para ello, podría invocar habilidades como una de flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como ocurre con cualquier función de IA, prueba e itera para averiguar qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque no hay suficiente contraste entre el texto y los colores del fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado implementar un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes. Utilizarás un agente personalizado para generarla.

En este ejercicio, harás lo siguiente:

- revisar un agente personalizado de accesibilidad existente.
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revísalo para entender cómo guiará a Copilot.

Vuelve al codespace, abre un terminal y cambia a una rama nueva basada en `main` para trabajar en la accesibilidad por separado de la PR de filtrado del Ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código para HTML, CSS y JavaScript.
   - una lista de problemas y errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes de la parte inferior de Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La elección del modo y del modelo varía según el ejercicio; cada paso lo indica cuando es relevante.

1. Selecciona **Agent** en el selector de agentes de la vista Chat si todavía no está seleccionado.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes, situado en la parte inferior de la vista Chat (muestra el agente activo, que de forma predeterminada es **default**).
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía el siguiente prompt al agente de accesibilidad:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe haber un control para activar y desactivar el alto contraste; la preferencia de la persona usuaria debe persistir entre recargas de página mediante el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat, con la ruta del archivo y un diff que podrás revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> Este proceso probablemente tardará unos minutos. Copilot realizará cambios en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas, si las hay.

En el próximo ejercicio revisarás y dirigirás el trabajo mientras se desarrolla.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Con ellos, puedes convertir la experiencia y los estándares del equipo en agentes reutilizables que guíen a Copilot para realizar determinados tipos de trabajo con más eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se produzcan y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
