---
title: "ARRAYS PHP"
tags: ""
---

# Uso de Arrays en PHP

## Introducción

Un array es una estructura de datos que permite almacenar diferentes objetos de una misma clase. Dentro de estos, se puede acceder a los mismos de forma general mediante un índice que marca la posición del elemento dentro del array. Por norma general en los lenguajes derivados de C este índice suele empezar en 0 y, el primer elemento ocupará esta posición. Por tanto, para acceder a la posición *n* de un array tendremos que referirnos a la posición *n-1* en una notación entre corchetes.

```php
echo "El 5º elemento del array sera: ".$mi_array[4];
```

## Inicialización de Arrays

Los arrays se pueden crean como las variables en PHP. En muchas ocasiones, estas estructuras se suelen inicializar con una serie de valores que luego se recorrerán para mostrarlos o utilizarlos en alguna parte del código. Como PHP es un lenguaje donde las variables no están fuertemente tipadas, se creará el array cuando se inicialice con una serie de valores.

La típica definición e inicialización de un array en PHP sería como en el ejemplo siguiente:

```php
$posiciones = ['base', 'escolta', 'alero','ala-pivot','pivot'];
```

También se pueden definir los arrays haciendo uso de la función *array()* propia del lenguaje PHP. En el ejemplo siguiente se utiliza esta definición que produciría el mismo resultado que en ejemplo anterior.

```php
$posiciones = array("base","escolta", "alero","ala-pivot","pivot");
```

Si queremos inicializar una variable que se defina por defecto como un array pero que no tenga valores asignados, podemos utilizar la siguiente instrucción. Hay que especificar que este array contendrá las posiciones de la 0 a la 4. 

```php
$posiciones = array(5);
```

Igualmente, si se define el array sin ningún número de posiciones asignadas, éste se definirá como un array vacío.

```php
$posiciones = array();
```

Los arrays pueden contener cualquier tipo de elementos del lenguaje, desde elementos de texto, elementos numéricos, objetos, etc.

```php
$posiciones = [1,2,3,4,5];
```

## Arrays asociativos

Dentro de PHP se conocen como arrays asociativos, aquellas estructuras para las que se define una clave y un valor. En estos arrays no es posible realizar un acceso por posición y tendrá que especificarse la clave para poder obtener el valor. De esta forma podemos crear un array que contenga asociativo de la siguiente forma:

```php
$posiciones = ["base"=>"1", "escolta"=>"2","alero"=>"3","ala-pivot"=>"4","pivot"=>"5"];
```

Los valores que guarda este array serían los valores numéricos 1,2,3,4,5. Por tanto, las claves que permiten acceder al valor serían respectivamente base,escolta, alero, ala-pivot, pivot.

## Arrays multidimensionales

Los arrays pueden definirse de forma recursiva, donde un array contiene en su declaración otro array en su interior. Este tipo de arrays denominados arrays multidimensionales o matrices. Este tipo de arrays se suele ver como una tabla si tiene dos dimensiones aunque puede tener más dimensiones si fuera necesario. Se pueden definir este tipo de arrays utilizando la función array, tal y como muestra el siguiente ejemplo.

```php
<?php
  $equipos = array(
    array("Boston Celtics", "Toronto Raptors", "Atlanta Hawks", "Orlando Magic"),
    array("L.A. Lakers", "Golden State Warriors", "Portland Trail Blazers", "Sacramento Kings")
  );
	//Como no se han definido claves se accede mediante índices a estos valores.
  echo $equipos[1][3]; //Mostraría el valor Sacramento Kings 
  echo $equipos[0][0]; //Mostraría el valor Boston Celtics
?>
```

También podemos utilizar claves personalizadas para crear este array multidimensional.

```php
<?php
  $equipos["este"][0] = "Boston Celtics";
  $equipos["este"][1] = "Toronto Raptors";
  $equipos["este"][2] = "Atlanta Hawks";
  $equipos["este"][3] = "Orlando Magic";
  $equipos["oeste"][0] = "L.A. Lakers";
  $equipos["oeste"][1] = "Golden State Warriors";
  $equipos["oeste"][2] = "Portland Trail Blazers";
  $equipos["oeste"][3] = "Sacramento Kings";
  echo $equipo["este"][1]; //Mostraría el valor Toronto Raptors
  echo $animal["oeste"][3]; //Mostraría el valor Sacramento Kings 
?>
```

