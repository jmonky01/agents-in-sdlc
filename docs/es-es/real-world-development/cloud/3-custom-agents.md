---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados en tareas o ámbitos concretos del desarrollo. Puedes definirlos mediante archivos Markdown en `.github/agents` y proporcionarles instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad o uno que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen puntos en común: ambos se definen principalmente mediante archivos Markdown e indican a la IA cómo realizar tareas. Una forma sencilla de diferenciarlos es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Para ello, podría utilizar habilidades como una que gestione solicitudes de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones y adáptalas a tu entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- descubrirás cómo se definen los agentes personalizados.
- asignarás una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que les cuesta leer el tema oscuro actual debido al poco contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Utilizarás un agente personalizado para implementar esta funcionalidad de accesibilidad con rapidez.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve a tu codespace y abre `.github/agents/accessibility.md`.
2. Revisa el frontmatter YAML y los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores habituales.

## Crea y asigna una incidencia

Mission control es el centro desde el que puedes trabajar con los agentes, asignar tareas, supervisarlas y proporcionar indicaciones adicionales. Asignarás una tarea para crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Add high contrast mode to website` en el campo **Title**.
5. Escribe esta descripción:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe haber un conmutador para que el usuario pueda activar el alto contraste. La preferencia debe guardarse en el almacenamiento local del navegador.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Captura del cuadro de asignación del agente en la nube, con el agente de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**.

Copilot comenzará a trabajar en segundo plano.

## Resumen y próximos pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y ámbitos concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio:

- descubriste cómo se definen los agentes personalizados.
- asignaste una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y dirigirás la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar el uso de agentes personalizados en una organización][org-custom-agents]
- [Preparar el uso de agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Ejercicio anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Ejercicio siguiente: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
