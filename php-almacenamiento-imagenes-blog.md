---
title: "PHP ALMACENAMIENTO IMAGENES-BLOG"
tags: ""
---

# Almacenamiento de imágenes en MySQL con Blob

El tipo de datos de gran objeto binario (BLOB) es un tipo de datos de MySQL que puede almacenar datos binarios como los de archivos de imagen, multimedia y PDF.

Al crear aplicaciones que requieren una base de datos estrechamente acoplada donde las imágenes deben estar sincronizadas con los datos relacionados (por ejemplo, un portal de empleados, una base de datos de clientes o una serie de productos), puede resultarle conveniente almacenar imágenes como las de fotos en una base de datos de MySQL junto con otra información relacionada.

Aquí es donde entra el tipo de datos BLOB de MySQL. De esta forma se elimina la necesidad de crear un sistema de archivos independiente para almacenar imágenes. El esquema también centraliza la base de datos, haciéndola más portátil y segura porque los datos están aislados del sistema de archivos. Crear copias de seguridad también es más sencillo, ya que que puede crear un solo archivo MySQL dump que contenga todos sus datos.

Para realizar la prueba de este ejemplo podemos crear la siguiente tabla en la base de datos junto con la base de datos que se ha estado utilizando:

```sql
CREATE TABLE mascotas(
	id_mascota INT (2) AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR (50),
  foto MEDIUMBLOB,
  id_equipo INT (2) REFERENCES equipo(codigo_eq)
)
```

Cuando ya tenemos la tabla creada en la base de datos que contenga campos BLOB podemos probar a almacenar imagenes desde una web en PHP. Para ello, creamos un formulario que no permita subir una imagen.

```php+HTML
    <form action="procesarBLOB" method="post" enctype="multipart/form-data">
        <label for="nombre">Introduce el nombre de la mascota</label>
        <input type="text" name="nombre" id="nombre">
        <label for="imagen">Sube la imagen de la mascota</label>
        <input type="file" name="imagen" id="imagen">
        <select name="equipo">
          	//Cargamos el desplegable con los equipos de la base de datos
            <?php
            $conexion=conectarBD();
            $query = "SELECT nombre_eq FROM equipos";
            $resultado = mysqli_query ($conexion, $query);

            while($fila = mysqli_fetch_assoc($resultado)){
                echo "<option>".$fila['nombre']."</option>";
            }
            desconectarBD($conexion);
            ?>
        </select>
        <button type="submit">Enviar datos</button>
    </form>
```

Ahora llega el momento de gestionar el script PHP que almacene la información incluyendo el campo BLOB que contendrá la imagen, pero antes debemos obtener el código de equipo a partir del nombre.

```php
$nombre= $_POST['nombre'];
$imagen= $_FILES['imagen']['tmp_name'];
$nombre_eq = $_POST['equipo'];

//Obtenemos el ID del equipo para poder almacenarlo en la BD
    $conexion=conectarBD();
    $sentencia = mysqli_prepare($conexion,"SELECT codigo_eq FROM equipo WHERE nombre_eq=?");
    mysqli_stmt_bind_param($sentencia,"s",$nombre_eq);

    mysqli_stmt_bind_result($sentencia,$codigo_eq);

    mysqli_stmt_execute($sentencia);

    mysqli_stmt_fetch($sentencia);
		desconectarBD($conexion);
```

A continuación, llega el momento de preparar toda la información para poder almacenarla en la base de datos junto con una imagen en BLOB

```php
$imagen_preparada=file_get_contents($imagen);
$sql = "INSERT INTO mascotas(nombre,foto,id_equipo) VALUES (?,?,?)";
$conexion=conectarBD();
$sentencia= mysqli_prepare($conexion,"INSERT INTO mascotas(nombre,foto,id_equipo) VALUES (?,?,?)");
//Hay que notar que utilizamos el modificador s para la imagen ya preparada pues la función file_get_contents nos la convierte a texto
mysqli_stmt_bind_param($sentencia,"ssi",$nombre,$imagen_preparada,$codigo_eq);
mysqli_stmt_execute($sentencia);
desconectarBD($conexion);
```

Con la imagen ya almacenada podemos realizar la lectura de esta imagen de la siguiente forma:

```php+HTML
<?php
$sql = "SELECT id_mascota,nombre,foto FROM mascotas";
$resultado = mysqli_query($conexion,$sql);
//Visualizar los resultados de las imagenes y entre ellos las consultas
?>
<table border = '1' align = 'center'> <caption>Mascotas</caption>
  <tr>
    <th>Id Mascota</th>
    <th>Nombre</th>
    <th>Foto</th>
  </tr>

  <?php
  while ($fila = mysqli_fetch_assoc($resultado)) {
    echo '<tr>';
    echo '<td>' . $fila['id_mascota'] . '</td>';
    echo '<td>' . $fila['nombre'] . '</td>';
    echo '<td>' .
      '<img src = "data:image/png;base64,' . base64_encode($fila['foto']) . '" width = "50px" height = "50px"/>'
      . '</td>';
    echo '</tr>';
  }
  ?>
    </table>
```

## Referencias

Castellano, E. S. P. L. (s. f.). *Manejo de datos BLOB con PHP y MySQL*. Programación en Castellano. Recuperado 17 de noviembre de 2021, de https://programacion.net/articulo/manejo_de_datos_blob_con_php_y_mysql_194

Ndungu, F. (2021, 18 enero). *Cómo utilizar el tipo de datos BLOB de MySQL para almacenar imágenes con PHP en Ubuntu 18.04*. DigitalOcean. Recuperado 17 de noviembre de 2021, de https://www.digitalocean.com/community/tutorials/how-to-use-the-mysql-blob-data-type-to-store-images-with-php-on-ubuntu-18-04-es
