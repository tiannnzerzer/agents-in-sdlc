---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente en la nube de Copilot que se ocupara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones que han realizado y orientar una sesión en curso.

En este ejercicio, vas a:

- explorar la página de agentes para supervisar las tareas del agente en la nube.
- orientar una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de pedir al agente que cree un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como el agente ya está actualizando los estilos y añadiendo controles de selección, parece lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Revisa el estado de todas las tareas que has asignado al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel lateral. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea.

> [!NOTE]
> Las acciones que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de PR que aparece a la derecha. Permite consultar la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que haya elegido Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que la configuración ha terminado y que el trabajo ha empezado. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los que se muestran en el ejemplo.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot mientras trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba ejecutando en el momento en que enviaste el mensaje.

La tarea tardará varios minutos, así que puedes aprovechar la espera para repasar lo que has aprendido y explorado.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro desde el que se supervisan y orientan las tareas del agente en la nube de GitHub Copilot. Puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su alcance o proporcionar instrucciones adicionales.

Has:

- explorado mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, pasarás a [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
