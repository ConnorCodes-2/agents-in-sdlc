---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de las incidencias, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de QA definido.

En esta lección, harás lo siguiente:

- explorar cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP.
- crear y revisar un perfil reutilizable de control de calidad.
- seleccionar el agente QA y revisar sus resultados según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. En este taller definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones tienen funciones distintas: las instrucciones del repositorio describen los estándares del equipo, la habilidad `quality-checks` agrupa comprobaciones repetibles, Playwright MCP proporciona herramientas para el navegador y el perfil QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos y comunicar los resultados. El perfil no sustituye a esos recursos ni requiere otra conversación.

## Crear el perfil QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá tanto las comprobaciones como los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar para cada requisito si se ha superado, ha fallado o está bloqueado, y aportar pruebas que lo justifiquen. Antes de cambiar el código de implementación, debe pedir permiso. No debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Crea solo el perfil para que pueda revisarlo.
   ```

## Revisar el perfil

Antes de utilizar el agente nuevo, revisa el perfil para confirmar que Copilot ha recogido el flujo de QA y sus límites.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero permite mostrar un nombre claro.
3. Comprueba que las instrucciones del perfil indican que QA debe partir de los requisitos, seguir las instrucciones del repositorio, ejecutar la habilidad `quality-checks` y utilizar Playwright MCP.
4. Confirma que QA debe aportar pruebas, pedir permiso antes de modificar el código de implementación y abstenerse de confirmar cambios o abrir PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que corrija el perfil antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación ya disponibles.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que aún no aparezca en la lista. Aun así, el comando anterior lo activa.

2. Pide al agente QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones del plan. ¿Está lista para una PR?
   ```

3. Deja que el agente QA realice el trabajo.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado y reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- has creado y revisado un perfil de control de calidad reutilizable.
- has seleccionado el agente QA y revisado sus resultados para la incidencia de filtrado.

La implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación están listos para su revisión. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
