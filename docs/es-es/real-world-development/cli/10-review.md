---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo continuo de Tailspin Toys. En concreto:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- has añadido las valoraciones por estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, desarrollado la funcionalidad con Autopilot y revisado el resultado en modo Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` y utilizado la habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- has revisado el cambio de filtrado completo y autorizado Agent Merge para su PR.
- has explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones por estrellas:** muestra la propiedad `starRating` existente y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos combinado el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y con la exploración del navegador mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y comunicó los resultados en el formato nuevo. QA reunió esos resultados con la revisión de requisitos y cobertura antes de la PR.

Las pruebas añadidas deben cubrir carencias reales; también puede ser correcto que QA no necesite añadir ninguna. Revisa el código y las pruebas antes de autorizar la fusión y vuelve a generar las pruebas de verificación afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para definir el enfoque antes de desarrollar, **Interactive** para trabajar en cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres desarrollar, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más cosas que explorar

Has terminado el flujo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir usándola: con código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
