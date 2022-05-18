---
title: "FORMULARIOS PHP"
tags: ""
---

# Tratamiento de Formularios en PHP

## Recordatorio de uso de formularios

Los **formularios web** son uno de los principales puntos de interacción entre un usuario y un sitio web o aplicación. Los formularios permiten a los usuarios la introducción de datos, que generalmente se envían a un servidor web para su procesamiento y almacenamiento, o se usan en el lado del cliente para provocar de alguna manera una actualización inmediata de la interfaz.

El HTML de un **formulario web** está compuesto por uno o más **controles de formulario** (a veces llamados **widgets**), además de algunos elementos adicionales que ayudan a estructurar el formulario general; a menudo se los conoce como **formularios HTML**. Los controles pueden ser campos de texto de una o varias líneas, cajas desplegables, botones, casillas de verificación o botones de opción, y se crean principalmente con el elemento [``](https://developer.mozilla.org/es/docs/Web/HTML/Elemento/input), aunque hay algunos otros elementos que también hay que conocer.

### El elemento FORM

Todos los formularios comienzan con un elemento <form>, como este:

```html
<form action="/paginademanejo.php" method="post">

</form>
```

- El atributo `action` define la ubicación (URL) donde se envían los datos que el formulario ha recopilado cuando se validan.
- El atributo `method` define con qué método HTTP se envían los datos (generalmente `get` o `post`).

### Los elementos `<label>`, `<input>` y `<textarea>`

Un ejemplo de formulario de contacto sin mucha complejidad donde la parte para la entrada de datos contiene tres campos de texto, cada uno con su elemento `<label>` correspondiente:

- El campo de entrada para el nombre es un campo de texto de una sola línea.
- El campo de entrada para el correo electrónico es una entrada de datos de tipo correo electrónico, es decir, un campo de texto de una sola línea que acepta solo direcciones de correo electrónico.
- El campo de entrada para el mensaje es `<textarea>`  que es un campo de texto multilínea.

En términos de código HTML, para implementar estos controles de formulario necesitamos algo como lo siguiente:

``` html
<form action="/paginademanejo.php" method="post">
 <ul>
  <li>
    <label for="name">Nombre:</label>
    <input type="text" id="name" name="user_name">
  </li>
  <li>
    <label for="mail">Correo electrónico:</label>
    <input type="email" id="mail" name="user_mail">
  </li>
  <li>
    <label for="msg">Mensaje:</label>
    <textarea id="msg" name="user_message"></textarea>
  </li>
 </ul>
</form>
```

En el elemento `<input>` el atributo más importante es *type*. Este atributo es muy importante porque define la forma en que el elemento `<input>` aparece y se comporta. 

### Tipos de elementos más comunes

Los **tipos de elementos más importantes para los formularios HTML con PHP** son:

| **Elemento**          | **Descripción**                                              |
| --------------------- | ------------------------------------------------------------ |
| input type="text"     | Caja de texto                                                |
| input type="password" | Caja de texto donde se muestran asteriscos en lugar de los caracteres escritos |
| input type="checkbox" | Cajas seleccionables que permite escoger múltiples opciones  |
| input type="radio"    | Cajas seleccionables en grupos que sólo permiten escoger una opción |
| input type="submit"   | Botón para enviar el formulario                              |
| input type="file"     | Cajas de texto y botón que permite subir archivos            |
| input type="hidden"   | Elemento escondido. Especialmente útil para tokens de seguridad |
| option                | Una opción posible dentro de un elemento element             |
| select                | Lista de opciones de elementos option                        |
| textarea              | Texto multilínea                                             |

### El elemento Button

Se necesita añadir un botón para permitir que el usuario envíe sus datos una vez que haya completado el formulario. Esto se hace con el elemento `<button>` añade lo siguiente justo encima de la etiqueta de cierre `</form>`:

```html
<li class="button">
  <button type="submit">Envíe su mensaje</button>
</li>
```

El elemento `<button>` también acepta un atributo de *type*, que a su vez acepta uno de estos tres valores: `submit`, `reset` o `button`.

- Un clic en un botón `submit` (el valor predeterminado) envía los datos del formulario a la página web definida por el atributo `action` del elemento `<form>`.
- Un clic en un botón `reset` restablece de inmediato todos los controles de formulario a su valor predeterminado. Desde el punto de vista de UX, esto se considera una mala práctica, por lo que debes evitar usar este tipo de botones a menos que realmente tengas una buena razón para incluirlos.
- Un clic en un botón `button` no hace... ¡nada! Eso suena tonto, pero es muy útil para crear botones personalizados: puedes definir su función con JavaScript.

## Acceso a datos de formularios en PHP

El modo de inserción de datos por parte del usuario en una página web se realiza a partir de formularios definidos en HTML que se enlazan con la página PHP que tratará los datos. Así, un ejemplo básico de un formulario definido en HTML puede ser el siguiente:

```html
<html>
<body>
<form action="formulario.php" method="post">
    Nombre: <input type="text" name="nombre"><br>
    Email: <input type="text" name="email"><br>
    <button type="submit" value="Enviar">
</form>
</body>
</html>
```

Este código en HTML establecerá dos etiquetas (Nombre e Email) que se definen como campos de texto y permiten al usuario rellanarlos con valores. Adicionalmente, se establece un botón que desencadenará la acción de enviar la información desde el usuario al servidor, donde serán tratados los datos por la página definida en el campo *action*. En este ejemplo se envían los datos utilizando el protocolo HTTP mediante la función HTTP Post.

Cuando los datos llegan al servidor estos pueden ser tratados mediante php. Un ejemplo de un tratamiento sencillo de estos datos se muestra a continuación donde a partir de la superglobal $_POST podemos acceder mediante el nombre del campo definido en el formulario al valor que haya introducido el usuario.

```php+HTML
<html>
<body>
	Hola <?php echo $_POST["nombre"]; ?><br>
	Tu email es: <?php echo $_POST["email"]; ?>
</body>
</html>
```

El mismo ejemplo implementado con el método HTTP Get, se implementaría de la siguiente forma:

```html
<html>
<body>
<form action="formulario.php" method="get">
    Nombre: <input type="text" name="nombre"><br>
    Email: <input type="text" name="email"><br>
    <button type="submit" value="Enviar">
</form>
</body>
</html>
```

Y el tratamiento realizado por la web en php sería el siguiente:

```php+HTML
<html>
<body>
  Hola <?php echo $_GET["nombre"]; ?><br>
  Tu email es: <?php echo $_GET["email"]; ?>
</body>
</html>
```

Estos dos métodos permiten acceder a los datos que han sido insertados por el usuario en un formulario y presentan una serie de diferencias a tener en cuanta a la hora de seleccionar el método utilizado a la hora de acceder a los mismos.

## Los métodos GET y POST

**$_GET** y **$_POST** son variables superglobales que forman arrays de *keys* y *values*, donde los *keys* son los nombres del formulario (atributo "*name*") y los *values* son los datos de entrada de los usuarios. Al ser un array asociativo no podremos acceder a él mediante índices y el hecho de que sean ***superglobals*** hace que sean accesibles desde el script independientemente del ámbito.

**$_GET** es un array de variables que se pasan al script a través de los **parámetros de URL**. La información que se envía es visible para todo el mundo y tiene limitada la cantidad de información que se puede enviar a 2000 caracteres. Las URLs con los datos enviados pueden guardarse en marcadores, lo que puede ser útil en ciertos casos. $_GET sólo se emplea para el envío de información reducida y no sensible.

**$_POST** es un array de variables que se pasan al script a través del método **HTTP POST**. La información que se envía no es visible para los demás ya que los nombres y variables van embebidas en el ***body*** del **HTTP request**. No tiene límites en la cantidad de información a enviar.

## Ejercicios

1. Crea un formulario básico de login y una página que muestre con php los valores introducidos por el usuario.
2. Lee el nombre y los apellidos, el salario (numero con decimales) y la edad de una persona (un número entero) en un formulario. Recoge los datos y con ellos calcula un nuevo salario para esa persona en base a esta situación:
   * Si el salario es mayor de 2000 euros, no cambiará.
   * Si el salario está entre 1000 y 2000 euros:
     * Si además la edad es mayor de 45 años se aumenta un 3%.
     * Si la edad es menor o igual a 45 años se aumenta un 10%.
   * Si el salario es menor de 1000 euros:
     * Los menores de 30 años cobrarán, a partir de ahora, exactamente 1100 euros.
     * De 30 a 45 años, aumenta un 5%.
     * A los mayores de 45 años, sube un 10%.
