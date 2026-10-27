---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Revisarás los cambios y, si hace falta, darás comentarios a Copilot para que los mejore.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisarás las propuestas de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

El agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión. Para mantener la seguridad, se aplican varias restricciones:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions, que crea un entorno independiente y efímero para su trabajo.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- De forma predeterminada, el acceso a recursos externos está limitado, incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, puede que aparezca una advertencia que indica que el firewall bloquea a GitHub Copilot. Es un comportamiento previsto: el acceso de Copilot a recursos externos está limitado de forma predeterminada e incluye las llamadas a servidores MCP externos. Si quieres, puedes [configurar o desactivar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios concretos pueden variar. Al revisarlos, busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.
6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no tiene por qué ser unidireccional. Puedes mencionarlo en los comentarios de la PR, como harías con otros integrantes del equipo, o añadir comentarios en líneas concretas del código. Copilot los verá e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no hay un prompt único que debas usar. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código un encabezado con una breve descripción de su función.
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes elegir una de las ideas anteriores u otra mejora de documentación que quieras incluir en el código base.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para aplicar los cambios.
3. Si quieres volver a la solicitud, selecciona **Back to pull request**.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer un commit nuevo en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR preliminar como lista para revisión y combinarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a la rama del agente en tu Codespace y probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ya ha creado la función de juegos relacionados. Como antes, puedes iterar y pedirle cambios mediante un comentario nuevo en la pestaña **Conversation**. Por ejemplo, puedes cambiar el número de juegos relacionados que se muestran o pedir que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Copilot verá el comentario e iniciará otra sesión para aplicar los cambios.

## Revisa las funciones de accesibilidad

Por último, revisarás la PR creada por el agente personalizado de accesibilidad. Debe incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera unos minutos a que termine.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los controles de la interfaz de usuario para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a la rama del agente en tu Codespace y probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la configuración se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar las funciones de acuerdo con las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, igual que hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando le asignas elementos reales del backlog y te ocupas de otras tareas. Para practicarlo, crea más incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que las personas interesadas en financiar un juego puedan dejar sus datos en la página de detalles.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estos ejercicios has:

- **inspeccionado los archivos de instrucciones personalizadas** incluidos en el repositorio para ver más adelante cómo influyen en el trabajo del agente.
- **asignado incidencias al agente en la nube de Copilot** y observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **creado y usado un agente personalizado** de accesibilidad para añadir controles de alto contraste y modo claro.
- **usado la página de agentes de Copilot como mission control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir aprendiendo, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo Agent de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional de `/delegate` al agente en la nube.

En tu propio repositorio, puedes probar estas ideas:

- asignar al agente una incidencia de refactorización o de cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el entorno del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
