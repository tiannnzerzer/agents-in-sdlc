---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, conoce la herramienta y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas. Puedes delegar tareas, solicitar cambios y mantener la concentración. Instalarás la herramienta con npm, que ya está disponible en GitHub Codespaces.

En esta lección, vas a:

- instalar GitHub Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación breve.
- encontrar la incidencia de filtrado con el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog creciente. Copilot CLI lleva esa capacidad al terminal, donde ya trabajan muchos desarrolladores. En esta lección instalarás y autenticarás la CLI.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. GitHub Codespaces incluye Node.js, así que usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, se te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones para autenticarte con el código de dispositivo y autorizar Copilot CLI.
3. Copilot CLI mostrará este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio de Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, puede que ya tengas una sesión autenticada con GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo.

## Conoce la CLI

Los comandos que escribes en el prompt normal del shell se ejecutan directamente en Codespaces. Cuando se inicia Copilot CLI, puedes escribir instrucciones en lenguaje natural para el agente y usar comandos de barra para controlar la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI usa la rama que está activa en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el acceso directo del taller

Normalmente, Copilot CLI solicita permiso para usar herramientas fuera de los permisos establecidos. En este taller, lo reiniciarás con `--yolo`, un acceso directo aprobado por el usuario que elimina esas solicitudes dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` activa todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual; nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub que se usarán más adelante. Codespaces limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para comprobar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies instrucciones o añadas un agente, usarás `copilot --resume` para volver a la conversación y a la rama de la funcionalidad.

## Resumen y próximos pasos

Has instalado GitHub Copilot CLI con npm, te has autenticado, has confiado en el repositorio y has encontrado la incidencia mediante el servidor MCP integrado de GitHub.

A continuación, [harás tu primer cambio específico][next-lesson] y pedirás a Copilot CLI que muestre las valoraciones de los juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
