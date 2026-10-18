---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que programar. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue los estándares. Pero ¿qué ocurre con las pruebas, lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son una buena opción. Ayudan a Copilot a entender cómo ejecutar operaciones.

En esta lección, harás lo siguiente:

- explorar la habilidad existente `quality-checks`.
- personalizar el formato de sus resultados.
- volver a cargar la habilidad y ejecutarla.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos. El equipo quiere mejorar el informe para que sus resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son carpetas con un archivo Markdown `SKILL.md`. Este archivo contiene frontmatter con un nombre y una descripción, una introducción a la habilidad y pautas sobre cuándo utilizarla. También puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las habilidades no necesitan archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo tiene el archivo `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas como recursos del repositorio, o en la carpeta personal `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot al ejecutar las pruebas y lint.
4. Fíjate en que aún no hay una sección **Results output formatting**.

## Ejecutar la habilidad antes de modificarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute sus tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Revisa el informe final.

## Personalizar el informe

Queremos que el informe indique qué se ejecutó, si se completó correctamente y qué resultados devolvieron las herramientas. Actualicemos la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Revisa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En concreto:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
