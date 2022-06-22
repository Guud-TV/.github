# Gestión de código

<aside>
⚠️ A pesar de que se sugiere el usoo de `git-flow`, en ningún momento debe usarse el comando `finish` para terminar ningún branch. Esto realizaría un `merge` en lugar de el flujo correcto que es una `pull request`.

</aside>

# ¿Qué es Gitflow?

Gitflow es un modelo alternativo de creación de ramas en Git en el que se utilizan ramas de función y varias ramas principales. Fue [Vincent Driessen en nvie](http://nvie.com/posts/a-successful-git-branching-model/) quien lo publicó por primera vez y quien lo popularizó. En comparación con el desarrollo basado en troncos, Gitflow tiene diversas ramas de más duración y mayores confirmaciones. Según este modelo, los desarrolladores crean una rama de función y retrasan su fusión con la rama principal del tronco hasta que la función está completa. Estas ramas de función de larga duración requieren más colaboración para la fusión y tienen mayor riesgo de desviarse de la rama troncal. También pueden introducir actualizaciones conflictivas.

Gitflow puede utilizarse en proyectos que tienen un ciclo de publicación programado, así como para la [práctica recomendada de DevOps](https://www.atlassian.com/es/devops/what-is-devops/devops-best-practices) de [entrega continua](https://www.atlassian.com/es/continuous-delivery). Este flujo de trabajo no añade ningún concepto o comando nuevo, aparte de los que se necesitan para el [flujo de trabajo de ramas de función](https://www.atlassian.com/es/git/tutorials/comparing-workflows/feature-branch-workflow). Lo que hace es asignar funciones muy específicas a las distintas ramas y definir cómo y cuándo deben estas interactuar. Además de las ramas de `función`, utiliza ramas individuales para preparar, mantener y registrar publicaciones. Por supuesto, también puedes aprovechar todas las ventajas que aporta el flujo de trabajo de ramas de función: solicitudes de incorporación de cambios, experimentos aislados y una colaboración más eficaz.

# Inicio

En realidad, Gitflow es una especie de idea abstracta de un flujo de trabajo de Git. Esto quiere decir que ordena qué tipo de ramas se deben configurar y cómo fusionarlas. Explicaremos brevemente los objetivos de las ramas a continuación. El conjunto de herramientas de git-flow es una herramienta de línea de comandos propiamente dicha que tiene un proceso de instalación. El proceso de instalación de git-flow es sencillo. Los paquetes de git-flow están disponibles en varios sistemas operativos. En los sistemas OSX, puedes ejecutar `brew install git-flow`. En Windows, tendrás que [descargar e instalar git-flow](https://git-scm.com/download/win). Después de instalar git-flow, puedes utilizarlo en tu proyecto ejecutando `git flow init`. Git-flow es un contenedor para Git. El comando `git flow init` es una prolongación del comando predeterminado `[git init](https://www.atlassian.com/es/git/tutorials/setting-up-a-repository/git-init)` y no cambia nada de tu repositorio aparte de crear ramas para ti.

# Funcionamiento

![https://wac-cdn.atlassian.com/dam/jcr:a13c18d6-94f3-4fc4-84fb-2b8f1b2fd339/01%20How%20it%20works.svg?cdnVersion=309](https://wac-cdn.atlassian.com/dam/jcr:a13c18d6-94f3-4fc4-84fb-2b8f1b2fd339/01%20How%20it%20works.svg?cdnVersion=309)

# Ramas principales y de desarrollo

En lugar de una única rama `main`, este flujo de trabajo utiliza dos ramas para registrar el historial del proyecto. La rama `main` o principal almacena el historial de publicación oficial y la rama `develop` o de desarrollo sirve como rama de integración para las funciones. Asimismo, conviene etiquetar todas las confirmaciones de la rama `main` con un número de versión.

El primer paso es complementar la rama `main` predeterminada con una rama `develop`. Una forma sencilla de hacerlo es que un desarrollador cree de forma local una rama `develop` vacía y la envíe al servidor:

```bash
git branch develop
git push -u origin develop
```

Esta rama contendrá el historial completo del proyecto, mientras que `main` contendrá una versión abreviada. A continuación, otros desarrolladores deberían clonar el repositorio central y crear una rama de seguimiento para `develop`.

A la hora de utilizar la biblioteca de extensiones de git-flow, ejecutar `git flow init` en un repositorio existente creará la rama `develop`:

```bash
$ git flow init
Initialized empty Git repository in ~/project/.git/
No branches exist yet. 
Base branches must be created now.
Branch name for production releases: [main]
Branch name for "next release" development: [develop]
How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
git branch* develop main
```

# Ramas de función

Todas las funciones nuevas deben residir en su propia rama, que se puede enviar al repositorio central para copia de seguridad/colaboración. Sin embargo, en lugar de ramificarse de `main`, las ramas `feature` utilizan la rama `develop` como rama primaria. Cuando una función está terminada, se vuelve a fusionar en develop. Las funciones no deben interactuar nunca directamente con `main`.

![https://wac-cdn.atlassian.com/dam/jcr:34c86360-8dea-4be4-92f7-6597d4d5bfae/02%20Feature%20branches.svg?cdnVersion=309](https://wac-cdn.atlassian.com/dam/jcr:34c86360-8dea-4be4-92f7-6597d4d5bfae/02%20Feature%20branches.svg?cdnVersion=309)

Ten en cuenta que las ramas `feature` combinadas con la rama `develop` conforman, a todos efectos, el flujo de trabajo de ramas de función. Sin embargo, el flujo de trabajo Gitflow no termina aquí.

Las ramas `feature` suelen crearse a partir de la última rama `develop`.

# Creación de una rama de función

Sin las extensiones de git-flow:

```bash
git checkout develop
git checkout -b feature_branch
```

Cuando se utiliza la extensión de git-flow:

```
git flow feature start feature_branch
```

Sigue trabajando y utiliza Git como lo harías normalmente.

# Finalización de una rama de función

Cuando hayas terminado con el trabajo de desarrollo en la función, el siguiente paso es fusionar `feature_branch` en `develop`. Para ello debemos publicar (si no lo hemos hecho ya) el branch en **GitHub** y generar una **Pull Request**

Sin las extensiones de git-flow:

```bash
git checkout develop
git push feature_branch
```

Con las extensiones de git-flow:

```
git flow feature publish feature_branch
```

Para finalizar, deber crear la [Pull Request en GitHub](https://www.notion.so/Gesti-n-de-c-digo-0aea794b90074608965211e54052f2f5)

# Ramas de publicación

![https://wac-cdn.atlassian.com/dam/jcr:8f00f1a4-ef2d-498a-a2c6-8020bb97902f/03%20Release%20branches.svg?cdnVersion=309](https://wac-cdn.atlassian.com/dam/jcr:8f00f1a4-ef2d-498a-a2c6-8020bb97902f/03%20Release%20branches.svg?cdnVersion=309)

Cuando `develop` haya adquirido suficientes funciones para una publicación (o se acerque una fecha de publicación predeterminada), debes bifurcar una rama `release` (o de publicación) a partir de `develop`. Al crear esta rama, se inicia el siguiente ciclo de publicación, por lo que no pueden añadirse nuevas funciones una vez pasado este punto (en esta rama solo deben producirse las soluciones de errores, la generación de documentación y otras tareas orientadas a la publicación). Cuando está lista para el lanzamiento, la rama `release` se fusiona en `main` y se etiqueta con un número de versión. Además, debería volver a fusionarse en `develop`, ya que esta podría haber progresado desde que se iniciara la publicación.

Utilizar una rama específica para preparar publicaciones hace posible que un equipo perfeccione la publicación actual mientras otro equipo sigue trabajando en las funciones para la siguiente publicación. Asimismo, crea fases de desarrollo bien definidas (por ejemplo, es fácil decir: "Esta semana nos estamos preparando para la versión 4.0" y verlo escrito en la estructura del repositorio).

Crear ramas `release` es otra operación de ramificación sencilla. Al igual que las ramas `feature`, las ramas `release` se basan en la rama `develop`. Se puede crear una nueva rama `release` utilizando los siguientes métodos.

Sin las extensiones de git-flow:

```bash
git checkout develop
git checkout -b release/0.1.0
```

Cuando se utilizan las extensiones de git-flow:

```bash
git flow release start 0.1.0
Switched to a new branch 'release/0.1.0'
```

En cuanto la publicación esté lista para su lanzamiento, se fusionará en las ramas `main` y `develop`, y luego se eliminará la rama `release`. Es importante volver a fusionarla en la rama `develop` porque podrían haberse añadido actualizaciones importantes a la rama `release`, y las funciones nuevas tienen que poder acceder a ellas. Si en tu organización se da mucha importancia a la revisión de código, este sería el lugar ideal para una solicitud de incorporación de cambios.

Para finalizar una rama `release`, utiliza los siguientes métodos:

Sin las extensiones de git-flow:

```bash
git checkout main
git push release/0.1.0
```

O con la extensión de git-flow:

```bash
git flow release publish '0.1.0'
```

Para finalizar, deber crear la [Pull Request en GitHub](https://www.notion.so/Gesti-n-de-c-digo-0aea794b90074608965211e54052f2f5)

# Ramas de corrección

![https://wac-cdn.atlassian.com/dam/jcr:cc0b526e-adb7-4d45-874e-9bcea9898b4a/04%20Hotfix%20branches.svg?cdnVersion=309](https://wac-cdn.atlassian.com/dam/jcr:cc0b526e-adb7-4d45-874e-9bcea9898b4a/04%20Hotfix%20branches.svg?cdnVersion=309)

Las ramas de mantenimiento, de corrección o de `hotfix` sirven para reparar rápidamente las publicaciones de producción. Las ramas `hotfix` son muy similares a las ramas `release` y `feature`, salvo por el hecho de que se basan en la rama `main` y no en la `develop`. Esta es la única rama que debería bifurcarse directamente a partir de `main`. Cuando se haya terminado de aplicar la corrección, debería fusionarse en `main` y `develop` (o la rama `release` actual), y `main` debería etiquetarse con un número de versión actualizado.

Tener una línea de desarrollo específica para la corrección de errores permite que tu equipo aborde las incidencias sin interrumpir el resto del flujo de trabajo ni esperar al siguiente ciclo de publicación. Puedes concebir las ramas de mantenimiento como ramas `release` ad hoc que trabajan directamente con la rama `main`. Se puede crear una nueva rama `hotfix` utilizando los siguientes métodos:

Sin las extensiones de git-flow:

```bash
git checkout main
git checkout -b hotfix_branch
```

Cuando se utilizan las extensiones de git-flow:

```bash
git flow hotfix start hotfix_branch
```

Al igual que al finalizar una rama `release`, una rama `hotfix` se fusiona tanto en `main` como en `develop`.

```bash
git push hotfix_branch
```

O con la extensión de git-flow:

```bash
git flow hotfix publish hotfix_branch
```

Para finalizar, deber crear 2  [Pull Request en GitHub](https://www.notion.so/Gesti-n-de-c-digo-0aea794b90074608965211e54052f2f5), una para para rama `main` y otra para la rama `develop`

# Resumen

En este artículo, hemos explicado el flujo de trabajo de Gitflow. Gitflow es uno de los muchos estilos de [flujos de trabajo de Git](https://www.atlassian.com/es/git/tutorials/comparing-workflows) que podéis utilizar tu equipo y tú.

Algunos puntos clave que debes saber sobre Gitflow son los siguientes:

- Este flujo de trabajo es ideal para los flujos de trabajo de software basados en publicaciones.
- Gitflow ofrece un canal específico para las correcciones de producción.

El flujo general de Gitflow es el siguiente:

1. Se crea una rama `develop` a partir de `main`. 
2. Se crea una rama `release` a partir de la `develop`.
3. Se crean ramas `feature` a partir de la `develop`.
4. Cuando se termina una rama `feature`, se fusiona en la rama `develop`.
5. Cuando la rama `release` está lista, se fusiona en las ramas `develop` y `main`.
6. Si se detecta un problema en `main`, se crea una rama `hotfix` a partir de `main`.
7. Una vez terminada la rama `hotfix`, esta se fusiona tanto en `develop` como en `main`.