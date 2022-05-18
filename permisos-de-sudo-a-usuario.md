---
title: "PERMISOS DE SUDO A USUARIO"
tags: ""
---

# PERMISOS DE SUDO A USUARIO

Permite cambiar de el usuario christian a super usuario pero para realizar esta función debemos ser super usuarios 
```shell
sudo usermod -aG sudo christian
```
Reiniciar la maquina 

```shell
shutdown -r
```
