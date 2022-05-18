---
title: "WORDPRESS XAMP"
tags: ""
---

## Wordpress con XAMP 

Descargamos el archivo Wordpress desde el sitio oficial

Vamos a la rutad del Xamp una vez iniciado 

```shell
cd /opt/lampp/htdocs

```
cambiamos el usuarios de acceso a los directorios para poder crear nuevas carpetas en cuestion a los diversos cms creados

```shell
cd /chown kikoirles

```
movemos los archivos de wordpress a htdocs

```shell
mv /home/kikoirles/Descargas/wordpresversion5 /opt/lamp/htdocs/wordpresversion5

```
Iniciamos el phpmyadmin accediendo a localhost/phpmyadmin en el navegador

* Creamos el usuarios de la base de datos
* Creamos la base de datos 

La consola debemos hacer click en  las tres rayas para acceder a mas opciones

Cambiamos los permisos de del directorio wordpress con

```shell
cd /opt/lampp/htdocs/wordpress

```
```shell
chmod -R 777 .

```

Accedemos a wordpress mediante el localhost del navegador y comenzamos la instalación mediante los datos que hemos instalado.
