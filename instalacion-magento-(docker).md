---
title: "INSTALACION MAGENTO (DOCKER)"
tags: ""
---

# Instalación de Magento en Docker

## Instalar Docker en Debian 10

Para poder realizar todo el procedimiento necesitamos tener instalado el subsistema Docker en nuestra máquina. Para realizar todo el procedimiento vamos a utilizar la herramienta docker compose que nos permite realizar la configuración de diversos parámetros de los contenedores.

Actualizamos el índice de los repositorios para instalar unas herramientas adiciones que necesitamos para realizar la instalación:

```shell
apt-get update
```

Ejecutamos la instalación de las herramientas adicionales:

```shell
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Descargamos y añadimos el repositorio de Docker a los repositorios del sistema:

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
```

```shell
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
```

Por último, actualizamos el índice de los repositorios de sistema e instalamos todo el sistema Docker:

```shell
apt-get update
```

```shell
apt-get install docker-ce docker-ce-cli containerd.io
```

## Configuración de Magento con docker-composer

Para realizar la instalación de Magento, vamos a utilizar el repositorio de un desarrollador que permite la instalación de Magento, con el servidor asociado,  además de una base de datos y la aplicación phpMyAdmin para la gestión de la misma.

Para descargar la configuración de este desarrollador vamos a clonar su repositorio de GitHub y a realizar las modificaciones que consideremos para adaptarlas a nuestro sistema. Para ello, utilizando el comando git de Debian:

```shell
git clone https://github.com/alexcheng1982/docker-magento2.git
```

Esto descargará todos los ficheros del repositorio en el cual empezaremos a realizar las modificaciones que queremos implementar. Por defecto, el script establece una base de datos mysql que vamos a cambiar por mariadb puesto que la versión definida por defecto produce un error que no permite arrancar el container de la misma. 

Para realizar esta modificación tenemos que situarnos dentro del directorio ```docker-magento2``` con el comando cd y editamos con nano,vi o nuestro editor favorito el fichero *docker-compose.yml* 

```shell
nano docker-compose.yml
```

Este fichero contiene una sección donde se declaran los servicios. Dentro de este se establecen 3 contenedores *(alexcheng/magento2, db, phpmyadmin)* que contendrán el servidor web, la base de datos y el gestor gráfico de la base de datos. 

* En el primer contenedor que utiliza *Apache* como servidor web se establece una correspondencia de puertos entre la máquina y el contenedor donde el puerto 80 de la máquina virtual (a la derecha) se corresponderá con el puerto 80 del contenedor. Se establece además con la etiqueta **links** una asociación con el contendor *db* que se define a continuación. Además, se crea un volumen donde se mapea el directorio denominado *magento-data* de la máquina virtual con el directorio */var/www/html* del contenedor que estamos definiendo. También se define una referencia a un fichero que contendrá variables de configuración.
* En el contenedor *db* seleccionamos la imagen a descargar, se genera un volumen donde se almacenarán los datos de la base de datos y se referencia al fichero que contendrá las variables de configuración.
* Por último, el contendor phpmyadmin descargará la imagen phpmyadmin/phpmyadmin y realiza un mapeado de los puertos donde el puerto 8850 se corresponderá con el puerto 80 del contenedor. Además se realiza un enlace de este contenedor con el contendor *db* definido anteriormente.

En el apartado *volumes:* de la configuración se declaran los volúmenes que se crearán durante la instalación. 

```shell
version: '3.0'
services:
  web:
    image: alexcheng/magento2
    ports:
      - "80:80"
    links:
      - db
    volumes:
      - magento-data:/var/www/html
    env_file:
      - env
  db:
    image: mariadb
    volumes:
      - db-data:/var/lib/mariadb/data
    env_file:
      - env
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8580:80"
    links:
      - db
volumes:
  magento-data:
  db-data:
```

Para completar la instalación modificaremos las variables de entorno que se definen en el fichero *env* para que queden similares a las siguientes:

```shell
MYSQL_HOST=db
MYSQL_ROOT_PASSWORD=toor
MYSQL_USER=magento
MYSQL_PASSWORD=magento
MYSQL_DATABASE=magento


MAGENTO_LANGUAGE=es_ES
MAGENTO_TIMEZONE=Europe/Madrid
MAGENTO_DEFAULT_CURRENCY=EUR
MAGENTO_URL=http://dir_ip_de_tu_maquina
MAGENTO_BACKEND_FRONTNAME=admin
MAGENTO_USE_SECURE=0
MAGENTO_BASE_URL_SECURE=0
MAGENTO_USE_SECURE_ADMIN=0


MAGENTO_ADMIN_FIRSTNAME=administrador
MAGENTO_ADMIN_LASTNAME=Tienda IES Paco Molla
MAGENTO_ADMIN_EMAIL=amdinistrador@iespacomolla.es
MAGENTO_ADMIN_USERNAME=admin
MAGENTO_ADMIN_PASSWORD=nimda
```

Si todo el proceso se realiza correctamente, todos los containers deben estar instalados de forma correcta y aparecerá un mensaje similar al siguiente:

```shell
Starting docker-magento2_db_1 ... done
Starting docker-magento2_web_1        ... done
Starting docker-magento2_phpmyadmin_1 ... done
```

Al ejecutar el comando ```docker ps``` obtendremos un listado informandonos de los tres contenedores que se han instalado.

```shell
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
14503fd73154        alexcheng/magento2      "/sbin/my_init"          10 hours ago        Up 2 minutes        0.0.0.0:80->80/tcp     docker-magento2_web_1
fd8bed81af89        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   10 hours ago        Up 2 minutes        0.0.0.0:8580->80/tcp   docker-magento2_phpmyadmin_1
993f017f3cce        mariadb                 "docker-entrypoint.s…"   10 hours ago        Up 2 minutes        3306/tcp               docker-magento2_db_1
```

Accediendo a la dirección IP de nuestra máquina virtual podremos realizar la instalación del CMS teniendo en cuenta que el host que contiene la base de datos no es *localhost* sino *bd* y que el usuario, la contraseña y la base de datos creada durante la instalación serán *magento*.

### Referencias

	* [Repositorio de Alex Cheng](https://github.com/alexcheng1982/docker-magento2)
