---
title: "INSTALACION JOOMLA NGINX"
tags: ""
---

# Instalación de JOOMLA en LEMP

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

## Creación de la base de datos de JOOMLA
El último de los requerimientos para poder tener el CMS funcional es la creación de una base de datos y un usuario desde que este pueda acceder a la misma y realizar las pertinentes modificaciones.

```shell
mysql -u root -p
```

```shell
sudo mysql -u root -p
```

Then create a database called joomladb

```shell
CREATE DATABASE joomladb;
```

Next, create a database user called joomladbuser and set password

```shell
CREATE USER 'joomladbuser'@'localhost' IDENTIFIED BY 'barcelona22';
```

Then grant the user full access to the database.

```shell
GRANT ALL ON joomladb.* TO 'joomladbuser'@'localhost' WITH GRANT OPTION;
```

Finally, save your changes and exit.

```shell
FLUSH PRIVILEGES;
EXIT;
```

How to download Joomla

We’re ready to download Joomla and begin configuring it. First, run the commands below to download the latest version of Joomla from its repository.

To view Joomla releases, see this page.

At the time of this writing, the latest version is 4.0.4. Future version will have different links to download from.

Run the commands below to download and extract Joomla version 4.0.4.

``` shell
cd /tmp
```

```shell
wget https://downloads.joomla.org/cms/joomla4/4-0-2/Joomla_4-0-2-Stable-Full_Package.zip
```

```shell
sudo unzip -d /var/www/joomla /tmp/Joomla_4-0-2-Stable-Full_Package.zip
```

Then run command below to allow www-data user to own the Joomla directory.

```shell
sudo chown -R www-data:www-data /var/www/joomla/
```

```shell
sudo chmod -R 755 /var/www/joomla/
```

How to configure Nginx for Joomla

We have downloaded Joomla content into a new folder we called Joomla. Now, let’s configure Nginx to create a new server block to use with our Joomla website. You can create as many server blocks with Nginx.

To do that, run the commands below to create a new configuration file called joomla.conf in the /etc/nginx/sites-available/ directory to host our Joomla server block.

```shell
sudo nano /etc/nginx/sites-available/joomla.conf
```
The new Joomla server blocks configurations should look similar to the line below. Take notes of the highlighted lines.

+ The first server block listens on port 80.  It contains a 301 redirect to redirect HTTP to HTTPS.
    
+ The second server block listens on port 443. It contains a 301 redirect to redirect www to non-www domain.
    
```shell
server {
    listen 80;
    listen [::]:80;
    root /var/www/joomla;
    index  index.php index.html index.htm;
    server_name  example.com www.example.com;

    client_max_body_size 100M;
    autoindex off;
    
    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    # deny running scripts inside writable directories
    location ~* /(images|cache|media|logs|tmp)/.*.(php|pl|py|jsp|asp|sh|cgi)$ {
      return 403;
      error_page 403 /403_error.html;
    }

    location ~ .php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

Save the file and exit.

After saving the file above, run the commands below to enable the new file that contains our Joomla server block. Restart Nginx after that.

```shell
sudo ln -s /etc/nginx/sites-available/joomla.conf /etc/nginx/sites-enabled/
```

```shell
sudo systemctl restart nginx.service
```

At this stage, Joomla is ready and can be launched by going to the server’s IP or hostname.

```shell
http://localhost
```

### Encrypt JOOMLA  
https://websiteforstudents.com/how-to-install-joomla-on-ubuntu-linux-with-nginx/
