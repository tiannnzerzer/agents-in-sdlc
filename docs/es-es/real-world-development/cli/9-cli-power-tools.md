---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas de interfaz de línea de comandos (CLI), GitHub Copilot CLI incluye comandos de barra para interactuar con la herramienta. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explorarás otros comandos útiles.

En esta lección, vas a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- explorar los modelos disponibles con `/model`.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de la CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes acercarte al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara los resultados. Si la conversación ya es breve, quizá no haya un cambio importante.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si vas a empezar una tarea distinta y quieres una conversación nueva.

## Elige el modelo

Los distintos modelos tienen capacidades diferentes, y cada desarrollador puede tener sus preferencias. Copilot CLI permite consultar y seleccionar el modelo.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, por ejemplo `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas e información del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien acotada, y revisa el resultado con tu flujo habitual. Para profundizar en el trabajo asíncrono, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre internamente. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- usado `/model` para explorar los modelos disponibles.
- aprendido cómo `/share` puede exportar o compartir sesiones.
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

Hay muchos más comandos de barra. Para cerrar este recorrido y repasar lo aprendido, continúa con el [repaso y los próximos pasos][next-lesson].

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
