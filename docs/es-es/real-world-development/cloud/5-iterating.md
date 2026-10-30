---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, desarrollara una sección de juegos relacionados y añadiera funciones de accesibilidad con modos de alto contraste y claro. Ahora revisarás los cambios y, si es necesario, darás indicaciones a Copilot para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Hay que revisar el código generado y seguir los procesos habituales. Revisarás las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican medidas para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions dentro de un entorno independiente y efímero.
- Los flujos de trabajo de GitHub Actions requieren la aprobación de una persona antes de ejecutarse.
- El [acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) creada por el agente: la que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que el firewall bloquea a GitHub Copilot. Es normal: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Puedes [personalizar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado y los comentarios TSDoc. Comprueba también las convenciones de TypeScript y los encabezados de comentarios que proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo que esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR. Las comprobaciones del proyecto Astro deberían superarse; puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios en línea del código. Copilot utilizará esos comentarios para iniciar otra sesión y resolverlos. Como los resultados no son deterministas, no hay un prompt único. Por ejemplo, puedes pedirle que:

- añada encabezados breves que describan cada archivo de código.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**. Puedes utilizar una de las ideas anteriores o proponer otra mejora.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, comprueba que la PR incluye una nueva confirmación.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte la PR de borrador en lista para revisión y combínala.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisarás la PR creada por Copilot para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo que esperan aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Comprueba que las comprobaciones del proyecto Astro se han completado correctamente. Puede tardar unos minutos.
9. **Opcional:** cambia a la rama de Copilot en tu codespace para probar manualmente la funcionalidad. En el terminal, sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes pedirle cambios con un comentario nuevo en la pestaña **Conversation**; por ejemplo, ajustar el número de juegos mostrados o añadir encabezados y comentarios TSDoc. Ten en cuenta que esta incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás la PR de accesibilidad creada mediante el agente personalizado. Debe incluir el modo de alto contraste que solicitaste en el ejercicio 3 y el modo claro que pediste en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera unos minutos a que termine.

4. Selecciona **Files changed** para revisar el código generado.
5. Presta atención a:
   - los componentes de interfaz para cambiar entre modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios en las hojas de estilo en cascada (CSS) o el estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las aplicaciones de Internet enriquecidas accesibles (ARIA, por sus siglas en inglés) y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo que esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Comprueba que las comprobaciones del proyecto Astro se han completado correctamente. Puede tardar unos minutos.
10. **Opcional:** cambia a la rama de Copilot en el codespace para probar manualmente las funciones de accesibilidad:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para que aplique buenas prácticas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita los cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog y ocuparte mientras tanto de otras cosas. Para practicar, crea más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- añadir paginación a la página de lista de juegos.
- validar las entradas y gestionar errores en los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **inspeccionaste los archivos de instrucciones personalizados del repositorio** para reconocer su influencia en los resultados.
- **asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **creaste y utilizaste un agente personalizado** para implementar modos de alto contraste y claro.
- **utilizaste la página de agentes de Copilot como mission control** para supervisar y orientar la sesión de accesibilidad.
- **revisaste e iteraste sobre las PR del agente en la nube**, mencionaste a `@copilot` para solicitar cambios y aprobaste flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Para seguir explorando, los otros entornos complementan lo que has practicado:

- [Recorrido de VS Code](../../vscode/): explora Copilot Chat en modo agente y la integración de MCP directamente en el IDE.
- [Recorrido de CLI](../../cli/): entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para configurar el agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
