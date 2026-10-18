---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades que llevan tiempo en el backlog porque no hay tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para actualizar código y añadir funciones de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en problemas más amplios.

En esta lección explorarás cómo:

- personalizar el entorno de generación de código.
- realizar las operaciones de forma segura.
- redactar incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. La documentación del sitio no está en buen estado; de hecho, falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles muestre otros juegos de la misma categoría para que la gente pueda seguir explorando. Por ahora, basta con una implementación sencilla para probar la experiencia de usuario; esta tarea tiene menor prioridad que otras incidencias.

Son ejemplos de trabajo que puede aplazarse y que conviene asignar al agente de GitHub Copilot en la nube. Mientras trabaja de forma asíncrona, puedes ocuparte de otras tareas y volver después para revisar los resultados.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios en borrador para mostrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no es mágico: aún tienes que pensar. Hasta las operaciones que parecen sencillas pueden tener aspectos complejos, así que [define cuidadosamente el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Antes de asignarle trabajo al agente en la nube, revisa en modo de solo lectura los archivos del repositorio para reconocer su efecto más adelante.

Abre estos archivos en github.com o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube, busca comentarios TSDoc, convenciones TypeScript y encabezados de comentario en la PR resultante. Estas pautas proceden de los archivos de instrucciones y volverás a revisarlas en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

Para crear código suele hacer falta un entorno con herramientas y scripts de configuración. Esto también se aplica al agente de Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] para crear su entorno. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar el trabajo. La plantilla del laboratorio ya incluye este flujo. Instala Node.js y las dependencias, descarga los navegadores de prueba y configura la base de datos SQLite utilizada por la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *estos pasos*. Copilot recibirá su propio token para realizar sus operaciones.
    permissions:
      # Para clonar el repositorio como parte de la configuración (por ejemplo, para instalar dependencias), necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación usa Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en la base SQLite local para que haya datos para las compilaciones y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay dos puntos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutarlo manualmente desde la pestaña **Actions** del repositorio para probarlo.

## Añadir documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica reduce la productividad y dificulta el mantenimiento o la incorporación de nuevos desarrolladores. Copilot puede generar documentación, así que esta incidencia es ideal para el agente en la nube.

1. Abre el repositorio en github.com en una pestaña nueva.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo mencionado antes), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la PR; debería aparecer una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de una sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador nuevo.

16. Observa la sesión en directo y cómo Copilot explora el código y analiza el estado, se detiene para decidir el plan adecuado y crea código.

El agente puede tardar varios minutos. Una de sus ventajas principales es el trabajo asíncrono: mientras se ocupa de esta tarea, puedes dedicarte a otra. Aprovecharemos esa posibilidad y le asignaremos una segunda incidencia.

## Añadir una sección de juegos relacionados

El agente en la nube permite dividir el trabajo para que puedas ocuparte de otras tareas mientras él aborda una incidencia. Asignémosle el trabajo de la sección de juegos relacionados que solicita el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después aparecerá un emoji 👀 en el primer comentario de la incidencia, señal de que Copilot está trabajando en ella.

![Copilot muestra el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. Como el agente en la nube trabaja de forma autónoma, no es necesario supervisarlo continuamente; revisa el resultado cuando termine. Ahora puedes centrarte en crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- realizar las operaciones de forma segura.
- redactar incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes [crear y utilizar agentes personalizados][next-lesson]. El agente en la nube también puede utilizar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
