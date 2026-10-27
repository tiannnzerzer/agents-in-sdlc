---
slug: es-es/real-world-development/cloud
title: "Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[El agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Le asignas tareas en GitHub y el agente las procesa en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras tú sigues con otras tareas.

En estos ejercicios añadirás instrucciones personalizadas para el agente y le asignarás una incidencia de GitHub. Revisarás y usarás agentes personalizados, supervisarás y orientarás las sesiones desde el panel de agentes y, por último, revisarás las solicitudes de incorporación de cambios del agente e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas para el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente en la nube de Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Gestionar agentes][ex4] | Supervisión | Supervisa y orienta las sesiones del agente |
| [5. Iterar][ex5] | Revisión | Revisa las PR, itera sobre el trabajo de Copilot y decide los siguientes pasos |

## Requisitos previos

Antes de empezar este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente en la nube de Copilot habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que usa este taller; puedes completar todos los recorridos con él.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible en **Copilot Student, Pro, Pro+, Business y Enterprise**; no está disponible en Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este recorrido no cubre Model Context Protocol (MCP). Para aprender a usar servidores MCP con Copilot, consulta el [recorrido de CLI][cli] o el [recorrido de VS Code][vscode].

## Primeros pasos

**[Empieza por los requisitos previos][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
