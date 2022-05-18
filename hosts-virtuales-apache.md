---
title: "HOSTS VIRTUALES APACHE"
tags: ""
---

# Hosts Virtuales con Apache

## Estructura de directorios
Los directorios que van a ser servidos por Apache tienen que quedar enclavados debajo del directorio root de Apache ```/var/www/```. Se crearán un directorio por cada host virtual que queramos crear.

Es una buena estrategía y una configuración ampliamente utilizada en los servicios de hosting la creación de una carpeta llamada *publichtml* que contiene los ficheros web a servir. Esto permite que ciertas aplicaciones web que necesiten almacenar información fuera de la carpeta donde tienen almacenados los ficheros *html* puedan hacerlo sin tener que crear carpetas en el directorio ```/var/www/``` donde se pueden mezclar con las carpetas de otras aplicaciones web. El LMS Moodle es un ejemplo, pues situa una carpeta moodle_data en el directorio superior al directorio donde lo hayamos instado.

# Creamos los directorios de ejemplo

```
mkdir -p /var/www/iespacomolla.es/public_html
```
```
mkdir -p /var/www/lacanal.es/public_html
```
Por defecto, el usuario que utiliza Apache para acceder al contenido de un directorio es *www-data*. Por ello, debemos cambiar el propietario y grupo de estos directorios con el siguiente comando:

```
chown -R www-data:www-data /var/www/iespacomolla.es/public_html
```
```
chown -R www-data:www-data /var/www/lacanal.es/public_html
```
Debemos modificar los permisos para conceder permiso de lectura y ejecución al directorio web y todos los ficheros contenidos en él. Ejecutamos el siguiente cambio de permisos que afectará al directorio raíz de Apache:
```
chmod -R 755 /var/www
```

## Creating Default Pages for Each Virtual Host

Vamos a crear una página sencilla que nos permita diferenciar entre los hosts virtuales que vamos a configurar.
```shell
nano /var/www/iespacomolla.es/public_html/index.html
```

```html
<html>
  <head>
    <title>Bienvenido a iespacomolla.es</title>
  </head>
  <body>
    <h1>¡Genial! El host virtual iespacomolla.es está funcionando</h1>
  </body>
</html>
```
Guardamos y cerramos el editor y hacemos un proceso similar para el sitio lacanal.es

## Configurar los hosts virtuales
Los hosts virtuales se configurarán con ficheros *.conf* situados en el directorio de configuración ```/etc/apache2/sites-available```

Apache crea un fichero de configuración por defecto llamado *000-default.conf* que puede ser utilizado como punto de partida. Creamos una copia de este fichero para cada uno de los dominios que vamos a configurar con el siguiente comando.

```shell
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/iespacomolla.es.conf
```

```shell
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/lacanal.es.conf
```
Si abrimos el fichero de configuración podemos realizar las siguientes modificaciones

```
nano /etc/apache2/sites-available/iespacomolla.es.conf
```

Este fichero de configuración responde por defecto a las peticiones hechas en el puerto 80. Vamos a realizar una serie de cambios en esta configuración, añadiendo una serie de nuevas directivas.

La directiva ServerAdmin nos permite definir una cuenta de correo electrónico donde el administrador del sitio puede recibir notificaciones. 
```
ServerAdmin administrador@iespacomolla.es
```
Podemos añadir a este fichero de configuración las siguientes directivas. La primera, definir  la petición DNS a la que responde el servidor. 

```
ServerName iespacomolla.es
```

La segunda, definir un Alias que permita responder con este mismo sitio a una petición diferente. Esto nos permite dar respuesta al caso en que un usuario utilice o no la www para referirse a nuestro sitio web.

```
ServerAlias www.iespacomolla.es
```
A continuación, cambiamos la localización de la raíz perteneciente a este host virtual. Con la directiva DocumentRoot que apuntará al directorio que hemos creado para este host.

```
DocumentRoot /var/www/iespacomolla.es/public_html
```

De esta forma el fichero ```/etc/apache2/sites-available/iespacomolla.es.conf``` quedará de forma parecida al siguiente:

```shell
<VirtualHost *:80>
        ServerAdmin administrador@iespacomolla.es
        ServerName iespacomolla.es
        ServerAlias www.iespacomolla.es
        DocumentRoot /var/www/iespacomolla.es/public_html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Tras crear este fichero podemos realizar una copia de este fichero y modificarlo para el site lacanal.es

## Activar los hosts virtuales

Apache no tomará la configuración que hemos creado hasta que la activemos con la herramienta a2ensite para cada uno de nuestros hosts virtuales.

```shell
a2ensite iespacomolla.es.conf
```

```shell
a2ensite lacanal.es.conf
```
Adicionalmente, vamos a desactivar el sitio por defecto desactivando el fichero de configuración 000-default.conf con el comando a2dissite:

```
a2dissite 000-default.conf
```
Para finalizar, reiniciamos el servidor Apache para que los cambios tomen efecto.

```
systemctl restart apache2
```

## Configurando el fichero local hosts para realizar pruebas
Si no tenemos disponibles dominios que podamos asociar a los sitios web, podemos modificar el fichero hosts de forma local para comprobar el correcto funcionamiento de la configuración realizada. 

Esa configuración intentará resolver las peticiones de forma local y no propagarla a los servidores de internet. Esta modificación la tendremos que realizar en el equipo desde donde realizamos la petición con el navegador.

En Linux y MacOS el fichero de configuración que contiene el sistema de nombres local se puede modificar con el siguiente comando.

```shell
nano /etc/hosts
```
En Windows ejecutando dentro del símbolo del sistema con privilegios de administración el siguiente comando ```notepad %windir%\system32\drivers\etc\hosts``` conseguimos acceder al fichero hosts. 

Dentro del fichero hosts podemos añadir una directiva donde aparezca en primera instancia la dirección IP del servidor, seguido de un espacio y el nombre DNS del que queramos realizar la traducción.

```shell
192.168.2.110 iespacomolla.es
192.168.2.110 lacanal.es
```
Guardamos y cerramos el fichero y las traducciones ya podrán ser realizadas desde el navegador.

## Comprobar los resultados

La comprobación del funcionamiento de los hosts virtuales, la podemos realizar de forma sencilla comprobando la respuesta a las direcciones que hemos configurado

```
http://iespacomolla.es
```
```
http://lacanal.es
```