También se pueden anidar multiples funciones array para generar un de más de dos dimensiones, tal y como se ejemplifica a continuación.

```php
<?php
  $equipos= array(
  						array( //Conferencia Este
                array("Boston Celtics","Toronto Raptors","Philadelphia 76ers"), //División Atlántica
                array("Chicago Bulls","Detroit Pistons","Indiana Pacers"), //División Central
                array("Atlanta Hawks","Miami Heat", "Orlando Magic") //División Sureste
              ), array( //Conferencia Oeste
                array("L.A. Clippers","Golden State Warriors","Phoenix Suns"), //División Pacífico
                array("San Antonio Spurs","Houston Rockets","Memphis Grizzlies"), //División Suoeste
                array("Portland Trail Blazers","Denver Nuggets", "Utah Jazz") //División Noroeste
              )
	)

  echo $equipo[0][1][2]; //Mostraría el valor Indiana Pacers
  echo $equipo[1][0][0]; //Mostraría el valor L.A. Clippers 
?>
```

## Lectura de un array

Por norma general, los arrays contienen un número variable de elementos, lo que implica en muchos casos el desconocimiento de la estructura del array o de las claves utilizadas en el mismo. Se puede solucionar este problema utilizando bucles que permitan recorrer cada uno de los valores en el array. En un primer caso podríamos considerar utilizar un bucle for como en el siguiente ejemplo. 

```php
$equipos = ["Atlanta Hawks","Boston Celtics","Brooklyn Nets","Charlotte Hornets","Chicago Bulls","Cleveland Cavaliers","Dallas Mavericks","Denver Nuggets","Detroit Pistons","Golden State Warriors","Houston Rockets","Indiana Pacers","LA Clippers","LA Lakers","Memphis Grizzlies","Miami Heat","Milwaukee Bucks","Minnesota Timberwolves","New Orleans Pelicans","New York Knicks","Oklahoma City Thunder","Orlando Magic","Philadelphia Sixers","Phoenix Suns","Portland Trail Blazers","Sacramento Kings","San Antonio Spurs","Toronto Raptors","Utah Jazz","Washington Wizards"];
for($i=0;$i<count($equipos);$i++){
  echo "El equipo ".$i+1." es, $equipos[i]";
}
```

En este ejemplo que mostraría un listado similar al siguiente:

- El equipo 1 es Atlanta Hawks
- El equipo 2 es Boston Celtics
- ...

No obstante cabe resaltar que tal y como hemos definido el array este estaría identificado por una serie de índices numéricos a los que podemos acceder con una variable que puede ir aumentándose en cada pasada del bucle. El caso es diferente si nos encontramos con una array asociativo como el que tenemos a continuación.

```php
$equipos = array("Atlanta"=>"Hawks","Boston"=>"Celtics","Brooklyn"=>"Nets","Charlotte"=>"Hornets","Chicago"=>"Bulls","Cleveland"=>"Cavaliers","Dallas"=>"Mavericks","Denver"=>"Nuggets","Detroit"=>"Pistons","Golden"=>"State Warriors","Houston"=>"Rockets","Indiana"=>"Pacers","LA"=>"Clippers","LA"=>"Lakers","Memphis"=>"Grizzlies","Miami"=>"Heat","Milwaukee"=>"Bucks","Minnesota"=>"Timberwolves","New Orleans"=>"Pelicans","New York"=>"Knicks","Oklahoma City"=>"Thunder","Orlando"=>"Magic","Philadelphia"=>"Sixers","Phoenix"=>"Suns","Portland"=>"Trail Blazers","Sacramento"=>"Kings","San Antonio"=>"Spurs","Toronto"=>"Raptors","Utah"=>"Jazz","Washington"=>"Wizards");
```

En este caso, no existe un índice numérico que pueda recorrerse mediante un bucle. Por ello, existe un bucle especial que permite recorrer este y otros arrays creando una variable que almacena en cada pasada del bucle que recorrerá uno por uno todos los objetos que contiene el array.

Podemos recorrer un array indexado mediante un bucle *foreach* de la siguiente forma:

```php
foreach ($equipos as $nombreequipo){
  echo "$nombreequipo";
}
```

