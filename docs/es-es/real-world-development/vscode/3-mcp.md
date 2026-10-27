---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Modo Agent][previous-lesson] · [Siguiente ejercicio: Agentes personalizados →][next-lesson]

Has creado el filtrado con el modo Agent. Antes de abrir una solicitud de incorporación de cambios (PR), comprobarás que funciona en el navegador. Configurarás el servidor Model Context Protocol (MCP) de Playwright para que Copilot controle un navegador real y pruebe la funcionalidad.

En este ejercicio, vas a:

- aprender qué es MCP y cómo amplía Copilot con herramientas.
- añadir el servidor MCP de Playwright al espacio de trabajo.
- pedir a Copilot que pruebe manualmente el filtrado en un navegador.
- publicar tu rama y abrir una PR.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] es un estándar que permite a los agentes de IA comunicarse con herramientas y servicios externos. Los servidores MCP ofrecen herramientas y recursos, y actúan como puente entre el agente y los servicios.

- **[Servidor MCP de GitHub][github-mcp-server]**: da acceso a API para gestionar repositorios, incidencias y PR.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona automatización del navegador con Playwright.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Comprueba que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

El servidor GitHub remoto viene incluido en la plantilla y permite consultar el backlog. VS Code se conecta por HTTP y solicita la autenticación de GitHub la primera vez que Copilot usa sus herramientas.

## Añade Playwright MCP

Añade un segundo servidor para que Copilot controle el navegador.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.
3. Opcionalmente, selecciona **Start** sobre la entrada `playwright` y confirma que confías en el servidor.

El servidor `playwright` es local. VS Code lo inicia mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, lo cual es necesario en Codespaces.

> [!NOTE]
> El proyecto ya usa Playwright para las pruebas de extremo a extremo. Si el agente indica que falta un navegador, pídele que ejecute `npx playwright install chromium`.

## Prueba la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Revisa el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide aclaraciones o correcciones antes de abrir una PR.

## Publica la rama y crea una PR

1. Abre **Source Control** en Codespaces y revisa los cambios de Copilot.
2. Prepara los cambios y genera un mensaje de confirmación con **Sparkle**.
3. Selecciona **Publish** para enviar la rama al repositorio.
4. En Copilot Chat, selecciona **New Chat** y confirma que está seleccionado **Agent**.
5. Pide a Copilot que encuentre la incidencia de filtrado por categoría y editor, y que cree una PR para la rama actual asociada a esa incidencia.
6. Revisa y autoriza las operaciones de GitHub que solicite Copilot.
7. Abre la PR para revisarla, pero no la combines todavía.

## Resumen y próximos pasos

Has configurado Playwright MCP, probado el filtrado en el navegador y publicado la rama con una PR abierta.

A continuación, [crearás un agente personalizado][next-lesson] para tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
