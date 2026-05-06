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
```shell
/var/ossec/bin/manage_agents
```
Selecciona A para añadir un nuevo agente
Introduce nombre del agente (ej: WIN-PC01)
Asigna la IP del equipo Windows
Añadir o obtner KEY "DRmYTQ0N2RmMjk1ZjJhNGM0Yjk2NzIxYzM1NDhjZDlmMzZkZWI0MmJhZTY3OWZjMjIxNjE4ODhjMjgw"

```shell
/var/ossec/bin/manage_agents
```
```shell
Choose your action: A,E,L,R or Q: E

Available agents:
   ID: 001, Name: PC_WINDOWS, IP: 192.168.2.182
Provide the ID of the agent to extract the key (or '\q' to quit): 001

Agent key information for '001' is:
MDAxIFBDX1dJTkRPV1MgMTkyLjE2OC4yLjE4MiAzMGIyNDRmYTQ0N2RmMjk1ZjJhNGM0Yjk2NzIxYzM1NDhjZDlmMzZkZWI0MmJhZTY3OWZjMjIxNjE4ODhjMjgw
```
<img width="1546" height="533" alt="image" src="https://github.com/user-attachments/assets/e75d9b5f-53f2-4d2d-9390-4286be1a47b0" />


