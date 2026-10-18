---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para modificar las tarjetas de juegos, revisa el cambio en el navegador reenviado y fusiónalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página de inicio no la muestran. Pedirás al agente que la presente, revisarás el cambio y lo fusionarás en tu primera solicitud de incorporación de cambios.

En esta lección, harás lo siguiente:

- iniciar una conversación centrada en una rama de funcionalidad.
- pedir al agente que realice un pequeño cambio en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación y comprobarla en el navegador reenviado.
- abrir y fusionar tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y acotado, ideal para la primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama seleccionados en el terminal en lugar de crear un worktree dedicado. Las conversaciones guardadas te permiten volver al mismo diálogo; los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tus prompts y las respuestas del agente, la actividad de herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Inicia una conversación para empezar a implementar la funcionalidad.

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Muestra la propiedad starRating de cada juego sobre 5 en las tarjetas de juegos de la página de lista. Si la valoración es null, muestra "No rating yet". Conserva el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles del juego y creará el código necesario. ¡Has añadido una funcionalidad con Copilot CLI!

## Revisar el diff

Antes de fusionar los cambios generados por IA, aunque sean pequeños, revísalos.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los fallos.
5. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista diff.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica al ejemplo.

## Comprobar los cambios

No basta con leer el código y dar por supuesto que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda comprobar en el navegador el cambio de las valoraciones por estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y fusionar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora crearás una solicitud de incorporación de cambios (PR) para integrar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Crea un commit con los cambios revisados de las valoraciones por estrellas y un mensaje adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI publicará el commit existente al crear la PR y mostrará la URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad.
- has indicado al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- has revisado el cambio con `/diff`.
- has iniciado la aplicación y comprobado las valoraciones en un navegador reenviado.
- has abierto y fusionado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para implementar una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
