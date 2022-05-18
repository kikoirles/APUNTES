---
title: "IMPLANTACIÓN DE GIT"
tags: ""
---

# IMPLANTACIÓN DE GIT

### INDICE

* INSTALAMOS GIT
* GIT CLONE
* GIT ADD
* GIT PUSH
* GIT PULL
* GIT COMANDOS EXTRA
 

Primeramente creamos la estructura de directorios donde almacenaremos el contenido de nuestro repositorio git.

**AVISO**:*Dede haberse implementado XAMP anteriormente*

```shell
mkdir /home/christian/Documentos/iawe
```

Deberemos crear un enlace simbólico. [Haz click info](https://cambiatealinux.com/ln-crear-un-enlace-simbolico-al-fichero-o-directorio)

```shell
ln -s /opt/lampp/htdocs/ /home/christian/Documentos/iawe
```
Deberemos visualizar el siguiente contenido.

```shell
ls /home/christian/Documentos/iawe
```
**Resultado**
![image.png](https://boostnote.io/api/teams/v1XyxrATp/files/692ceb564834c646f3a4e910b76004b3d21d30a5dbbd3083f46b760d4ca6bc6b-image.png)

### INSTALAMOS GIT 

Realizamos la instalación de los paquetes git
**AVISO**:*cambiar a usuario con permisos de administración*

```shell
apt-get update
```
```shell
apt-get install git
```
Ahora crearemos el la vinculación con el repositorio git para ello deberemos situarnos el el directorio donde queramos vincularlo. 

```shell
cd /home/kikoirles/Documentos/iawe
```

### GIT CLONE

El git clone se utiliza para crear una copia de un repositorio o rama específica dentro de un repositorio.[Haz click info](https://github.com/git-guides/git-clone)

```shell
git clone https://github.com/kikoirles/IAWE-ejemplos-php.git
```

En este punto no nos pedira la contraseña muy importante nos pedira un token en git-hub que previamente habremos creado bastara con renovarlo.
[Como crealo](https://docs.github.com/es/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)

![image.png](https://boostnote.io/api/teams/v1XyxrATp/files/ecf00be76a7e38cd4fa73a8340340fad7226782fdb5c79acf913a2a84ff155d5-image.png)


```shell
kikoirles@IAWE:~/Documentos/iawe$ git clone https://github.com/kikoirles/IAWE-ejemplos-php.git
Clonando en 'IAWE-ejemplos-php'...
Username for 'https://github.com': kikoirles (USERNAME)
Password for 'https://kikoirles@github.com':  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 (TOKEN)
```
Dentro habrá un ejemplo **IAWE-ejemplos-php** para comprobar que es el repositorio de Git. 

```shell
*** Por favor cuéntame quien eres.



Corre



  git config --global user.email "you@example.com"

  git config --global user.name "Tu Nombre"



para configurar la identidad por defecto de tu cuenta.

Omite --global para configurar tu identidad solo en este repositorio.



fatal: no es posible auto-detectar la dirección de correo (se obtuvo 'root@IAWE.(none)')

root@IAWE:/home/christian/Documentos/IAWE/IAWE-ejemplos-php# ls /home/christian/Documentos/IAWE/
```

### GIT ADD

El git add nos permite añadir o agregar archivos a la area de preparación del repositorio git. Pero no los envía hasta no hacer el Push que confirma el envío. 

Primeramente debemos situarnos el el directorio raíz de nuestro git que en mi caso es **IAWE-ejemplos-php** que contiene los archivos nuevos que habremos creado para subirlos al repositorio.

```shell
cd /home/christian/Documentos/IAWE/IAWE-ejemplos-php
```
Con un ls nos mostrara todos los archivo que se añadirán o se remplazaran. 

```shell
ls -l
```
Para añadirlos hacemos un git add seguido de un commit para confirmar y introducir un argumento. 

```shell
git add .
```

```shell
git commit -m "Añadiendo programa"
```

Es posible que nos salte un error como este ya que no estamos identificados.

```shell
*** Por favor cuéntame quien eres.
Corre
  git config --global user.email "you@example.com"
  git config --global user.name "Tu Nombre"

para configurar la identidad por defecto de tu cuenta.
Omite --global para configurar tu identidad solo en este repositorio.

fatal: no es posible auto-detectar la dirección de correo (se obtuvo 'root@IAWE.(none)')
```

La solución es meter los datos de tu cuenta de git-hub que son el correo y el nombre de usuario que se guardaran en variable de configuración global de de nuestra instalación Git.  

```shell
git config --global user.email "cristianirles14@gmail.com"
```
```shell
git config --global user.name "kikoirles"
```

### GIT PUSH

Previamente después de hacer un Git add deberemos realizar un push para enviar confirmaciones a la rama maestra principal que por defecto suele ser main.

```shell
git push
```
Nos pedirá de nuevo el usuario las credenciales del Git-hub el usuario y el token que actuara como contraseña.
<br>
**NOTA**: *Guardar el token en un Note o archivo para tenerlo con mejor acceso*

```shell
Username for 'https://github.com': kikoirles
Password for 'https://kikoirles@github.com':
```
### GIT PULL
*En pocas palabras se define como Exportar tu Git-hub*<br>
Lo usas para traerte y fusionarte los repositorio remotos de tu Git-hub con los archivos locales y poder tener todos los cambios actualizados que estan registrados en tu Git-hub.

```shell
cd /home/christian/Documentos/IAWE/IAWE-ejemplos-php
```

```shell
git pull . 
```

```shell
Desde .
 * branch            HEAD       -> FETCH_HEAD
Ya está actualizado.
```

### GIT COMANDOS EXTRA

Comandos de Git no tan importantes pero considero que son los mas utilizados según sus funciones.  

|    COMANDO EN SHELL   | DESCRIPCIÓN |
| -- | -- |
| **git status** | Muestra la lista de los archivos que se han cambiado junto con los archivos que están por ser preparados o confirmados.|
| **git show-branch** |  Enumere  las ramas de seguimiento | 
| **git branch --all** | Enumere tanto las ramas de seguimiento remoto como las ramas locales.  |
| **git branch -d [branch-name]** | Borra una rama |
| **git checkout -b [branch-name]** | Permite crea ramas y te ayuda a navegar entre ellas. Por ejemplo, el siguiente comando crea una nueva y automáticamente se cambia a ella|
| **git remote -v** | Permite ver todos los repositorios remotos. Listará todas las conexiones junto con sus URLs |
| **git diff** | El siguiente comando se usa para ver los conflictos que hay entre ramas antes de fusionarlas  |
