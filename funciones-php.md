---
title: "FUNCIONES PHP"
tags: ""
---

# Funciones en PHP

## Introducción

Las funciones nos permiten reutilizar el código fuente que creamos para poder utilizarlo en múltiples ocasiones. Cuando se desarrolla un software debe encapsularse toda acción que sea susceptible de ser llamada en múltiples ocasiones dentro de una función que permita ser llamada y reutilizada.

Cuando se defina una función, se debe pensar de forma genérica e intentar que ese código sea reutilizable en la mayor cantidad de ocasiones. En ciertas ocasiones se puede modificar un poco el código de las funciones para incorporar mayor funcionalida y que por tanto, esta función pueda ser utilizada en mayor número de casos. 

La estructura de una función sencilla en PHP tiene una estructura similar a la siguiente:

```php
<?php
	function primera_funcion($argumento1, $argumento2,..., $argumentoN) {
		echo 'Hola mundo';
  	return $retval;
	}
?>
  
```

Si la función tiene que devolver un valor a la línea de código donde se invocó, como última instrucción hay que poner la orden *return* seguida del valor que se devuelve. Si no se devuelve un valor, sino que sólo se ejecutan instrucciones dentro del código de la propia función, puede prescindirse de la sentencia *return.*

En PHP existe la posibilidad de declarar funciones dentro de bulcles condicionales, y estas no podrán ser referenciadas a menos que se hayan procesado la difinición de la misma.

```php
<?php
$makefoo = true;
/* No se puede llamar a foo() desde aquí porque todavía no existe, pero
si se puede llamar a bar() */
bar();
if ($makefoo) {
  function foo ()
	{
		echo "No existo hasta que la ejecución del programa llegue hasta mi.\n";
	} }
/* Ahora se puede llamar con seguridad a foo() porque $makefoo se a evaluado a true */
if ($makefoo) foo();
function bar()
{
  echo "Existo desde que el programa comienza.\n";
}
?>
```

## Parámetros de las funciones

Una función puede recibir una serie de parámetros para poder trabajar con ellos. Estos parámetros pueden ser o constantes o variables, en el caso de estas últimas se pueden pasar por valor, realizando una copia de los mismo dentro del ámbito de la función o por referencia en las que es posible modificar el valor cuando que estas tienen antes de recibirlas la función.

### Parámetros pasados por valor 

Es el comportamiento por defecto del lenguaje PHP. Cuando una variable se pasa por valor a una función, dentro de la misma se trabaja con una copia de la misma que tendrá el valor que tendría la variable cuando se llamó a la función. En el siguiente ejemplo se muestra el funcionamiento de los parámetros pasados por valor.

```php
<?php
  $var1=1;
	$var2=2;
	
	sumardos($var1,$var2); //Llamada a la función sumardos

	echo "$var1"; //El valor que aparecería por pantalla es 1

	function sumardos ($variable1, $variable2){
    $variable1=$variable1+$variable2;
    echo "$variable1"; //El valor que aparecería por pantalla es 3
   }
	
?>
```

### Parámetros pasados por referencia

PHP también ofrece la posibilidad de pasar los parámetros por referencia, en este caso en lugar de realizar una copia del valor que tiene la variable cuando se realiza la llamada, la función recibe la posición de memoria donde la variable se encuentra almacenada. Esto, permite que aquellos cambios que se producen dentro de la función afecten a su valor fuera del ámbito de la misma. Cuando se desee pasar un parámetro por referencia se debe añadir el caracter *&* justo antes del nombre de la variable. En el siguiente ejemplo se muestra como realizar el paso por referencia de una variable en una función.

```php
<?php
  $var1=1;
	$var2=2;
	$varTotal = 0;
	
	sumardos($var1,$var2, &$varTotal); //Llamada a la función sumardos

	echo "$varTotal"; //El valor que aparecería por pantalla es 3

	function sumardos ($variable1, $variable2, &$variable3){
    $variable3=$variable1+$variable2;
    echo "$variable3"; //El valor que aparecería por pantalla es 3
   }
	
?>
```

### Parámetros por defecto

