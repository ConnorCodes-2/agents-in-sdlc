---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Hay varias formas de proporcionar contexto específico a Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que indican cómo generar el código.

En este ejercicio, harás lo siguiente:

- explorar cómo el contexto del proyecto, las pautas de programación y los estándares de documentación llegan a Copilot a través de las instrucciones personalizadas del repositorio y de los archivos de instrucciones con ámbito de ruta.
- generar el primer bloque de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*.
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código generado adopta el nuevo estándar.
- confirmar la actualización de las instrucciones y el trabajo de filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el próximo ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que definas. Copilot no es determinista. El objetivo es observar la *tendencia* del cambio de comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe comenzar con un bloque de comentarios que describa su propósito.

Los archivos de instrucciones proporcionan a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto, relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está desarrollando, las buenas prácticas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos. Sirven para indicar pautas para lenguajes específicos, como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación detallada sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices de proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares de programación generales.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro y capa de datos) o tareas.
- Usa lenguaje natural, mantén las pautas claras y proporciona ejemplos de cómo debe ser el código y de cómo no debe ser.

No hay una única manera de crear archivos de instrucciones, del mismo modo que tampoco hay una única forma de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, verás que hay instrucciones para distintas tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno lo ofrece de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente lo explicará cuando sea relevante.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y mucho más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript (por ejemplo, los que cumplen `**/*.test.ts`).
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y proporcionar a Copilot ejemplos para generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos, añadirás reglas para los comentarios TSDoc y los encabezados de comentario a nivel de archivo.

## Crear una rama para los cambios

Sigue las buenas prácticas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para ver el efecto de las instrucciones personalizadas, primero envía un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas del editor de ejercicios anteriores para que Copilot solo tenga el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ir el helper.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para permitir que Copilot aplique cambios a los archivos.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales y el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará cambios, que normalmente afectarán al helper y a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, aplicables a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. Si ocurre, no pasa nada: el objetivo sigue siendo observar la mejora de la *coherencia* después de actualizar las instrucciones.

## Añadir un estándar nuevo al repositorio

Como se ha indicado antes, `.github/copilot-instructions.md` proporciona a Copilot información a nivel de proyecto. Añadamos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero aún no incluye una regla para documentar el código. Por eso, el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes para pedir a Copilot que añada encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Cada función exportada debe tener un comentario TSDoc que describa su propósito, sus parámetros y el valor devuelto.
   - Antes de las importaciones o de cualquier otro código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para directrices generales, o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt**:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales y el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales desde la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con sus identificadores y nombres.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Has guiado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real que se utilizará en el próximo ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones son recursos del repositorio como cualquier otro, así que se gestionan con el mismo proceso de control de versiones.

1. Abre un terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están los cambios de las instrucciones y del helper ejecutando:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya cambiado para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios de apoyo (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede encontrar la incidencia, crear una solicitud que la cierre y fusionar los cambios.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub, así que no es necesario hacer nada más. La primera vez que Copilot utilice una de ellas, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una solicitud desde la rama actual que describa tanto la actualización como la base del helper de editoriales:

   ```text
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que cierra esa incidencia y menciona que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si procede.
5. Una vez creada la solicitud, pídele a Copilot que la fusione y que vuelva a `main`:

   ```text
   Fusiona la solicitud en main. Después, vuelve localmente a main y descarga los últimos cambios para tener el repositorio actualizado.
   ```

6. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones del proyecto. Después, con Copilot Chat en VS Code, has:

- enviado un prompt de generación de código y observado el resultado con las instrucciones *existentes*.
- añadido un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.
- vuelto a enviar el mismo prompt y observado cómo el código propuesto seguía el nuevo estándar.
- confirmado y enviado la actualización de las instrucciones y la base del helper a `main` para que el siguiente ejercicio pueda desarrollarla.

A continuación, pondrás en práctica las instrucciones en el [modo agente][next-lesson], donde Copilot añadirá una funcionalidad al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
