---
title: "Lección 0 - Requisitos previos"
description: "Crea una copia propia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás una copia del repositorio Tailspin Toys y un [codespace][codespaces], cuyo terminal integrado utilizarás para instalar y ejecutar Copilot CLI en la siguiente lección.

En esta lección, harás lo siguiente:

- crear una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- crear un Codespace y confirmar que el proyecto está listo.

## Configurar el repositorio del laboratorio

Trabajarás con una copia propia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][tailspin-template]. El repositorio nuevo incluye todos los archivos necesarios para el laboratorio.

1. En una ventana nueva del navegador, ve a la [plantilla Tailspin Toys][tailspin-template].
2. Crea una copia propia seleccionando **Use this template** y, después, **Create a new repository**.
3. Si vas a realizar el taller en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio nuevo en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), ya que la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, la habilidad `quality-checks` y el backlog. Si utilizas una copia antigua, confirma con la persona que facilita el taller que contiene todos los archivos necesarios.

## Crear un Codespace

A continuación, utilizarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un editor completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración. La plantilla instalará las dependencias del proyecto, Chromium para Playwright y la base de datos local.
5. Si aparece la pregunta **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y siguientes pasos

Ya has preparado el entorno. En esta lección:

- has creado una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- has creado un Codespace y confirmado que el proyecto está listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en el Codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
