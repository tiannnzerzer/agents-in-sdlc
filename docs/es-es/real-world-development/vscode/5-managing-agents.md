---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Agentes personalizados][previous-lesson]

En modo Agent, Copilot explora el código, propone cambios, edita archivos y ejecuta comandos en tu entorno. Copilot Chat muestra las acciones mientras se realizan y permite revisar los cambios, aceptarlos o rechazarlos y enviar prompts de seguimiento.

En este ejercicio, vas a:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar los cambios de archivos propuestos.
- pedir al agente que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado está implementando el modo de alto contraste. El equipo de diseño solicita también un modo claro. Continuarás en la misma conversación para que el agente conserve el contexto del trabajo.

## Supervisa el progreso de Copilot

1. Vuelve a la vista de Copilot Chat del ejercicio anterior.
2. Observa las llamadas a herramientas, los archivos que consulta, los comandos que ejecuta y las ediciones propuestas.
3. Para cada diferencia, selecciona **Keep** para aceptar el cambio o **Undo** para descartarlo.
4. Abre **Files changed** para consultar todos los archivos modificados en la sesión.
5. Espera a que termine el modo de alto contraste y lee el resumen.

> [!NOTE]
> Si Copilot solicita permiso para ejecutar un comando, revisa la solicitud y autoriza solo los comandos que consideres adecuados.

## Pide al agente que añada el modo claro

La conversación actual contiene el contexto del agente, incluidos los componentes, los estilos y el uso de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Revisa cada cambio nuevo y selecciona **Keep** o **Undo**.
3. Si el agente toma una dirección que no quieres, deténlo con **Stop** y envía una aclaración.

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos cambiados.
3. Inspecciona las diferencias y confirma que se han tenido en cuenta los atributos ARIA, la navegación con teclado, el HTML semántico y la persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel **Source Control**.

## Resumen y próximos pasos

Has supervisado la sesión, revisado los cambios propuestos y enviado un prompt de seguimiento para ampliar el trabajo con el modo claro.

En el [siguiente ejercicio][next-lesson] publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios del agente local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo Agent][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
