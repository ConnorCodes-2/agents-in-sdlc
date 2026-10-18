---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio, harás lo siguiente:

- explorar cómo llegan al agente el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta.
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás un prompt *antes y después*. El agente en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Verás la influencia de las instrucciones al revisar las solicitudes de incorporación de cambios que genere más adelante.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo:

- La capa de datos siempre debe contar con pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su propósito.

Los archivos de instrucciones permiten que Copilot conozca estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot. Puedes indicar convenciones de programación, bibliotecas y tipos de comentarios, y crear directrices para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general del proyecto, como la pila tecnológica, lo que se está desarrollando, las buenas prácticas y otras pautas globales.
- `.github/instructions/*.instructions.md` proporciona instrucciones para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque posible:

- Mantén `copilot-instructions.md` centrado en las pautas generales del proyecto, como la descripción, la estructura y los estándares globales.
- Usa `*.instructions.md` para instrucciones concretas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) y tareas.
- Utiliza lenguaje natural, mantén las instrucciones claras y proporciona ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de utilizar IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás instrucciones para tareas como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar estos archivos. Cada entorno lo ofrece de forma distinta, por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo explicará cuando sea relevante.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

Lee los archivos incluidos en el repositorio: un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para diversas tareas. Ábrelos en la interfaz web de GitHub o en un codespace.

1. Abre `.github/copilot-instructions.md`.
2. Explora su descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa la guía anidada **GitHub Actions Workflows**, que se aplica a todas las interacciones con Copilot.
3. Abre `.github/instructions` y explora las instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa que el campo `applyTo` define un glob relativo a la raíz del repositorio; en este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones y a archivos de ejemplo. Esto permite dividir instrucciones grandes en archivos reutilizables y proporcionar ejemplos a Copilot. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentación en el código. En los próximos pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Añadir un nuevo estándar al repositorio

Este será el único cambio que harás en esta lección: añadirás una regla general para documentar el código con TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube la utilizará en las tareas posteriores.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que apunte la incidencia, pero confirmarás este cambio por separado.

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27.
4. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, puedes crear instrucciones generales en `.github/copilot-instructions.md` o archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos. El archivo general es el lugar adecuado para estándares como la regla de comentarios de documentación.

## Confirmar, enviar y fusionar la actualización

El agente en la nube lee las instrucciones de la rama de destino de la incidencia. En el próximo ejercicio, cuando asignes una incidencia a Copilot, este creará una rama desde `main`. Por eso, la actualización de las instrucciones debe llegar a `main` para que el agente la utilice.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y fusiónala.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar el cambio en `main`. La rama permite practicar un flujo más parecido al de un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás su efecto en las solicitudes de incorporación de cambios que genere el agente durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
