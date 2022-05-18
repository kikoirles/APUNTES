---
title: "GESTION DE FICHEROS PHP"
tags: ""
---

# Gestión de ficheros con PHP

Php al ser un lenguaje ejecutado en el lado del servidor tiene la capacidad de realizar una serie de opciones de gestión de ficheros almacenados en el servidor. Para ello dispone de una serie de funciones que nos permitirán, por ejemplo, crear carpetas, crear ficheros, mover ficheros, renombrarlos o borrarlos. En este apartado también es fundamental, las directivas que permiten a PHP obtener y almacenar un fichero en el servidor que haya sido seleccionado por un usuario mediante un formulario.

A continuación, se presentan las principales funciones y uso de las mismas para trabajar con ficheros en PHP

## Creación de carpetas

PHP dispone de la función mkdir que permite crear carpetas dentro del servidor. La función tiene la siguiente estructura: 

```php
mkdir(
    string $pathname,
    int $mode = 0777,
    bool $recursive = false,
    resource $context = ?
): bool
```

En ella podemos observar que en el primer parámetro se definirá la ruta del directorio a crear, ruta que puede ser absoluta o relativa al lugar donde está almacenada la página PHP que ejecuta la función. A continuación, se define los permisos que tendrá el directorio creado, para finalizar con un valor booleano que indicará si se pueden crear subdirectorios a partir de este. La última opción de *context* está relacionada con opciones definidas para el tratamiento de flujos.

Podemos utilizar esta función de la siguiente forma:

```php
$ruta_a_crear = "./".$_POST['nombre']."/"; 
mkdir($ruta_a_crear,0774,true);
```

## Listado de archivos de una carpeta

Otra de las funciones que podemos utilizar cuando trabajamos con un directorio es mostrar los elementos que contiene el mismo. Para ello PHP dispone de la función `scandir` que tiene el siguiente prototipo:

```php
scandir(string $directory, int $sorting_order = SCANDIR_SORT_ASCENDING, resource $context = ?): array
```

En ella le pasamos el directorio y una constante que indica si queremos que aparezcan los nombres de archivo de forma ascendente o descendente y nos devolverá en un array el conjunto de ficheros que tenemos almacenados en el mismo. 

Un ejemplo de uso del mismo puede ser el siguiente:

```php
$ruta_actual = getcwd();
$ficheros = scandir($ruta_a_crear,SCANDIR_SORT_DESCENDING);

foreach ($ficheros as $fichero){
  echo "$fichero<br>";
}
```

En el ejemplo se ha utilizado la función `getcwd` que devuelve mediante un string la ruta del directorio actual. También se puede utilizar la función `chdir` para cambiar el directorio actual a otro definido como parámetro.

## Creación de ficheros

Para crear un fichero mediante funciones PHP se utilizará la función `fopen()` que permite la creación y apertura si el fichero existe de un flujo para poder realizar operaciones de lectura o modificaciones de los mismos. El prototipo de la función `fopen ()`es el siguiente:

```php
fopen(
    string $filename,
    string $mode,
    bool $use_include_path = false,
    resource $context = ?
): resource
```

Los principales parámetros a definir son, el nombre del fichero que puede incluir la ruta del mismo, además del modo de apertura del mismo y el tercero permite indicar si se desea realizar la búsqueda de este fichero en el directorio *include* definido por defecto durante la instalación de PHP. Los modos de apertura de un fichero pueden ser:

