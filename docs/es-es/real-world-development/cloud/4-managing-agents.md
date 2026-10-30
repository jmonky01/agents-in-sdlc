---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente en la nube de Copilot tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar las sesiones que siguen en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo opciones para cambiar de modo, pedirás al agente que incluya el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube

Consulta el estado actual de las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar tareas para completar la solicitud.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque utilizado.

4. Revisa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha para consultar la PR y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que añada una opción para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que Copilot haya elegido y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos, hasta que la sesión indique que ha terminado de prepararse y ha empezado a trabajar.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añade también un modo claro. Debe haber un conmutador para este modo, para que los usuarios puedan seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea del agente en la nube en la página de agentes, con el diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot lo reconoce y lo incorpora a su trabajo.

## Deja que Copilot complete la tarea

Copilot incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje. El proceso puede tardar varios minutos.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas del agente en la nube. Permite consultar tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o dar más indicaciones.

En este ejercicio:

- exploraste mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot termina las funciones de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
