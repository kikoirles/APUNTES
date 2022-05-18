---
title: "ACCESO A BASE DE DATOS PHP"
tags: ""
---

# Conceptos Básicos de Acceso a Bases de Datos en PHP

## Introducción

Todo lenguaje de programación suele proporcionar una serie de directivas, bien sea de forma oficial a través de una serie de funciones o mediante una librería que permiten acceder e interaccionar con bases de datos de muchos tipos. El lenguaje PHP soporta el acceso a diversas bases de datos, aunque la mayor parte de las aplicaciones web programadas en este lenguaje acceden a un servidor MariaDB/MySQL. En la documentación oficial del proyecto PHP se enumeran las siguientes bases de datos con las PHP puede interaccionar:

* [CUBRID](https://www.php.net/manual/en/book.cubrid.php)
* [DB++](https://www.php.net/manual/en/book.dbplus.php)
* [dBase](https://www.php.net/manual/en/book.dbase.php)
* [filePro](https://www.php.net/manual/en/book.filepro.php)
* [Firebird/InterBase](https://www.php.net/manual/en/book.ibase.php)
* [FrontBase](https://www.php.net/manual/en/book.fbsql.php)
* [IBM DB2](https://www.php.net/manual/en/book.ibm-db2.php)
* [Ingres](https://www.php.net/manual/en/book.ingres.php)
* [MongoDB](https://www.php.net/manual/en/set.mongodb.php)
* [MySQL](https://www.php.net/manual/en/set.mysqlinfo.php)
* [OCI8](https://www.php.net/manual/en/book.oci8.php)
* [Paradox](https://www.php.net/manual/en/book.paradox.php)
* [PostgreSQL](https://www.php.net/manual/en/book.pgsql.php)
* [SQLite3](https://www.php.net/manual/en/book.sqlite3.php)
* [SQLSRV](https://www.php.net/manual/en/book.sqlsrv.php)
* [tokyo_tyrant](https://www.php.net/manual/en/book.tokyo-tyrant.php)

Para poder interactuar con estas bases de datos, PHP utiliza librerías que pueden instalarse mediante extensiones o directamente compilando el código fuente de la librería que suele ser proporcionada por el fabricante o proyectos generalmente *open source* que permiten el acceos a las mismas.

## Definición de la base de datos de ejemplo

Para los siguientes ejemplos en los que se interactúas con la base de datos se utilizará la siguientes tablas que quedan definidas por el siguiente script SQL:

```sql
SET FOREIGN_KEY_CHECKS = 0;
DROP TABLE IF EXISTS equipo;
DROP TABLE IF EXISTS jugador;
DROP TABLE IF EXISTS lesion;
DROP TABLE IF EXISTS entrenador;
DROP TABLE IF EXISTS jugador_lesion;
SET FOREIGN_KEY_CHECKS = 1;

CREATE TABLE entrenador(
    codigo_ent INT(5) PRIMARY KEY,
    nombre VARCHAR(30),
    edad INT(3)
);

CREATE TABLE lesion(
    codigo_les INT(5) PRIMARY KEY,
    nombre VARCHAR(50),
    tiempo_rec INT(5)
);

CREATE TABLE equipo (
    codigo_eq INT(2) PRIMARY KEY,
    nombre_eq VARCHAR(30),
    ciudad VARCHAR(30),
    division VARCHAR(10),
    conferencia VARCHAR(5),
    codigo_ent_eq INT(5) UNIQUE REFERENCES entrenador(codigo_ent)
);

CREATE TABLE jugador (
    codigo_jug INT(5) PRIMARY KEY,
    nombre VARCHAR(30),
    posicion INT(2),
    peso DECIMAL(4,1),
    fecha_nac DATE,
    codigo_eq_jug INT(2) REFERENCES equipo(codigo_eq)
);


CREATE TABLE jugador_lesion(
    codigo_jug_jl INT(5) REFERENCES jugador(codigo_jug),
    codigo_les_jl INT(5) REFERENCES lesion(codigo_les),
    PRIMARY KEY (codigo_jug_jl,codigo_les_jl)
);

INSERT INTO entrenador VALUES(1,'Eric Spoelstra',50);
INSERT INTO entrenador VALUES(2,'Billy Donovan',55);
INSERT INTO entrenador VALUES(3,'Brad Stevens',44);
INSERT INTO entrenador VALUES(4,'Michael Malone',49);
INSERT INTO entrenador VALUES(5,'Gregg Popovich',71);
INSERT INTO entrenador VALUES(6,'Frank Vogel',47);
INSERT INTO entrenador VALUES(7,'Nate Bjorkgren',45);

INSERT INTO equipo VALUES (1,'Heat','Miami','Sureste','Este',1);
INSERT INTO equipo VALUES (2,'Bulls','Chicago','Central','Este',2);
INSERT INTO equipo VALUES (3,'Celtics','Boston','Atlántico','Este',3);
INSERT INTO equipo VALUES (4,'Nuggets','Denver','Noroeste','Oeste',4);
INSERT INTO equipo VALUES (5,'Spurs','San Antonio','Suroeste','Oeste',5);
INSERT INTO equipo VALUES (6,'Lakers','Los Ángeles','Pacífico','Oeste',6);

INSERT INTO jugador VALUES (1,'Jimmy Butler',2,220,STR_TO_DATE('1990/06/05','%Y/%m/%d'),1);
INSERT INTO jugador VALUES (2,'Jimmy Boston',3,250,STR_TO_DATE('1990/06/05','%Y/%m/%d'),1);
INSERT INTO jugador VALUES (3,'Tayler Herro',2,180,STR_TO_DATE('1990/06/05','%Y/%m/%d'),1);

```

## Acceso a MySQL/MariaDB desde PHP

Cuando queremos interaccionar con bases de datos MySQL/MariaDB desde PHP tenemos que realizar tres procesos:

1. Crear una conexión con la base de datos.
2. Ejecutar una instrucción SQL y obtener los resultados de ejecución de la misma.
3. Cerrar la conexión con la base de datos.

A continuación se muestra un ejemplo del proceso de conexión y desconexión de la base de datos, en ejemplos posteriores se mostrará la forma de interactuar con la base de datos dependiendo del tipo de operación realizada.

### Conexión a la base de datos.

Para realizar la conexión a la base de datos, se utiliza la función que proporciona la librería *msqli* y los parámatros que puede recibir la misma.

```php
$con=mysqli_connect('localhost', 'nombre_usuario', 'contraseña_usuario', 'esquema_al_que_conectar');
```

Hay que notar que la función *mysqli_connect* devolverá un identificador de la conexión si esta se produce de forma satisfactoria o el valor FALSE en caso de producirse algún error durante la conexión. De esta forma se puede realizar una comprobación que nos indique si la conexión se ha realizado de forma correcta con el siguiente código.

```php
<?php
  $con=mysqli_connect('localhost', 'miguel', 'leugim', 'iawe');
	if($con!=FALSE){
    echo "La conexión se ha realizado correctamente.";
  }else{
    echo "¡ERROR! No se ha podido conectar a la base de datos."
  }
?>
```

El identificador de conexión que obtenemos durante el establecimiento del canal con la base de datos, permitirá interactuar con la misma a través de las funciones que proporciona y en el ejemplo, estará asignado a la variable *$con*.

### Desconexión a la base de datos.

La operación contraria a la anterior se debe realizar para asegurarnos que la conexión con la base de datos se cierra de forma adecuada sin producir ningún error. Así, la librería *msqli* ofrece la función *close* que permite realizar el proceso de desconexión de la base de datos de forma correcta.

```php
$con=mysqli_connect('localhost', 'miguel', 'leugim', 'iawe'); //Proceso de conexión 
mysqli_close($con);
```

En el caso de la desconexión la comprobación previa de la variable donde reside el identificador de conexión es muy recomendable con la intención de evitar fallos que puedan comprometer la ejecución de la aplicación web, tal y cómo se muestra en el ejemplo siguiente.

```php
if ($con) //Si $con no se ha asigando correctamente contendrá FALSE y no realizará el cierre de la conexión.
  msqli_close($con);
```

Todo este procedimiento, aunque aconsejable, es opcional, puesto que PHP lo realiza de forma automática cuando es script de PHP termina su ejecución.

## CRUD (Create, Read, Update, Delete) con PHP

Son las operaciones básicas que se realizan cuando se interactua mediante una aplicación web escrita en PHP.

## Consulta de datos con PHP

Los datos insertados en la base de datos se pueden consultar embebiendo una consulta SQL dentro del código a través de una función que obtenemos de la interfaz msqli. Dentro de las operaciones básicas de interacción con la base de datos, está es la más compleja, pues retornará un conjunto de valores que posterioremente podrán ser tratados por el código de nuestra aplicación.

De esta forma la ejecución de una consulta SQL en PHP se realiza de forma muy similar a la siguiente manera:

```php
$con=mysqli_connect('localhost', 'miguel', 'leugim', 'iawe'); //Proceso de conexión 
$resultados = mysqli_query($nombreConexion, "SELECT * FROM jugador");
```

Como vemosm la variable result tendrá asignada un estructura que contenga todos los jugadores que hay almacenados en la base de datos y todos los campos que componen cada registro de un jugador. Para poder acceder a ellos se pueden utilizar las funciones que nos permiten recorrer colecciones tal y como se muestra en el siguiente ejemplo.

```php
while ($fila = mysql_fetch_array ($resultados))
   {
      echo ("<TR>");
      echo ("<TD>$fila[codigo_jug]</TD>\n");
      echo ("<TD>$fila[nombre]</TD>\n");
  		echo ("<TD>$fila[posicion]</TD>\n");
  		echo ("<TD>$fila[peso]</TD>\n");
  		echo ("<TD>$fila[fecha_nac]</TD>\n");
      echo ("</TR>");		
    }
    mysql_free_result($resultados);
```

Opcionalmente, se puede definir un segundo parámetro a la función *msqli_fetch_array* que permite establecer si el array que retorna está indexado [0],[1],[2]... o utiliza el nomnbre de las columnas para acceder a los mismos. Tal y como se muestra en el ejemplo.

```php
$fila = mysqli_fetch_array($resultados, MYSQLI_NUM);
echo ($fila[0]." ".$fila[1]);  //Acceso mediante posiciones

$fila = mysqli_fetch_array($resultados, MYSQLI_ASSOC);
echo ($fila["nombre"]." ".$fila["posicion"]); //Acceso mediante el nombre de las columnas 

$fila = mysqli_fetch_array($resultados, MYSQLI_BOTH);
echo ($fila[0], $fila["posicion"]);  //Acceso mediante ambos métodos.
```

## Inserción de datos con PHP

La ejecucuón de las operaciones que no devuelven un conjunto de resultados y solo el número de filas que se han modificado. En el caso de la inserción de datos podemos 

```php
$resultado = mysqli_query($nombreConexion, "INSERT INTO entrenador VALUES(8,'Jeff Van Gundy',65)");
```

Podemos comprobar que el resultado de la consulta se realice correctamente comprobando que la variable *$resultado* ha almacenado un valor TRUE.

```php
if ($resultado === TRUE) {
 echo "Se ha creado un nuevo registro";
} else {
 echo "Error en la inserción de documentos";
}
```

## Modificación de datos con PHP

En el caso de la modificación de datos el procedimiento es muy similar al mostrado anteriormente solamente cambiando la sentencia SQL a ejecutar.

```php
$resultado = mysqli_query($nombreConexion, "UPDATE FROM entrenador SET entrenador.edad=66 WHERE codigo_ent=8");
```

Podemos comprobar que el resultado de la consulta se realice correctamente comprobando que la variable *$resultado* ha almacenado un valor TRUE.

```php
if ($resultado === TRUE) {
 echo "Se ha modificado el registro";
} else {
 echo "Error en la actualización de registros";
}
```

## Borrado de datos con PHP

En el caso de la modificación de datos el procedimiento es muy similar al mostrado anteriormente solamente cambiando la sentencia SQL a ejecutar.

```php
$resultado = mysqli_query($nombreConexion, "DELETE FROM entrenador WHERE entrenador.codigo_ent=8");
```

Podemos comprobar que el resultado de la consulta se realice correctamente comprobando que la variable *$resultado* ha almacenado un valor TRUE.

```php
if ($resultado === TRUE) {
 echo "Se ha borrado el registro";
} else {
 echo "Error en el borrado de registros";
}
```

## Ejercicios

1. Genera un formulario para poder insertar y obteniendo los datos mediante el método *post* almacena los registros en la base de datos.
2. Realiza una función llamada *aumentarEdadCincuentones* que reciba por parámetro un número entero y modifique la edad, aumentando a la edad el valor recibido por parámetro  a aquellos entrenadores cuya edad que sea igual o mayor de 50 años.
3.  Crea un script PHP que permita eliminar los datos de los equipos almacenados en la base de datos que pertenezcan a la conferencia oeste. Existe algún problema al ejecutar esta sentencia y si es el caso como se podría solucionar.
4. Crea un script PHP que muestre en una tabla los datos de todos los jugadores que aparecen almacenados en la base de datos.
5. Crea una función que muestre por pantalla los jugadores que forman parte de la plantilla de los Miami Heat.
