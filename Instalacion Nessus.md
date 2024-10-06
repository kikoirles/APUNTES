# Manual de Instalación de Nessus

## Índice

1. [Requisitos previos](#requisitos-previos)
2. [Descarga de Nessus](#descarga-de-nessus)
3. [Instalación de Nessus](#instalación-de-nessus)
4. [Inicialización y Configuración](#inicialización-y-configuración)
5. [Acceso a la Interfaz Web](#acceso-a-la-interfaz-web)
6. [Activación de Nessus](#activación-de-nessus)
7. [Actualización de Plugins](#actualización-de-plugins)
8. [Conclusión](#conclusión)

## Requisitos previos

Antes de proceder con la instalación de Nessus, asegúrate de tener lo siguiente:

- Sistema operativo Linux (Debian/Ubuntu)
- Acceso a usuario root o permisos de sudo
- Conexión a Internet
- Navegador web moderno

## Descarga de Nessus

1. Ve al sitio oficial de Tenable para descargar Nessus:
   
   [Página de descarga de Nessus](https://www.tenable.com/downloads/nessus)

2. Selecciona tu sistema operativo (en este caso, **Ubuntu/Debian**).
   
   También puedes utilizar `wget` para descargar directamente el paquete. Por ejemplo:

   ```shell
   wget https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/14300/download?i_agree_to_tenable_license_agreement=true -O Nessus-latest.deb
   ``` 

## Instalación de Nessus
Navega hasta la ubicación del archivo descargado y utiliza el siguiente comando para instalar Nessus:

```shell
sudo dpkg -i Nessus-latest.deb
```

Instala las dependencias faltantes con el siguiente comando:

```shell
sudo apt --fix-broken install
```

Verifica si Nessus se ha instalado correctamente:

```shell
sudo systemctl status nessusd
```

Habilita Nessus para que inicie automáticamente en cada arranque:

```shell
sudo systemctl enable nessusd
```

## Inicialización y Configuración
Inicia el servicio de Nessus:

```shell
sudo systemctl start nessusd
```

Espera unos minutos para que Nessus se inicialice. Una vez que esté listo, abre tu navegador y navega a la interfaz web de Nessus:

```shell
https://localhost:8834
```
Es posible que recibas una advertencia de certificado no seguro; puedes proceder de todos modos, ya que es una advertencia estándar para las conexiones HTTPS locales.

Acceso a la Interfaz Web
Al acceder por primera vez, se te pedirá que selecciones el tipo de instalación de Nessus. Las opciones incluyen:

Nessus Essentials (Gratis)
Nessus Professional (Licencia paga)
Nessus Manager
Selecciona Nessus Essentials si deseas utilizar la versión gratuita, o la que mejor se ajuste a tus necesidades.

Proporciona una dirección de correo electrónico válida para recibir un código de activación.

## Activación de Nessus
Después de recibir el código de activación en tu correo, introdúcelo en el campo solicitado en la interfaz web de Nessus.

Una vez activado, Nessus descargará e instalará automáticamente los plugins más recientes. Este proceso puede tardar varios minutos dependiendo de la velocidad de tu conexión.

## Actualización de Plugins
Después de la instalación inicial, es importante mantener los plugins actualizados. Puedes actualizar los plugins manualmente desde la interfaz web o mediante el siguiente comando en la terminal:

```shell
sudo /opt/nessus/sbin/nessuscli update
```
También puedes programar las actualizaciones automáticas dentro de la interfaz de Nessus.

## Conclusión
Has completado la instalación y configuración de Nessus. Ahora puedes comenzar a escanear vulnerabilidades en tu red o aplicaciones.

Para acceder a la interfaz de Nessus en el futuro, simplemente navega a:

```shell
https://localhost:8834
```
Recuerda utilizar la herramienta de manera ética y de acuerdo a las leyes locales.
