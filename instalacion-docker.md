---
title: "INSTALACION DOCKER"
tags: ""
---

# INSTALACION DOCKER 

Procedemos instalacion de docker pero antes deberemos actualizar los paquetes de nuestros repositorios

```shell
apt-get update
```

INSTALACION

```shell
apt-get install docker
```

```shell
apt-get install docker.io
```

Una vez instalado deberemos buscar contenedores ya previamente ya compilados en la pagina web de [DOCKER HUB](https://hub.docker.com/) 

### DOCKER RUN 

çdocker run El comando se usa para ejecutar un contenedor desde una imagen especificando el Image ID o Repository y/o Tag nombre.

```shell
$ docker run {image}
```

Ejemplo:

```shell
$ docker run nginx
```

El comando anterior ejecuta una instancia de <span class="NormalTextRun SpellingErrorV2 SCXW251451022 BCX0">nginx</span> aplicación en un host de Docker, si ya existe. Si no existe en el host de Docker, sale al concentrador de Docker (de forma predeterminada) y extrae la imagen. Pero esto se hace la única primera vez. Para las siguientes ocasiones, se reutiliza la misma imagen.

Si desea ejecutar una versión particular de una imagen, especifique su versión separada por dos puntos. Esto se conoce como Tag. En caso de que no especifique ninguna etiqueta, Docker la considerará de forma predeterminada como la última.

Además, si desea ejecutar el contenedor en segundo plano en un modo separado para volver al indicador después de que Docker lance el contenedor, use -d bandera.

Ejemplo:

```shell
$ docker run nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
33847f680f63: Pull complete
dbb907d5159d: Pull complete
8a268f30c42a: Pull complete
b10cf527a02d: Pull complete
c90b090c213b: Pull complete
1f41b2f2bf94: Pull complete
Digest: sha256:8f335768880da6baf72b70c701002b45f4932acae8d574dedfddaf967fc3ac90
Status: Downloaded newer image for nginx:latest
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2021/08/15 12:13:23 [notice] 1#1: using the "epoll" event method
2021/08/15 12:13:23 [notice] 1#1: nginx/1.21.1
2021/08/15 12:13:23 [notice] 1#1: built by gcc 8.3.0 (Debian 8.3.0-6)
2021/08/15 12:13:23 [notice] 1#1: OS: Linux 5.8.0-1039-azure
2021/08/15 12:13:23 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2021/08/15 12:13:23 [notice] 1#1: start worker processes
2021/08/15 12:13:23 [notice] 1#1: start worker process 33
2021/08/15 12:13:23 [notice] 1#1: start worker process 34
```
### DOCKER PS

docker ps El comando enumera todos los contenedores en ejecución y cierta información básica sobre ellos. Como el ID del contenedor, el nombre de la imagen, la hora en que se crea el contenedor, el estado actual y el nombre del contenedor. Cada contenedor recibe un nombre aleatorio (si no se especifica explícitamente) y un ID.

Ejemplo:

```shell
$ docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES 
133f5e0267a5   nginx     "/docker-entrypoint.…"   10 seconds ago   Up 10 seconds   80/tcp    jolly_elion 
```

Para enumerar todos los contenedores en ejecución y no en ejecución / salidos a la vez, puede usar:

```shell
$ docker ps -a
```

Ejemplo:

```shell
$ docker ps -a 
CONTAINER ID   IMAGE         COMMAND                  CREATED        STATUS                    PORTS     NAMES 
fcec129f0eb4   nginx         "/docker-entrypoint.…"   46 hours ago   Exited (0) 46 hours ago             interesting_ishizaka 
6e8b1e441aa6   hello-world   "/hello"                 2 days ago     Exited (0) 2 days ago               keen_shirley 
```

### DOCKER LS

Me gusta ps mando, ls también se puede utilizar para enumerar contenedores. -a flag se puede usar para listar todos los contenedores (no solo los que están en ejecución).

```shell
$ docker container ls
```

Ejemplo:

```shell
$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
15796e91c30b   redis     "docker-entrypoint.s…"   2 seconds ago    Up 1 second     6379/tcp   flamboyant_neumann
904390b65d45   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   80/tcp     nginx_new
```

### DOCKER STOP 

docker stop El comando se usa para detener un contenedor en ejecución. Aquí tenemos que poner el nombre o ID del contenedor junto con esto.

```shell
$ docker stop {container-id}
```

En caso de éxito, devolvería el nombre o ID de la ventana acoplable.

Ejemplo:

```shell
$ docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES 
133f5e0267a5   nginx     "/docker-entrypoint.…"   50 seconds ago   Up 49 seconds   80/tcp    jolly_elion 
```

Esto devolverá el CONTAINER ID que puede utilizar para detener el contenedor.

```shell
$ docker stop 133f5133f5
```

Para este ejemplo y los siguientes, tenga en cuenta que no es necesario especificar un valor completo de CONTAINER ID. Aceptará hasta la pieza, lo que lo hace único entre otros contenedores en ejecución, ya que Docker sabe qué contenedor detener.
rm Command

docker rm comando elimina un contenedor detenido o salido.

```shell
$ docker rm {CONTAINER NAME or ID}
```

Ejemplo:

```shell
$ docker rmi 133f5133f5
```

### DOCKER EXEC

Podemos utilizar exec comando para ir dentro de un contenedor en ejecución. Esto es útil para depurar contenedores en ejecución o hacer algunas cosas dentro de un contenedor.

```shell
$ docker exec –it {container} {command}
```

Ejemplo:

Suponga que quiere lanzar bash shell (suponiendo que la imagen tenga Bash disponible, también puede usar otros shells disponibles) dentro de un contenedor llamado unruffled_meninsky en modo interactivo, use:

```shell
$ docker exec –it unruffled_meninsky /bin/bash
```

Esto debería llevarlo dentro del contenedor en un bash cáscara. Aqui la bandera -i significa modo interactivo y -t para la terminal. Si solo desea ejecutar uno o más comandos y salir del contenedor, puede usar:

```shell
$ docker exec unruffled_meninsky cat /etc/hosts
127.0.0.1	localhost 
::1	localhost ip6-localhost ip6-loopback 
fe00::0	ip6-localnet 
ff00::0	ip6-mcastprefix 
ff02::1	ip6-allnodes 
ff02::2	ip6-allrouters 
172.17.0.2	cd2eed4acf34 
```

### DOCKER LOGS

En caso de que un contenedor se inicie en modo separado y queramos ver sus registros, podemos usar logs comando para revisar sus registros:

```shell
$ docker logs {CONTAINER NAME or ID}
```

Ejemplo:

```shell
$ docker logs 7da6dcebaf9c
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2021/08/15 12:14:09 [notice] 1#1: using the "epoll" event method
2021/08/15 12:14:09 [notice] 1#1: nginx/1.21.1
2021/08/15 12:14:09 [notice] 1#1: built by gcc 8.3.0 (Debian 8.3.0-6)
2021/08/15 12:14:09 [notice] 1#1: OS: Linux 5.8.0-1039-azure
2021/08/15 12:14:09 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2021/08/15 12:14:09 [notice] 1#1: start worker processes
2021/08/15 12:14:09 [notice] 1#1: start worker process 31
2021/08/15 12:14:09 [notice] 1#1: start worker process 32
```

Para copiar archivos entre un contenedor y un sistema de archivos localhost, puede usar cp mando.

```shell
$ docker container cp {CONTAINER NAME or ID:SRC_PATH} {DEST_PATH}|-
```

Ejemplo:

```shell
$ docker container cp quirky_cray:/etc/nginx/nginx.conf nginx.conf.bkp
```

### DOCKER EXPORT

El comando de contenedor de Docker ofrece una opción para exportar el sistema de archivos de un contenedor como un archivo TAR.

```shell
$ docker container export {CONTAINER NAME or ID}
```

inspect Command

Podemos verificar información detallada sobre un contenedor usando inspect comando como:

```shell
$ docker inspect {CONTAINER NAME or ID}
```

OR

```shell
$ docker container inspect {CONTAINER NAME or ID}
```
### DOCKER KILL

Un contenedor en funcionamiento se puede matar usando kill comando con un opcional --signal or -s bandera. Se pueden especificar varios contenedores para matarlos de una vez.

```shell
$ docker kill {CONTAINER NAME or ID} [--signal VAL]
```

Ejemplo:

```shell
$ docker kill cd9005a0b5d2 -s 9
cd9005a0b5d2
```


### DOCKER STATS 

Para mostrar una transmisión en vivo del uso de recursos de un contenedor, puede usar stats mando:

```shell
$ docker container stats {CONTAINER NAME or ID}
```

Ejemplo:

```shell
$ docker container stats thirsty_volhard
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
904390b65d45   thirsty_volhard   0.00%     3.406MiB / 7.775GiB   0.04%     1.02kB / 0B   0B / 8.19kB   3
```

### DOCKER TOP 

Me gusta top comando en Linux, podemos usarlo con Docker para obtener una lista de procesos en ejecución.

```shell
$ docker container top {CONTAINER NAME or ID}
```

Ejemplo:

```shell
$ docker container top thirsty_volhard
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2603                2582                0                   12:34               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            2659                2603                0                   12:34               ?                   00:00:00            nginx: worker process
systemd+            2660                2603                0                   12:34               ?                   00:00:00            nginx: worker process
```

### DOCKER RENAME 

Para cambiar el nombre de un contenedor existente, use rename mando.

```shell
$ docker container rename {OLD CONTAINER NAME} {NEW CONTAINER NAME}
```

Ejemplo:

```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
904390b65d45   nginx     "/docker-entrypoint.…"   7 minutes ago   Up 7 minutes   80/tcp    nginx_container
```
```shell
$ docker container rename nginx_container nginx_new
```

```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
904390b65d45   nginx     "/docker-entrypoint.…"   7 minutes ago   Up 7 minutes   80/tcp    nginx_new
```

### DOCKER DIFF

Podemos inspeccionar cambios en archivos o directorios en el sistema de archivos de un contenedor con diff mando.

```shell
$ docker container diff {CONTAINER NAME or ID}
```

Ejemplo:

```shell
$ docker container diff nginx_new
C /var
C /var/cache
C /var/cache/nginx
A /var/cache/nginx/uwsgi_temp
A /var/cache/nginx/client_temp
A /var/cache/nginx/fastcgi_temp
A /var/cache/nginx/proxy_temp
A /var/cache/nginx/scgi_temp
C /etc
C /etc/nginx
C /etc/nginx/conf.d
C /etc/nginx/conf.d/default.conf
C /run
A /run/nginx.pid
```