| `mode` | Descripción                                                  |
| :----- | :----------------------------------------------------------- |
| `'r'`  | Apertura para sólo lectura; coloca el puntero al fichero al principio del fichero. |
| `'r+'` | Apertura para lectura y escritura; coloca el puntero al fichero al principio del fichero. |
| `'w'`  | Apertura para sólo escritura; coloca el puntero al fichero al principio del fichero y trunca el fichero a longitud cero. Si el fichero no existe se intenta crear. |
| `'w+'` | Apertura para lectura y escritura; coloca el puntero al fichero al principio del fichero y trunca el fichero a longitud cero. Si el fichero no existe se intenta crear. |
| `'a'`  | Apertura para sólo escritura; coloca el puntero del fichero al final del mismo. Si el fichero no existe, se intenta crear. En este modo, [fseek()](https://www.php.net/manual/es/function.fseek.php) solamente afecta a la posición de lectura; las lecturas siempre son pospuestas. |
| `'a+'` | Apertura para lectura y escritura; coloca el puntero del fichero al final del mismo. Si el fichero no existe, se intenta crear. En este modo, [fseek()](https://www.php.net/manual/es/function.fseek.php) no tiene efecto, las escrituras siempre son pospuestas. |
| `'x'`  | Creación y apertura para sólo escritura; coloca el puntero del fichero al principio del mismo. Si el fichero ya existe, la llamada a **fopen()** fallará devolviendo **`false`** y generando un error de nivel **`E_WARNING`**. Si el fichero no existe se intenta crear. Esto es equivalente a especificar las banderas `O_EXCL|O_CREAT` para la llamada al sistema de `open(2)` subyacente. |
| `'x+'` | Creación y apertura para lectura y escritura; de otro modo tiene el mismo comportamiento que `'x'`. |
| `'c'`  | Abrir el fichero para sólo escritura. Si el fichero no existe, se crea. Si existe no es truncado (a diferencia de `'w'`), ni la llamada a esta función falla (como en el caso con `'x'`). El puntero al fichero se posiciona en el principio del fichero. Esto puede ser útil si se desea obtener un bloqueo asistido (véase [flock()](https://www.php.net/manual/es/function.flock.php)) antes de intentar modificar el fichero, ya que al usar `'w'` se podría truncar el fichero antes de haber obtenido el bloqueo (si se desea truncar el fichero, se puede usar [ftruncate()](https://www.php.net/manual/es/function.ftruncate.php) después de solicitar el bloqueo). |
| `'c+'` | Abrir el fichero para lectura y escritura; de otro modo tiene el mismo comportamiento que `'c'`. |
| `'e'`  | Establecer la bandera 'close-on-exec' en el descriptor de fichero abierto. Disponible solamente en PHP compilado en sistemas que se ajustan a POSIX.1-2008. |

Un ejemplo del uso para la creación de un fichero puede ser:

```php
?php
     $archivo = fopen("datos.txt", "w+b");    // Abrir el archivo, creándolo si no existe
    if( $archivo == false )
      echo "Error al crear el archivo";
    else
      echo "El archivo ha sido creado";
    fclose($archivo);   // Cerrar el archivo
?>
```

Esto crea un flujo con el fichero enlazado con el cual podremos interactuar, por ejemplo, de la siguiente forma:

```php
if( $archivo == false ) {
      echo "Error al crear el archivo";
    }
    else
    {
        // Escribir en el archivo:
         fwrite($archivo, "Estamos probando\r\n");
         fwrite($archivo, "el uso de archivos ");
         fwrite($archivo, "en PHP");
        // Fuerza a que se escriban los datos pendientes en el buffer:
         fflush($archivo);
    }
```

O también con la función `file_put_contents`.

```php
<?php
    $cadena = file_get_contents("datos.txt");
    $cadena .= "\r\nCadena de texto hacia el fichero!";
     file_put_contents("datos.txt", $cadena);
?>
```

## Comprobación de existencia de un fichero o directorio

Si necesitamos averiguar si existe un archivo o carpeta utilizaremos la función de PHP **file_exists()**, la cual nos devolverá **true** si existe **false** en caso contrario:

```php
<?php
     if( file_exists("datos.txt") == true )
        echo "<p>El archivo existe</p>";
    else
        echo "<p>El archivo no se ha encontrado</p>";
     if( file_exists("miCarpeta") == true )
        echo "<p>El directorio existe</p>";
    else
        echo "<p>El directorio no existe</p>";
     if( file_exists("./miCarpeta/") == true )
        echo "<p>El directorio existe</p>";
    else
        echo "<p>El directorio no existe</p>";
?>
```

## Información de archivos y directorios

PHP ofrece algunas funciones para obtener información de ficheros y directorios que estén almacenados en el servidor. Algunos ejemplos de estas son las siguientes:

Algunas funciones que nos devolverán información sobre un archivo son:

- **filesize()**: devuelve el tamaño de un archivo en bytes.

- **filetype()**: devuelve el tipo de fichero de que se trata: **file, dir, block, char, fifo, link o unknown**.

- **filemtime()**: devuelve la fecha y hora de la última modificación del archivo, en formato **UNIX Timestamp**.

- **fileperms()**: devuelve la mascara de permisos (en sistemas UNIX).

- **stat()** y **fstat()**: devuelven información sobre un archivo abierto con **fopen()**.

  La diferencia entre ellas es que a **stat()** se le pasa como parámetro el nombre del archivo, y a **fstat()** el recurso obtenido con **fopen()**.

Otras funciones con las que podemos obtener información sobre los archivos son:

- **is_executable()**: devuelve **true** si el archivo es ejecutable.
- **is_readable()**: comprueba si existe un archivo se puede leer.
- **is_writable()**: comprueba si existe un archivo y se puede escribir en él.
- **is_file()**: indica si el nombre pasado como parámetro es un archivo.
- **is_link()**: indica si el nombre pasado como parámetro es un enlace simbólico.
- **is_dir()**: indica si el nombre pasado como parámetro es un directorio.

## Otras operaciones con archivos y directorios

En la gestión de archivos con PHP también tenemos funciones que permiten la copia, mover y eliminar archivos que tengamos almacenados.

Para copiar y eliminar archivos disponemos de las funciones **copy()** y **unlink()**, respectivamente. Hay que tener en cuenta que al copiar un archivo a otro directorio, si existe uno con el mismo nombre será sobreescrito. Si lo que necesitamos es **renombrar o mover archivos**, en ambos casos utilizaremos la función **rename()**. Todas estas funciones devuelven **true** o **false** si ha ocurrido algún error.

Un ejemplo de la utilización de estas funciones es el siguiente:

```php
<?php
    // Copiar el archivo a otra ruta;
    copy("datos.txt", "c:\\datos.txt");
    // Copiar el archivo con otro nombre:
    copy("datos.txt", "datos-2.txt");
    copy("datos.txt", "datos-3.txt");
    copy("datos.txt", "datos-4.txt");
    // Renombrar el archivo:
    rename("datos-2.txt", "datos---2.txt");
    // Renombrar carpetas:
    rename("miCarpeta1", "miCarpeta1-1");
    rename("./miCarpeta2", "./miCarpeta2-2");
    // Mover el archivo:
    rename("datos-3.txt", "c:\\datos-3-3.txt");
    // Eliminar el archivo:
    unlink("datos-4.txt");
    echo "Proceso finalizado";
?>
```

## Lectura de archivos y envio al navegador para su descarga

Otra función útil es `readfile` que utilizándolo de la siguiente forma permitirá incluir el fichero para que al usuario le aparezca la pantalla de descarga del fichero dentro del navegador.

```php
<?php
    $archivo = "test.pdf";
    if( file_exists($archivo) )
    {
        // Enviamos el PDF al cliente
         header("Content-type: application/pdf");
         header("Content-Disposition: attachment; filename=".$archivo);
         header("Content-length: ".filesize($archivo));
         readfile($archivo);
    }
?>
```

## Subida de archivos desde un formulario

Para permitir que un usuario realice una subida de ficheros desde el navegador, tenemos que crear un formulario que acepte la transferencias de ficheros. Para ello, tenemos que especificar dentro de la definición del formulario el parámetro `enctype="multipart/form-data"` que nos permite cargar ficheros utilizando el método *post*. Dentro del formulario, tenemos que añadir un campo *input* de tipo *file* que nos mostrará la opción para cargar el fichero.

```html
<form action="subirArchivo.php" method="post" enctype="multipart/form-data">
  <label for="nombre">
    Introduce tu nombre de usuario
  </label>
  <input name="nombre" id="nombre" type="text">
  <label for="subida">Añade el fichero</label>
  <input name="subida" id="subida" type="file">
  <button type="submit" name="enviar">Enviar datos</button>
</form>
```

Ahora tendríamos que generar el fichero PHP que se encargue de leer y almacenar el fichero que el usuario transfiera. Para ello, antes de ejecutar nuestro *script* en PHP tenremos que configurar una serie de opciones dentro de la configuración propia del lenguaje que podemos configurar dentro del fichero *php.ini* que define la configuración.

Las siguientes directivas del fichero son:

* **file_uploads**: tiene que tener el valor *on* para permitir la carga de ficheros.
* **upload_max_filesize**: permite configurar el tamaño máximo del fichero cargado que por defecto es de 2 Megabytes.
* **upload_tmp_dir**: permite definir el directorio donde se almacenarán temporalmente los ficheros subidos.
* **post_max_size**: define el tamaño máximo de los datos que se pueden recibir con el método *POST*.
* **max_file_uploads**: establece el número máximo de archivos que se pueden cargar de una vez.
* **max_input_time**: define el número máximo de segundos que un script puede analizar los datos de entrada.
* **memory_limit**: indica el número máximo de memoria RAM que puede consumir un script en el servidor.
* **max_execution_time**: establece el número máximo de segundos que se permite ejecutar a un script de PHP.

Con todos estos parámetros configurados para permitirnos trabajar con ficheros del tamaño que nos interese y el tiempo que necesitemos para poder procesarlos correctamente con nuestros scripts.

Para procesar un fichero que nos hayan cargado con el método *post* tenemos el array *$_FILES* que nos permite acceder con el nombre definido en *input* del formulario. El fichero subido se almacenará en un directorio temporal y podremos acceder a su información mediante este array bidimensional, el cual, almacenará en las filas el nombre de los ficheros cargados y en las columnas los datos a los que podemos acceder de cada uno de los ficheros. Estas propiedades que podemos acceder de los ficheros son las siguientes:

- **`tmp_name:`** La ruta temporal donde se carga el archivo se almacena en esta variable.
- **`name`**: El nombre real del archivo se almacena en esta variable.
- **`size`**: Indica el tamaño del archivo cargado en bytes.
- **`type`**: Contiene el tipo mime del archivo cargado.
- **`error`**: Si hay un error durante la carga del archivo, esta variable se rellena con el mensaje de error correspondiente. En el caso de que se cargue correctamente el archivo, contiene 0, que puede comparar utilizando la constante `UPLOAD_ERR_OK`.

Podemos ver un ejemplo de la utilización de un archivo cargado mediante el formulario anterior:

```php
<?php
    if (!$_FILES['subida']['error']==UPLOAD_ERR_OK){
        echo "El fichero no se ha cargado correctamente".$_FILES['subida']['error'];
    }else{
        echo "La información del fichero es la siguiente<br>";
        echo "El fichero está almacenado en ".$_FILES['subida']['tmp_name']."<br>";
        echo "El nombre del fichero es ".$_FILES['subida']['name']."<br>";
        echo "El tamaño del fichero es ".$_FILES['subida']['size']."<br>";
        echo "El tipo del fichero es ".$_FILES['subida']['type']."<br>";
      
      //Separa nombre y extensión dentro de un array
        $NombreSeccionado=explode(".",$_FILES['subida']['name']);
        echo "La extensión del fichero es ".strtolower(end($NombreSeccionado));
```

Este fichero subido que está almacenado en un directorio temporal, lo podremos mover para almacenarlo en la ruta donde tenga que quedar almacenado de forma definitiva con la función *move_uploaded_file* que tiene el siguiente prototipo.

```php
move_uploaded_file(string $filename, string $destination): bool
```

Así en el ejemplo utilizaremos una variable que almacene la ruta donde se debe almacenar de forma definitiva el fichero.

```php
if(move_uploaded_file($_FILES['subida']['tmp_name'],$ruta_a_crear.$_FILES['subida']['name']))
  echo "Se ha almacenado correctamente el fichero";
else
  echo "Ha habido un problema en la carga del fichero";
```
