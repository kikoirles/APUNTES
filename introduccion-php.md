---
title: "INTRODUCCION PHP"
tags: ""
---

# Conceptos Básicos de PHP

## Introducción

PHP es un lenguaje de script que suele utilizarse de forma embebida dentro de un fichero con extensión .php que puede contener tambien código HTML. Estas páginas PHP serán procesadas en el servidor y servidas como ficheros HTML al cliente.

Para que se interprete el código en php de una página se pueden utilizar la siguientes etiquetas.

```php
<?php
	echo 'Hola Mundo';
?>
```

 En el caso anterior se mostrará un mensaje un párrafo con el valor Hola Mundo dentro de la página web HTML

En php se permite utilizar múltiples bloques dentro de un mismo fichero con la apertura de diferentes etiquetas *<?php*

```php
<?php
	if ($expression) {
?>
		<strong>This is true.</strong> 
<?php
	} else { 
?>
		<strong>This is false.</strong> 
<?php
	} ?>
```

Este ejemplo realiza lo esperado, ya que cuando PHP encuentra las etiquetas **?>** de fin de bloque, empieza a escribir lo que encuentra tal cual hasta que encuentra otra etiqueta de inicio de bloque. El ejemplo anterior es, por supuesto, inventado. Para escribir bloques grandes de texto generamente es más eficiente separalos del código PHP que enviar todo el texto mediante las funciones **echo()**, **print()** o similares.

## Comentarios

PHP propone una estructura de comentarios similar a la utilizada por **C**,**C++** y **UNIX**. Un comentario es una línea en PHP utilizada para clarificar cualquier aspecto del código PHP. En PHP son soportados los comentario multilínea o de una línea simple.

```php
<?php
	echo "Esto es una prueba"; // Comentario estilo c++ 
/* Comentario multi-linea
	con varias lineas de comentario */
echo "Otra prueba";
echo "Prueba final"; # Comentario estilo shell de Unix
?>
```

