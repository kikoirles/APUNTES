## Comandos básicos
- **`enable`**: Entra en el modo privilegiado.
- **`configure terminal`**: Entra en el modo de configuración global.
- **`exit`**: Sale del modo actual.
- **`show running-config`**: Muestra la configuración activa.
- **`show version`**: Muestra la información del sistema, como la versión del software y la memoria.
- **`show interfaces`**: Muestra el estado y las estadísticas de todas las interfaces.
- **`show ip dhcp binding`**: Muestra las direcciones IP asignadas por el servidor DHCP.

## Comandos de configuración
- **`hostname [nombre]`**: Cambia el nombre del dispositivo.
- **`interface [tipo] [número]`**: Entra en el modo de configuración de una interfaz específica.
- **`ip address [dirección IP] [máscara]`**: Asigna una dirección IP a una interfaz.
- **`shutdown`**: Desactiva una interfaz.
- **`no shutdown`**: Activa una interfaz.
- **`ip route [dirección IP] [máscara] [puerta de enlace]`**: Configura una ruta estática.

## Comandos de diagnóstico
- **`ping [dirección IP]`**: Realiza una prueba de conectividad (ping) hacia una dirección IP.
- **`traceroute [dirección IP]`**: Realiza un seguimiento de la ruta hacia un destino.
- **`show ip route`**: Muestra las rutas en la tabla de enrutamiento.
- **`show ip interface brief`**: Muestra un resumen de las interfaces IP, sus direcciones IP y su estado.
- **`show interfaces status`**: Muestra el estado y errores de las interfaces.

## Comandos de administración
- **`write memory`**: Guarda la configuración actual en la memoria de inicio.
- **`reload`**: Reinicia el dispositivo.
- **`show logging`**: Muestra el registro de eventos del sistema.

## Comandos de seguridad
- **`enable secret [contraseña]`**: Establece una contraseña secreta para el acceso al modo privilegiado.
- **`line console 0`**: Entra en la configuración de la consola.
- **`password [contraseña]`**: Establece una contraseña para acceder al dispositivo.
- **`login`**: Habilita el inicio de sesión en la consola.
- **`enable password [contraseña]`**: Establece una contraseña de acceso al modo privilegiado (menos seguro que `enable secret`).

## Comandos de VLAN
- **`vlan [número]`**: Crea una VLAN.
- **`name [nombre de VLAN]`**: Asigna un nombre a la VLAN.
- **`show vlan brief`**: Muestra un resumen de las VLANs configuradas en el dispositivo.

## Comandos para troubleshooting básico
- **`show version`**: Ver versión, uptime, información del dispositivo y firmware.
- **`show interfaces`**: Muestra el estado detallado de las interfaces.
- **`show ip arp`**: Muestra la tabla ARP que asigna direcciones IP a direcciones MAC.
- **`show ip route`**: Muestra la tabla de enrutamiento IP.
- **`show ip interface brief`**: Muestra un resumen de las interfaces y su estado.
- **`show running-config`**: Ver la configuración actual del dispositivo.
- **`show mac address-table`**: Muestra la tabla MAC, que asigna direcciones MAC a puertos físicos.
- **`show ip dhcp binding`**: Muestra las direcciones IP asignadas por DHCP.

## Comandos de hardware
- **`show inventory`**: Muestra información sobre los módulos de hardware del dispositivo.
- **`show memory`**: Muestra el uso de la memoria del dispositivo.
- **`show processes cpu`**: Muestra el porcentaje de uso de la CPU del dispositivo.
- **`show environment`**: Muestra información sobre el entorno del dispositivo, como la temperatura interna y el estado de los ventiladores.

## Comandos de QoS (Calidad de Servicio)
- **`show policy-map interface`**: Muestra las políticas de calidad de servicio aplicadas a las interfaces.
- **`policy-map`**: Configura una política de QoS.
- **`class-map`**: Crea una clase de tráfico para QoS.
- **`service-policy`**: Aplica una política de QoS a una interfaz.

## Comandos de NAT (Traducción de direcciones de red)
- **`show ip nat translations`**: Muestra las traducciones de direcciones NAT activas.
- **`ip nat inside source`**: Configura la traducción de direcciones internas a externas.
- **`ip nat outside source`**: Configura la traducción de direcciones externas a internas.

