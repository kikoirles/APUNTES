# Comandos más importantes de Huawei

## Comandos básicos

- **`system-view`**: Entra en el modo de configuración global.
- **`quit`**: Sale del modo actual.
- **`enable`**: Cambia al modo privilegiado.
- **`display current-configuration`**: Muestra la configuración activa.
- **`display version`**: Muestra la información del sistema, como la versión del software y la memoria.
- **`display interface`**: Muestra el estado y las estadísticas de todas las interfaces.

## Comandos de configuración

- **`hostname [nombre]`**: Cambia el nombre del dispositivo.
- **`interface [tipo] [número]`**: Entra en el modo de configuración de una interfaz específica.
- **`ip address [dirección IP] [máscara]`**: Asigna una dirección IP a una interfaz.
- **`shutdown`**: Desactiva una interfaz.
- **`undo shutdown`**: Activa una interfaz.
- **`ip route-static [dirección IP] [máscara] [puerta de enlace]`**: Configura una ruta estática.

## Comandos de diagnóstico

- **`ping [dirección IP]`**: Realiza una prueba de conectividad (ping) hacia una dirección IP.
- **`tracert [dirección IP]`**: Realiza un seguimiento de la ruta hacia un destino.
- **`display ip routing-table`**: Muestra las rutas en la tabla de enrutamiento.
- **`display interface brief`**: Muestra un resumen de todas las interfaces y su estado.

## Comandos de administración

- **`save`**: Guarda la configuración actual en la memoria de inicio.
- **`reset`**: Reinicia el dispositivo o una interfaz específica.
- **`reboot`**: Reinicia el dispositivo.
- **`display logbuffer`**: Muestra el registro de eventos del sistema.

## Comandos de seguridad

- **`enable secret [contraseña]`**: Establece una contraseña secreta para el acceso al modo privilegiado.
- **`user-interface console 0`**: Entra en la configuración de la consola.
- **`set authentication password [contraseña]`**: Establece una contraseña de acceso.
- **`set super password [contraseña]`**: Establece la contraseña para acceder al modo privilegiado.
- **`authentication-mode [modo]`**: Configura el modo de autenticación de la consola.

## Comandos de VLAN

- **`vlan [número]`**: Crea una VLAN.
- **`name [nombre de VLAN]`**: Asigna un nombre a la VLAN

## Comando para troubleshooting basico 

- **`dis ver`**: Ver version uptime dev info firmware
- **`dis ens`**: Mostrar el numero de serie de (ESN)
- **`dis arp`**: Adress resolution mapeo de dirrecion ip a dirreccion MAC
- **`dis esd`**: Serial number detalle sobre los equipos e interfaces conectadas 
  
