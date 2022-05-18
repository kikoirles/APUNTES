---
title: "VIRTUAL HOST CIFRADOS Y  AUTOFIRMADOS"
tags: ""
---

# Seguridad en Apache
## Cifrado de conexiones con un certificado autofirmado


### Introducción

Para preparar un entorno de desarrollo donde probar nuestras aplicaciones web o la configuración de aquellos CMSs que podamos descargar de Internet, es conveniente trabajar con un sistema muy similar a aquel que pueda pasar a producción.

Hoy en día la seguridad en la web está fundamentada en gran medida en el cifrado de conexiones extremo a extremo utilizando la capa de transporte TLS, sucesora de SSL (*Secure Socket Layer*).

Una forma de preparar un entorno de desarollo es activar esta capa mediante un certificado autofirmado que pese a que no está expedido por ninguna entida certificadora de confianza, nos permite utilizar el protocolo *https* para realizar pruebas sin tener contratado un dominio en Internet.

Si el entorno en el que estemos haciendo pruebas tiene que pasar a estar disponible en Internet, se deberían modificar los certificados utilizados por unos que estuvieran firmados por una entidad certificadora reconocida en Internet. En este caso la Internet Security Research Group (ISRG) creo una entidad llamada [Let's Encrypt](https://letsencrypt.org) que ofrece la posibilidad de expedir certificados validos en Internet a coste cero.

### Crear el certificado SSL

La TLS y la SSL funcionan utilizando una combinación de un certificado público y una clave privada. La clave SSL se mantiene secreta en el servidor. Se utiliza para cifrar contenido que se envía a los clientes. El certificado SSL se comparte de forma pública con cualquiera que solicite el contenido. Puede utilizarse para descifrar el contenido firmado por la clave SSL asociada.

Podemos crear un par de clave y certificado autofirmados con OpenSSL en un único comando:

```shell
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```
Si estudiamos el comando podemos encontrar los siguientes apartados:

* **openssl**: es la herramienta de línea de comandos básica para crear y administrar certificados, claves, y otros archivos de OpenSSL.

* **req**: este subcomando especifica que deseamos usar la administración de la solicitud de firma de certificados (CSR) X.509. El “X.509” es un estándar de infraestructura de claves públicas al que se adecuan SSL y TLS para la administración de claves y certificados a través de él. Queremos crear un nuevo certificado X.509, por lo que usaremos este subcomando.

* **-x509**: modifica aún más el subcomando anterior al indicar a la utilidad que deseamos crear un certificado autofirmado en lugar de generar una solicitud de firma de certificados, como normalmente sucede.

* **-nodes**: indica a OpenSSL que omita la opción para proteger nuestro certificado con una frase de contraseña. Necesitamos que Apache pueda leer el archivo, sin intervención del usuario, cuando se inicie el servidor. Una frase de contraseña evitaría que esto suceda porque tendríamos que ingresarla tras cada reinicio.
* **-days 365**: esta opción establece el tiempo durante el cual el certificado se considerará válido. En este caso, lo configuramos por un año.
* **-newkey rsa:2048**: especifica que deseamos generar un nuevo certificado y una nueva clave al mismo tiempo. No creamos la clave que se requiere para firmar el certificado en un paso anterior, por lo que debemos crearla junto con el certificado. La parte rsa:2048 le indica que cree una clave RSA de 2048 bits de extensión.

* **-keyout**: esta línea indica a OpenSSL dónde colocar el archivo de clave privada generado que estamos creando.

* **-out**: indica a OpenSSL dónde colocar el certificado que creamos.
Como se mencionó anteriormente, estas opciones crearán un archivo de clave y un certificado. Se harán algunas preguntas sobre nuestro servidor con el fin de insertar la información de forma correcta en el certificado.

Ahora tenemos que responder a aquellas preguntas que recopilan la información del servidor para generar el certificado adecuado. La línea más importante es aquella en la que se solicita Common Name (e.g. server FQDN or YOUR name). Debemos introducir el nombre de dominio asociado con su servidor o, lo más probable, la dirección IP pública de su servidor.

La información que se solicita para generar el certificado es la siguiente:

```shell
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Alicante
Locality Name (eg, city) []:Petrer
Organization Name (eg, company) [Internet Widgits Pty Ltd]:IES Poeta Paco Mollá
Organizational Unit Name (eg, section) []: Departamento de Informática
Common Name (e.g. server FQDN or YOUR name) []: 192.168.2.110 o srvMiguel.iespacomolla.es
Email Address []:admininstrador@iespacomolla.es
Los dos archivos que creó se ubicarán en los subdirectorios correspondientes en /etc/ssl.
```

### Configurar Apache para usar SSL
Hemos creado nuestros archivos de clave y certificado en el directorio /etc/ssl. Ahora solo debemos modificar nuestra configuración de Apache para aprovecharlos.

Aplicaremos algunos ajustes a nuestra configuración:

1. Crearemos un fragmento de configuración para especificar configuraciones SSL seguras predeterminadas y activaremos esta configuración.
2. Modificaremos el archivo de host virtual de Apache SSL incluido para apuntar a los certificados SSL que generamos.
3. Modificaremos el archivo de host virtual no cifrado para redireccionar las solicitudes de forma automática al host virtual cifrado.

Al terminar, deberíamos contar con una configuración SSL segura.

#### Crear un fragmento de configuración de Apache con ajustes de cifrado seguro

Primero, crearemos un fragmento de configuración de Apache para definir algunos ajustes de SSL. Con esto, se configurará Apache con un conjunto de cifrado SSL seguro y se habilitarán algunas características avanzadas que ayudarán a mantener protegido nuestro servidor. Los parámetros que configuraremos pueden utilizarse a través de cualquier hosting virtual que habilite SSL.

Creamos un nuevo fichero de configuración en el directorio /etc/apache2/conf-available. Nombramos al fichero *ssl-params.conf* para que quede claro su propósito:

```shell
nano /etc/apache2/conf-available/ssl-params.conf
```

Para configurar Apache SSL de forma segura, usaremos las recomendaciones que Remy van Elst da en el sitio de [Cipherli.st](https://syslink.pl/cipherlist/). Este sitio está diseñado para proporcionar configuraciones de cifrado fáciles de utilizar para software popular.

Para nuestros propósitos, podemos copiar las configuraciones proporcionadas en su totalidad. Sólo haremos un pequeño cambio. Deshabilitaremos el encabezado Strict-Transport-Security (HSTS).

La precarga del HSTS proporciona una mayor seguridad, pero puede tener consecuencias importantes si se habilita accidentalmente o de forma incorrecta.

Pegue la configuración en el archivo ssl-params.conf que abrimos:

```shell
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
SSLHonorCipherOrder On

# Disable preloading HSTS for now.  You can use the commented out header line that includes
# the "preload" directive if you understand the implications.
# Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"

Header always set X-Frame-Options DENY
Header always set X-Content-Type-Options nosniff

# Requires Apache >= 2.4
SSLCompression off
SSLUseStapling on
SSLStaplingCache "shmcb:logs/stapling-cache(150000)"

# Requires Apache >= 2.4.11
SSLSessionTickets Off
```
Guardamos y cerramos el fichero cuando las modificaciones estén hechas. Teniendo en cuenta que esta configuración no está activida utilizaremos el siguiente comando para activarla en Apache.

```shell
a2enconf ssl-params
```
Para que la configuración se tome en cuenta debemos reiniciar el servidor Apache

```shell
systemctl restart apache2.service
```
Esta configuración utiliza dos módulos que pueden no estar activados por defecto en Apache. Estos dos módulos son el módulo ```ssl``` y el módulo ```headers``` que podemos activar con la herramienta *a2enmod* 

### Modificamos el archivo de host virtual de Apache SSL predeterminado

A continuación, modificaremos /etc/apache2/sites-available/default-ssl.conf, el archivo de host virtual de SSL predeterminado. 

Una buena opción es crear una copia de seguridad del archivo original de host virtual de SSL:

```shell
cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
```

Ahora, modificamos el archivo de host virtual de SSL para realizar ajustes:

```shell
nano /etc/apache2/sites-available/default-ssl.conf
```

Haremos algunos ajustes menores en el archivo. Configuraremos las cosas normales que querríamos ajustar en un archivo de host virtual (dirección de correo electrónico de ServerAdmin, ServerName, etc.), y ajustaremos las directivas SSL para que apunten a nuestros archivos de certificados y claves.

Tras realizar estos cambios, el fichero de configuración del virtual host por defecto con conexión SSL debería quedar de la siguiente forma:

```shell
<IfModule mod_ssl.c>
        <VirtualHost _default_:443>
                ServerAdmin administrador@iespacomolla.es
                ServerName 192.168.2.110 o srvMiguel.iespacomolla.es

                DocumentRoot /var/www/html

                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined

                SSLEngine on

                SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
                SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

        </VirtualHost>
</IfModule>
```

####Modificar el archivo de host HTTP para el redireccionamiento a HTTPS

En su estado actual, el servidor proporcionará tanto tráfico HTTP no cifrado como tráfico HTTPS cifrado. Para una mayor seguridad, en la mayoría de los casos se recomienda redireccionar HTTP a HTTPS de forma automática.

Para ajustar el archivo de host virtual no cifrado de modo que se redireccione todo el tráfico y cuente con cifrado SSL, podemos abrir el archivo /etc/apache2/sites-available/000-default.conf:

```shell
nano /etc/apache2/sites-available/000-default.conf
```
Dentro de los bloques de configuración de VirtualHost, debemos añadir una directiva Redirect, que dirija todo el tráfico a la versión SSL del sitio:

```shell
<VirtualHost *:80>
        . . .

        Redirect "/" "https://your_domain_or_IP/"

        . . .
</VirtualHost>
```

Guarde y cierre el archivo cuando haya terminado y procedemos a recargar el servidor

```shell
systemctl restart apache2
```
Podemos habilitar mod_ssl, el módulo SSL de Apache y mod_headers, que necesitan algunas de las configuraciones de nuestro fragmento SSL, con el comando a2enmod:

```shell
sudo a2enmod ssl
sudo a2enmod headers
```

A continuación, podemos habilitar nuestro host virtual SSL con el comando a2ensite:

```shell
sudo a2ensite default-ssl
```
También debemos habilitar nuestro archivo ssl-params.conf, para leer los valores que configuramos:

```shell
sudo a2enconf ssl-params
```

En este punto, nuestro sitio y los módulos necesarios quedarán habilitados. Deberíamos comprobar que no haya errores de sintaxis en nuestros archivos. Podemos hacerlo escribiendo lo siguiente:

```shell
sudo apache2ctl configtest
```
Si la operación se completa de forma correcta, obtendrá un resultado similar a este:

Output
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK

La primera línea es solo un mensaje que le indica que la directiva ServerName no está configurada a nivel global. Si quiere deshacerse de ese mensaje, puede establecer ServerName en el nombre de dominio o la dirección IP de su servidor en /etc/apache2/apache2.conf. Esto es opcional, ya que el mensaje no causará problemas.

Si el resultado contiene Syntax OK, en su archivo de configuración no habrá errores de sintaxis. Podemos reiniciar Apache de forma segura para implementar nuestros cambios:

sudo systemctl restart apache2
Paso 5: Probar el cifrado
Ahora, estamos listos para probar nuestro servidor SSL.

Abra su navegador web y escriba https:// seguido del nombre de dominio o el IP de su servidor en la barra de direcciones:

```shell
https://server_domain_or_IP
```
