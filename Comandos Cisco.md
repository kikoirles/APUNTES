# Comandos más importantes de Cisco

## Comandos básicos

- **`enable`**: Cambia al modo privilegiado.
- **`disable`**: Vuelve al modo de usuario.
- **`configure terminal`**: Entra al modo de configuración global.
- **`exit`**: Sale del modo actual o de la sesión.
- **`show running-config`**: Muestra la configuración actual en ejecución.
- **`show startup-config`**: Muestra la configuración de inicio almacenada en la memoria NVRAM.
- **`show version`**: Muestra la información del sistema, como la versión del sistema operativo, la memoria, etc.
- **`show interfaces`**: Muestra el estado de todas las interfaces del router o switch.

## Comandos de configuración

- **`hostname [nombre]`**: Cambia el nombre del dispositivo.
- **`interface [tipo] [número]`**: Entra en el modo de configuración de una interfaz específica.
- **`ip address [dirección IP] [máscara]`**: Asigna una dirección IP a la interfaz seleccionada.
- **`no shutdown`**: Activa la interfaz seleccionada.
- **`shutdown`**: Desactiva la interfaz seleccionada.
- **`ip route [dirección de red] [máscara] [puerta de enlace]`**: Establece una ruta estática.

## Comandos de diagnóstico

- **`ping [dirección IP]`**: Realiza una prueba de conectividad (ping) hacia una dirección IP.
- **`traceroute [dirección IP]`**: Realiza un seguimiento de la ruta hacia un destino.
- **`show ip route`**: Muestra las rutas de la tabla de enrutamiento.
- **`show ip interface brief`**: Muestra un resumen del estado de las interfaces IP.

## Comandos de administración

- **`copy running-config startup-config`**: Guarda la configuración actual en la memoria de inicio.
- **`erase startup-config`**: Borra la configuración de inicio.
- **`reload`**: Reinicia el dispositivo.
- **`show log`**: Muestra el registro de eventos del sistema.

## Comandos de seguridad

- **`enable secret [contraseña]`**: Establece una contraseña secreta para el acceso al modo privilegiado.
- **`line con 0`**: Entra en la configuración de la consola.
- **`password [contraseña]`**: Establece la contraseña de acceso.
- **`login`**: Activa la autenticación en la consola o terminal.
- **`enable password [contraseña]`**: Establece una contraseña para acceder al modo privilegiado.

## Comandos de VLAN

- **`vlan [número de VLAN]`**: Crea una VLAN.
- **`name [nombre de VLAN]`**: Asigna un nombre a la VLAN.
- **`show vlan brief`**: Muestra un resumen de las VLANs configuradas.

## Comandos de SNMP

- **`snmp-server community [comunidad] [acceso]`**: Configura la comunidad SNMP.
- **`snmp-server enable traps`**: Habilita los traps SNMP.

## Comandos de NAT (Network Address Translation)

- **`ip nat inside source list [número] interface [interfaz] overload`**: Configura NAT para la traducción de direcciones internas.
- **`show ip nat translations`**: Muestra las traducciones de NAT activas.

## Comandos de QoS (Quality of Service)

- **`class-map [nombre]`**: Crea una clase de tráfico.
- **`policy-map [nombre]`**: Crea una política de QoS.
- **`service-policy input [nombre]`**: Aplica la política de entrada en una interfaz.
