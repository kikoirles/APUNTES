# Instalacion wazuh
Debemos partir de una sitemas linux instalado para ello 

Actualizamos el índice de los repositorios y el sistema operativo

```shell
apt-get update && apt upgrade
```
En caso de no tener instalado curl

```shell
apt-get install curl
```

Luego instalamos el siguiente conjunto de àquetes para el sistema

```shell
apt install vim curl apt-transport-https unzip wget libcap2-bin software-properties-common lsb-release gnupg2
```

```shell
root@WAZUH01:~# curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```

Si la instalacion ha funcionado correctamente mostrara un contraseña y usuario que deberemos colocar a el dasboard para iniciar sesion por primera vez 

```shell
INFO: --- Summary ---
INFO: You can access the web interface https://<wazuh-dashboard-ip>
    User: admin
    Password: <ADMIN_PASSWORD>
INFO: Installation finished.
```

Accedemos en el navedor a nuestra ip 

```shell
https://192.168.2.108:443
```

habilitar Wazuh al arrancar el sistema
```shell
sudo systemctl enable wazuh-manager
```
