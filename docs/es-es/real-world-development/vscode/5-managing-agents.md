---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra en tiempo real las llamadas a herramientas y las ediciones. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con nuevas indicaciones sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- guiarás al agente durante la sesión para añadir un modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide añadir también un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, ampliarás la tarea en la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, revisa las diferencias:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para revisar todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que termine el modo de alto contraste. El agente debería mostrar un mensaje que resuma los cambios.

## Guiar al agente para añadir un modo claro

Cuando esté listo el modo de alto contraste, amplía la misma conversación con una opción para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de los componentes, los estilos y el uso de almacenamiento local.

1. Envía esta indicación en el campo de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Observa cómo el agente examina los cambios existentes y propone añadir el modo claro.
3. Revisa cada edición y selecciona **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar el área de los controles—, detenlo con el botón **Stop** junto al campo de chat y envía una indicación para aclarar lo que necesitas.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplify the CSS — use CSS variables instead of duplicating selectors»), corregir el enfoque («don't add a third toggle, integrate light mode into the existing settings panel») o ampliar el alcance («also add keyboard shortcuts for both toggles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación con teclado, semántica del lenguaje de marcado de hipertexto (HTML) y almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has supervisado y guiado una sesión del modo agente de Copilot Chat: has revisado las ediciones, aceptado o rechazado cambios y enviado indicaciones adicionales para ampliar el trabajo sin perder el contexto.

En este ejercicio has:

- supervisado el trabajo de un agente en tiempo real.
- aceptado y rechazado ediciones individuales.
- guiado una sesión en curso con indicaciones adicionales.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una PR y revisarás los cambios que ha realizado el agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
