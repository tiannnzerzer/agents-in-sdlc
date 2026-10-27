---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Instrucciones personalizadas][previous-lesson]

Es probable que casi todas las organizaciones tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) preliminar para que un desarrollador la revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot, explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que no hay documentación. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que los visitantes puedan seguir explorando. No necesita una implementación pulida todavía; solo algo que permita hacer pruebas de aceptación de la experiencia de usuario (UX). Aunque esta tarea bloquea ese trabajo, hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son buenas candidatas para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el resultado y comprobar que todo está bien.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Igual que con un desarrollador humano, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de asignarla, Copilot crea una PR preliminar para hacer un seguimiento del trabajo, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez completada. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones que parecen sencillas pueden ser complejas al examinarlas con detalle. Por eso, [define con cuidado el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste un ejercicio práctico sobre ellas, has visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente, examina en modo de solo lectura los archivos de instrucciones que incluye el repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub de tu repositorio o en un Codespace si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas directrices proceden de los archivos de instrucciones. Volveremos a señalarlos al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración para asegurar que todo está listo, independientemente de quién realice el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube usa [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo especial de configuración][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puedes instalar las herramientas y dependencias necesarias. El repositorio del taller ya incluye esta configuración para facilitar el ejercicio. Prepara Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que lo ejecute Copilot.

## Añade documentación

La mayoría de los proyectos tienen información desactualizada o carecen de documentación. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar desarrolladores nuevos. Copilot puede ayudar a crear documentación, así que esta es una buena incidencia para asignar al agente en la nube. El agente la generará en segundo plano y volverás a revisar su trabajo en un ejercicio posterior.

1. En una pestaña nueva del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que acaba de generar Copilot. El título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo descrito antes—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan y, después, crea código.

La tarea puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es trabajar de forma asíncrona para que puedas ocuparte de otras cosas. Aprovecharás esa capacidad asignándole otra tarea y, mientras tanto, dedicarás tu atención a crear funcionalidades para la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: puedes ocuparte de una tarea mientras el agente trabaja en otra. Aunque añadir juegos relacionados para el equipo de diseño quizá no tarde mucho, ese tiempo se puede dedicar a otros trabajos. Asigna también esta tarea al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, deberías ver varios ojos 👀 en el primer comentario de la incidencia. Indican que Copilot está trabajando en ella.

![Copilot muestra el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software: no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Mientras tanto, pasarás a crear y usar agentes personalizados.

## Resumen y próximos pasos

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, pasarás a crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que ya has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
