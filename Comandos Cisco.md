# Comandos más importantes de Cisco

## Comandos básicos

- **`enable`**: Entra en el modo privilegiado.
- **`disable`**: Sale del modo privilegiado.
- **`configure terminal`**: Entra en el modo de configuración global.
- **`exit`**: Sale de un submodo.
- **`show running-config`**: Muestra la configuración activa.
- **`show version`**: Muestra la versión del sistema operativo y la información del hardware.
- **`show interfaces`**: Muestra el estado de las interfaces y estadísticas.

## Comandos de configuración

- **`hostname [nombre]`**: Cambia el nombre del dispositivo.
- **`interface [tipo] [número]`**: Entra en el modo de configuración de una interfaz específica.
- **`ip address [dirección IP] [máscara]`**: Asigna una dirección IP a una interfaz.
- **`shutdown`**: Desactiva una interfaz.
- **`no shutdown`**: Activa una interfaz.
- **`ip route [dirección de destino] [máscara] [puerta de enlace]`**: Configura una ruta estática.

## Comandos de diagnóstico

- **`ping [dirección IP]`**: Realiza una prueba de conectividad (ping).
- **`traceroute [dirección IP]`**: Realiza un seguimiento de la ruta hacia un destino.
- **`show ip route`**: Muestra las rutas en la tabla de enrutamiento.
- **`show ip interface brie`**: Muestra un resumen de las interfaces.

## Comandos de administración

- **`copy running-config startup-config`**: Guarda la configuración activa en la memoria de inicio.
- **`reload`**: Reinicia el dispositivo.
- **`show logging`**: Muestra los mensajes del registro del sistema.

## Comandos de seguridad

- **`enable secret [contraseña]`**: Establece una contraseña secreta para el acceso al modo privilegiado.
- **`line con 0`**: Configura la consola de línea.
- **`password [contraseña]`**: Establece una contraseña para la consola.
- **`login`**: Habilita la autenticación en la consola.

## Comandos de VLAN

- **`vlan [número de VLAN]`**: Crea una VLAN.
- **`name [nombre de VLAN]`**: Asigna un nombre a la VLAN.
- **`show vlan brief`**: Muestra un resumen de las VLANs configuradas.
- **`switchport mode access`**: Configura la interfaz como un puerto de acceso para una VLAN.
- **`switchport access vlan [número de VLAN]`**: Asigna la VLAN a una interfaz.
- **`show vlan brief`**: Muestra un resumen de todas las VLANs configuradas.
- **`show interfaces trunk`**: Muestra las interfaces en modo trunk y las VLANs que están pasando a través de ellas.

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
