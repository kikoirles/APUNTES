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
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
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

# Añadir agente de Windows al servidor

Instalación del agente Wazuh en Windows

Descarga el instalador del agente desde la documentación oficial de Wazuh.

```shell
https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi
```
Ejecuta el instalador en el equipo Windows.

Durante la instalación, configura la IP del Wazuh Manager (Linux).

Registrar el agente en el servidor Linux
