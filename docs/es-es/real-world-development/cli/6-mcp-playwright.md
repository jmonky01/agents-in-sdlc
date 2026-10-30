---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y explora la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y proporcionar a Copilot más herramientas y automatizaciones. Los servidores MCP permiten que Copilot vaya más allá de las funciones integradas en la CLI.

En esta lección:

- descubrirás qué es el Protocolo de contexto de modelo (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot utilice el sitio como lo haría un usuario para automatizar las comprobaciones y aumentar la confianza en que los cambios funcionan como se espera.

## ¿Qué es el Protocolo de contexto de modelo (MCP)?

El [Protocolo de contexto de modelo (MCP)][mcp-blog-post] permite a los agentes de inteligencia artificial (IA) comunicarse con herramientas y servicios externos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente y los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: ofrece automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. En el cuadro **Command**, introduce:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para cerrar el cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora pide a Copilot que haga lo mismo mediante el servidor MCP de Playwright.

1. Pide a Copilot que utilice Playwright MCP para validar la funcionalidad:

   ```plaintext
   Inicia la aplicación y utiliza Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que hayas iniciado al terminar.
   ```

> [!NOTE]
> No es imprescindible pedir a Copilot que utilice el servidor MCP; normalmente lo detectará por sí mismo. Mencionarlo ayuda a orientar la tarea y puede hacer que los resultados sean más coherentes.

2. Observa cómo Copilot enumera las acciones que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que el comportamiento es el esperado.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te proporcionará un informe.

## Resumen y próximos pasos

Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- aprendiste qué es MCP y cómo lo utiliza Copilot CLI.
- añadiste el servidor MCP de Playwright si todavía no estaba disponible.
- pediste al agente que controlara un navegador y explorara el filtrado.

A continuación, [crearás un agente de QA personalizado][next-lesson] que combinará la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
