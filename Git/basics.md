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

### Git flow :sunglasses:

Gitflow es basicamente un patron o una metodologia que existe para llevar las versiones de tu proyecto de manera facil de entender y facil de escalar. Una pequeña explicación de como manejar tus ramas de manera responsable.

![gitflow](https://i.imgur.com/5jP67mK.png)

En la imagen se aprecian 4 ramas, `master`,`develop`,`featureAmarillo`,`featureAzul`, daremos un ejemplo practico para demostrar como se organizan las ramas y la historia de **Git**.

Vemos la primera rama `master`, usualmente a esta rama se le deja la responsabilidad de ser la version estable y probada de nuestro codigo, con esta rama se deberia tener sumo cuidado en cambiar su contenido.

Observamos la rama `develop` la cual parte de la version estable de la `master`, en donde se realizan las pruebas en entornos de producción antes de desplegar una version estable.

Y las ramas `featureAzul`y `featureAmarillo` son nuevas caracteristicas que tendra el proyecto.

#### git add **archivo o flag**

Este comando sirve para añadir los cambios hechos a un archivo o conjunto de ellos, con el flag **punto**`git add .` se agregan todos los archivos que han sufrido cambios desde el ultimo **commit**.

#### git commit -m "mensaje"

Este comando sirve para crear un registro en la historia de las versiones del proyecto, se coloca un mensaje descriptivo de los cambios realizados

> git commit -m "Mensaje descriptivo"

#### git status

Este comando sirve para ver los archivos que han sufrido cambios desde el ultimo commit, usualmente aparecen en color rojo los archivos que no han sido añadidos al stage, y en verde los que si con el comando `git add`

#### git checkout **flag** **rama**

Este comando tiene varios propositos, el principal es navegar entre ramas o crearlas, tambien tiene la función de eliminar los cambios hechos siempre y cuando no hayan sido agregados al stage.

Para cambiar entre ramas solo es necesario realiza el siguiente comando:

> git checkout nombre-rama

Para crear una rama escribir el siguiente comando:

> git checkout -b nombre-rama

Para eliminar los cambios que no han sido agregados:

> git checkout /ruta/al/archivo   |  git checkout .

#### git push **remoto** **rama**

Este comando tiene como finalidad subir los cambios a los que se les ha hecho commit a tu repositorio remoto, que suele ser `origin` el nombre que se le suele dar, recuerda que puedes tener mas de un repositorio remoto por proyecto.

> git push origin nombre-rama

#### git pull **remoto** **rama**

Este comando sirve para traer los cambios que existen en el repositorio remoto que no se posee en tu local.

> git pull origin nombre-rama

#### git merge **rama**

Este comando sirve para unir los cambios entre dos ramas, por ejemplo, cuando existe codigo en uno o varios archivos que necesitas en el desarrollo de un feature. Posicionas tu workspace en la rama que necesites con `git checkout nombre-rama` y colocas el comando indicando la rama de la cual quieres sacar el codigo.

> git merge nombre-rama-a-unir

Esto realiza un commit automatico añadiendo los cambios de la **nombre-rama-a-unir** a tu rama **nombre-rama**.
