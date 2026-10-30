---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. En el flujo habitual, localizas los archivos, haces los cambios, ejecutas las pruebas, depuras y repites el proceso. El modo agente de Copilot puede coordinar estos pasos.

El modo agente trabaja de forma autónoma en el IDE: explora el proyecto, actualiza los archivos necesarios, ejecuta tareas como las pruebas y corrige los errores que encuentra. En este ejercicio utilizarás el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio descubrirás cómo:

- el modo agente explora el proyecto, identifica archivos y coordina cambios.
- el modo agente implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste un asistente de editores; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz en el cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro acotados que utilizan el modelo de objetos del documento (DOM) y sus API estándar.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo Astro. Ejecútalo en el codespace:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, mostrará un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, el enlace local se redirige a una URL privada del tipo `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor que se ejecuta en el codespace.

### Explora el sitio

Mientras el sitio está en ejecución, revisa sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat en el codespace y selecciona **New Chat** para iniciar una conversación nueva. Cada ejercicio indica el modo y el modelo que debes seleccionar cuando corresponda.

La plantilla creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda planificar y modificar archivos, ejecutar comandos y utilizar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> La selección del modelo solo refleja el utilizado durante la creación del taller, no una preferencia por un modelo concreto. Las sugerencias pueden variar porque los modelos de lenguaje son probabilísticos.

> [!NOTE]
> Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt:

   ```plaintext
   Muéstrame el backlog de incidencias del repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista generada.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa las instrucciones que utilizarás para proporcionar contexto. El archivo [de instrucciones de interfaz][ui-instructions] explica cómo abordar nuevas funcionalidades.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa la arquitectura, los principios de diseño de componentes, las pruebas, la accesibilidad y los enlaces a instrucciones específicas de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar directrices, dividir conjuntos complejos y proporcionar ejemplos.

## Implementa el filtrado

Completar el filtrado requiere actualizar la lógica de la capa de datos, las pruebas y la página de listado de juegos. Copilot Agent Mode puede coordinar estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.
4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editor y categoría según los requisitos de la incidencia de GitHub correspondiente en el backlog. El ejercicio anterior ya creó un asistente de editores; conserva y perfecciona ese trabajo según sea necesario mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución, no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto, localiza la capa de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar y ejecutar pruebas. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que el agente termine.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea o dejar de detectar errores.
9. Revisa el código generado.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio y prueba la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has utilizado el modo agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. En este ejercicio:

- utilizaste el modo agente para implementar funcionalidades en la interfaz y la capa de datos.
- observaste cómo Copilot exploraba el proyecto, identificaba los archivos pertinentes y coordinaba los cambios.
- revisaste los cambios y las pruebas generados por el modo agente antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, puede que necesites paginación. Pide a Copilot que la implemente y ten en cuenta lo siguiente:

- sigue las buenas prácticas, incluidos los archivos de instrucciones existentes.
- decide si los usuarios podrán elegir el tamaño de página o si será fijo.
- proporciona a Copilot el contexto necesario para implementar el comportamiento deseado.
- itera con Copilot, solicitando cambios y proporcionando contexto adicional.

## Recursos

- [Modos ask, edit y agent de Copilot: diferencias y usos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
