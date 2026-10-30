---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias en línea, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para añadir un modo claro junto al alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, ampliarás la tarea en la sesión existente.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Las ediciones aparecen en línea con las opciones **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita.
3. Cuando proponga una edición, revisa las diferencias:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para consultar todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo para ejecutar un comando de terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Mostrará un mensaje final con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora ampliarás la misma conversación para añadir un conmutador del modo claro. Como continuarás en el mismo chat, el agente conservará el contexto de su trabajo: los componentes modificados, el enfoque de estilo y el uso del almacenamiento local.

1. En la entrada de Copilot Chat, envía este prompt:

   ```plaintext
   ¡Buen trabajo! Añade ahora un conmutador para el modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón, conservar la selección mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
   ```

2. El agente revisará los cambios existentes y propondrá los necesarios para el modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo** según corresponda.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar la interfaz de los conmutadores—, detenlo con el botón **Stop** junto a la entrada del chat y envía una indicación aclaratoria.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, solicita simplificar el CSS con variables, integrar el modo claro en el panel existente o añadir atajos de teclado.

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Puede incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: aplicaciones de Internet enriquecidas accesibles (ARIA, por sus siglas en inglés), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- supervisaste el trabajo de un agente en tiempo real.
- aceptaste y rechazaste ediciones de archivos individuales.
- orientaste una sesión en curso mediante prompts de seguimiento.

En el [siguiente ejercicio][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás lo generado por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