Los estilos de comentarios de una línea (es decir, // y #) actualmente sólo comentan hasta el final de la línea o el bloque actual de código PHP, lo primero que ocurra.

```php
<h1>Esto es un <?php # echo "simple";?> ejemplo.</h1> <p>La cabecera de arriba dice 'Esto es un ejemplo.'.
```

## Variables

En PHP las variables se representan como un signo de dólar seguido por el nombre de la variable. El nombre de la variable es sensible a minúsculas y mayúsculas. Los nombres de variables siguen las mismas reglas que otras etiquetas en PHP. Un nombre de variable valido tiene que empezar con una letra o una raya (underscore), seguido de cualquier número de letras, números y rayas. 

```php
<?php
$var = "IES";
$Var = "Paco Molla";
echo "$var, $Var"; //Imprime IES Paco Molla
$4site = 'sin asignar'; //Es incorrecto al comenzar en nombre de la variable con un número
$_4site = ''; //Valido, comienza con un subrayado.
?>
```

En PHP, las variables siempre se asignan por valor. Esto significa que cuando se asigna una expresión a una variable, el valor íntegro de la expresión original se copia en la variable de destino. Esto quiere decir que, por ejemplo, después de asignar el valor de una variable a otra, los cambios que se efectúen a una de esas variables no afectará a la otra.

PHP ofrece otra forma de asignar valores a las variables: *asignar por referencia*. Esto significa que la nueva variable simplemente referencia (en otras palabras, "se convierte en un alias de" ó "apunta a") la variable original. Los cambios a la nueva variable afectan a la original, y viceversa. Esto también significa que no se produce una copia de valores; por tanto, la asignación ocurre más rápidamente. De cualquier forma, cualquier incremento de velocidad se notará sólo en los bucles críticos cuando se asignen grandes matrices u objetos.

Para asignar por referencia, simplemente se antepone un signo ampersand "&" al comienzo de la variable cuyo valor se está asignando (la variable fuente). Por ejemplo, el siguiente trozo de código produce la salida 'Mi nombre es Bob' dos veces:

```php
 <?php
	$nombre1 = 'IES'; // Asigna el valor 'IES' a $nombre1
	$nombre2 = &$nombre1; // Referencia $nombre1 vía $nombre2.
	$nombre2 = "Mi nombre es $nombre2"; // Modifica $nombre2
	echo $nombre1; // $nombre1 también se modifica.
	echo $nombre2; 
?>
```

No es necesario inicializar variables en PHP, sin embargo, es una muy buena práctica. Las variables no inicializadas tienen un valor predeterminado de acuerdo a su tipo - FALSE, cero, una cadena vacía o una matriz vacía. La función **isset()** se puede usar para detectar si una variable ya ha sido inicializada.

## Ambito de las variables

El ámbito de una variable es el contexto dentro del que la variable está definida. La mayor parte de las variables PHP sólo tienen un ámbito simple. Este ámbito simple también abarca los ficheros incluidos y los requeridos.

```php
<?php
	$a = 1;
	include "fichero.inc"; 
?>
```

Aquí, la variable *$a* será visible dentro del script incluido fichero.php. De todas formas, dentro de las funciones definidas por el usuario aparece un ámbito local a la función. Cualquier variable que se use dentro de una función está, por defecto, limitada al ámbito local de la función. Por ejemplo:

```php
<?php
  $a = 1; /* variable global */
  function Test()
  {
      echo $a; /* referencia a una variable de ámbito local */
  }
  Test(); 
?>
```

Este script no produce salida alguna porque dentro de la función Test() se hace referencia a una variable local *$a* que en realidad no está definida dentro de ese contexto. Podemos acceder desde las funciones a las variables globales con el modificador **global** o a través de un array denominado **$GLOBALS**

```php
<?php 
  $a = 1; 
	$b = 2;
  function Sum(){
    global $a, $b;
    $b = $a + $b; 
  }
  Sum();
  echo $b;

	function Res(){
    $GLOBALS["b"] = $GLOBALS["a"] - $GLOBALS["b"];
  }
	Res();
	echo $b;
?>
```

## Constantes

Una constante es un identificador para expresar un valor simple. Como el nombre sugiere, este valor no puede variar durante la ejecución del script. (Las constantes especiales **__FILE__** y **__LINE__** son una excepción a esto, ya que actualmente no lo son). Una constante es sensible a mayúsculas por defecto. Por convención, los identificadores de constantes suelen declararse en mayúsculas.

Un nombre de constante válido empieza con una letra o un caracter de subrayado, seguido por cualquier número de letras, números, o subrayados. El alcance de una constante es global, es decir, es posible acceder a ellas sin preocuparse por el ámbito de alcance.

* Las constantes no van precedidas por un símbolo de dolar ($)

- Las contantes solo pueden ser definidas usando la función **define()** , nunca por

  simple asignación

- Las constantes pueden ser definidas y se accede a ellas sin tener en cuenta las

  reglas de alcance del ámbito.

- Las constantes no pueden ser redefinidas o eliminadas después de establecerse; y

- Las constantes solo puede albergar valores escalares

- Se puede utilizar la función **constant()**, para obtener el valor de una constante

```php
<?php
  define("CONSTANT", "Hola");
  echo CONSTANT; // Imprime "Hola"
  echo Constant; // Imprime "Constant" e imprime una alerta. 
	echo MAXSIZE;
  echo constant("MAXSIZE"); // Imprime lo mismo que la anterior 
?>
```

## Tipos de datos

El tipo de una variable normalmente no lo indica el programador; en su lugar, lo decide PHP en tiempo de ejecución dependiendo del contexto en el que se utilice esa variable.

### Booleanos

Este es el tipo más simple. Un booleano expresa un valor de verdad. Puede ser **TRUE** o **FALSE**. Para especificar un literal booleano, use alguna de las palabras clave **TRUE** o **FALSE**. Ambas son insensibles a mayúsculas y minúsculas.

```php
$var1 = True; // asigna el valor TRUE a $var1
```

### Enteros

Los enteros se pueden especificar usando una de las siguientes sintaxis:

```php
$a = 1234; # número decimal
$a = -123; # un número negativo
$a = 0123; # número octal (equivalente al 83 decimal)
$a = 0x12; # número hexadecimal (equivalente al 18 decimal)
```

### Números en punto flotante

Los números en punto flotante (double) se pueden especificar utilizando cualquiera de las siguientes sintaxis:

```php
$a = 1.234; 
$a = 1.2e3; 
$a = 7E-10;
```

### Cadenas

Las cadenas de caracteres se pueden especificar usando uno de dos tipos de delimitadores.

Si la cadena está encerrada entre dobles comillas ("), las variables que estén dentro de la cadena serán expandidas (sujetas a ciertas limitaciones de interpretación). el carácter de barra invertida ("\") se puede usar para especificar caracteres especiales:

```php
$a = "Hola mundo $a \n";
```

La segunda forma de delimitar una cadena de caracteres usa el carácter de comilla simple (‘). Cuando una cadena va encerrada entre comillas simples, los únicos caracteres de escape que serán comprendidos son \\ y \'. Esto es por convenio, así que se pueden tener comillas simples y barras invertidas en una cadena entre comillas simples. Las variables *no* se expandirán dentro de una cadena entre comillas simples.

```php
$a = 'Hola mundo $a \n'; //Nada impide tener un \n, es solo por convenio, $a no imprime el valor de la variable
```

Las cadenas se pueden concatenar usando el operador **.** (punto)

```php
<?php
  /* Asignando una cadena. */ 
  $str = "Esto es una cadena";
  /* Añadiendo a la cadena. */
  $str = $str . " con algo más de texto";
  /* Otra forma de añadir, incluye un carácter de nueva línea protegido. */ 
  $str .= " Y un carácter de nueva línea al final.\n";
  /* Esta cadena terminará siendo '<p>Número: 9</p>' */ 
	$num = 9;
  $str = "<p>Número: $num</p>";
  /* Esta será '<p>Número: $num</p>' */ 
	$num = 9;
  $str = '<p>Número: $num</p>';
  /* Obtener el primer carácter de una cadena */ 
	$str = 'Esto es una prueba.';
  $first = $str[0];
  /* Obtener el último carácter de una cadena. */ 
	$str = 'Esto es aún una prueba.';
  $last = $str[strlen($str)-1];
?>
```

### NULL

El valor especial **NULL** representa una variable que no tiene valor. **NULL** es el único valor posible del tipo NULL. Una variable es considerada como null si:

* Se le ha asignado la constante **NULL**. 
* No ha sido definida con valor alguno. 
* Ha sido eliminada con **unset().**

Existe un solo valor de tipo null, y ese es la palabra clave **NULL**, insensible a mayúsculas y minúsculas.

```php
$var = NULL;
```

**Ejercicios**

- Declara una constante que se llame PI
- Declara una variable que vamos a llamar: numeroEntero
- En la siguiente instrucción (nueva línea en el script) asígnale el valor numérico: 1625
- Luego, en la siguiente instrucción (nueva línea) se deberá mostrar el valor de dicha variable en la página web con el siguiente texto: «El valor de &numeroEntero es …..»  (en los puntos suspensivos debe aparecer el valor 1625)
- Muestra por pantalla el resultado de la suma de el valor que contiene la variable numeroEntero con la constante PI
- Crea una variable que sea una referencia a la variable numeroEntero y en la siguiente instrucción asigna el valor 'un número'.
- Muestra el texto «El valor de &numeroEntero ahora es …..» 

## Operadores

Los operadores nos permiten realizar operaciones entre dos operandos que normalmente pueden ser variables y en algunos casos valores constantes. Existen varios grupos de operadores entre los que destacan, los operadores aritméticos, de asignación, de comparación, lógicos, de incremento y decremento, etc...

### Operadores aritméticos

Son aquellos operadores disponibles en todo lenguaje de programación que permiten realizar operaciones de este tipo

| **Operación** | **Nombre**     | **Resultado**                  |
| ------------- | -------------- | ------------------------------ |
| $a + $b       | Suma           | Suma de $a y $b.               |
| $a - $b       | Resta          | Diferencia entre $a y $b.      |
| $a * $b       | Multiplicación | Producto de $a y $b.           |
| $a / $b       | División       | Cociente de $a y $b.           |
| $a % $b       | Módulo         | Resto de la operación $a / $b. |
| -$a           | Negación       | El opuesto de $a               |

### Operadores de asignación

El operador básico de asignación es "**=**". A primera vista podrías pensar que es el operador de comparación "igual que". Pero no. Realmente significa que el operando de la izquierda toma el valor de la expresión a la derecha, (esto es, "toma el valor de").

El valor de una expresión de asignación es el propio valor asignado. Esto es, el valor de "$a = 3" es 3. Esto permite hacer cosas curiosas como

```php
$a = ($b = 4) + 5; // ahora $a es igual a 9, y $b vale 4.
```

Además del operador básico de asignación, existen los "operadores combinados" para todas las operaciones aritméticas y de cadenas que sean binarias. Este operador combinado permite, de una sola vez, usar una variable en una expresión y luego establecer el valor de esa variable al resultado de la expresión. Por ejemplo:

```php
$a = 3;
$a += 5; // establece $a a 8, como si hubiésemos escrito: $a = $a + 5;
$b = "Hola ";
$b .= "Ahí!"; // establece $b a "Hola Ahí!", igual que si hiciésemos $b = $b . "Ahí!";
```

Los nuevos signos de operación-asignación son:

```php
+= -= *= /= %= &= ^= .= >>= y <<=
```

### Operadores de comparación

Estos operadores permiten realizar la comparación de dos valores y son principalmente utilizados para la evaluación dentro de las estructuras de control.

| **Operación** | **Nombre**        | **Resultado**                                                |
| ------------- | ----------------- | ------------------------------------------------------------ |
| $a == $b      | Igualdad          | Compara si el valor de los dos operandos es el mismo.        |
| d$a === $b    | Identidad         | Compara si el valor es el mismo y, además, el tipo coincide  |
| $a != $b      | No igual          | Cierto si el valor de $a no es igual al de $b.               |
| $a !== $b     | No idéntico       | Cierto si $a no es igual a $b, o si no tienen el mismo tipo. |
| $a < $b       | Menor que         | Cierto si $a es estrictamente menor que $b.                  |
| $a > $b       | Mayor que         | Cierto si $a es estrictamente mayor que $b.                  |
| $a <= $b      | Menor o igual que | Cierto si $a es menor o igual que $b.                        |
| $a >= $b      | Mayor o igual que | Cierto si $a es mayor o igual que $b.                        |

Otro operador condicional es el operador "**?:**" (o ternario), que funciona como en C y otros muchos lenguajes.

```php
(expr1) ? (expr2) : (expr3);
```

La expresión toma el valor *expr2* si *expr1* se evalúa a cierto, y *expr3* si *expr1* se evalúa a falso.

```php
$cad = $a > $b ? “a es mayor que b” : “a no es mayor que b”;
```

### Operadores de Incremento/decremento

PHP soporta los operadores de pre y post decremento y incremento al estilo de C. Estos operadores no afectan a valores booléanos y incrementar NULL resulta en 1.

| **Operación** | **Nombre**      | **Resultado**                                                |
| ------------- | --------------- | ------------------------------------------------------------ |
| ++$a          | Pre-incremento  | Incrementa $a en 1, y devuelve $a (ya incrementado)          |
| $a++          | Post-incremento | Devuelve $a (sin incrementar), y después lo incrementa en 1. |
| --$a          | Pre-decremento  | Decrementa $a en 1, y después lo devuelve.                   |
| $a--          | Post-decremento | Devuelve $a, y después lo incrementa en 1.                   |

Un ejemplo del uso de estos operadores podría ser el siguiente:

```php
<?php
  echo "<h3>Postincremento</h3>";
  $a = 5;
  echo "Debería ser 5: " . $a++ . "<br>\n"; echo "Debería ser 6: " . $a . "<br>\n";
  echo "<h3>Preincremento</h3>";
  $a = 5;
  echo "Debería ser 6: " . ++$a . "<br>\n"; echo "Debería ser 6: " . $a . "<br>\n";
  echo "<h3>Postdecremento</h3>";
  $a = 5;
  echo "Debería ser 5: " . $a-- . "<br>\n"; echo "Debería ser 4: " . $a . "<br>\n";
  echo "<h3>Predecremento</h3>";
  $a = 5;
  echo "Debería ser 4: " . --$a . "<br>\n"; echo "Debería ser 4: " . $a . "<br>\n"; 
?>
```

### Operadores lógicos

Los operadores lógicos realizan operaciones dependiendo del valor booleano de los operandos.

| **Operación** | **Nombre**   | **Resultado**                               |
| ------------- | ------------ | ------------------------------------------- |
| $a and $b     | Y            | Cierto si $a y $b son ciertos.              |
| $a or $b      | O            | Cierto si $a o $b es cierto.                |
| $a xor $b     | O Exclusivo. | Cierto si $a o $b es cierto, pero no ambos. |
| ! $a          | No           | Cierto si $a es falso.                      |
| $a && $b      | Y            | Cierto si $a y $b son ciertos.              |
| $a \|\| $b    | O            | Cierto si $a o $b es cierto.                |

La razón de que haya dos operadores distintos para las operaciones *Y* y *O* lógicas es que tienen distinta precedencia.

### Operador de cadenas de texto

Para operar con cadenas sólo disponemos de un operador: la concatenación de cadenas representada por el punto ‘**.**’.Por ejemplo:

```php
$a = 1;
$b = 2;
$c = “El resultado de “ . $a . “ + “ . $b . “ es “ . ($a + $b);
```

Que dejaría en *$c* la cadena “*El resultado de 1 + 2 es 3*”. Antes de cada concatenación se realizarán las conversiones de tipo que fueran necesarias (en el ejemplo, los enteros se convierten a cadenas.)

### Precedencia y asociatividad de operandos

La precedencia de los operandos resuelve el orden en el que se evalúa una expresión múltiple que no ha sido delimitada con paréntesis. Por ejemplo, *1 + 5 \* 3* en PHP daría como resultado *1 + (5 \* 3) = 16* y no *(1 + 5) \* 3 = 18* ya que el producto tiene mayor precedencia que la suma.

La tabla muestra la asociatividad de los operandos en PHP, y está ordenada en orden decreciente de precedencia (los más prioritarios al inicio de la lista):

| Asociatividad | Operadores                                                   |
| :------------ | :----------------------------------------------------------- |
| No-Asociativo | `clone` `new`                                                |
| Derecha       | `**`                                                         |
| No-Asociativo | `++` `--` `~` `(int)` `(float)` `(string)` `(array)` `(object)` `(bool)``@` |
| Izquierda     | `instanceof`                                                 |
| No-Asociativo | `!`                                                          |
| Izquierda     | `*` `/` `%`                                                  |
| Izquierda     | `+` `-` `.`                                                  |
| Izquierda     | `<<` `>>`                                                    |
| No-Asociativo | `<` `<=` `>` `>=`                                            |
| No-Asociativo | `==` `!=` `===` `!==` `<>` `<=>`                             |
| Izquierda     | `&`                                                          |
| Izquierda     | `^`                                                          |
| Izquierda     | `|`                                                          |
| Izquierda     | `&&`                                                         |
| Izquierda     | `||`                                                         |
| Derecha       | `??`                                                         |
| Izquierda     | `? :`                                                        |
| Derecha       | `=` `+=` `-=` `*=` `**=` `/=` `.=` `%=` `&=` `|=` `^=` `<<=` `>>=` `??=` |
| No-Asociativo | `yield from`                                                 |
| No-Asociativo | `yield`                                                      |
| No-Asociativo | `print`                                                      |
| Izquierda     | `and`                                                        |
| Izquierda     | `xor`                                                        |
| Izquierda     | `or`                                                         |

La asociatividad de izquierda quiere decir que la expresión es evaluada desde la izquierda a la derecha, la asociatividad de derecha quiere decir lo contrario.

```php
<?php
  $a = 3 * 3 % 5; // (3 * 3) % 5 = 4
  $a = true ? 0 : true ? 1 : 2; // (true ? 0 : true) ? 1 : 2 = 2
  $a = 1;
  $b = 2;
  $a = $b += 3; // $a = ($b += 3) -> $a = 5, $b = 5 
?>
```

## Estructuras de Control

Para controlar la ejecución de una aplicación tenemos disponible dentro del lenguaje una serie de estructuras de que permiten definir en base a una serie de condiciones el flujo que seguirá la misma. Estas estructuras de control se deben utilizar de la mejor forma para que la legibilidad del programa sea fácil y no se estructure el mismo de tal forma que seguir el procedimiento de ejecución sea tarea casi imposible.

### If

Permite la ejecución condicional de fragmentos de código. Tiene la siguiente forma.

```php
 <?php
if (expr)
sentencia ?>
```

Como se describe en la sección sobre expresiones, *expr* se evalúa a su valor condicional (boolean). Si *expr* se evalúa como **TRUE**, PHP ejecutará la *sentencia*, y si se evalúa como **FALSE** - la ignorará. Se puede encontrar más información sobre los valores evaluados como **FALSE** en la sección sobre el tipo de datos booleano.

El siguiente ejemplo mostraría *a es mayor que b* si *$a* fuera mayor que *$b*:

```php
 <?php
  if ($a > $b){
    print "a es mayor que b";
  }
?>
```

## else

A menudo queremos ejecutar una sentencia si se cumple una cierta condicion, y una sentencia distinta si la condición no se cumple. Esto es para lo que sirve **else**. **else** extiende una sentencia **if** para ejecutar una sentencia en caso de que la expresión en la sentencia **if** se evalúe como **FALSE**. Por ejemplo, el siguiente código mostraría *a es mayor que b* si *$a* fuera mayor que *$b*, y *a NO es mayor que b* en cualquier otro caso:

```php
<?php
   if ($a > $b) {
   		print "a es mayor que b"; 
   }else{
    	print "a NO es mayor que b"; 
   }
?>
```

### elseif

Es una combinación de **if** y **else**. Como **else**, extiende una sentencia **if** para ejecutar una sentencia diferente en caso de que la expresión **if** original se evalúa como **FALSE**. No obstante, a diferencia de **else**, ejecutará esa expresión alternativa solamente si la expresión condicional **elseif** se evalúa como **TRUE**. Por ejemplo, el siguiente código mostraría *a es mayor que b*, *a es igual a b* o *a es menor que b*:

```php
<?php
 	if ($a > $b) {
  	print "a es mayor que b"; } 
	elseif ($a == $b) {
  	print "a es igual que b"; } 
	else {
  	print "a es mayor que b"; }
?>
```

### while

Los bucles **while** son los tipos de bucle más simples en PHP. La forma básica de una sentencia **while** es:

```php
while (expr) sentencia
```

El significado de una sentencia **while** es simple. Le dice a PHP que ejecute la(s) sentencia(s) anidada(s) repetidamente, mientras la expresión while se evalúe como **TRUE**. El valor de la expresión es comprobado cada vez al principio del bucle, así que incluso si este valor cambia durante la ejecución de la(s) sentencia(s) anidada(s), la ejecución no parará hasta el fin de la iteración (cada vez que PHP ejecuta las sentencias en el bucle es una iteración). A veces, si la expresión while se evalúa como **FALSE** desde el principio de todo, la(s) sentencia(s) anidada(s) no se ejecutarán ni siquiera una vez.

El siguiente ejemplo imprime números del 1 al 10:

```php
<?php 
  $i = 1;
	while ($i <= 10) {
			print $i++; /* el valor impreso sería $i antes del incremento (post-incremento) */
} 
?>
```

### do..while

Los bucles **do..while** son muy similares a los bucles while, excepto que las condiciones se comprueban al final de cada iteración en vez de al principio. La principal diferencia frente a los bucles regulares **while** es que se garantiza la ejecución de la primera iteración de un bucle **do..while** (la condición se comprueba sólo al final de la iteración), mientras que puede no ser necesariamente ejecutada con un bucle **while** regular (la condición se comprueba al principio de cada iteración, si esta se evalúa como **FALSE** desde el principio la ejecución del bucle finalizará inmediatamente).

Hay una sola sintaxis para los bucles **do..while**:

```php
 <?php
	$i = 0; 
	do {
     print $i;
 	} while ($i>0);
?>
```

El bucle de arriba se ejecutaría exactamente una sola vez, después de la primera iteración, cuando la condición se comprueba, se evalúa como **FALSE** (*$i no es más grande que 0*) y la ejecución del bucle finaliza.

## for

Los bucles **for** son los bucles más complejos en PHP. La sintaxis de un bucle **for** es:

```php
for (expr1; expr2; expr3) sentencia
```

La primera expresión (*expr1*) se evalúa (ejecuta) incondicionalmente una vez al principio del bucle.

Al comienzo de cada iteración, se evalúa *expr2* . Si se evalúa como **TRUE**, el bucle continúa y las sentencias anidadas se ejecutan. Si se evalúa como **FALSE**, la ejecución del bucle finaliza.

Al final de cada iteración, se evalúa (ejecuta) *expr3*.

Cada una de las expresiones puede estar vacía. Que *expr2* esté vacía significa que el bucle debería correr indefinidamente (PHP implicitamente lo considera como **TRUE**, al igual que C). Esto puede que no sea tan inútil como se podría pensar, puesto que a menudo se quiere salir de un bucle usando una sentencia break condicional en vez de usar la condición de for.

```php
<?php
 /* ejemplo 1 */
for ($i = 1; $i <= 10; $i++) { 
  print $i;
}
/* ejemplo 2 */
for ($i = 1; ;$i++) {
	if ($i > 10) {
    break;
	}
	print $i;
}
/* ejemplo 3 */
$i = 1; for (;;) {
     if ($i > 10) {
         break;
     }
     print $i;
     $i++;
}
 /* ejemplo 4 */
for ($i = 1; $i <= 10; print $i, $i++) ; 
?>
```

## switch

La sentencia **switch** es similar a una serie de sentencias **if** en la misma expresión. En muchas ocasiones, se quiere comparar la misma variable (o expresión) con nuchos valores diferentes, y ejecutar una parte de código distinta dependiendo de a qué valor es igual. Para ello sirve la sentencia **switch**.

```php
<?php
$i=2;
switch ($i) {
    case 0:
print "i es igual a 0<br>\n";
        break;
    case 1:
print "i es igual a 1<br>\n";
        break;
    case 2:
print "i es igual a 2<br>\n";
break; }
?>
```

Un caso especial es el **default** case. Este case coincide con todo lo que no coincidan los otros case. Por ejemplo:

```php
<?php
   switch ($i) {
  case 0:
  print "i es igual a 0"; break;
  case 1:
  print "i es igual a 1"; break;
  case 2:
  print "i es igual a 2"; break;
  default:
  print "i no es igual a 0, 1 o 2";
} 
?>
```

La expresión **case** puede ser cualquier expresión que se evalúe a un tipo simple, es decir, números enteros o de punto flotante y cadenas de texto. No se pueden usar aquí ni arrays ni objetos a menos que se conviertan a un tipo simple.