- **`dis bgp peer`**: Detalles de los BGP sesiones bgp activos
- **`dis ip int b`**:s efectivamente utilizado para mostrar un resumen de las interfaces de red
- **`dis int brief`**: Obtener Vlan Error de interfaz
- **`ping -a 192.168.1.1 8.8.8.8`**: Ping a Vlan(192.168.1.1 Conectividad
- **`dis cur`**: Ver la configuración actual del dispositivo
- **`dis arp brief`**:Muestra la tabla ARP (Address Resolution Protocol) del dispositivo, que asigna direcciones IP a direcciones MAC en la red local
- **`tracert -a 192.168.1.1 8.8.8.8`**: se utiliza para rastrear la ruta (hops) que toma un paquete desde el origen a destino.

## Comandos de hardware 

- **`dis hardware`**: Muestra información detallada sobre los módulos de hardware del dispositivo (tarjetas de red, módulos de memoria, etc.).
- **`dis memory`**: Muestra el uso de la memoria del dispositivo, incluyendo la memoria total, utilizada, libre y el porcentaje de utilización.
- **`dis cpu-usage`**: Muestra el porcentaje de uso de la CPU del dispositivo, indicando la carga de trabajo en el procesador.
- **`dis system-resource`**: Muestra el uso de recursos del sistema, como CPU, memoria y otros componentes, proporcionando una visión general de la carga del sistema

- **`dis fun`**: Descripción: Muestra el estado de los ventiladores del dispositivo, incluyendo su velocidad y cualquier error relacionado con ellos.
- **`dis power`**: Descripción: Muestra el estado de la fuente de alimentación del dispositivo, indicando si está funcionando correctamente y detalles sobre su rendimiento
- **`dis environment`**: Descripción: Muestra información sobre el entorno del dispositivo, como la temperatura interna, el estado de los ventiladores, y otros parámetros ambientales importantes.
- **`dis logbuffer`**: Descripción: Muestra los mensajes de registro almacenados en el búfer del sistema, lo que ayuda a identificar problemas pasados o eventos importantes ocurridos en el dispositivo.
- **`dis alarm`**: Descripción: Muestra las alarmas activas o los eventos de error registrados en el sistema, permitiendo conocer problemas críticos en el dispositivo.
- **`dis process`**:Descripción: Muestra los procesos que están ejecutándose en el dispositivo, incluyendo el uso de CPU de cada proceso.

## Plantilla configuracion Huawei BASE

```shell
${BEGIN_WIFI}
#
wlan global country-code ES
y
#
interface Wlan-Bss2
 port hybrid tagged vlan 1
#
interface Wlan-Bss3
 port hybrid tagged vlan 1
#
wlan
 wmm-profile name WIFICLIENTE id 1
 traffic-profile name WIFICLIENTE id 1
 security-profile name WIFI24 id 1
  security-policy wpa2
  wpa2 authentication-method psk pass-phrase cipher ${PASS_SSID_24GHZ} encryption-method ccmp
 service-set name WIFI24 id 0
  Wlan-Bss 2
  ssid ${SSID_24GHZ}
  traffic-profile id 1
  security-profile id 1
 radio-profile name WIFI24 id 1
  wmm-profile id 1
 security-profile name WIFI5 id 2
  security-policy wpa2
  wpa2 authentication-method psk pass-phrase cipher ${PASS_SSID_5GHZ} encryption-method ccmp
 service-set name WIFI5 id 1
  Wlan-Bss 3
  ssid ${SSID_5GHZ}
  traffic-profile id 1
  security-profile id 2
 radio-profile name WIFI5 id 2
  wmm-profile id 1
#
interface Wlan-Radio0/0/0
 radio-profile id 1
 service-set id 0 wlan 1
#
interface Wlan-Radio0/0/1
 radio-profile id 2
 service-set id 1 wlan 1
#
${END_WIFI}
#
```

## Plantilla configuracion Huawei DHCP

- **`system-view`**:

```shell
${BEGIN_DHCP}
dhcp enable
dhcp server ping packet 3
dhcp server ping timeout 400
#
ip pool POOLDHCP
 gateway-list ${IP_LAN}
 network ${RED_LAN} mask ${MASK_LAN}
 excluded-ip-address ${DHCP_PRIMERA_IP_EXCL} ${DHCP_ULTIMA_IP_EXCL}
 dns-list ${DHCP_DNS_PRIMARIO} ${DHCP_DNS_SECUNDARIO}
#
interface ${INTERFAZ_LAN}
 dhcp select global
#
${END_DHCP}
#
```

## Plantilla configuracion Huawei DMZ para redes NEBA , FTTH , HFC

- **`system-view`**:

## NEBA
```shell
acl number 3200
 rule 5 deny tcp destination-port eq 22 
 rule 10 permit ip 

int GigabitEthernet0/0/8.24
nat static global interface LoopBack 0 inside 192.168.1.10 netmask 255.255.255.255
Y

int cellular0/0/0
nat static global current-interface inside 192.168.1.10 netmask 255.255.255.255 acl 3200
y
#
```


## FTTH
```shell
acl number 3200
 rule 5 deny tcp destination-port eq 22 
 rule 10 permit ip 

int GigabitEthernet0/0/8.1500
nat static global interface LoopBack 0 inside 100.74.255.242 netmask 255.255.255.255
Y

int cellular0/0/0
nat static global current-interface inside 100.74.255.242 netmask 255.255.255.255 acl 3200
Y
#
```

## HFC
```shell
acl number 3200
 rule 5 deny tcp destination-port eq 22 
 rule 10 permit ip 

int GigabitEthernet0/0/8
nat static global interface LoopBack 0 inside 100.74.255.242 netmask 255.255.255.255
Y

int cellular0/0/0
nat static global current-interface inside 100.74.255.242 netmask 255.255.255.255 acl 3200
Y
#
```

