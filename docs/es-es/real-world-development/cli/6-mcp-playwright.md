---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Configura el servidor MCP de Playwright en Copilot CLI y úsalo para explorar el filtrado en el navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, escribir código implica algo más que programar. También hay que trabajar con datos y servicios externos y proporcionar automatizaciones adicionales a Copilot. Los servidores MCP permiten que Copilot vaya más allá de las funciones integradas en CLI y utilice otras herramientas y servicios.

En esta lección, harás lo siguiente:

- aprender qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadir el servidor MCP de Playwright si todavía no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y end-to-end son importantes, pero para validar los cambios en la interfaz hay que interactuar con ella. Permitirás que Copilot use el sitio como lo haría una persona para automatizar la comprobación de los cambios y aumentar la confianza en que se comportan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a las herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] para mejorar su descubrimiento y facilitar las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Como nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y luego <kbd>Tab</kbd>.
5. Pega este comando en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Antes has confirmado manualmente que la funcionalidad funciona. Ahora pedirás a Copilot que haga lo mismo con el servidor MCP de Playwright.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y el plan acordado. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que hayas iniciado cuando termines.
   ```

> [!NOTE]
> No es necesario indicar a Copilot que use el servidor MCP, porque normalmente lo detecta por sí mismo. Aun así, especificarlo puede ayudarle a elegir la herramienta adecuada, obtener resultados más coherentes y ahorrar tokens.

2. Observa cómo Copilot indica los pasos que realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
