---
title: "Ejercicio 3: Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás una solicitud de incorporación de cambios.

En este ejercicio, harás lo siguiente:

- aprender qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas disponibles para Copilot.
- añadir el servidor MCP de Playwright al espacio de trabajo.
- pedir a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicar la rama y abrir una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o las herramientas locales, por ejemplo, los paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub. El agente puede, por ejemplo, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. El agente puede, por ejemplo, navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para mejorar su descubrimiento y facilitar las contribuciones al ecosistema.

> [!CAUTION]
> Por seguridad, trata los servidores MCP como cualquier otra dependencia de tu proyecto. Antes de usar un servidor MCP, revisa detenidamente su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Verás que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Esta entrada `github` viene incluida en la plantilla del proyecto, por lo que Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server]. No es necesario instalarlo localmente: VS Code se conecta a través de HTTP y te autenticas en GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo tenga este aspecto:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un codespace, donde no hay un escritorio para mostrarlo.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas end-to-end, así que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot informa más adelante de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **a petición**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparece una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá autorización la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de comprobar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente utiliza elementos estructurados y etiquetados —como botones, enlaces y elementos de lista—, igual que la tecnología de asistencia. Una comprobación funcional rápida también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Usa el servidor MCP de Playwright para abrir un navegador en la aplicación en ejecución, en http://localhost:4321, y verifica la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Bórralo y aplica un filtro de editorial; confirma que la lista muestra solo los juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, realizará cada paso y te informará de los resultados. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar ejecutándose en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero, publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, en github.com o con la interfaz de línea de comandos de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree: puede encontrar la incidencia pertinente y asociarla a la solicitud.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Asegúrate de que **Agent** esté seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una solicitud:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y asóciala con la incidencia correcta.
   ```

4. Si se te solicita, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte; sigue las instrucciones para autorizarlo.
5. Observa cómo Copilot busca las incidencias, encuentra la correcta y crea la solicitud.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
