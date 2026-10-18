---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar el enfoque, Autopilot para desarrollar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una funcionalidad pequeña al proyecto, pero los cambios más amplios necesitan un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarte a crear lo correcto de la manera adecuada. En esta lección y las siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia, implementarás una funcionalidad, comprobarás que el código es correcto y que se comporta como esperas y, finalmente, la fusionarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de la funcionalidad con la misma conversación y rama. Normalmente usarías ramas o solicitudes de incorporación de cambios distintas para los diferentes tipos de archivos, pero en este taller simplificaremos el flujo para centrarnos en los conceptos principales.

En esta lección, harás lo siguiente:

- iniciar una conversación nueva de Copilot desde una incidencia de GitHub.
- definir los requisitos en modo Plan.
- implementar la funcionalidad con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Durante el trabajo también actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente QA y abrirás la PR.

## Escenario

El catálogo de Tailspin Toys crece y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de empezar a programar. Utilizarás el modo Plan para resolver esas decisiones y autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA no cambia los fundamentos del desarrollo; al contrario, los hace más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad nueva.
6. Crear una solicitud de incorporación de cambios (PR).
7. Después de la revisión del código y de que se complete correctamente la integración continua, fusionar los cambios.

> [!NOTE]
> Los detalles exactos dependen del equipo y de la organización, pero la mayoría de los procesos se parecen al que se indica aquí.

Este enfoque estándar ayuda a asegurar que el código generado por IA cumpla los requisitos y pase el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que la tarea está terminada.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar el filtrado, vuelve al codespace y prepara el repositorio y el terminal.

1. Vuelve al codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de las valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las flechas, resalta **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza por `#7` o por un número parecido. El carácter `#` permite incorporar al contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir el enfoque de una funcionalidad o tarea antes de delegarla en Copilot. Dedica tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia añadida antes, escribe este prompt:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que te haga preguntas sobre el comportamiento de los filtros. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que corrija las partes que no coincidan con tus decisiones.

## Aprobar Autopilot

Una vez escrito y revisado el plan, podrás implementarlo con Autopilot. Este modo permite que Copilot itere sobre el problema hasta considerar que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en la versión instalada.
2. Confirma que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias que faltan o de un conflicto de puerto, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y comprobar la implementación

Hay que revisar el código generado antes de fusionarlo, como cualquier otro cambio. Revisa el código e inicia el sitio para comprobar su funcionamiento.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier fallo.

## Explorar la funcionalidad

Ahora que el código parece correcto, comprueba que se ejecuta. Pide a Copilot que inicie la aplicación y abre el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar en el navegador la funcionalidad de filtrado. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas en el plan.
4. Comprueba que las opciones para restablecer los filtros y el comportamiento sin resultados coinciden con la incidencia y con tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot desde una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, [usarás instrucciones personalizadas][next-lesson] para que el código generado siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
