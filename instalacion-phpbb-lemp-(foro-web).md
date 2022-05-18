---
title: "INSTALACION PHPBB LEMP (FORO WEB)"
tags: ""
---

# Instalación de phpBB en LEMP

## Instalación de NGINX

Para realizar la instalación de un foro mediante la plataforma phpBB necisitamos un entorno LAMP o un entorno LEMP instalado en un servidor. En nuestro caso vamos a proceder a la instalación mediante un entorno con un servidor NGINX

```shell
apt install nginx
```

En principio, al realizar la instalación en Debian 10 el servidor web se configurará para arrancarse al iniciar el sistema, no obstante podemos modificar este comportamiento, al igual que parar y iniciar el sistema utilizando los siguientes comandos.

Parar el servicio:

```shell
systemctl stop nginx.service 
```

Arrancar el servicio:

```shell
systemctl start nginx.service
```

Habilitar el arranque del servicio al iniciar el sistema:

```shell
systemctl enable nginx.service
```
Deshabilitar el arranque automático del servicio al iniciar el sistema:

```shell
systemctl disable nginx.service
```

## Instalación de MariaDB

Este gestor de foros también necesita una base de datos para almacenar la información que maneja esta aplicación web. En este tipo de aplicaciones se suele utilizar MariaDB/MySQL como base de datos.

```shell
apt install mariadb-server mariadb-client
```
Analogamente, se pueden utilizar los comandos *stop*, *start*, *enable*, *disable* en *systemctl* para manejar el servicio.

MariaDB/MySQL tienen disponible un script que permite automatizar una serie de tareas iniciales para mejorar la seguridad del sistema, ejecutando el siguiente comando:

```shell
mysql_secure_installation
```

Este script nos realizará diversas preguntas que contestaremos como se muestra a continuación.

```shell
Enter current password for root (enter for none): Presionamos Enter pues *root* no tiene contraseña.
Set root password? [Y/n]: Y
New password: Introducimos la contraseña de root
Re-enter new password: Repeat la contraseña definida en el paso anterior
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]:  Y
Reload privilege tables now? [Y/n]:  Y
```

Finalmente, reiniciamos el servicio de mariadb para asegurarnos que todo funciona correctamente.

```shell
systemctl restart mariadb.service
```

## Instalación de PHP-FPM y las librerías necesarias

Con el siguiente comando realizaremos la instalación del interprete PHP-FPM y las librerías que son necesarias para que phpBB funcione correctamente.

```shell
apt install php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

phpBB exige realizar ciertos cambios en la configuración por defecto de php y por ello tenemos que modificar el fichero ```/etc/php/7.4/fpm/php.ini```con los siguientes cambios.
```OJO la version php```

```shell
max_execution_time = 180
max_input_time = 60
memory_limit = 256M
upload_max_filesize = 64M
```

## Creación de la base de datos de phpBB
El último de los requerimientos para poder tener el CMS funcional es la creación de una base de datos y un usuario desde que este pueda acceder a la misma y realizar las pertinentes modificaciones.

```shell
mysql -u root -p
```

Creamos una nueva base de datos.

```shell
CREATE DATABASE phpbb;
```

Creamos un nuevo usuario de la base de datos.

```shell
CREATE USER 'phpbbuser'@'localhost' IDENTIFIED BY 'barcelona22';
```

Concedemos los privilegios necesarios para que el usuario que hemos creado pueda acceder a la base de datos.

```shell
GRANT ALL ON phpbb.* TO 'phpbbuser'@'localhost' WITH GRANT OPTION;
```

Recargamos los privilegios de los usuarios de la base de datos y salimos del shell de MariaDB.

```shell
FLUSH PRIVILEGES;
EXIT;
```

## Descargamos el zip de phpBB desde la página oficial

Este paso lo podemos realizar de diversas formas.

1. Utilizando links.
2. Con la herramienta wget.
3. Transfiriendo el zip desde el host con el comando scp.

Para realizar la descompresión del archivo necesitamos la herramienta *unzip* que podemos descargar de los repositorios oficiales de Debian.

Vamos a crear la ruta donde instalaremos nuestra instancia de phpBB y cambiamos a ese directorio

```shell
mkdir -p /var/www/foroiespacomolla.es/public_html
cd /var/www/foroiespacomolla.es/public_html
```
Podemos descargar el archivo actualizando a la útlima versión disponible de phpBB, podemos encontrar la última versión en [web de descarga de phpBB](https://www.phpbb.com/downloads/)

```shell
wget https://download.phpbb.com/pub/release/3.3/3.3.1/phpBB-3.3.1.zip
```
Descomprimimos el fichero descargado en este directoio. Este proceso creará una carpeta llamada *phpBB3*.

```shell
unzip phpBB-3.2.1.zip
```

Podemos realizar la modificación del propietario y los permisos para establecer aquellos que encajen con la configuración de Nginx.

```shell
chown -R www-data:www-data /var/www/foroiespacomolla.es
```

Cambiamos los permisos del directorio que contendrá la web.

```shell
chmod -R 755 /var/www/foroiespacomolla.es
```

## Configuración de Nginx

Creamos un nuevo fichero que contendrá la configuración especifica del site que contendrá nuestra instalación de phpBB

```shell
nano /etc/nginx/sites-available/phpbb
```

Para configurar correctamente, tanto las particularidades del site como la forma de interpretar los fichero php podemos utilizar la siguiente configuración ```OJO  la version php```

```shell
server {
    listen 80;
    listen [::]:80;
    root /var/www/foroiespacomolla.es/public_html/phpBB3;
    index  index.php index.html index.htm;
    server_name foroiespacomolla.es www.foroiespacomolla.es;

    location / {
    try_files $uri $uri/ @rewriteapp;        
    }

    location /install/ {
     try_files $uri $uri/ @rewrite_installapp;
    }

    location ~ \.php(/|$) {
    fastcgi_split_path_info  ^(.+\.php)(/.+)$;
    fastcgi_index            index.php;
    fastcgi_pass             unix:/var/run/php/php7.4-fpm.sock;
   	 include                  fastcgi_params;
    fastcgi_param   PATH_INFO       $fastcgi_path_info;
    fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param DOCUMENT_ROOT $realpath_root;
    try_files $uri $uri/ /install/app.php$is_args$args;
    }

     location @rewrite_installapp {
      rewrite ^(.*)$ /install/app.php/$1 last;
     }

}
```
Guardamos y salimos del fichero de configuración.

Para activar el fichero de configuración realizaremos un enlace simbólico que se situe en la carpeta *sites-enabled*

```shell
ln -s /etc/nginx/sites-available/phpbb /etc/nginx/sites-enabled/
```
Adicionalmente, si tenemos el sitio por defecto activado podemos desactivarlo eliminando el enlace que tenemos en la carpeta *sites-enabled* con el siguiente comando:

```shell
rm /etc/nginx/sites-enabled/default
```

Reiniciamos nginx para que tome la nueva configuración que acabamos de crear

```shell
systemctl restart nginx.service
```

Si todo se ha realizado de forma correcta podremos comenzar la instalación de phpBB desde el navegador utilizando la dirección DNS con la que hemos configurado el servidor o la dirección IP a la que responda el mismo.
