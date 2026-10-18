---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code te muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diff, aceptar o rechazar cambios y dirigir la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio, harás lo siguiente:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar las ediciones de archivos propuestas.
- dirigir el agente durante la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño añade otra petición: también quiere un control de modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que quiera ejecutar.
3. Cuando proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que el agente ha modificado en la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Dirigir el agente para añadir el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control de modo claro. Como continúas en la misma sesión de chat, el agente conserva el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. En el cuadro de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Buen trabajo. Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: guardar la preferencia mediante el almacenamiento local y permitir activarlo desde la misma zona de la interfaz. La persona usuaria debe poder activar el alto contraste y el modo claro de forma independiente.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios existentes y propondrá las incorporaciones necesarias para el modo claro.
3. Revisa cada edición nueva, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te gusta—, puedes detenerlo con el botón **Stop** junto al cuadro de chat y enviar un mensaje de seguimiento para aclarar lo que quieres.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para solicitar ajustes («Simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («No añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («Añade también atajos de teclado para ambos modos»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios del agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se siguen las pautas de accesibilidad del agente: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la próxima lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición cuando aparecía, has aceptado o rechazado cambios y has enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- la supervisión del trabajo de un agente en tiempo real.
- la aceptación y el rechazo de ediciones individuales.
- cómo dirigir una sesión con prompts de seguimiento.

En la [próxima lección][next-lesson], publicarás tu trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
