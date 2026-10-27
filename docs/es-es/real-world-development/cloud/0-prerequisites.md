---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente en la nube de Copilot, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [Codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo del agente.

## Configura el repositorio del taller

Crearás una instancia propia a partir de la [plantilla][template-repository], que contiene los archivos necesarios para el taller.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

## Crea un Codespace

Usarás un Codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

   ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación del Codespace puede tardar varios minutos. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller se puede realizar en un Codespace o en un contenedor de desarrollo local. Ambos incluyen los requisitos previos. Para trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

Has creado una copia del repositorio y has iniciado la creación del Codespace que usarás para trabajar junto al agente en la nube de Copilot.

## Próximo paso

A continuación, añadirás instrucciones personalizadas para el agente en la nube. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
