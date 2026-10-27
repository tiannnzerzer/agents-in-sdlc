---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, todo el flujo de filtrado y calidad está en una conversación y una rama.

En esta lección, vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionar el cambio completo y las pruebas de verificación.
- crear la PR del filtrado.
- activar Agent Merge solo después de revisar el cambio y confirmar que se ha combinado.

## Escenario

Has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo pendiente de la PR sin quitar al desarrollador el control de la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo necesario para integrar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR, como las comprobaciones de integración continua (CI) fallidas, los comentarios de revisión y las necesidades de rebase. Después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Revisa el código, crea la PR y permite que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y la combinará cuando se superen todas las comprobaciones.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la PR. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionado el cambio completo y las pruebas de verificación.
- creado la PR de filtrado.
- activado Agent Merge después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás más comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
