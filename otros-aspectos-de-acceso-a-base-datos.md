---
title: "OTROS ASPECTOS DE ACCESO A BASE DATOS"
tags: ""
---

Implantación de Aplicaciones WebAdministración de Sistemas Informáticos y en Red
Aspectos avanzados del acceso a datos en PHP
Miguel Angel Tomás Amat
•
2 nov (Última modificación: 3 nov)
OtrosAspectosDelAccesoDatos.md
Texto
EjemploPaginadorSencillo.md
Texto
Comentarios de la clase

# Aspectos adicionales del acceso a datos en PHP

## Introducción

La mayor parte de las operaciones realizadas contra la base de datos utilizando el lenguaje PHP son las operaciones CRUD, además de los procesos de conexión y desconexión de la misma. Adicionalmente, existen procesos que mejoran, aseguran o permiten aumentar la efectividad del trabajo con la base de datos. A continuación, se presentan algunas mejoras de estos procesos para intentar cubrir las distintas posibilidades que pueden aparecer en este aspecto durante el desarrollo de una aplicación web.

## Sentencias preparadas con parámetros

Aunque existen diferentes modos de ejecutar consultas con parámetros, componiendo las sentencias sql a través de los parámetros obtenidos desde, por ejemplo, un formulario, PHP ofrece funciones que permiten crear las sentencias sql con un añadido de seguridad que será interesante de cara a evitar posibles errores. 

Este proceso mejora la eficiencia de la consultas, al poder analizarlas previamente a ser enlazadas y ejecutadas por parte del SGBD. Este forma de trabajo tiene las siguientes ventajas:

1. Aunque la sentencia pueda ser ejecutada en múltiples ocasiones con parámetros distintos, el proceso de optimización solo se realiza una sola vez.
2. Los parámetros enlazados de este modo permiten disminuir el tamaño de las conexiones realizadas sobre la base de datos, al pasar cada vez que tenemos que ejecutar sólo estos en lugar de toda la sentencia completa. 
3. Previene injecciones SQL, al transmitir los parámetros mediante un protocolo diferenciado. Este método de trabajo previene la ejecución de inyecciones SQL.

La ejecución de sentencias con parámatros modifica los pasos a realizar, teniendo que definir la sentencia con una serie de caracteres que representan los parámetros, preparar la sentencia y hacer la sustitución de los caracteres especiales por los parámetros que tendrá la sentencia. 

Durante la sustitución de los parámetros de la sentencia tenemos que especificar el tipo de datos por el que vamos a realizar la sustitución mediante la función *mysqli_stmt_bind_param*. Esta definición de parámetros utiliza los siguientes modificadores para definirse.

- i - entero (*integer*)
- d - real de doble precisión (*double*)
- s - cadena de caracteres (*string*)
- b - Objetos binarios BLOB (*Binary Large Object*)

Para realizar esta conversión la función bind recibe un parámetro como cadena de caracteres que especifica los tipos de datos por posición. Por ejemplo, la cadena "sis", definiría que el primer parámetro de la sentencia SQL es un *string*, el segundo un *integer* y el tercero otro *string*. Esta definición de parámetro mejora la seguridad de las sentencias ejecutadas sobre la base de datos no permitiendo inyecciones SQL.

```php
mysqli_stmt_bind_param($sentenciaPreparada, "isi", $cod_entrenador,$nom_entrenador,$edad_entrenador);
```

En el ejemplos siguiente se puede ver todo el procedimiento incluyendo la conexión a la base de datos y la ejecución de la sentencia SQL.

```php
//Parámetros a sustituir
  $cod_entrenador = 15;
  $nom_entrenador = "Jeff Van Gundy";
  $edad_entrenador = 59;
//Conexión con la base de datos
  $con = msqli_connection('mariadb','miguel','leugim','iawe');
  if (!$con){
    echo "ERROR. No se ha podido realizar la conexión con la base de datos.";
  }else{
    //Definimos la sentencia que queremos ejecutar en la base de datos
    $sentenciaSQL = "INSERT INTO entrenador VALUES (?,?,?)";
    
    //La sentencia que queremos ejecutar se optimiza y almacena en la base de datos
    $sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
    
    //Se sustituyen los parámetros en la consulta
    mysqli_stmt_bind_param($sentenciaPreparada, "isi", $cod_entrenador,$nom_entrenador,$edad_entrenador);
    
    //Se realiza la ejecución de la consulta
    $resultado = mysqli_stmt_execute($sentenciaPreparada);
      
    if ($resultado){
      echo "La sentencia se ha ejecutado correctamente.";
    }else{
      echo "ERROR. No se ha podido insertar el registro en la base de datos.";
    }
    
    //Cierre de elementos
    mysqli_stmt_close($sentenciaPreparada);
    mysqli_close($con);
  }

```

