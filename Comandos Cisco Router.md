# Comandos más importantes de Cisco Router 

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
- **`show ip route`**: Para ver la RIB (Routing Information Base)
- **`show ip cef`**: Para ver la FIB (Routing Information Base)

## Comandos de administración

- **`system-view`**: Entra en el modo de usuario privilegiado
- **`write memory`**: Guarda la configuración
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


