---
title: "INSTALACION LEMP"
tags: ""
---

# Instalación de la pila LEMP
La pila LEMP es un conjunto de aplicaciojnes que permiten servir aplicaciones web dinámicas. El acrónimo LEMP se compone del uso de Linux, NGINX (leído EngineX), la base de datos MariaDB y el lenguaje de script PHP.

## Instalación del servidor NGINX
Para poder recibir peticiones de un navegador y reponder con una serie de ficheros que compongan la web, esta pila de software necesita un servidor web, en este caso NGINX. Este servidor web destaca por su estabilidad y el rendimiento que proporciona.

Actualizamos el índice de los repositorios y el sistema operativo

```shell
apt-get update && apt upgrade
```

Iniciamos la instalación del servidor NGINX

```shell
apt-get install nginx
```

En Debian 10 la instalación por defecto de NGINX configura el servicio para que arranque automáticamente al iniciar el sistema.

Comprobamos accediendo con nuestro navegador a la dirección IP o nombre DNS del mismo

```shell
http://ip_o_dominio
```
## Instalación de MariaDB
Para instalar el software que se encargará de almacenar la información de la web, vamos a utilizar MariaDB. Para realizar la instalación ejecutamos el comando:

```shell
apt-get install mariadb-server
```
Cuando terminamos la instalación podemos ejecutar el scrip *mysql_secure_installation* que permite ajustar las directivas iniciales de seguridad

```shell
mysql_secure_installation
```
Por defecto nos pide la contraseña del usuario root de la base de datos. Con la instalación recién realizada la contraseña de root será la cadena vacia y por lo tanto pulsamos ENTER para indicarlo.

En los siguientes pasos definimos una nueva contraseña para root y repetimos la contraseña para asegurar que esta sea correcta. 

A continuación contestaremos afirmativamente al resto de preguntas que eliminará el acceso remoto con el usuario root, eliminará una base de datos de ejemplo, no permitirá el acceso a la base de datos como anónimo y recargará los privilegios.

Podemos conectar a la consola de texto de MariaDb ejecutando el comando 

```shell
mysql -u root -p
```

Con el prompt del sistema modificado mostrando ```MariaDB [(none)]> ``` podremos ejecutar sentencias SQL para trabajar con la base de datos.

En muchas ocasiones puede ser útil crear un usuario que no sea el usuario root para acceder a la base de datos. Esto lo podemos realizar dentro del prompt de MariaDb con las siguientes sentencias SQL.

Creamos un usuario con el siguiente comando. La opción '%' indica que permitiremos conexiones remotas al servidor MariaDb identificandose con este usuario. Si en lugar de '%', definimos la opción 'localhost' solo se permitirían conexiones de este usuario realizadas localmente.

```sql
CREATE USER 'nombre_usuario'@'localhost' IDENTIFIED BY 'contraseña';
```
Concedemos permisos al usuario que acabamos de crear con el siguiente comando:

```sql
GRANT ALL ON *.* TO 'nombre_usuario'@'localhost' WITH GRANT OPTION;
```
Después de la palabra ON se definen las bases de datos y las tablas donde el usuario que acabamos de crear tendrá todos los permisos, en este caso utilizando los dos asteriscos definimos todas las bases de datos y todas las tablas. Con la opción WITH GRANT OPTION permitiremos que este usuario pueda crear nuevos usuarios y otorgarle los permisos.

Con el comando siguiente recargamos los privilegios para los usuarios:

```sql
FLUSH PRIVILEGES;
```
Para salir del prompt de MariaDb ejecutamos el comando:

```sql
exit
```

##Instalando el procesador de PHP
Si queremos servir páginas dinámicas que puedan acceder a la base de datos recién instalada, tenemos que realizar la instalación de PHP.

NGINX requiere un programa externo que maneje el procesado de PHP y actúe como puente entre el servidor web y el interprete de PHP. Esta funcionamiento permite mejorar el rendimiento de muchos sitios que utilizan PHP pero requiere un proceso de configuración adicional. Este software que actua como puente es el páquete php-fpm, acrónimo de *PHP fastCGI process manager* y modificar NGINX para redireccionar las peticiones PHP a este programa. Necesitaremos como poco instalar la librería php-mysql para poder acceder a la base de datos MariaDb.

Instalar los paquetes php-fpm y php-mysql

```shell
apt-get install php-fpm php-mysql
```

##Configurar NGINX para utilizar el proceso php-fpm
NGINX implementa el concepto de bloques de servidor o *server blocks*, concepto similar a los hosts vituales en Apache y que pueden generar configuraciones diferentes para dos o más hosts que se sirven con un mismo servidor.

En Debian 10, NGINX tiene habilitado un bloque de servidor por defecto que está configurado para servir los ficheros web que se encuentran en la ruta ```/var/www/html```.

Para cada sitio web vamos a crear un directorio y dejaremos el directorio por defecto para direccionar hacía allí todas las peticiones que no concuerden con ningún bloque de servidor.

Creamos el directorio ráiz de nuestro sitio web
Create the root web directory for your_domain as follows:

```shell
mkdir /var/www/iespacomolla.es/public_html
```
Luego podemos cambiar el propietario del directorio que acabamos de crear asignándolo a algún usuario del sistema (el usuario servidor web lo creamos en caso de no tenerlo en el sistema).

```shell
chown -R servidorweb:servidorweb /var/www/iespacomolla.es
```
A continuación, creamos un nuevo fichero de configuración dentro del directorio de ```/etc/nginx/sites-available```.

```shell
nano /etc/nginx/sites-available/iespacomolla.es
```
Dentro de este fichero creamos una configuración similar a la siguiente cuidado linea 94:

```shell
server {
    listen 80;
    listen [::]:80;

    root /var/www/iespacomolla.es/public_html;
    index index.php index.html index.htm;

    server_name iespacomolla.es;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }
}
```
Esta configuración básica escucha el puerto 80 y sirve los fichero que contiene el directorio que acabamos de crear para el sitio iespacomolla.es. Con esta configuración responde a las peticiones que se realicen a la dirección IP o el nombre DNS definido en la directiva server_name. Además, establecemos que todo fichero con extensión *.php*
y será procesado con php-fpm antes de que NGINX devuelva los resultados.

Para activar tu configuración creamos un enlace que se almacene en el directorio *sites-enabled* con el siguiente comando:

```shell
ln -s /etc/nginx/sites-available/iespacomolla.es /etc/nginx/sites-enabled/
```
NGINX recargará el fichero de configuración del nuevo sitio web cuando se recargue.

Podemos realizar una búsqueda de errores sintácticos si ejecutamos el siguiente comando:

```shell
nginx -t
```

Si no aparecen errores de configuración podemos recargar el servicio de NGINX para cargar la nueva configuración

```shell
systemctl reload nginx
```

##Creamos un fichero PHP para probar nuestro sitio.
Para comprobar que nuestra configuración funciona, podemos crear un fichero con una función php que retorne la información de la instalación de la instalación de PHP.

```shell
nano /var/www/iespacomolla.es/public_html/index.php
```
```php
<?php
phpinfo();
?>
```
Ahora podemos probar en el navegador visitando el dominio o la dirección IP que hemos establecido en la configuración activada en *sites-enabled*

```shell
http://iespacomolla.es
```
Podremos ver la información de la versión de PHP que tengamos instalada.

Después de probar los sitios eliminamos los archivos info.php. Plantean una amenaza de seguridad, dado que contienen información confidencial sobre su servidor y usuarios no autorizados pueden acceder a ellos.