Esta forma de recorrer los arrays toma por una parte la variable que contiene todos los equipos *$equipos* y crea una nueva variable *$nombreequipo* donde se va almacenando en cada pasada el valor de cada una de las cadenas de caracteres que contiene el array. En caso de tener un array asociativo tendríamos que declarar una variable que contenería las claves del array y por otro los valores del mismo.

```php
foreach($equipos as $claveequipo=>$nombreequipo){
  echo "El equipo de la ciudad $claveequipo se llama $nombreequipo";
}
```

Con un array multidimensional tendríamos que utilizar bucles anidados para recorrer cada una de las dimensiones o utilizar alguna de las funciones que se describirán más adelante para poder trabajar con arrays de este tipo.

```php
<?php
  $equipos = array(
    array("Boston Celtics", "Toronto Raptors", "Atlanta Hawks", "Orlando Magic"),
    array("L.A. Lakers", "Golden State Warriors", "Portland Trail Blazers", "Sacramento Kings")
  );
//Utilizando el acceso mediante índices
	for($i=0;$i<count($array);$i++) {
		for($j=0;$j<count($array[$i]);$j++) {
			echo $array[$i][$j];
      
//Utilizando un bucle foreach
   foreach ($equipos as $conferencias) {
     echo ("Equipos de una conferencia");
     echo ("--------------------------");
   	foreach ($conferencias as $equipo) {
        echo "El nombre del equipo es: $equipo";
    }
    
}
?>
```

## Funciones de trabajo con arrays

PHP ofrece de forma nátiva una serie de funciones que permiten realizar operaciones comunes con los mismos que faciliten la escritura de código. A continuación se muestra un listado de las funciones de PHP con una descripción.

