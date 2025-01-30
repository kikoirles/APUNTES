# Guía de Instalación de MISP en Ubuntu 22.04-server

## 1. Actualizar el sistema

Es importante asegurarse de que el sistema esté actualizado antes de comenzar la instalación.

```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Instalar dependencias básicas

Instalaremos las dependencias necesarias para MISP, incluyendo Apache, MySQL y PHP.

```bash
sudo apt install -y apache2 mariadb-server python3 python3-pip python3-dev \
libapache2-mod-php7.4 php7.4 php7.4-cli php7.4-dev php7.4-mysql php7.4-redis \
php7.4-zip php7.4-xml php7.4-mbstring php7.4-curl php7.4-gd php7.4-intl \
php7.4-bcmath php7.4-opcache redis-server zip unzip git gnupg \
python3-venv virtualenv
```

## 3. Configurar MySQL

Iniciamos el servicio de MySQL y configuramos la base de datos para MISP.

```bash
sudo systemctl start mariadb
sudo mysql_secure_installation
```

Luego, creamos la base de datos y el usuario para MISP:

```sql
CREATE DATABASE misp;
CREATE USER 'misp'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';
GRANT ALL PRIVILEGES ON misp.* TO 'misp'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 4. Descargar y configurar MISP

Clonamos el repositorio de MISP y configuramos los permisos adecuados.

```bash
sudo mkdir /var/www/MISP
sudo chown www-data:www-data /var/www/MISP
sudo chmod 750 /var/www/MISP
cd /var/www/MISP
sudo -u www-data git clone https://github.com/MISP/MISP.git /var/www/MISP
sudo -u www-data git submodule update --init --recursive
```

## 5. Configurar Apache

Configuramos Apache para servir MISP.

```bash
sudo cp /var/www/MISP/INSTALL/apache.misp.centos /etc/httpd/conf.d/misp.conf
sudo ln -s /etc/httpd/conf.d/misp.conf /etc/apache2/sites-available/misp.conf
sudo a2ensite misp
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## 6. Configurar MISP

Copiamos y editamos los archivos de configuración de MISP.

```bash
sudo -u www-data cp /var/www/MISP/app/Config/bootstrap.default.php /var/www/MISP/app/Config/bootstrap.php
sudo -u www-data cp /var/www/MISP/app/Config/database.default.php /var/www/MISP/app/Config/database.php
sudo -u www-data cp /var/www/MISP/app/Config/core.default.php /var/www/MISP/app/Config/core.php
sudo -u www-data cp /var/www/MISP/app/Config/config.default.php /var/www/MISP/app/Config/config.php
```

Editamos `/var/www/MISP/app/Config/database.php` para reflejar las credenciales de la base de datos que configuramos anteriormente.

## 7. Iniciar los servicios de MISP

Finalmente, iniciamos los servicios necesarios para que MISP funcione correctamente.

```bash
sudo systemctl restart apache2
sudo systemctl restart redis-server
sudo systemctl restart mariadb
```

## 8. Acceder a MISP

Ahora puedes acceder a MISP a través de tu navegador web en `http://tu_dominio_o_ip`.

Para obtener más detalles y configuraciones avanzadas, te recomiendo consultar la documentación oficial de MISP ([misp.github.io](https://misp.github.io/MISP/xINSTALL.ubuntu2204.html?utm_source=chatgpt.com)).

Además, puedes ver una introducción a MISP en el siguiente video:

[Introducción a MISP - Compartiendo Inteligencia de Amenazas](https://www.youtube.com/watch?v=vPtzkaB3wXw&utm_source=chatgpt.com)
