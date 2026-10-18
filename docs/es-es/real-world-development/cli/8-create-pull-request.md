---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás todos los cambios y abrirás una solicitud de incorporación de cambios (PR). La PR de las valoraciones por estrellas la fusionaste manualmente; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separarías la funcionalidad, las instrucciones, la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en una única PR.

En esta lección, harás lo siguiente:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación.
- crear la PR de filtrado.
- habilitar Agent Merge solo después de revisar el trabajo y confirmar que se ha fusionado la PR.

## Escenario

Durante el flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo pendiente para integrar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige fallos de integración continua (CI), responde a los comentarios de revisión y reaplica los cambios sobre la rama de destino (rebase) cuando sea necesario. Después, habilita la fusión automática de GitHub cuando el repositorio lo permita.

Hasta ahora, has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad, pero revisa el trabajo y asegúrate de que está listo antes de habilitarlo.

## Usar Agent Merge para gestionar la PR

Revisarás el código, crearás la PR y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando termine correctamente, la fusionará.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, las reglas de protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y, si el repositorio lo permite, completa manualmente la fusión revisada.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de PR. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado el cambio completo y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge solo después de revisar el trabajo y has confirmado la fusión.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
