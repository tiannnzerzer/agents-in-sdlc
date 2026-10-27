---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio, vas a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de código y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil comparar su efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar las convenciones de código, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, relevante para la mayoría de las solicitudes que se envían a Copilot desde el chat o la CLI. Puede describir la pila tecnológica, lo que se está creando, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para tareas o tipos de archivo concretos. Puedes usarlos para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación exhaustiva sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en la información del proyecto, como qué se está creando, la estructura del proyecto y los estándares generales de código.
- Usa `*.instructions.md` para las directrices específicas de tipos de archivo, como las pruebas unitarias, los componentes Astro o la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural. Redacta directrices claras e incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de usar la IA. La experimentación te permitirá descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para varios tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente a cada entorno indicará cuándo usarla.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un `copilot-instructions.md` principal y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript que siga el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y dirigir a Copilot a ejemplos que puede seguir al generar código. Las rutas que aparecen ahí son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Añade un nuevo estándar al repositorio

Esta será la única edición que harás en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará esta regla más adelante, cuando trabaje en incidencias que le asignes.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que apunte la incidencia, pero así podrás confirmar los cambios por separado.

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade el estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de código, pero todavía no exige documentación en el código; por eso el ayudante generado no tenía comentarios de documentación.
3. Justo debajo de los estándares existentes, añade estas líneas Markdown para indicar a Copilot que incluya encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en el ejercicio anterior, los archivos de instrucciones se pueden crear en el repositorio (`.github/copilot-instructions.md`) para definir directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para un estándar general, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, envía y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, Copilot creará una rama desde `main`. Por tanto, debes integrar las instrucciones en `main` para que el agente pueda usarlas.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar los cambios directamente en `main`. Se incluye el paso de la rama para reflejar cómo se haría en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente durante el resto de este recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
