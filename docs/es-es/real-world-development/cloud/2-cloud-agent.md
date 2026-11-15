---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica, como problemas de seguridad sin resolver, código heredado que hay que actualizar o funcionalidades que llevan tiempo en el backlog. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, puedes delegar tareas y dedicar tiempo a otros asuntos.

Explorarás cómo:

- personalizar el entorno en el que se genera código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación para TypeScript (TSDoc) a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para hacer pruebas de aceptación de la experiencia. Esta tarea bloquea el trabajo del equipo, aunque hay otras incidencias con más prioridad.

Son tareas que pueden quedar relegadas y que se pueden asignar al agente en la nube de Copilot. El agente trabaja de forma asíncrona; después puedes revisar su trabajo y comprobar que el resultado sea correcto.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna, Copilot configura un entorno, crea una PR en borrador para registrar el progreso y comienza la tarea. Puedes consultar la sesión mientras sigue en curso o cuando haya terminado. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de unas instrucciones bien delimitadas

GitHub Copilot no es magia: sigue siendo necesario pensar. Incluso las tareas aparentemente sencillas pueden tener complejidad oculta. [Define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian al incorporar inteligencia artificial (IA) generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas. Si ya completaste el ejercicio práctico, has visto `.github/copilot-instructions.md` en funcionamiento; si no, revísalo ahora. Antes de asignar trabajo al agente, examina en modo de solo lectura los archivos de instrucciones del repositorio para poder reconocer su efecto más adelante.

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Más adelante se volverán a revisar estas instrucciones.

## Configurar el entorno de desarrollo del agente en la nube

Crear código suele requerir configurar un entorno y ejecutar scripts de preparación. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. El laboratorio ya incluye esta configuración: proporciona Node.js, las dependencias, los navegadores de Playwright y la base de datos SQLite migrada y preparada para la aplicación Astro.

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios para estos pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio en los pasos de configuración, por ejemplo, para instalar dependencias, se necesita el permiso `contents: read`. Si no se clona el repositorio, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (toda la aplicación ahora usa Astro + Drizzle/libSQL)
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

      # Migra y prepara la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Tiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions**. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque la documentación es importante, muchos proyectos tienen información desactualizada o carecen de ella. Esa deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación; más adelante revisarás su trabajo.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de asignación de la incidencia a Copilot](../../../_images/ex4-assign-copilot-details.png)
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR nueva, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno con el flujo de trabajo anterior, analizando el proyecto y definiendo un enfoque para la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR y busca el aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza el estado, planifica y crea código.

La tarea puede tardar varios minutos. Mientras Copilot trabaja en segundo plano, asígnale otra tarea y dedica el tiempo a desarrollar una funcionalidad.

## Añadir una sección de juegos relacionados

Una ventaja del agente en la nube de GitHub Copilot es que permite dividir el trabajo: puedes centrarte en una tarea mientras el agente trabaja en otra. Asigna al agente la sección de juegos relacionados que solicita el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, en el cuadro de asignación, selecciona **Assign**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

La primera incidencia recibirá un símbolo de ojos para indicar que Copilot está trabajando en ella. El agente también empezará a trabajar en la segunda solicitud. Como trabaja de forma similar a un ingeniero de software, no es necesario vigilarlo continuamente; revisa el resultado cuando termine.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

## Resumen y pasos siguientes

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona y pedirle que aborde deuda técnica, cree funcionalidades o ayude a migrar código entre frameworks.

En este ejercicio has:

- personalizado el entorno de generación de código.
- considerado cómo realizar las operaciones de forma segura.
- aprendido por qué es importante delimitar bien las incidencias.
- asignado incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. El [agente en la nube también puede usar servidores del Model Context Protocol (MCP)][cloud-agent-mcp] y las instrucciones personalizadas que has explorado.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
