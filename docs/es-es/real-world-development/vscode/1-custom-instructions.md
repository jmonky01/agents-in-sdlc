---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Modo agente →][next-lesson]

El contexto es fundamental cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una manera determinada, Copilot necesita disponer de esa orientación. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- generarás la primera parte de los datos para el filtrado (un asistente de editores) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios de instrucciones y la base del filtrado en `main` para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot es probabilístico. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener siempre un resultado idéntico.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias directrices de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario (UI) debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe comenzar con un bloque de comentarios que describa su propósito.

Utilizarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para aplicar estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias, como las convenciones de programación, las bibliotecas y los comentarios. Se pueden definir para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **cada** solicitud del repositorio y contiene contexto general.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> En un IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan las instrucciones del repositorio y los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad][custom-instructions-support].

### Buenas prácticas para gestionar instrucciones

Los ejemplos del proyecto muestran una forma representativa de organizar las directrices:

- Mantén `copilot-instructions.md` centrado en el contexto general del proyecto y los estándares de programación.
- Utiliza `*.instructions.md` para tipos de archivo, lenguajes o tareas concretos.
- Escribe instrucciones claras en lenguaje natural y proporciona ejemplos.

No hay una única manera correcta de crear instrucciones o utilizar IA; la experimentación te ayudará a encontrar lo que funciona para tu proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot pueden beneficiarse de una colección completa de archivos de instrucciones. En este proyecto encontrarás ejemplos para [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Por ejemplo, VS Code ofrece **Configure Chat → Generate Agent Instructions** y Copilot CLI, `/init`.
>
> Para encontrar plantillas e ideas, explora [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Revisa los archivos existentes desde el editor o la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**.
3. Abre `.github/instructions` y explora los archivos para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa `applyTo`, que define el patrón de archivos al que se aplican las instrucciones.
5. Revisa las directrices para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás esa directriz.

## Crea una rama para los cambios

Crea una rama para trabajar en la nueva instrucción y el asistente de editores.

1. Vuelve al codespace.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

Para observar el efecto de las instrucciones, primero enviarás un prompt con las directrices actuales. Después actualizarás las instrucciones y enviarás el mismo prompt.

1. En el codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Cierra las pestañas abiertas de ejercicios anteriores y abre `src/lib/publishers.ts` para proporcionar a Copilot el contexto del archivo.
3. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todos los editores con su nombre e identificador. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, que pueden incluir el asistente y sus pruebas.
6. Observa que el asistente propuesto tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores; estas convenciones proceden de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa si el código propuesto no incluye comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Debido a la naturaleza probabilística de Copilot, puede que incluya comentarios incluso antes de actualizar las instrucciones. No pasa nada: lo importante es observar si mejora la coherencia tras el cambio.

## Añade un estándar al repositorio

El archivo `.github/copilot-instructions.md` contiene información general del proyecto para Copilot. Actualiza sus estándares de programación.

1. Abre `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, aproximadamente en la línea 27. Todavía no exige documentación dentro del código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio se guardan en `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivo o tareas concretos se guardan en archivos `*.instructions.md`.

## Repite el prompt y observa el cambio

Ahora enviarás el mismo prompt para comparar el resultado con las instrucciones actualizadas.

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia.
2. Abre de nuevo `src/lib/publishers.ts` para centrar el contexto.
3. Envía el mismo prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todos los editores con su nombre e identificador. Aplica los cambios al archivo.
   ```

4. Observa si el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editores para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editores de la base de datos.
    */
   ```

5. Comprueba si la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que aplique un estándar nuevo al código que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo control de código fuente que el resto del repositorio. Confirma los cambios y publícalos.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que las instrucciones y los cambios del asistente están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para el asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros archivos auxiliares (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios (PR)

Asocia la rama con la incidencia sobre las instrucciones del backlog. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo. La primera vez que Copilot utilice una herramienta de GitHub, puede que VS Code te pida iniciar sesión.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del asistente de editores para el trabajo de filtrado posterior.
   ```

4. Revisa cada solicitud de permiso y selecciona **Allow** solo si apruebas la acción.
5. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para actualizar el repositorio.
   ```

6. Revisa las acciones de GitHub y los comandos de terminal y selecciona **Allow** cuando corresponda.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- explorar cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar las instrucciones y la base del asistente en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en [modo agente][next-lesson] para añadir una funcionalidad en el proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
