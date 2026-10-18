---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos algunos más.

En esta lección, harás lo siguiente:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usar `/model` para consultar los modelos disponibles.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para el trabajo en paralelo, los worktrees y la delegación a un agente en la nube.

## Escenario

Has terminado el flujo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente a [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas complejas o amplias, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando hace falta; puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve al codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara los resultados. El cambio puede ser pequeño si la conversación ya es breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` para empezar una conversación nueva si cambias a una tarea que no está relacionada.

## Elegir un modelo

Los modelos tienen capacidades distintas y cada desarrollador puede tener sus preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieras utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir el trabajo y lo aprendido en equipo ayuda a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para exportarla a Markdown localmente.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa los resultados con el flujo habitual. Para profundizar en el trabajo asíncrono, continúa con el [taller de agentes en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver qué ocurre internamente. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- has usado `/model` para examinar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para el trabajo en paralelo, los worktrees y la delegación a la nube.

Hay más comandos de barra y funciones que puedes explorar. Para terminar el recorrido, [repasa lo aprendido][next-lesson] y descubre cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
