---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo Agent →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Los archivos de instrucciones permiten proporcionar a Copilot directrices sobre la generación de código.

En este ejercicio, vas a:

- explorar cómo llegan a Copilot las instrucciones del repositorio y las instrucciones específicas de rutas.
- generar un primer ayudante de datos para los editores con las instrucciones actuales.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código generado sigue el nuevo estándar.
- confirmar los cambios en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia del cambio, no obtener siempre el mismo resultado.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene estándares para las pruebas unitarias de la capa de datos, el modo oscuro, los comentarios TSDoc y los comentarios de cabecera de los archivos.

Las instrucciones personalizadas aportan contexto y preferencias a Copilot. Puedes especificar convenciones, bibliotecas y comentarios para todo el repositorio o para tipos de archivo y tareas concretos:

- `.github/copilot-instructions.md` se envía en todas las solicitudes del repositorio y debe contener información general del proyecto.
- `.github/instructions/*.instructions.md` contiene directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición. Copilot Chat, Copilot CLI y el agente en la nube pueden usar tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.

## Explora las instrucciones del proyecto

1. En el editor de Codespaces, abre `.github/copilot-instructions.md` y revisa la descripción del proyecto y las directrices de código.
2. Explora `.github/instructions` y localiza las instrucciones para Astro, Drizzle y las pruebas.
3. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`.
4. Abre `.github/instructions/drizzle.instructions.md` y examina sus enlaces a otras instrucciones y ejemplos.

## Genera código con las instrucciones actuales

Primero pedirás a Copilot que cree un ayudante de datos para editores sin añadir el estándar de documentación.

1. Vuelve a tu Codespace y abre un terminal.
2. Crea una rama para los cambios:

   ```bash
   git checkout -b custom-instructions
   ```

3. En Copilot Chat, selecciona **New Chat** para iniciar una conversación nueva.
4. Abre `src/lib/publishers.ts` y selecciona **Agent** en el selector de agentes.
5. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

6. Revisa la función tipada propuesta y los archivos de prueba. Observa si faltan comentarios TSDoc y un comentario de cabecera.

> [!CAUTION]
> Copilot puede añadir comentarios aunque no se los hayas pedido. El objetivo es observar si el estándar actualizado hace que el resultado sea más coherente.

## Añade un estándar del repositorio

1. Abre `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements** y añade estas reglas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. Guarda el archivo.

## Repite el prompt con las instrucciones actualizadas

1. En Copilot Chat, selecciona **New Chat**.
2. Vuelve a `src/lib/publishers.ts` y envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

3. Revisa el encabezado del archivo y el comentario TSDoc propuestos. Confirma que describen el código con precisión.

## Confirma los cambios y crea una PR

1. Abre un terminal y ejecuta `git status` para comprobar los cambios.
2. Añade el archivo de instrucciones, el ayudante y las pruebas relacionadas:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

4. En Copilot Chat, selecciona **Agent** y pide que encuentre la incidencia de actualización de instrucciones y cree una PR desde la rama actual que incluya el ayudante de editores.
5. Revisa y autoriza las acciones de GitHub que solicite Copilot.
6. Cuando la PR esté creada, pide a Copilot que la combine en `main` y actualice tu rama local.
7. Revisa y autoriza las acciones de GitHub y los comandos de terminal que solicite Copilot.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y cómo una nueva directriz afecta al código generado. Has repetido el prompt, actualizado las instrucciones y confirmado el ayudante de datos para usarlo en el siguiente ejercicio.

A continuación, usarás el modo Agent para [añadir el filtrado][next-agent].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[next-agent]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[awesome-copilot]: https://github.com/github/awesome-copilot
