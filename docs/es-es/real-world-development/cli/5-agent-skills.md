---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has validado manualmente el funcionamiento y usado archivos de instrucciones para aplicar los estándares. También hay que ejecutar pruebas, lint y otras comprobaciones de integración continua (CI). Las **habilidades de agente** ayudan a Copilot a realizar estas tareas.

En esta lección, vas a:

- explorar la habilidad existente `quality-checks`.
- personalizar el formato de los resultados.
- volver a cargar la habilidad y ejecutarla.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para una tarea, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. Cada habilidad se encuentra en una carpeta con un archivo Markdown `SKILL.md`, cuyo frontmatter incluye el nombre y la descripción. La carpeta también puede incluir subcarpetas con scripts y recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas en el repositorio o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían las pruebas y el lint.
4. Comprueba que aún no hay una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Añadirás instrucciones para que el informe indique qué se ejecutó y qué resultado obtuvo.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el nuevo informe y compáralo con el primero.

## Resumen y próximos pasos

Has explorado y personalizado una habilidad de agente existente, y has vuelto a cargarla y ejecutarla. El cambio acompañará al filtrado en la PR de la funcionalidad.

A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia y no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
