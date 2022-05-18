---
title: "INTALL DOCKER DEBIAN"
tags: ""
---

# Instalación y Uso de Docker en Debian 10

## Introducción
Docker trata de simplificar la gestión de los procesos de aplicación utilizando secciones que se denominan contenedores. Los contenedores permiten ejecutar aplicaciones en procesos cuyos recursos están aislados del resto de procesos en ejecución. El concepto es similar a las máquinas virtuales, no obstante los contenedores de Docker permiten una mejor gestión de los recursos de la maquina, más portables y dependientes del sistema operativo que actua como host.

## Instalación de Docker

Docker está disponible en los repositorios oficiales de Debian, no obstante es posible que la versión disponible no sea la más actualizada. Con el siguiente comando podríamos instalar ka versión disponible desde el repositorio oficial de Debian:

```shell
apt install docker.io docker-compose
```
No obstante, para asegurarnos de instalar la última versión desde el repositorio oficial de Docker tenemos que añadir la clave GPG que permitirá asegurarnos que las descargas desde este repositorio son válidas.

Antes de comenzar el proceso, nos aseguramos que los repositorios estén actualizados:

```shell
apt update
```
Para permitir la instalación de paquetes a través de apt mediante HTTPS, instalamos los siguientes paquetes.

```shell
apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```
Después descargamos la clave GPG de los repositorios oficiales de Docker y la añadimos al sistema de paquetes apt:

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
```
Añadimos el repositorio de Docker a APT sources:

```shell
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```
Actualizamos los repositorios del sistema para que APT tenga en cuenta los paquetes que aparecen del repositorio de Docker que acabamos de añadir.

```shell
apt update
```
Podemos ejecutar el siguiente comando para conocer si el paquete a instalar es aquel que está disponible en los repositorios de Docker.

```shell
apt-cache policy docker-ce
```
Si el paquete candidato a ser instalado es aquel que será descargado desde los repositorios de Docker procedemos a su instalación:

```shell
apt install docker-ce
```
Cuando el proceso termine, tendremos instalado Docker en nuestro servidor, el servicio iniciado y configurado para que se inicie al arrancar el sistema. Podemos comprobar si el servicio está en funcionamiento con el comando:

```shell
systemctl status docker
```
Durante la instalación de Docker además del servicio (*daemon*) se instala la línea de comandos (CLI) y el cliente Docker. 

## Utilizar el comando Docker

El comando docker funciona como cualquier otro comando de Linux con la palabra docker seguida de una serie de argumentos. La sintaxis del comando tiene la siguiente forma:

```shell
docker [option] [command] [arguments]
```
Para ver el resto de subcomandos disponibles podemos introducir el comando docker sin argumentos

```shell
docker
```
```
As of Docker 18, the complete list of available subcommands includes:

Output
attach      Attach local standard input, output, and error streams to a running container
build       Build an image from a Dockerfile
commit      Create a new image from a container's changes
cp          Copy files/folders between a container and the local filesystem
create      Create a new container
diff        Inspect changes to files or directories on a container's filesystem
events      Get real time events from the server
exec        Run a command in a running container
export      Export a container's filesystem as a tar archive
history     Show the history of an image
images      List images
import      Import the contents from a tarball to create a filesystem image
info        Display system-wide information
inspect     Return low-level information on Docker objects
kill        Kill one or more running containers
load        Load an image from a tar archive or STDIN
login       Log in to a Docker registry
logout      Log out from a Docker registry
logs        Fetch the logs of a container
pause       Pause all processes within one or more containers
port        List port mappings or a specific mapping for the container
ps          List containers
pull        Pull an image or a repository from a registry
push        Push an image or a repository to a registry
rename      Rename a container
restart     Restart one or more containers
rm          Remove one or more containers
rmi         Remove one or more images
run         Run a command in a new container
save        Save one or more images to a tar archive (streamed to STDOUT by default)
search      Search the Docker Hub for images
start       Start one or more stopped containers
stats       Display a live stream of container(s) resource usage statistics
stop        Stop one or more running containers
tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
top         Display the running processes of a container
unpause     Unpause all processes within one or more containers
update      Update configuration of one or more containers
version     Show the Docker version information
wait        Block until one or more containers stop, then print their exit codes
To view the options available to a specific command, type:
docker docker-subcommand --help
```
Con ```docker info``` podemos encontrar la información del sistema sobre el propio comando.

## Trabajar con imágenes de Docker
Los contenedores de Docker se construyen a partir de imágenes de Docker. Estas imágenes están disponibles en el Docker Hub desde donde podemos descargarlas. Cualquiera puede subir estas imágenes al Docker Hub y ahí podemos encontrar la mayor parte de las distribuciones de Linux y paquetes de software tendrán imágenes alojadas allí.

Existe una distribución de ejemplo que podemos descargar para comprobar que toda nuestra instalación funciona correctamente.

```shell
docker run hello-world
```
Cuando ejecutamos el comando ```docker run``` se comprueba que la imagen no esté disponible de forma local, si no está disponible se procede a descargarla desde el repositorio de Docker Hub. Cuando esta imagen se descarga, Docker crea un contenedor para la imagen y ejecuta la misma.

Podemos buscar las imágenes disponibles en Docker Hub utilizando el siguiente comando:

```shell
docker search ubuntu
```
Con ello, obtendremos un listado del software disponible dentro del Docker Hub donde tenemos un listado de imágenes, el numero de votos que tienen cada una y referencias a si son oficiales o si se crean de forma automatizada.

Con el comando ```pull``` podemos descargar la imagen y que esté disponible a nivel local. A diferencia del comando ```run``` el comando ```pull``` solamente descarga, sin ejecutar el contenedor.

```shell
docker pull ubuntu
```

Una vez descargada la imagen podemos ejecutarla en un contenedor con el comando ```run```.

Para ver las imagenes que se han descargado en nuestra máquina podemos ejecutar el comando:

```shell
docker images
```
Estas imágenes que tenemos descargadas pueden ser modificadas para generar nuevas imágenes que pueden ser subidas a Docker Hub con el comando ```push```.

## Ejecutar un contenedor en Docker
La ejecución del contenedor como el de la imagen de Ubuntu, podemos utilizar los modificadores *-i* y *-t* que permite acceder al un shell dentro del contenedor donde podemos ejecutar comandos.

```shell
docker run -it ubuntu
```

Se mostrará el prompt similar al siguiente donde se muestra el *id* del contenedor que en el ejemplo será d9b100f2f636 y que es necesario conocer para poder gestionar el contenedor.

```shell
Output
root@d9b100f2f636:/#
```

## Gestionar los contenedores de Docker
En una máquina pueden existir multitud de contenedores en Docker. Para ver aquellos contenedores que están activos en el sistema podemos ejecutar el siguiente comando:

```shell
docker ps
```
Para ver todos los contenedores, tanto aquellos que están activos como inactivos podemos añadir el modificador *-a* al comando anterior.

```shell
docker ps -a
```
Para ver el último contenedor que hemos creado utilizaremos el modificador *-l*.

```shell
docker ps -l
```
Para arrancar o parar un contenedor utilizamos la orden seguida de los subcomandos *start* o *stop* y el identificador del contenedor o el nombre del contenedor (*friendly name*)

```shell
docker start d42d0bbfbd35
```
o

```shell
docker stop friendly_volhard
```

Para eliminar un contendor podemos utilizar el subcomando *rm* identificando el contenedor con el ID o el nombre del contenedor. Para averiguar el nombre del contenedor o ID podemos utilizar el comando ```docker ps -a```.

```shell
docker rm elegant_neumann
```
Como parámetro del comando *run* podemos utilizar el modificador *--name* para designar un nombre a un contenedor. También podemos utilizar el modificador *--rm* para que el contendor se borre cuando el contenedor se pare.

## Guardando los cambios de un contenedor a una imagen de Docker

Cuando ejecutamos una imagen de Docker podremos crear, modificar y borrar ficheros como si trabajaramos con una máquina virtual. Los cambios que realicemos en la imagen solo se modificarán en el contenedor que hayamos desplegado, perdiéndose cuando borremos ese contenedor. 

A partir de los contenedores y los cambios que se hayan realizado en el mismo, podemos generar nuevas imágenes.

Para aplicar los cambios realizados en un contenedor a una imagen utilizamos la orden commit

```shell
docker commit -m "Cambios realizados" -a "Nombre del autor" id_contenedor repositorio/nuevo_nommbre_imagen
```
* **-m**. Permite definir los cambios realizados en el commit de la imagen con un mensaje. 
* **-a**. Permite especificar el autor de los cambios realizados.
* **id_contenedor**. Es el código alfanumérico que identifica el contenedor cuando ejecutamos el comando ```docker ps -a```.
* **repositorio**. A menos que se hayan creado repositorios adicionales en Docker Hub, el repositorio suele ser el nombre de usuario con el que nos hemos registrado en Docker Hub.

Un ejemplo del comando anterior podría ser el siguiente:

```shell
docker commit -m "added Node.js" -a "sammy" d42d0bbfbd35 sammy/ubuntu-nodejs
```

Cuando realizamos un commit a una imagen, esta se guarda de forma local. Si ejecutamos el comando ```docker images``` podemos ver las imágenes descargadas en local.

También podemos crear imágenes a partir de un fichero Dockerfile, que permite automatizar la instalación de software en una nueva imagen. 

## Subir nuevas imágenes a un nuevo repositorio

Para subir las imágenes al Docker Hub tenemos que logearnos primero con nuestro usuario y contraseña de Docker Hub.

```shell
docker login -u usuario_docker
```
Con este comando nos aparecerá la opción para introducir el usuario y la contraseña de Docker Hub. Si el procedimiento ha funcionado correctamente podremos subir la imagen al repositorio con el siguiente comando:

```shell
docker push usuario_docker/nombre_imagen_docker
```
Cuando tenemos la imágen subida podremos realizar en cualquier máquina un comando ```docker pull``` y desplegar nuestra configuración de forma rápida y con menor gestión de recursos.
