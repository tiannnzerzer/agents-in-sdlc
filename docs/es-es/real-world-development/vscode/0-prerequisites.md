---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia del repositorio Tailspin Toys, iniciarás un [Codespace][codespaces] y comprobarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Crearás una instancia propia a partir de la [plantilla][template-repository], que contiene los archivos necesarios.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio, con el formato **organization-or-user-name/repository-name**.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio desde la plantilla, se crea automáticamente un backlog de incidencias de GitHub. No tienes que crear las incidencias del taller.

## Crea un Codespace

Usarás un Codespace para realizar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.
3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

La creación puede tardar varios minutos.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> También puedes completar el taller en un contenedor de desarrollo local. Abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite.

## Usa Copilot Chat y el modo Agent

GitHub Copilot Chat debería estar instalado en un Codespace. Si no está instalado, puedes [instalarlo desde Visual Studio Code Marketplace][copilot-chat-extension]. Puede que también tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del Codespace.
3. Escribe `Hello world` en Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si se te solicita, sigue las instrucciones para iniciar sesión en GitHub.

## Resumen

Has creado una copia del repositorio e iniciado un Codespace para escribir código.

## Próximo paso

Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson] para explicar a Copilot las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[codespaces]: https://github.com/features/codespaces
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
