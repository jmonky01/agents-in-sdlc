---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así es posible delegar tareas repetitivas y dedicar más tiempo a los retos de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. La documentación del sitio que creó el equipo inicial está incompleta. Como primer paso, quieren añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego. Por ahora basta con una implementación que permita probar la experiencia. Aunque esta tarea bloquea las pruebas de diseño, hay otras incidencias de mayor prioridad.

Son tareas que pueden quedar relegadas y que se pueden asignar al agente en la nube de Copilot. Mientras el agente trabaja de forma asíncrona, el desarrollador puede ocuparse de otras tareas y volver después para revisar los resultados.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot][copilot-agents] puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot][assign-issue]. Después, Copilot crea una PR en borrador para registrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace que desaparezca la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden tener aspectos complejos, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Antes de asignar trabajo al agente, revisa los archivos de instrucciones que incluye el repositorio para reconocer su influencia en los resultados.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation`, revisa en la PR los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que proceden de estos archivos.

## Configura el entorno de desarrollo del agente en la nube

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante el flujo de trabajo de configuración `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. El taller ya lo ha configurado para instalar Node.js y las dependencias, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro.

El flujo de trabajo de configuración preparado para el taller es:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura solo los permisos mínimos necesarios para estos pasos. Copilot utilizará su propio token.
    permissions:
      # Para clonar el repositorio y, por ejemplo, instalar dependencias, se necesita contents: read. Si no lo clonas aquí, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación ahora es Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El flujo es similar a cualquier otro flujo de trabajo de GitHub:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot trabaje en la PR.
- El activador `workflow_dispatch` permite ejecutarlo manualmente desde la pestaña **Actions** del repositorio para probar su funcionamiento.

## Añade documentación

La falta de documentación es una forma de deuda técnica que reduce la productividad y dificulta mantener el código o incorporar desarrolladores al equipo. Asignar esta tarea al agente en la nube permite que genere la documentación en segundo plano; revisarás el resultado en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe esta descripción:

   ```plaintext
   Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente para añadir comentarios de documentación donde aclaren el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Espera a que Copilot cree una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno con el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR y comprueba que Copilot haya empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Recorre la sesión en directo y observa cómo Copilot explora el código, analiza su estado, decide un plan y crea código.

El trabajo puede tardar varios minutos. Mientras Copilot trabaja de forma asíncrona, asígnale otra tarea y dedica el tiempo a otras actividades.

## Añade una sección de juegos relacionados

Divide el trabajo para que el agente pueda implementar una funcionalidad mientras tú te ocupas de otras tareas.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en el campo **Title**.
5. Añade esta descripción:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Añadir un asistente de acceso a datos en `src/lib/` que devuelva los demás juegos de la misma categoría que un juego, sin incluir ese juego
   - Mostrar en la página de detalles una sección «Related games» que utilice el asistente
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo asistente
   - Asegurarse de que todas las pruebas se superan antes de crear la PR
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Poco después, puede que aparezca 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente en la nube de Copilot trabaja de forma parecida a un desarrollador, así que puedes revisar su trabajo cuando termine en lugar de supervisarlo activamente. A continuación, crearás y utilizarás agentes personalizados. El Protocolo de contexto de modelo (MCP) también permite al agente en la nube utilizar servidores de herramientas externas.

## Resumen y próximos pasos

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents], al que puedes asignar incidencias para que las resuelva de forma asíncrona. Puede ayudar a reducir deuda técnica, crear funcionalidades o migrar código entre frameworks.

En este ejercicio:

- personalizaste el entorno de generación de código.
- revisaste cómo mantener seguras las operaciones.
- aprendiste por qué es importante definir claramente el alcance de las incidencias.
- asignaste incidencias a Copilot.

Mientras el agente en la nube trabaja, crearás y utilizarás agentes personalizados. El [agente en la nube también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para configurar el agente en la nube][cloud-agent-best-practices]

| [Ejercicio siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
