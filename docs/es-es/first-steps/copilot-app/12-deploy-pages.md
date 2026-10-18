---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale a Space Quiz un espacio público al publicarlo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección, harás lo siguiente:

- pedir a Copilot que configure una publicación en GitHub Pages.
- revisar el flujo de trabajo y la solicitud de incorporación de cambios antes de fusionarlos.
- seguir el despliegue y comprobar el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente aún necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible desde una URL pública sin añadir un servidor ni dependencias de entorno de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages. Añade un README que describa la aplicación y explique cómo ejecutarla localmente, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios y fusiónala cuando se superen las comprobaciones obligatorias. Supervisa el despliegue hasta que se complete correctamente y, después, indica la URL final del sitio publicado. Si no tienes permiso para habilitar Pages o fusionar, dime exactamente qué tengo que aprobar o hacer; no indiques que se ha completado correctamente hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y fusione una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa la diferencia, los permisos de despliegue y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba el despliegue

1. Confirma que el README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Fusiona los cambios solo cuando se superen las comprobaciones obligatorias de la solicitud de incorporación de cambios.
4. Sigue la ejecución del despliegue de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o fusionar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des por terminada la tarea hasta que el despliegue se complete correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado el despliegue en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
