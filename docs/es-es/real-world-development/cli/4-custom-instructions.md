---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental para trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene que Copilot tenga acceso a esas pautas. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código se quiere, sino también *cómo* debe estructurarse. Ahora que has implementado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, harás lo siguiente:

- explorar cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizar el archivo de instrucciones para garantizar el cumplimiento de los estándares de programación.
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`; los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Los archivos de instrucciones permiten que Copilot tenga en cuenta esas prácticas al realizar las tareas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot pueda producir sugerencias pertinentes. Puedes indicar convenciones de programación, bibliotecas y preferencias sobre los comentarios. Se pueden crear instrucciones para todo el repositorio o para tipos de archivo específicos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener contexto general del proyecto.
- `.github/instructions/*.instructions.md` contiene pautas para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

## Explorar los archivos de instrucciones del proyecto

El proyecto de ejemplo ya incluye archivos de instrucciones. Revísalos para ver qué orientación tiene Copilot antes de hacer cambios.

1. Vuelve al codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Revisa la descripción breve del proyecto y las pautas. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de la carpeta `.github/instructions`, incluidos los de Astro, la capa de datos Drizzle y las pruebas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que determina mediante un glob a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a archivos existentes. Las rutas son relativas al propio archivo de instrucciones.

## Actualizar las instrucciones del equipo

Actualiza el archivo principal para que el código TypeScript nuevo incluya documentación TSDoc.

1. Busca la sección **Code formatting guidance** de `.github/copilot-instructions.md`.
2. Añade al final de la sección esta viñeta:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar el cambio, sal y reanuda la conversación de filtrado para que Copilot use las instrucciones nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, ciérrala con `/exit`. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones nuevas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las pautas de código. ¿Puedes actualizar el código que generaste para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los comentarios TSDoc nuevos describen correctamente el código.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza las instrucciones del repositorio y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- has actualizado el archivo para exigir el cumplimiento de los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para que las pruebas y lint se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
