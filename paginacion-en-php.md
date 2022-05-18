---
title: "PAGINACION EN PHP"
tags: ""
---

# Ejemplo de un paginador sencillo

Cuando realizamos las consultas que devuelven multitud de resultados, podemos realizar un cribado de los resultados para mostrarlos en forma de páginas con un enlace que nos permita movernos por cada una de las páginas que genere el conjunto de resultados.

A continuación, se explican paso por paso el algoritmo para realizar la paginación de los resultados de forma sencilla.

En primer lugar, obtenemos de la url que el número de página para obtener la página donde vamos a situarnos. Para ello utilizamos el array $_GET de donde obtendremos el parámetro con el que se nos envía la página.

* Comprobamos si existe la clave y por tanto el valor en que se nos pasará el numero de página.
* Utilizamos la función *array_key_exists* para comprobar que la clave *pagina* en este caso existe.
* Inicializamos la variable $pagina a un valor de 1.

```php
$pagina=1;
if(array_key_exists('pagina',$_GET)){
	$pagina = $_GET['pagina'];
}
```

Realizamos la cuenta del número total de resultados que queremos mostrar. Para ello ejecutamos una consulta con un count sobre la tabla a mostrar, en este caso *municipios* y obtenemos el total de los mismos.

* Utilizamos la función *mysqli_fetch_row* porque sabemos que la consulta ejecutada solamente devolverá un valor.

```php
$resultado = mysqli_query($conexion,"SELECT COUNT(*) FROM municipios");
$fila = mysqli_fetch_row($resultado);
$totalmunicipios = $fila[0];
```

El siguiente paso es calcular el número total de páginas que serán necesarias para mostrar todos los resultados.

* Utilizamos la función *intval* que nos devuelve el número entero de la división realizada.

```php
$numeroPaginas = ceil($totalmunicipios/50);
```

Cuando tenemos los resultados totales y el numero de páginas nos permitirá crear la consulta SQL que con la cláusula LIMIT nos permitirá ir obteniendo de la base de datos los resultados en segmentos.

```php
$cincuentena = mysqli_query($conexion,"SELECT nombre FROM municipios LIMIT ".(($pagina-1)*50).",50");
```

Después mostraremos los resultados obtenidos de la consulta y los mostraremos utilizando, por ejemplo, una estructura de tabla.

```php
echo '<table border="1px solid">';
        while ($fila = mysqli_fetch_array($cincuentena)){
            echo '<tr>';
                echo "<td>".$fila['nombre']."</td>";
            echo "</tr>";
        }
        echo "</table><br>";
```

Finalmente, situaremos en la parte inferior de la web los enlaces a las páginas que únicamente irán llamándose a si mismas cambiando el parámetro página definido en la URL. Crearemos un enlace para cada una de las páginas mediante un bucle for.

```php
for ($i=0;$i<$numeroPaginas;$i++){
	echo '<a href="./paginadorSencillo.php?pagina='.($i+1).'">'.($i+1).'</a> | ';
}
```

El código completo quedaría de la siguiente forma:

```php+HTML
<?php
    include_once ('./funciones.php');
?>
<html>
<head>
    <title>Un ejemplo de paginar resultados</title>
</head>
<body>
    <?php
        $pagina=1;
        if(array_key_exists('pagina',$_GET)){
            $pagina = $_GET['pagina'];
        }

        $conexion=conectarBD();
        $resultado = mysqli_query($conexion,"SELECT COUNT(*) FROM municipios");

        $fila = mysqli_fetch_row($resultado);
        $totalmunicipios = $fila[0];
        echo $totalmunicipios;

        $numeroPaginas = intval($totalmunicipios/50);

        $cincuentena = mysqli_query($conexion,"SELECT nombre FROM municipios LIMIT ".(($pagina-1)*50).",50");

        echo '<table border="1px solid">';
        while ($fila = mysqli_fetch_array($cincuentena)){
            echo '<tr>';
                echo "<td>".$fila['nombre']."</td>";
            echo "</tr>";
        }
        echo "</table><br>";

        for ($i=0;$i<$numeroPaginas;$i++){
            echo '<a href="./paginadorSencillo.php?pagina='.($i+1).'">'.($i+1).'</a> | ';
        }

    ?>
</body>
</html>

```