## Funciones adicionales de sentencias preparadas.

* **mysqli_stmt_affected_rows**. Devuelve el número total de filas cambiadas, borradas, o insertadas por la última sentencia ejecutada.

```php
$sentenciaSQL = "INSERT INTO entrenador VALUES (?,?,?)";
$sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
mysqli_stmt_bind_param($sentenciaPreparada, "isi", $cod_entrenador,$nom_entrenador,$edad_entrenador);
echo "filas insertadas: ". mysqli_stmt_affected_rows($sentenciaPreparada));
```

* **mysqli_stmt_data_seek**. Busca una fila arbitraria en un conjunto de resultados de una sentencia.

```php
$sentenciaSQL = "SELECT * FROM jugador WHERE posicion = ?";
$sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
mysqli_stmt_bind_param($sentenciaPreparada, "i", 1);
mysqli_stmt_store_result($sentenciaPreparada);
mysqli_stmt_data_seek($sentenciaPreparada, 9); //Busca el resultado 10
mysqli_stmt_fetch($sentencia); //Obtenemos los valores
echo "El registro 10 es : ". mysqli_stmt_fetch($sentenciaPreparada);
```

* **mysqli_stmt_error**. Devuelve una descripción en forma de string del último error de una sentencia.

```php
$sentenciaSQL = "DELETE TABLE jugador "; //Claves ajenas apuntan a esta tabla por lo tanto dará ERROR.
$sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
mysqli_stmt_execute($sentenciaPreparada);
echo "Error: ". mysqli_stmt_error($sentenciaPreparada));
```

* **mysqli_stmt_fetch**. Obtiene los resultados de una sentencia preparadas en las variables vinculadas.

```php
$sentenciaSQL = "SELECT nombre,posicion FROM jugador WHERE posicion = ?";
$sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
mysqli_stmt_bind_param($sentenciaPreparada, "i", 1);
mysqli_stmt_execute($sentenciaPreparada);
mysqli_stmt_bind_result($sentenciaPreparada, $nombre, $poscion); //Vinculamos los resultados con las variables
while (mysqli_stmt_fetch($sentenciaPreparada)) {
  echo $nombre. $posicion;
}
```

* **mysqli_stmt_num_rows**. Devuelve el número de filas de un conjunto de resultados de una sentencia.

```php
$sentenciaSQL = "SELECT * FROM jugador WHERE posicion = ?";
$sentenciaPreparada = mysqli_prepare($con,$sentenciaSQL);
mysqli_stmt_bind_param($sentenciaPreparada, "i", 1);
mysqli_stmt_store_result($sentenciaPreparada);
echo "Número de filas: ". mysqli_stmt_num_rows($sentencia));
```

## Funciones de acceso a los resultados de las consultas.

* **mysqli_num_rows**. Obtiene el número de filas de un resultado.

```php
$resultado = mysqli_query($con, "SELECT * FROM jugador WHERE posicion = 1"));
$numeroFilas = mysqli_num_rows($resultado); //Obtenemos el número de filas que devulve el resultado
echo "El resultado tiene $row_cnt filas.";
```

* **mysqli_num_fields**. Obtiene el número de campos de un resultado.

```php
$resultado = mysqli_query($con, "SELECT * FROM entrenador"));
$numeroCampos = mysqli_num_fields($resultado); //Obtenemos el número de filas que devulve el resultado
echo "La consulta devuelve $numeroCampos para cada entrenador."; //codigo, nombre, edad serían los campos.
```

* **mysqli_fetch_assoc**. Obtener una fila de resultado como un array asociativo.

```php
$resultado = mysqli_query($con, "SELECT * FROM entrenador"));
while ($fila = mysqli_fetch_assoc($resultado)) {
	echo $fila["codigo"]." ".$fila["nombre"]." ".$fila["edad"];
}
```

* **mysqli_fetch_row**. Obtener una fila de resultados como un array enumerado.

```php
$resultado = mysqli_query($con, "SELECT * FROM entrenador")) {
while ($fila = mysqli_fetch_assoc($resultado)) {
	echo $fila[0]." ".$fila[1]." ".$fila[2];
}
```

* **mysqli_data_seek**. Ajustar el puntero de resultado a una fila arbitraria del resultado

