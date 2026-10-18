---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos en una aplicación suelen requerir actualizar varios archivos y ejecutar pruebas. El flujo de trabajo habitual de desarrollo implica localizar todos los archivos pertinentes, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a actualizarlo. La lista continúa.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo usar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE para facilitar la interacción y recibir comentarios rápidamente. El agente en la nube trabaja de forma asíncrona, como un colaborador del equipo, y se comunica contigo a través de incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto e identificar los archivos pertinentes y los cambios coordinados.
- implementar funcionalidades que abarquen la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genera el modo agente antes de fusionarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que se puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás con el modo agente de Copilot el trabajo restante de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para familiarizarte con sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar los juegos y ver sus detalles. Es una aplicación Astro única que genera páginas HTML estáticas en tiempo de compilación. Las páginas consultan una base de datos SQLite local directamente desde el frontmatter mediante Drizzle ORM: no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`; la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API estándar del DOM.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un aviso con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir el navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, seleccionar el enlace a localhost en el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos añadirle funciones. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo. Durante la creación del taller usamos Claude Sonnet 4.5 y por eso lo indicamos en estas instrucciones. El objetivo es que las sugerencias de código sean relativamente coherentes y la experiencia resulte cómoda. Sin embargo, como los LLM son probabilísticos, es normal que las sugerencias difieran de los ejemplos.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto y, aun así, completar la tarea.

3. Envía a Copilot este prompt para que consulte el backlog de incidencias:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según cuáles sean más útiles para el usuario.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista que genera Copilot.

Observa que Copilot también ha priorizado los elementos según cuáles cree que serán más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le proporcionará contexto. Utilizarás el archivo de interfaz de usuario (UI) [`ui.instructions.md`](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa sus directrices para añadir funcionalidades, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, facilidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar y dividir conjuntos de instrucciones grandes en archivos más pequeños, así como proporcionar a Copilot ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o perfeccionar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página con la lista de juegos para introducir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios en el código base. El modo agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de ejemplo para seleccionar el archivo de instrucciones de interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia pertinente del backlog:

   ```plaintext
   Actualiza el sitio para que permita filtrar por editorial y categoría según los requisitos de la incidencia correspondiente de GitHub en el backlog. Ya existe un helper de editoriales del ejercicio anterior: consérvalo y perfecciónalo si es necesario mientras completas el resto del trabajo en la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución, así que no es necesario iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: helpers de la capa de datos, la interfaz y las pruebas. A continuación, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, podrás aceptar o revertir cada cambio; no hace falta que selecciones ningún botón mientras siga trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que fallen algunas pruebas durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta que termina y no detecta errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que es importante revisar siempre el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has usado el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido que:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y en la capa de datos.
- el modo agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- es necesario revisar los cambios y las pruebas generados antes de fusionarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo aprendido en este ejercicio y pide a Copilot que actualice el sitio. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes y utiliza los archivos de instrucciones del proyecto.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si será fijo.
- proporciona a Copilot las pautas necesarias en el prompt.
- quizá tengas que iterar con GitHub Copilot y pedir cambios o proporcionar contexto adicional. Ese es el flujo normal de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
