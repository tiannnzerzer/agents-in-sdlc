---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes especializados en tareas concretas del flujo de trabajo de desarrollo. Los archivos Markdown en `.github/agents` pueden especificar sus instrucciones, herramientas y conocimientos. Cada archivo incluye frontmatter YAML con `name` y `description`, seguido de las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Un **agente personalizado** es el trabajador y las **habilidades** son las herramientas. Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades u otros agentes. En este ejercicio, el agente de accesibilidad revisará el sitio y añadirá un modo de alto contraste.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Pruébalo e itera para adaptarlo al entorno y a las tareas.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Algunos usuarios consideran que el tema oscuro de Tailspin Toys tiene poco contraste. El equipo de diseño solicita un modo de alto contraste que puedan activar o desactivar.

En este ejercicio, vas a:

- revisar un agente personalizado de accesibilidad existente.
- usar el agente en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

1. Vuelve a tu Codespace y crea una rama nueva a partir de `main`:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Comprueba el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

4. Revisa las responsabilidades, las buenas prácticas, los ejemplos de HTML, CSS y JavaScript, y los errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes definidos en `.github/agents` en el selector de agentes de Copilot Chat.

1. En Copilot Chat, selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona **Agent** en el selector de agentes.
3. Abre el selector de agentes y elige **Accessibility agent**.
4. Envía este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Revisa los cambios que Copilot propone y aplica a los archivos del proyecto.
6. Usa **Files changed** para revisar el conjunto de archivos modificados.

## Resumen y próximos pasos

Has revisado un agente personalizado y lo has usado en Copilot Chat para implementar el modo de alto contraste.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], y le pedirás que añada un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en Awesome Copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