```php
$resultado = mysqli_query($con, "SELECT * FROM entrenador")) {
mysqli_data_seek($resultado, 9); //Busca el resultado 10
$fila = mysqli_fetch_row($resultado); //Obtenemos los valores
echo "El registro 10 es : ". $fila[0]." ".$fila[1]." ".$fila[2];
```

## Ejecución de múltiples sentencias SQL

En ocasiones, sobre todo en procesos de inicialización, puede ser adecuado utilizar múltiples consultas y ejecutarlas todas de una vez en lugar de ejecutarlas una por una. Esta forma de ejecutar scripts SQL es muy interesante para inicializar una base de datos o si la aplicación web va ha instalarse en algún servidor al que no tengamos acceso a la base de datos de forma directa.

La librería *mysqli* ofrece el método *mysqli_multi_query* que permite ejecutar más de una sentencia SQL con una sola instrucción PHP. En este caso también se ha de considerar que cada una de las instrucciones SQL que se ejecuten devolverá un resultado y se necesitan métodos que permitan el acceso a los mismos. A continuación, se muestra un ejemplo de la utilización de *mysqli_multi_query*.

```php
<?php
$con = mysqli_connect("mariadb", "miguel", "leugim", "iawe");

if (!$con){
    echo "ERROR. No se ha podido realizar la conexión con la base de datos.";
  }else{
	$query  = "SELECT nombre FROM jugador;"; //Las sengtencias deben terminar con ; para que se ejecuten separadas
	$query .= "SELECT tiempo_rec FROM lesion"; //El operador .= concatena a query el valor derecho

//Ejecución de una consulta múltiple
if (mysqli_multi_query($con, $query)) {
    do {
        //Se guarda el primer conjunto de resultados
        if ($resultadoMulti = mysqli_store_result($con)) {
            while ($fila = mysqli_fetch_row($resultadoMulti)) {
                echo $fila[0];
            }
            mysqli_free_result($resultadoMulti); //Eliminamos el conjunto de resultados que ya ha sido leído
        }
        //Se inserta un salto de línea para separar los resultados
        if (mysqli_more_results($con)) { //Se comprueba si existen más resultados
            echo("<br />");
        }
    } while (mysqli_next_result($con)); //Se pasa al siguiente conjunto de resultados
}

//Cierre de la conexión
mysqli_close($con);
?>
```

Hay que notar que los resultados se almacenan por una parte para cada una de las sentencias que es ejecutada, y los resultados de esta pueden contener múltiples campos devueltos. En este ejemplo se utilizan funciones de apoyo que permiten manejar el conjunto de resultados obtenidos por una consulta.

* **mysqli_store_result**. Devuelve el conjunto de resultados de la última consulta, sea simple o múltiple.
* **mysqli_fetch_row**. Devuelve una fila de resultados como un array indexado.
* **mysqli_free_result**. Libera la memoria que almacena el último resultado.
* **mysqli_more_results**. Comprueba si hay más resultados en una consulta múltiple.
* **mysqli_next_result**. Prepara el siguiente resultado de una consulta múltiple.

## Limitar los resultados de una consulta con MySQL/MariaDB

Cuando persistimos datos en una base de datos MySQL/MariaDB podemos mediante la realización de consultas SQL utilizar la palabra reservada *LIMIT* para obtener un subconjunto de todos los resultados posibles. En las páginas escritas en PHP este factor de eficiencia es muy importante, pues, el procesamiento de muchos resultados o la obtención de muchos resultados realizando una conexión para cada uno de los registros que lo componen puede suponer una sobrecarga del servidor.

Este tipo de consultas facilita la creación de listados multipágina y en tablas con muchos registros es la forma adecuada de realizar las consultas. A continuación, se muestran ejemplos de la utilización de este elementop en MySQL/MariaDB.

```php
$sql = "SELECT nombre, apellidos, posicion FROM jugador LIMIT 30";
```

Con la sentencia anterior obtendríamos los primeros 30 resultados que devolviera la consulta.

Si quesieramos obtener un rango de valores podríamos utilizar el modificador *OFFSET* para seleccionar los registros según su posición que queramos seleccionar.

```php
$sql = "SELECT nombre, apellidos, posicion FROM jugador LIMIT 5 OFFSET 30";
```

Con la sentencia anterior obtendríamos 5 registros a partir del registro 30, este último sin incluir, es decir, los registros 31,32,33,34,35.

OtrosAspectosDelAccesoDatos.md
Mostrando EjemploPaginadorSencillo.md.