- [array_change_key_case](https://www.php.net/manual/es/function.array-change-key-case.php) — Cambia a mayúsculas o minúsculas todas las claves en un array
- [array_chunk](https://www.php.net/manual/es/function.array-chunk.php) — Divide un array en fragmentos
- [array_column](https://www.php.net/manual/es/function.array-column.php) — Devuelve los valores de una sola columna del array de entrada
- [array_combine](https://www.php.net/manual/es/function.array-combine.php) — Crea un nuevo array, usando una matriz para las claves y otra para sus valores
- [array_count_values](https://www.php.net/manual/es/function.array-count-values.php) — Cuenta todos los valores de un array
- [array_diff_assoc](https://www.php.net/manual/es/function.array-diff-assoc.php) — Calcula la diferencia entre arrays con un chequeo adicional de índices
- [array_diff_key](https://www.php.net/manual/es/function.array-diff-key.php) — Calcula la diferencia entre arrays empleando las claves para la comparación
- [array_diff_uassoc](https://www.php.net/manual/es/function.array-diff-uassoc.php) — Calcula la diferencia entre arrays con un chequeo adicional de índices que se realiza por una función de devolución de llamada suministrada por el usuario
- [array_diff_ukey](https://www.php.net/manual/es/function.array-diff-ukey.php) — Calcula la diferencia entre arrays usando una función de devolución de llamada en las keys para comparación
- [array_diff](https://www.php.net/manual/es/function.array-diff.php) — Calcula la diferencia entre arrays
- [array_fill_keys](https://www.php.net/manual/es/function.array-fill-keys.php) — Llena un array con valores, especificando las keys
- [array_fill](https://www.php.net/manual/es/function.array-fill.php) — Llena un array con valores
- [array_filter](https://www.php.net/manual/es/function.array-filter.php) — Filtra elementos de un array usando una función de devolución de llamada
- [array_flip](https://www.php.net/manual/es/function.array-flip.php) — Intercambia todas las claves de un array con sus valores asociados
- [array_intersect_assoc](https://www.php.net/manual/es/function.array-intersect-assoc.php) — Calcula la intersección de arrays con un chequeo adicional de índices
- [array_intersect_key](https://www.php.net/manual/es/function.array-intersect-key.php) — Calcula la intersección de arrays usando sus claves para la comparación
- [array_intersect_uassoc](https://www.php.net/manual/es/function.array-intersect-uassoc.php) — Calcula la intersección de arrays con una comprobación adicional de índices, los cuales se comparan con una función de retrollamada
- [array_intersect_ukey](https://www.php.net/manual/es/function.array-intersect-ukey.php) — Calcula la intersección de arrays usando una función de devolución de llamada en las claves para la comparación
- [array_intersect](https://www.php.net/manual/es/function.array-intersect.php) — Calcula la intersección de arrays
- [array_key_exists](https://www.php.net/manual/es/function.array-key-exists.php) — Verifica si el índice o clave dada existe en el array
- [array_key_first](https://www.php.net/manual/es/function.array-key-first.php) — Obtiene la primera clave de un array
- [array_key_last](https://www.php.net/manual/es/function.array-key-last.php) — Obtiene la última clave de un array
- [array_keys](https://www.php.net/manual/es/function.array-keys.php) — Devuelve todas las claves de un array o un subconjunto de claves de un array
- [array_map](https://www.php.net/manual/es/function.array-map.php) — Aplica la retrollamada a los elementos de los arrays dados
- [array_merge_recursive](https://www.php.net/manual/es/function.array-merge-recursive.php) — Une dos o más arrays recursivamente
- [array_merge](https://www.php.net/manual/es/function.array-merge.php) — Combina dos o más arrays
- [array_multisort](https://www.php.net/manual/es/function.array-multisort.php) — Ordena varios arrays, o arrays multidimensionales
- [array_pad](https://www.php.net/manual/es/function.array-pad.php) — Rellena un array a la longitud especificada con un valor
- [array_pop](https://www.php.net/manual/es/function.array-pop.php) — Extrae el último elemento del final del array
- [array_product](https://www.php.net/manual/es/function.array-product.php) — Calcula el producto de los valores de un array
- [array_push](https://www.php.net/manual/es/function.array-push.php) — Inserta uno o más elementos al final de un array
- [array_rand](https://www.php.net/manual/es/function.array-rand.php) — Seleccionar una o más claves aleatorias de un array
- [array_reduce](https://www.php.net/manual/es/function.array-reduce.php) — Reduce iterativamente un array a un solo valor usando una función llamada de retorno
- [array_replace_recursive](https://www.php.net/manual/es/function.array-replace-recursive.php) — Reemplaza los elementos de los arrays pasados al primer array de forma recursiva
- [array_replace](https://www.php.net/manual/es/function.array-replace.php) — Reemplaza los elementos del array original con elementos de array adicionales
- [array_reverse](https://www.php.net/manual/es/function.array-reverse.php) — Devuelve un array con los elementos en orden inverso
- [array_search](https://www.php.net/manual/es/function.array-search.php) — Busca un valor determinado en un array y devuelve la primera clave correspondiente en caso de éxito
- [array_shift](https://www.php.net/manual/es/function.array-shift.php) — Quita un elemento del principio del array
- [array_slice](https://www.php.net/manual/es/function.array-slice.php) — Extraer una parte de un array
- [array_splice](https://www.php.net/manual/es/function.array-splice.php) — Elimina una porción del array y la reemplaza con otra cosa
- [array_sum](https://www.php.net/manual/es/function.array-sum.php) — Calcular la suma de los valores de un array
- [array_udiff_assoc](https://www.php.net/manual/es/function.array-udiff-assoc.php) — Computa la diferencia entre arrays con una comprobación de indices adicional, compara la información mediante una función de llamada de retorno
- [array_udiff_uassoc](https://www.php.net/manual/es/function.array-udiff-uassoc.php) — Computa la diferencia entre arrays con una verificación de índices adicional, compara la información y los índices mediante una función de llamada de retorno
- [array_udiff](https://www.php.net/manual/es/function.array-udiff.php) — Computa la diferencia entre arrays, usando una llamada de retorno para la comparación de datos
- [array_uintersect_assoc](https://www.php.net/manual/es/function.array-uintersect-assoc.php) — Calcula la intersección de arrays con una comprobación de índices adicional, compara la información mediante una función de retrollamada
- [array_uintersect_uassoc](https://www.php.net/manual/es/function.array-uintersect-uassoc.php) — Calcula la intersección de arrays con una comprobación de índices adicional, compara la información y los índices mediante funciones de retrollamada por separado
- [array_uintersect](https://www.php.net/manual/es/function.array-uintersect.php) — Computa una intersección de arrays, compara la información mediante una función de llamada de retorno
- [array_unique](https://www.php.net/manual/es/function.array-unique.php) — Elimina valores duplicados de un array
- [array_unshift](https://www.php.net/manual/es/function.array-unshift.php) — Añadir al inicio de un array uno a más elementos
- [array_values](https://www.php.net/manual/es/function.array-values.php) — Devuelve todos los valores de un array
- [array_walk_recursive](https://www.php.net/manual/es/function.array-walk-recursive.php) — Aplicar una función de usuario recursivamente a cada miembro de un array
- [array_walk](https://www.php.net/manual/es/function.array-walk.php) — Aplicar una función proporcionada por el usuario a cada miembro de un array
- [array](https://www.php.net/manual/es/function.array.php) — Crea un array
- [arsort](https://www.php.net/manual/es/function.arsort.php) — Ordena un array en orden inverso y mantiene la asociación de índices
- [asort](https://www.php.net/manual/es/function.asort.php) — Ordena un array y mantiene la asociación de índices
- [compact](https://www.php.net/manual/es/function.compact.php) — Crear un array que contiene variables y sus valores
- [count](https://www.php.net/manual/es/function.count.php) — Cuenta todos los elementos de un array o algo de un objeto
- [current](https://www.php.net/manual/es/function.current.php) — Devuelve el elemento actual en un array
- [each](https://www.php.net/manual/es/function.each.php) — Devolver el par clave/valor actual de un array y avanzar el cursor del array
- [end](https://www.php.net/manual/es/function.end.php) — Establece el puntero interno de un array a su último elemento
- [extract](https://www.php.net/manual/es/function.extract.php) — Importar variables a la tabla de símbolos actual desde un array
- [in_array](https://www.php.net/manual/es/function.in-array.php) — Comprueba si un valor existe en un array
- [key_exists](https://www.php.net/manual/es/function.key-exists.php) — Alias de array_key_exists
- [key](https://www.php.net/manual/es/function.key.php) — Obtiene una clave de un array
- [krsort](https://www.php.net/manual/es/function.krsort.php) — Ordena un array por clave en orden inverso
- [ksort](https://www.php.net/manual/es/function.ksort.php) — Ordena un array por clave
- [list](https://www.php.net/manual/es/function.list.php) — Asignar variables como si fueran un array
- [natcasesort](https://www.php.net/manual/es/function.natcasesort.php) — Ordenar un array usando un algoritmo de "orden natural" insensible a mayúsculas-minúsculas
- [natsort](https://www.php.net/manual/es/function.natsort.php) — Ordena un array usando un algoritmo de "orden natural"
- [next](https://www.php.net/manual/es/function.next.php) — Avanza el puntero interno de un array
- [pos](https://www.php.net/manual/es/function.pos.php) — Alias de current
- [prev](https://www.php.net/manual/es/function.prev.php) — Rebobina el puntero interno del array
- [range](https://www.php.net/manual/es/function.range.php) — Crear un array que contiene un rango de elementos
- [reset](https://www.php.net/manual/es/function.reset.php) — Establece el puntero interno de un array a su primer elemento
- [rsort](https://www.php.net/manual/es/function.rsort.php) — Ordena un array en orden inverso
- [shuffle](https://www.php.net/manual/es/function.shuffle.php) — Mezcla un array
- [sizeof](https://www.php.net/manual/es/function.sizeof.php) — Alias de count
- [sort](https://www.php.net/manual/es/function.sort.php) — Ordena un array
- [uasort](https://www.php.net/manual/es/function.uasort.php) — Ordena un array con una función de comparación definida por el usuario y mantiene la asociación de índices
- [uksort](https://www.php.net/manual/es/function.uksort.php) — Ordena un array según sus claves usando una función de comparación definida por el usuario
- [usort](https://www.php.net/manual/es/function.usort.php) — Ordena un array según sus valores usando una función de comparación definida por el usuario

## Ejercicios

1. Define el siguiente array y cálcula la suma total de los valores que contiene.

   ```php
   $numeros=array(2, 24, 80, 5, 7, 20 ,32 ,45 ,67, 45, 78);
   ```

2. Calcula del array definido anteriormente el productorio de los valores pares del array por una parte y los valores impares por otra parte.

3. Crear un código en php que compruebe si el valor 76 está incluido en el array anterior.

4. Crear un código en php que elimine el último valor del array y lo muestre por pantalla. 

5. Crear un código en php que elimine los valores duplicados en el array.
