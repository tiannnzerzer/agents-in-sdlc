---
title: "Lección 2 - Añadir valoraciones con estrellas: un cambio rápido"
description: "Usa Copilot CLI para modificar las tarjetas de juegos, revisar el cambio en un navegador reenviado y combinar tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado la interfaz de línea de comandos (CLI) de GitHub Copilot y probado una conversación, harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración con estrellas en sus datos, pero las tarjetas de la página principal aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección, vas a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad.
- pedir al agente que haga un pequeño cambio en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación y confirmar el cambio en un navegador reenviado.
- abrir y combinar tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Cómo funciona una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama que están activos en el terminal, en lugar de crear un árbol de trabajo específico. Puedes volver a conversaciones guardadas, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tus prompts y las respuestas del agente, su actividad con herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia la CLI con este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto y localiza los archivos que muestran los detalles de los juegos para realizar el cambio.

## Revisa las diferencias

Revisa todos los cambios generados por IA antes de combinarlos, aunque sean pequeños.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones y pide a Copilot que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista. El código exacto puede variar; revisa el comportamiento, no una implementación concreta.

## Comprueba los cambios

Inicia el sitio para examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abre una solicitud de incorporación de cambios (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la PR y mostrará su URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

Has publicado tu primer cambio con GitHub Copilot CLI. En concreto, has:

- iniciado una conversación centrada en una rama de funcionalidad.
- pedido al agente un pequeño cambio en las tarjetas de juegos.
- revisado el cambio con `/diff`.
- ejecutado la aplicación y confirmado la valoración en un navegador reenviado.
- abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad mayor.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
