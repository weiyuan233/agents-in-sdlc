---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente en la nube de Copilot tres tareas para mejorar la experiencia y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas con solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar una sesión que sigue en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo la funcionalidad de alternancia, orientarás el trabajo del agente para que incluya ambos modos.

## Revisar las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores del Model Context Protocol (MCP).
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Revisa también el panel de PR de la derecha, donde puedes consultar la PR y los archivos modificados.

## Guiar al agente en la nube

Pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende de cómo lo haya llamado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos, hasta que el agente termine la configuración y empiece a trabajar. Lo notarás cuando aparezcan mensajes que indiquen que ha comenzado la tarea.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta indicación:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Cuadro de diálogo para guiar una sesión activa del agente en la nube](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje. La tarea puede tardar varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o añadir indicaciones.

En este ejercicio has:

- explorado mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las PR que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
