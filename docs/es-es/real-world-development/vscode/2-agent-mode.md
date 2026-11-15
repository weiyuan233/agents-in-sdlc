---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla puede requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo habitual consiste en localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. El modo agente de Copilot puede coordinar estos pasos.

El modo agente trabaja con más autonomía en el IDE: explora la estructura del proyecto, realiza cambios en varios archivos, ejecuta tareas como las pruebas y corrige los errores que encuentra. En este ejercicio lo usarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona, como un compañero de equipo, y se comunica contigo mediante incidencias y PR.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto e identificar los archivos pertinentes.
- el modo agente de GitHub Copilot puede implementar funciones que afectan a la interfaz de usuario y a la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para las editoriales; ahora completarás con Copilot el resto de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle desde su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Ejecútala desde el Codespace.

1. Vuelve al Codespace. Después del ejercicio anterior deberías estar en `main`.
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

   Cuando el servidor de Astro esté listo, verás un mensaje parecido a este:

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal se redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio en marcha, explora sus funciones principales:

- **Página principal**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con información adicional, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

La implementación inicial funciona, pero quieres añadir funciones. El proyecto de plantilla ya ha creado un backlog de incidencias. Pide a GitHub Copilot que lo muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y usar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Se incluye Claude Sonnet 4.5 porque se usó al crear el taller y se espera que produzca sugerencias relativamente coherentes. Como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden variar; es normal.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Envía esta indicación para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que Copilot enumere las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot prioriza los elementos según el valor que podrían aportar a los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás `.github/instructions/ui.instructions.md`, que describe cómo añadir funcionalidades al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace; así se pueden reutilizar directrices y ejemplos y dividir conjuntos de instrucciones grandes.

## Implementar el filtrado

Para completar el filtrado, tendrás que actualizar al menos tres partes de la aplicación:

- añadir o ajustar la lógica restante de filtrado en `src/lib/`.
- añadir o actualizar las pruebas del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Las pruebas deben ejecutarse y completarse correctamente antes de combinar los cambios. Pide al modo agente que implemente la funcionalidad.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente los requisitos de la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos de la capa de datos, la interfaz y las pruebas, los modifica y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar y ejecutar pruebas. Durante el proceso quizá veas los controles **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente sigue trabajando; al terminar podrás aceptar o deshacer todos los cambios.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas podrían fallar durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.
9. Revisa el código generado para detectar posibles problemas.

   ![Sesión completada con cambios propuestos](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has usado el modo agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. En este ejercicio has aprendido cómo:

- Copilot Agent Mode puede implementar funciones en la interfaz y la capa de datos.
- Copilot Agent Mode puede explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas que genera el modo agente antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una PR.

### Ejercicio de exploración adicional: implementar paginación

La lista de juegos puede crecer y necesitar paginación. Aplica lo aprendido y pide a Copilot que añada esta función. Ten en cuenta las prácticas existentes y los archivos de instrucciones, decide si el tamaño de página debe poder seleccionarse o ser fijo y proporciona a Copilot el contexto necesario. Es normal iterar y pedir cambios adicionales.

## Recursos

- [Copilot: modos Ask, Edit y Agent, y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
