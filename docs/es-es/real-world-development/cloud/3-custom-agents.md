---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir agentes en archivos Markdown dentro de `.github/agents` en el repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que realice determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` en el proyecto o, de forma global para el usuario, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La distinción más clara es que un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este ejercicio, el agente personalizado de accesibilidad revisará el sitio y lo actualizará conforme a las directrices de accesibilidad; para hacerlo, podría recurrir a habilidades como una que gestione el flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que funcionen en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio, explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es importante, quieres implementar esta funcionalidad cuanto antes. Para ello, usarás un agente personalizado que genere la funcionalidad.

## Revisa el agente personalizado de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revisa su contenido para comprender cómo guía a Copilot.

1. Vuelve a tu Codespace y abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - los errores y problemas habituales.

## Crea y asigna una incidencia

Mission control es el centro desde el que se gestionan los agentes del entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o dar instrucciones adicionales. Asignarás una incidencia para crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. En la lista de agentes personalizados, selecciona **Accessibility agent**.

   ![Asignación del agente en la nube, con los campos del agente personalizado y de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot para realizar determinados tipos de trabajo con mayor eficacia.

Has explorado:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, pasarás a [supervisar y orientar la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar una organización para usar agentes personalizados][org-custom-agents]
- [Preparar una empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Ejercicio anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