## Comandos de VPN (Red Privada Virtual)
- **`show vpn-sessiondb`**: Muestra las sesiones VPN activas.
- **`crypto ipsec transform-set`**: Configura un conjunto de transformaciones para IPsec.
- **`tunnel-group`**: Configura un grupo de túneles VPN.
- **`ipsec`**: Configura las políticas de IPsec para la VPN.

## Comandos de NetFlow
- **`ip flow ingress`**: Habilita NetFlow en una interfaz para recopilar datos de flujo.
- **`show ip flow`**: Muestra las estadísticas de NetFlow.

## Comandos de STP (Spanning Tree Protocol)
- **`show spanning-tree`**: Muestra el estado del protocolo Spanning Tree.
- **`spanning-tree vlan [número]`**: Configura Spanning Tree para una VLAN específica.

## Comandos de redundancia y alta disponibilidad
- **`show redundancy`**: Muestra el estado de la redundancia del dispositivo.
- **`standby [grupo] ip [dirección IP]`**: Configura una dirección IP virtual para HSRP (Hot Standby Router Protocol).

## Comandos de tráfico
- **`show interface [tipo] [número]`**: Muestra las estadísticas de tráfico de una interfaz específica.
- **`traffic-policy`**: Configura políticas de tráfico para manejar el ancho de banda y la calidad de servicio.

## Comandos de DHCP
- **`ip dhcp pool [nombre]`**: Configura un grupo de direcciones DHCP.
- **`ip dhcp exclude-address [dirección IP]`**: Excluye una dirección IP de la asignación DHCP.
- **`ip dhcp database`**: Muestra la base de datos del servidor DHCP.

## Plantilla de configuración WiFi en Cisco:

```shell
${BEGIN_WIFI}
#
interface Dot11Radio0
 ssid ${SSID_24GHZ}
 authentication open
 encryption wep 64 key 0 ${PASS_SSID_24GHZ}
 radio network mode mixed
#
interface Dot11Radio1
 ssid ${SSID_5GHZ}
 authentication open
 encryption wep 64 key 0 ${PASS_SSID_5GHZ}
 radio network mode mixed
#
${END_WIFI}
#
```

## Plantilla de configuración DHCP en Cisco

```shell
system-view:
${BEGIN_DHCP}
ip dhcp excluded-address ${DHCP_PRIMERA_IP_EXCL} ${DHCP_ULTIMA_IP_EXCL}
ip dhcp pool POOLDHCP
 network ${RED_LAN} ${MASK_LAN}
 default-router ${IP_LAN}
 dns-server ${DHCP_DNS_PRIMARIO} ${DHCP_DNS_SECUNDARIO}
#
interface ${INTERFAZ_LAN}
 ip address dhcp
 no shutdown
#
${END_DHCP}
#
```

## Plantilla de configuración DMZ para redes NEBA, FTTH, y HFC en Cisco:

# NEBA:
```shell
route-map DMZ_PPAL deny 10
match interface cellular0/2/0
!
ip nat inside source static 192.168.253.2 46.27.194.104 route-map DMZ_PPAL
!
route-map DMZ_LTE deny 10
match interface GigabitEthernet0/0/0.24
!
ip nat inside source static 192.168.253.2 62.87.126.214 route-map DMZ_LTE

****62.87.126.214--ipfija//46.27.194.104 DMZ*******
```

# FTTH:
```shell
route-map DMZ_PPAL deny 10
match interface cellular0/2/0
!
ip nat inside source static 100.74.255.242 77.231.20.220 (cambiar por la loopback) route-map DMZ_PPAL
!
route-map DMZ_LTE deny 10
match interface GigabitEthernet0/0/0.1500
!
ip nat inside source static 100.74.255.242 62.87.43.146 (cambiar por la ipfija) route-map DMZ_LTE
```

# HFC
```shell
route-map DMZ_PPAL deny 10
match interface cellular0/2/0
!
ip nat inside source static 100.74.255.242 77.228.182.188 route-map DMZ_PPAL
!
route-map DMZ_LTE deny 10
match interface Loopback0
!
ip nat inside source static 100.74.255.242 212.166.226.137 route-map DMZ_LTE
!
```