Otra de las opciones disponibles en PHP consiste en asignar parámetros que tienen un valor por defecto, en caso de no ser definidos cuando se realiza la llamada a la función. Cuando definimos una función que tiene parámetros establecidos por defecto, tienen que tenerse en cuenta un par de condiciones, por una parte, que el valor que establecemos tiene que ser una constante y por otra parte, que los valores establecidos por defecto tienen que definirse a la derecha de cualquier valor que no tiene un valor por defecto. Estas condiciones se ejemplifican en el siguiente código de ejemplo.

```php
<?php
  $nombre="Juan Manuel";
	$apellidos="Sánchez Morant";
	
	nombreCompleto($nombre,$apellidos);
	//Aparecería por pantalla el mensaje Juan Manuel Sanchez Morant

	nombreCompleto($nombre);
	//Aparecería por pantalla el mensaje Juan Manuel Sin apellidos

	function nombreCompleto ($variable1, $variable2="Sin apellidos"){ 
    //Valor por defecto constante y valor por defecto definido a la derecha del resto de valores.
    
    $nombreCompleto=$variable1." ".$variable2;
    echo "$nombreCompleto"; 
   }
?>
```

### Número de parámetros variable

Existe la posibilidad de definir un número de parámetros variable que permitirán a la función tratar con un número de parámetros cambiante en cada una de las llamadas. Para definir este número de parámetros variables se utilizan tres puntos antes de definir una variable que se comportará como un array. Este comportamiento también puede realizarse pasando un array como parámetro de la función. Como se muestra en el siguiente ejemplo:

```php
<?php
function productorio(...$números) {
    $total = 1;
    foreach ($números as $n) {
        $acc *= $n;
    }
    return $total;
}

echo sum(1, 2, 3, 4); //Mostraría 24 por pantalla.
?>
```

## Devolución de valores

Una función puede devolver los valores mediante una instrucción *return* a la instrucción que realizó la llamada. En la devolución tenemos cualquier tipo de valor, incluyendo listas y objetos. Así, en el siguiente ejemplo se devolvería el valor a la llamada donde se realizó la invocación.

```php
<?php
  $nombre="Juan Manuel";
	$apellidos="Sánchez Morant";
	
	echo nombreCompleto($nombre,$apellidos);
	//Aparecería por pantalla el mensaje Juan Manuel Sanchez Morant

	function nombreCompleto ($variable1, $variable2="Sin apellidos"){
    $nombreCompleto=$variable1." ".$variable2;
    return  $nombreCompleto; 
   }
?>
```

En este caso la invocación a la función *nombreCompleto* devolvería el valor del nombre y los apellidos concatenados en una cadena de caracteres que conformará la entrada de la función *echo* que lo mostrará por pantalla.

Existe la posibilidad de definir el tipo de valor a retornar por la función añadiendo el modificador *:* justo después de la definición de los parámetros, tal y como se muestra en el ejemplo siguiente.

```php
<?php
	function sum($a, $b): int {
    	return $a + $b;
	}
	echo sum(1, 2.5); //El valor mostrado por pantalla sería 3
?>
```

Cuando se quiera forzar a que no exista una conversión de tipos tal y como se muestra en el ejemplo anterior, se puede forzar a que los tipos de los datos sean estrictamente cumplidos tal y como se muestra en el siguiente ejemplo.

```php
<?php
  declare(strict_types=1);
	function sum($a, $b): int {
    	return $a + $b;
	}
	echo sum(1, 2.5); //PHP Fatal error:  Uncaught TypeError: Return value of sum() must be of the type int, float returned in
?>
```

## Ejercicios

1. Define un función que muestre todos los elementos de un array que se pase por parámetro. Nota: el array solamente tendrá una dimensión.

2. Crea una función que ponga la primera letra de cada palabra en mayúscula para una cadena de caracteres que reciba como parámetro.

3. Crea una función que recibiendo un número variable de parámetros numéricos calcule la media aritmética de los mismos.

4. Crea un función que pasados dos arrays los una en una matriz de dos diménsiones que se le pase como un tercer parámetro por referencia.

5. Crear una función que pasado un IBAN (Número de cuenta corriente) por parámetro devuelva en una array con las claves *pais, ccontrol, cbanco, csucursal, ccontrol, ccuenta*. Además realiza dentro de la función la comprobación de que el número IBAN es correcto.
