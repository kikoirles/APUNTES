---
title: "INSTALACION NODEJS"
tags: ""
---

# Instalación de la pila NodeJS

Node.js es una plataforma de JavaScript para programación general que permite a los usuarios crear aplicaciones de red de forma rápida. Al aprovechar JavaScript tanto en frontend como en backend, Node.js hace que el desarrollo sea más uniforme e integrado.

Para obtener la versión distro-stable de Node.js, podemos utilizar el administrador de paquetes apt. Primero, actualizamos su índice de paquetes locales:

```shell
sudo apt update
```

A continuación, realizamos la instalación del paquete Node.js desde los repositorios:

```shell
sudo apt install nodejs
```
Con el siguiente comando podemos comprobar la versión instalada a partir de los repositorios oficiales de Debian.

```shell
nodejs -v
```
Debido a un conflicto con otro paquete, el ejecutable de los repositorios de Debian se llama nodejs en vez de node.

## Instalación con un PPA


Para trabajar con una versión más reciente de Node.js, podemos agregar el **PPA** (*Personal Package Archive*) actualizado por NodeSource. En este habrá versiones de Node.js más actualizadas que en los repositorios oficiales de Debian y podremos elegir entre versiones más actualizadas o versiones LTS (*Long Term Support*).

Primero, actualizaremos el índice de paquetes locales e instalaremos curl, con el cual accederá al PPA:

```shell
sudo apt update
```
```shell
sudo apt install curl
```
A continuación, instalaremos el PPA para poder acceder a su contenido. Desde su directorio principal, ejecutamos curl para recuperar la secuencia de comandos de instalación de la versión que queremos intalar. En el ejemplo nos aseguramos de sustituir 10.x por la cadena de la versión deseada:

```shell
cd ~
```
```shell
curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
```

Ejecutamos la secuencia de comandos que se ha descargado:

```shell
bash nodesource_setup.sh
```
El PPA se agregará a la configuración y la caché de paquetes locales actualizándose de forma automática. Con el nuevo repositorio añadido podemos instalar el paquete Node.js

```shell
apt install nodejs
```

El paquete nodejs contiene el binario nodejs y npm, sistema de gestión de paquetes de nodejs. Npm utiliza un archivo de configuración en el directorio de inicio para hacer un seguimiento de las actualizaciones. Se creará la primera vez que se ejecute npm. 

Con el siguiente comando podemos verificar que npm esté instalado y crear el archivo de configuración:

```shell
npm -v
```

Para que algunos paquetes de npm funcionen (por ejemplo, aquellos para los cuales de sebe compilar código de fuente), debemos instalar el paquete build-essential:

```
apt install build-essential
```

Ahora tenemos instado en nuestro sistema las herramientas necesarias para trabajar con paquetes npm para los que se deba compilar el código fuente.

## Instalación con NVM
Una alternativa a la instalación de Node.js a través de apt es utilizar una herramienta llamada nvm, que significa “Node.js Version Manager”. En vez de funcionar en el nivel del sistema operativo, nvm funciona en un directorio independiente dentro de su directorio de inicio. Esto permite la instalación de varias versiones autónomas de Node.js sin que afecte a todo el sistema.

Controlar el entorno NodeJS con nvm permite acceder a las versiones más recientes de Node.js, además de conservar y administrar versiones anteriores.

Para descargar la secuencia de comandos de instalación de nvm de la página de GitHub del proyecto, podemos utilizar curl. A tener en cuenta en cuenta que el número de versión puede diferir del que se resalta aquí:

```shell
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh -o install_nvm.sh
```

Ejecutamos el fichero de script descargado:

```shell
bash install_nvm.sh
```
Este script instalará el software en un subdirectorio del directorio de inicio en ```~/.nvm```. También agregará las líneas necesarias al archivo ```~/.profile``` para utilizarlo.

Para recargar el fichero ```~/.profile``` debemos volver a iniciar sesión con el usuario con el que se ha realizado la instalación.

Con nvm instalado, podemos realizar la instación de versiones aisladas de Node.js. Para obtener información sobre las versiones de Node.js disponibles, escribimos el siguiente comando:

```shell
nvm ls-remote
```

Con el comando install podemos seleccionar la versión de NodeJS que queremos instalar

```shell
nvm install 8.11.1
```

Normalmente, nvm aplicará un cambio para utilizar la versión más reciente instalada. Podemos indicar a nvm que utilice la versión que acaba de descargar escribiendo:

```shell
nvm use 8.11.1
```

Si se han instalado varias versiones de Node.js, podemos ver cuál está instalada escribiendo:

```shell
nvm ls
```

Si queremos establecer como predeterminada una de las versiones: 

```shell
nvm alias default 8.11.1
```

Esta versión se seleccionará de forma automática cuando se genere una nueva sesión. También podemos hacer referencia a ella con el alias, como se muestra:

```shell
nvm use default
```

Cada versión de Node.js hará un seguimiento de sus propios paquetes y cuenta con npm para administrarlos.

También tenemos paquetes de instalación de npm en el directorio ```/node_modules``` del proyecto de Node.js. 

En el siguiente ejemplo se realiza la instalación del módulo express:

```shell
npm install express
```

Si queremos que el módulo esté disponible de manera general para que otros programas que utilizan la misma versión de Node.js puedan emplearlo, agregamos el indicador -g:

```shell
npm install -g express
```

Con ello, el paquete se instalará aquí:

```shell
~/.nvm/versions/node/node_version/lib/node_modules/express
```

Instalar el módulo de forma general permitirá ejecutar comandos de la línea de comandos, pero se debe vincular el paquete a la distribución local para poder solicitarlo desde un programa:

```shell
npm link express
```
Puede obtener más información sobre las opciones disponibles con nvm escribiendo lo siguiente:

Para eliminar cualquiera de las versiones, podemos ejecutar el siguiente comando:

```shell
apt remove nodejs
```

Con este comando se eliminarán el paquete y los archivos de configuración.

Para desinstalar una versión de Node.js que haya habilitado utilizando nvm, primero determinamos si la versión que se desea eliminar es o no la que se encuentra activa:

```shell
nvm current
```

Si la versión no está activa podremos ejecutar:

```shell
nvm uninstall node_version
```

Con este comando se desinstalará la versión seleccionada de Node.js.

Si la versión a eliminar es la que se encuentra activa, primero debemos desactivar nvm para habilitar sus cambios:

```shell
nvm deactivate
```
Ahora ya se permite desinstalar la versión actual con el comando uninstall anterior, que eliminará todos los archivos asociados con la versión deseada de Node.js, a excepción de aquellos en caché que se puedan utilizar para la reinstalación.
