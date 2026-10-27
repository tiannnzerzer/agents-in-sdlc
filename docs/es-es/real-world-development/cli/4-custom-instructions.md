---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga acceso a esa orientación. Los [archivos de instrucciones][instruction-files] describen tanto qué código quieres como la forma en que debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, vas a:

- explorar cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizar el archivo de instrucciones para que se sigan los estándares de código.
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para el desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas aportan contexto y preferencias a Copilot. Puedes especificar convenciones de código, bibliotecas y comentarios, tanto para todo el repositorio como para tipos de archivo o tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para lenguajes, tipos de archivo o tareas específicas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia sobre compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Revisa su contenido antes de modificarlo para poder observar el efecto del cambio.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora la breve descripción del proyecto y sus directrices de código. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del principio. Define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos del proyecto. Así se pueden dividir instrucciones amplias en archivos reutilizables más pequeños y enlazar ejemplos.

## Actualiza las instrucciones para reflejar las prácticas del equipo

Los archivos existentes son un buen punto de partida, pero falta una directriz. Modifica `copilot-instructions.md` para pedir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

La interfaz de línea de comandos (CLI) de GitHub Copilot carga las instrucciones del repositorio al iniciar una conversación. Cierra y reanuda la conversación de filtrado para que Copilot utilice las nuevas instrucciones sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot usa el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizado el archivo de instrucciones para que se sigan los estándares de código.
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
