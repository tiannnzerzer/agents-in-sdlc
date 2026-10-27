---
title: "Ejercicio 2 - Añadir funcionalidad con el modo Agent de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos pueden requerir modificaciones en varios archivos y ejecutar pruebas. El modo Agent de Copilot puede explorar el proyecto, actualizar archivos, ejecutar tareas y corregir errores detectados.

En este ejercicio, vas a:

- explorar el proyecto con el modo Agent de Copilot.
- implementar una funcionalidad que abarque la interfaz de usuario (UI) y la capa de datos.
- revisar los cambios y las pruebas antes de combinar el código.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior creaste un ayudante de editores; ahora completarás con Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Tailspin Toys es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los ayudantes de acceso a datos se encuentran en `src/lib/` y la interactividad se implementa con scripts Astro acotados y API estándar del DOM.

1. Vuelve a tu Codespace y confirma que estás en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

5. Cuando aparezca la dirección del servidor Astro, mantén pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciónala para abrir el sitio.
6. Explora la página principal y los detalles de algún juego.

En la página principal aparecen juegos con sus títulos, imágenes y descripciones. La página de detalles muestra información adicional, como el editor y la categoría.

## Explora el backlog con Copilot

Antes de implementar el código, pide a Copilot que consulte las incidencias del repositorio.

1. En Copilot Chat, selecciona **New Chat** y, después, **Agent**.
2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.
3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista generada.

## Revisa las instrucciones

Antes de generar código, revisa `.github/instructions/ui.instructions.md`, que describe cómo implementar funcionalidades en el sitio.

1. Abre `.github/instructions/ui.instructions.md`.
2. Revisa la arquitectura, las directrices de diseño, las pruebas y la accesibilidad, así como los enlaces a las instrucciones de Astro, Tailwind CSS y Drizzle.

## Implementa el filtrado

El filtrado requiere actualizar la lógica de datos en `src/lib/`, las pruebas y la página de listado de juegos. Pide a Copilot que complete el trabajo.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.
3. Confirma que está seleccionado **Agent** y que el modelo es **Claude Sonnet 4.5**.
4. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Revisa los archivos que Copilot modifica y selecciona **Continue** cuando solicite permiso para ejecutar pruebas.
6. Cuando termine, revisa el código, vuelve al navegador y prueba el filtrado.
7. Cuando hayas verificado y revisado los cambios, selecciona **Keep** en Copilot Chat.

> [!NOTE]
> Copilot puede pausar o mostrar pruebas fallidas mientras itera. Revisa el resultado final y confirma que las comprobaciones necesarias se han completado.

## Resumen y próximos pasos

Has usado el modo Agent de GitHub Copilot para explorar el proyecto y añadir capacidades a la interfaz y la capa de datos. Has revisado los cambios y las pruebas antes de integrarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una PR.

## Recursos

- [Modos ask, edit y agent de Copilot][choose-mode]
- [Modo Agent en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
