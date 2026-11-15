---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información de fondo, ese contexto debe estar disponible. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos específicos de rutas.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar para editoriales) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás la misma indicación y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la base del filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares. Copilot no es determinista. El objetivo es observar la tendencia después de actualizar las instrucciones, no obtener siempre exactamente el mismo resultado.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene estos requisitos y directrices de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación para TypeScript (TSDoc).
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Usarás archivos de instrucciones para proporcionar a Copilot la información necesaria para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para orientar sus sugerencias. Pueden especificar convenciones de programación, bibliotecas y comentarios. Se pueden crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información de nivel de proyecto pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción del trabajo, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos para tipos de archivo o tareas concretos, como TypeScript, Astro, componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerir las siguientes ediciones.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como su descripción, estructura y estándares generales.
- Usa `*.instructions.md` para tipos de archivo, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural y mantén las directrices claras. Incluye ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación permitirá encontrar el enfoque adecuado para cada proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. En este proyecto hay instrucciones para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. El proceso varía según el entorno; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Revisa los archivos de instrucciones que incluye el repositorio, ya sea en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas para **GitHub Actions Workflows**; se aplican a todas las interacciones con Copilot.
3. Explora la carpeta `.github/instructions`, que incluye instrucciones para Astro, la capa de datos Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en `applyTo`, que define un patrón global relativo a la raíz del repositorio. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Examina los enlaces a otros archivos de instrucciones y a archivos existentes. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> **Code formatting requirements** documenta los estándares del proyecto, pero todavía no exige documentación dentro del código. Añadirás reglas para comentarios TSDoc y encabezados de comentario.

## Crear una rama para los cambios

Crea una rama para trabajar en los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso lo indican cuando es necesario.

Primero enviarás una indicación con las instrucciones actuales. Más adelante actualizarás el archivo y repetirás la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, que pueden incluir el archivo auxiliar y sus pruebas.
6. Observa que la función propuesta tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de `.github/instructions/drizzle.instructions.md`, aplicables a `src/lib/*.ts`.
7. Observa si el código propuesto carece de comentarios TSDoc y de un encabezado de comentario para el archivo.

> [!CAUTION]
> Copilot puede añadir comentarios aunque no se le soliciten. Si ocurre, no pasa nada: el objetivo es observar la mejora de coherencia al actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

Añade un estándar de programación al archivo principal de instrucciones para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Todavía no incluye documentación dentro del código.
3. Debajo de los estándares existentes, añade:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio son adecuadas para estándares de todo el proyecto, como la regla de comentarios de documentación. Las instrucciones `*.instructions.md` sirven para lenguajes, tipos de archivo y tareas concretos.

## Repetir la indicación y observar los cambios

Envía la misma indicación con las instrucciones actualizadas y observa el resultado.

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para centrar Copilot en el archivo.
3. Envía la misma indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa si el archivo comienza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa si la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que aplique un estándar del proyecto al código real sobre el que se basará el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que otros archivos del repositorio.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que estén presentes los cambios de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y los archivos de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Abre una solicitud de incorporación de cambios (PR) para la rama y relaciónala con la incidencia sobre los estándares de documentación. Las herramientas de GitHub ya están conectadas al proyecto, así que puedes pedir a Copilot que encuentre la incidencia y cree la PR.

> [!NOTE]
> Este es el primer ejercicio en que Copilot actuará en GitHub. La plantilla ya conecta las herramientas de GitHub al espacio de trabajo; la primera vez que Copilot las use, VS Code podría pedirte que inicies sesión. Sigue las instrucciones para autorizarlo. Más adelante conocerás la conexión, basada en Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual, indicando que también se ha añadido la base de la función auxiliar:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Espera a que Copilot encuentre la incidencia y cree la PR.
5. Cuando Copilot solicite permiso para realizar acciones de GitHub, revisa la operación y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa cada operación y selecciona **Allow** si corresponde.

Ya has creado y combinado una PR con ayuda de GitHub Copilot.

## Resumen y pasos siguientes

En este ejercicio:

- has explorado cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones del repositorio y las instrucciones específicas de rutas.
- has enviado una indicación de generación de código, observado el resultado con las instrucciones existentes y generado la primera parte de la capa de datos para el filtrado: la función auxiliar de editoriales.
- has añadido un estándar general a `.github/copilot-instructions.md`.
- has repetido la indicación y observado cómo el código propuesto adopta el estándar.
- has confirmado las instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
