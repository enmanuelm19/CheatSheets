## Lo básico

En esta sección discutiremos los comandos basicos de **Git** y su relación con el git flow. Comandos:

* git init
* git clone **url**
* git add **archivo o flag**
* git commit -m **"mensaje"**
* git status
* git checkout **flag** **rama**
* git push **remoto** **rama**
* git pull **remoto** **rama**
* git merge **rama**

Segun las situaciones en la cual nos encontremos en el desarrollo de un proyecto, libro, etc. Que necesite control de versiones se usaran los comandos mencionados anteriormente.

#### git init

Sobre la raiz de tu proyecto

> git init

Este comando sirve para inicializar un repositorio que no tenga git como controlador de versiones, esto crea dentro de la raiz de tu proyecto una carpeta `.git/` en la cual se registran la historia de tu proyecto y sus versiones.

También se crea un archivo `.gitignore` donde se declaran los archivos que se van a ignorar, es decir, que no se llevara registro de los cambios en este archivo. Recomiendo totalmente dirigirse a [Gitignore](https://www.gitignore.io/) donde colocas las tecnologias/herramientas que usara tu proyecto y este te genera un documento muy acertado de los archivos a ignorar.

#### git clone **url**

Este comando se usa en el caso que trabajes con un proyecto que ya tenga configurado **Git** y este en un repositorio remoto (repositorio alojado en alguna pagina como **Github**,**Gitlab**,**Bitbucket**, etc). Escribir el comando posicionado en la carpeta donde quieres tener tu codigo localmente:

> git clone https://github.com/usuario/repositorio.git

Este comando descargara el codigo que se encuentre en el repositorio remoto y la estructura del proyecto en tu maquina local, descarga todas las ramas con su historia y contenido.
