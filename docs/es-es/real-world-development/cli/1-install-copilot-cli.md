---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu codespace, familiarízate con la herramienta y encuentra la incidencia de filtrado inicial."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación agéntico que se ejecuta en el terminal para explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas. Permite delegar tareas, solicitar cambios y mantener la concentración. Para empezar, instalarás la herramienta con npm.

En esta lección, harás lo siguiente:

- instalar GitHub Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- autorizar el repositorio del taller y probar una conversación.
- encontrar la incidencia de filtrado con el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI incorpora esta capacidad al terminal, donde ya trabajan muchos desarrolladores. Esta lección te ayudará a instalar la herramienta, autenticarte y prepararte para el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. GitHub Codespaces incluye Node.js, así que usarás npm.

1. Vuelve al codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple los requisitos de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debe aparecer el número de versión.

## Autenticarse en GitHub

La primera vez que inicies Copilot CLI, se te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un codespace puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya puedes continuar.

## Familiarizarse con la herramienta

Los comandos escritos en el prompt normal del shell se ejecutan directamente en el codespace. En Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que busque la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Indícame su URL y un breve resumen. No cambies nada.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de realizar un primer cambio breve.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama seleccionada en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Normalmente, Copilot CLI pide autorización antes de usar herramientas fuera de los permisos establecidos. Para este taller, lo reiniciarás con `--yolo`, un atajo aprobado por ti que elimina estas solicitudes dentro del codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

Para las lecciones posteriores, `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub. El codespace limita el acceso al ordenador local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o fusionarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm.
- te has autenticado con tu cuenta de GitHub.
- has autorizado el repositorio del taller y probado una conversación.
- has encontrado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [realizarás tu primer cambio específico][next-lesson] y mostrarás la valoración por estrellas en las tarjetas de juegos con Copilot CLI.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
