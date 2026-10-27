---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El cambio anterior era pequeño, pero las modificaciones mayores requieren un proceso más completo. La interfaz de línea de comandos (CLI) de GitHub Copilot permite seguir el flujo de trabajo de una organización para crear lo que se necesita de la forma adecuada. En esta lección y las siguientes seguirás un proceso de desarrollo habitual basado en agentes: partir de una incidencia, definir lo que hay que construir, validar el código y la funcionalidad y, finalmente, combinar el cambio.

> [!NOTE]
> Durante este flujo de trabajo, continuarás usando la misma conversación y rama. En un proyecto real, normalmente crearías ramas o solicitudes de incorporación de cambios distintas para diferentes tipos de archivo; aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección, vas a:

- iniciar una conversación de Copilot desde una incidencia de GitHub.
- definir los requisitos en modo Plan.
- implementar la funcionalidad en modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Mientras desarrollas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para decidir esos aspectos y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA no cambia los fundamentos del desarrollo. La mayoría de los equipos siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo.
2. Planificar lo que se debe crear.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido.

Este enfoque ayuda a comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre modos:

- **Interactive**: trabajas con el agente; este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente prepara primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, cambia a Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de valoraciones con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt empezará por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite acordar el enfoque antes de implementar una funcionalidad o realizar una tarea. Es especialmente útil para trabajos complejos.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y propondrá un enfoque.

3. Responde a las preguntas que haga sobre el funcionamiento del filtrado, según tus preferencias.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, podrás implementarlo con Autopilot.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción de nombre parecido en tu versión.
2. Confirma que el indicador de modo debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente. Revísalo primero. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema antes de considerar completas las comprobaciones.

## Revisa y verifica la implementación

Revisa el código generado y ejecuta el sitio antes de combinarlo.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver al modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Prueba la nueva funcionalidad

Inicia la aplicación y abre el sitio mediante el puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editor, incluidas las combinaciones acordadas en el plan.
4. Confirma que el reinicio de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección, has:

- iniciado una conversación de Copilot desde una incidencia de GitHub.
- definido los requisitos en modo Plan.
- implementado la funcionalidad en modo Autopilot.
- revisado el código.
- validado manualmente la funcionalidad en un navegador reenviado.

A continuación, [usarás instrucciones personalizadas][next-lesson] para que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
