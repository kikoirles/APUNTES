---
title: "INSTALACION XAMP"
tags: ""
---

# Instalacion de servidor xamp

Descargamos el .run de la web oficial de [XAMP](https://www.apachefriends.org/es/index.html)

```shell
sudo  ap-get update

```
cambiamos los permisos de ejecucion

```shell
chmod +x xampp-linux-x64-8.0.10-0-installer.run
```
cambiamos a super usuario con su e instalamos net-tools 

```shell
apt-get install net-tools
```

ejecutamos el run como super usuario

```shell
./xampp-linux-x64-8.0.10-0-installer.run
```

### PARA INICIAR EL SERVICIO 
```shell
  /opt/lampp/lampp start
```

### PARA DETENER EL SERVICIO 
```shell
  /opt/lampp/lampp stop
```


### PARA VER ESTADO DEL SERVICIO 
```shell
  /opt/lampp/lampp status
```
|SERVICIO | SHELL |
| ----------- | ----------- |
|Para arrancar el servicio | /opt/lampp/lampp start |
|Para detener el servicio | /opt/lampp/lampp stop |
|Para ver el estado del servicio| /opt/lampp/lampp status |
