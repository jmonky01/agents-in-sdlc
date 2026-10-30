---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una manera determinada o Copilot necesita información de fondo, ese contexto debe estar a su alcance. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts «antes y después»: el agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que el efecto es más difícil de observar en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las PR que genere el agente en los siguientes ejercicios.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias directrices de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario (UI) debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe comenzar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones proporcionarás a Copilot el contexto necesario para aplicar estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias, como las convenciones de programación, las bibliotecas y los comentarios. Se pueden definir para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **cada** solicitud del repositorio. Debe contener información pertinente para la mayoría de las interacciones, como la tecnología utilizada, una descripción del proyecto, buenas prácticas y directrices generales.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para lenguajes, tipos de archivos o tareas concretos, como TypeScript, Astro, componentes de interfaz o pruebas.

> [!NOTE]
> En un IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan las instrucciones del repositorio y los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad][custom-instructions-support].

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran una forma representativa de organizar las instrucciones:

- Mantén `copilot-instructions.md` centrado en el contexto general del proyecto y los estándares de programación.
- Utiliza `*.instructions.md` para tipos de archivo, lenguajes o tareas concretos.
- Escribe las directrices en lenguaje natural, con claridad y ejemplos.

No hay una única manera correcta de crear instrucciones o utilizar IA; la experimentación te ayudará a encontrar lo que funciona para tu proyecto.

> [!TIP]
> Los proyectos que utilizan GitHub Copilot pueden beneficiarse de una colección completa de archivos de instrucciones. En este proyecto encontrarás ejemplos para [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada entorno ofrece una forma distinta de hacerlo; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas e ideas, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Revisa los archivos de instrucciones incluidos en el repositorio desde el editor o la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`, que define el patrón de archivos al que se aplican las instrucciones. Por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Añade un estándar al repositorio

Añadirás una regla general para que el código incluya comentarios TSDoc y un encabezado de comentarios. El agente en la nube la utilizará en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero confirmarás los cambios de forma ordenada.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

El archivo `.github/copilot-instructions.md` contiene información general del proyecto para Copilot. Actualiza sus estándares de programación.

1. Abre `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, aproximadamente en la línea 27. Observa que todavía no exige documentación en el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio se guardan en `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivo o tareas concretos se guardan en archivos `*.instructions.md`. El archivo del repositorio es el lugar adecuado para estándares generales como los comentarios de documentación.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, Copilot partirá de `main`, así que los cambios deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios (PR) desde `custom-instructions-cloud` hacia `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar los cambios directamente en esa rama. La rama sirve para practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y próximos pasos

En este ejercicio:

- exploraste cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- añadiste un estándar general al archivo `.github/copilot-instructions.md`.

Verás cómo se aplica en las PR que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
