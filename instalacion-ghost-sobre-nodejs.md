---
title: "INSTALACION GHOST SOBRE NODEJS"
tags: ""
---

# Instalando Ghost sobre NodeJS
Para realizar la instalación de Ghost vamos a necesitar una serie de software adicional a NodeJS para que funcione nuestra plataforma de Blogs.

## Instalación de NGINX

En este caso el servidor web NGINX actuará como proxy invertido (*reverse proxy*) para realizar la instalación de la aplicación Ghost.

```shell
apt install nginx
```

## Instalación de MariaDB

Para realizar la instalación de MariaDB a través de los repositorios oficiales de Debian, podemos utilizar el siguiente comando 

```shell
apt install mariadb-server
```

Cuando hemos realizado la instalación de MariaDB, podemos ejecutar el script *mysql_secure_installation* que nos permitirá crear una contraseña para el usuario root y desactivar diversos parámetros que mejorarán la seguridad del sistema.

Con la base de datos ya asegurada podemos utilizar los siguientes comandos para entrar como root en la terminal de la base de datos y crear un nuevo usuario.

```shell
mysql -u root -p
```
Ya en la terminal de MariaDb ejecutamos.
```sql
CREATE USER 'nombre_usuario'@'%' IDENTIFIED BY 'password';
```
Otorgamos privilegios al usuario que acabamos de crear con:

```sql
GRANT ALL ON *.* TO 'nombre_usuario'@'%' WITH GRANT OPTION;
```
Por último, forzamos que se recarguen los privilegios de los usuario de la BBDD:

```sql
FLUSH PRIVILEGES;
```
Y ejecutamos el comando ```exit;``` para salir de la terminal de MariaDb.


## Instalar NodeJS y NPM
Ghost es un CMS basado en Node.js y para dar un soporte continuado utiliza las versiones LTS (*Long Term Support*) de esta plataforma.

Descargamos e instalamos Node.js (deberemos tener sudo y curl isntalados):

```shell
apt-get install curl 
```
```shell
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
```
```shell
apt install nodejs
```
## Instalar y configurar Ghost
### Instalar los comando de Ghost

Ghost-CLI es una interfaz de comandos para poder instalar y actualizar Ghost. Estos comandos permiten instalar la base de datos, configurar NGINX como servidor web que actua como proxy inverso, habilitar SSL/TLS como protocolo de seguridad utilizando Let's Encrypt como entidad certificadora, renovar de forma automática los certificados SSL e inicializar Ghost como un servicio del sistema.

Para realizar la instalación de las herramientas Ghost-CLI debemos ejecutar el siguiente comando.

```shell
npm install -g ghost-cli@latest
```
### Instlación de Ghost
Las herramientas de línea de comandos que acabamos de instalar nos permitirán realizar la instalación de Ghost. Para ello, tenemos de crear un directorio raiz para su instalación.

```shell
mkdir -p /var/www/ghost
```

***Cuidado***

Instalar Ghost en el directorio ```/root``` o ```/home/{usuario}``` no funcionará correctamente y dará como resultado una instalación fallida. Solamente el directorio /var/www/{directorio} tendrá los permisos correctos para realizar la instalación. Para realizar la instalación con exito tendremos que asignar la propiedad de ese directorio a un usuario del sistema que no sea root.

```shell
chown ghostuser:ghostuser /var/www/ghost
```
Y para asegurarnos que los permisos estén correctamente establecidos ejecutamos el siguiente comando:

```shell
chmod 775 /var/www/ghost
```
Si nos situamos en el directorio que hemos creado para realizar la instalación, debemos asegurarnos que esté vacio con el comando ```ls -a```

Procedemos a la instalación de Ghost teniendo en cuenta que debemos estar logeados con un usuario que no sea root. Siguiendo el ejemplo anterior el usuario *ghostuser*

```shell
ghost install
```

Por defecto, Ghost muestra un error si el sistema operativo utilizado para realizar la instalación no es Ubuntu.

Continuaremos con la instalación respondiendo sí. A continuación, nos aparecerán una serie de preguntas para realizar la configuración de la aplicación web.

  
? Enter your blog URL: https://iespacomolla.es
? Enter your MySQL hostname: localhost
? Enter your MySQL username: nombre_usuario de MariaDB
? Enter your MySQL password: contraseña del usuario nombre_usuario
? Enter your Ghost database name: nombre de la base de datos de ghost (p.e *ghostdb*)

Durante el proceso de instalación se configurará la instancia creada de este sistema de blogs.

```
Setting up "ghost" system user
? Do you wish to set up "ghost" mysql user? yes
? Do you wish to set up Nginx? yes
? Do you wish to set up SSL? yes
? Enter your email (used for Let's Encrypt notifications) administrador@iespacomolla.es
? Do you wish to set up Systemd? yes
? Do you want to start Ghost? yes
```

Cuando se termina la instación podemos ejecutar el siguiente comando para comprobar los procesos de Ghost que están funcionando:

```shell
ghost ls
```

### Completar la instalación

Para finalizar el proceso de instalación, podrémos acceder a la página principal de ghost añadiendo ```/ghost``` al final de la URL o la dirección IP que utilicemos para acceder, *p.e* ```https://iespacomolla.es/ghost```

Para finalizar el proceso nos aparecerá una serie de asistentes que permiten crear una cuenta de usuario configurando el correo electrónico, contraseña y el título de la web. Adicionalmente, también nos aparecerán asistentes para invitar a otros usuarios que colaboren en la web. Tras aceptar o declinar este último paso ya podremos acceder al área de administración de la web para configurar nuestra instancia de Ghost.


## Comandos adicionales

Si durante la instación o configuración encontramos algún problema podemos ejecutar los siguientes comandos para diagnosticarlos o consultar la documentación:

```shell
ghost doctor
```
o

```shell
ghost --help
```
