---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has usado GitHub Copilot para mejorar la experiencia y añadir funcionalidades: has pedido que documente el código, que añada una sección de juegos relacionados y que implemente funciones de accesibilidad con modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la inteligencia artificial (IA) generativa. Hay que revisar el código generado y seguir los procesos habituales. Antes de habilitar la revisión para el resto del equipo, revisarás las propuestas de Copilot para la documentación, los juegos relacionados y la accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Model Context Protocol (MCP).

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) creada por el agente: la que añade documentación al código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR indique que GitHub Copilot está bloqueado por un firewall. Es lo esperado: el acceso del agente a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que puede incluir nuevos comentarios TSDoc y otra documentación. Los cambios exactos varían. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario, según los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el aviso de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos se pongan en cola en la sección de comprobaciones de la PR. Las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente; pueden tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración en una PR no tiene por qué ser unidireccional. Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código; Copilot los verá e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay una indicación única. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario con una breve descripción al principio de cada archivo de código.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot** como harías con otra persona. Usa una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y aplica los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, busca el nuevo commit en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el aviso de que hay flujos de trabajo pendientes de aprobación y, si aparece, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Comprueba que las comprobaciones del proyecto para la aplicación Astro se completen correctamente. Puede tardar unos minutos.
8. **Opcional:** cambia a la rama del agente en tu Codespace para probar manualmente la funcionalidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes pedir más cambios mediante un comentario en la pestaña **Conversation**; por ejemplo, ajustar el número de juegos que se muestra o añadir encabezados y comentarios TSDoc. Ten en cuenta que esta incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa la PR de accesibilidad creada con el agente personalizado. Debe incluir el modo de alto contraste de la incidencia del ejercicio 3 y el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR incluirá la etiqueta **[WIP]**. Espera unos minutos a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación y presta atención a:
   - los componentes de interfaz para alternar entre los modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) para el alto contraste y el modo claro.
   - los atributos de accesibilidad, como las etiquetas de aplicaciones de Internet enriquecidas y accesibles (ARIA) y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los modos.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el aviso de que hay flujos de trabajo pendientes de aprobación y, si aparece, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que las comprobaciones del proyecto para la aplicación Astro se completen correctamente. Puede tardar unos minutos.
9. **Opcional:** cambia a la rama del agente en tu Codespace para probar las funciones de accesibilidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado el trabajo de Copilot para aplicar buenas prácticas. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario de la PR como hiciste con las solicitudes anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicar, crea incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles de los juegos.
- implementar la paginación en la página con la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para comprobar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como mission control** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado las PR del agente y has iterado sobre ellas**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y pasos siguientes

Has completado el itinerario del agente en la nube. Para seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP en el entorno de desarrollo integrado (IDE).
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA, Playwright MCP, Agent Merge y la opción `/delegate` para volver a delegar en el agente en la nube.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
