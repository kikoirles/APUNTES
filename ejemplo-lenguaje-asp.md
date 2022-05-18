---
title: "EJEMPLO LENGUAJE ASP"
tags: ""
---

# Ejemplos de uso de ASP

A continuación, aparecen una serie de ejemplos que pueden ser utilizados para comparar ASP y PHP. Adicionalmente, se puede con estos ejemplos comprobar el funcionamiento de un servidor IIS en Windows Server.

## Ejemplo 1

Escribir un texto

```asp
<html>
<body>
<%
response.write("<h2>TITULO</h2>")
response.write("<p style='color:#0000ff'>PUEDES USAR HTML CON ASP RESPONSE.WRITE</p>")
%>
</body>
</html>
```



## Ejemplo 2

Uso de variables en ASP

```asp
<html>
<body>
<%
dim nombre
nombre="Luffy"
response.write("Mi nombre es: " & nombre)
%>
</body>
</html>

Fuente: https://www.ejemplode.com/19-asp/485-ejemplo_de_uso_de_variables_en_asp.html#ixzz7HqrQrPCl
```

## Ejemplo 3

Utilización del valor nulo o vacío

```asp
<%

If not isNull(VARIABLE) then
'si no es nulo

else
'si es nulo

end if

%>
```

## Ejemplo 4

Uso de funciones que muestran la hora y la fecha actuales. Según la hora del día muestra un mensaje. Muestra otro mensaje de forma aleatoria.

```asp
<!DOCTYPE html>
<html lang="es">
<head>
<title>Servidor educado</title>
</head>
<body bgcolor="Wheat">
<font color="Teal" SIZE="5"><b>
El servidor educado le informa que son las <% = Time %> del día <% = Date %>
</b></font>
<p><b>
<% If Hour(Now) < 8 Then %>
Estas no son horas de conectarse, estoy durmiendo!
<% ElseIf Hour(Now) < 15 Then %>
Estoy trabajando, no moleste.
<% Else %>
La tarde es para descansar, ver la tele, dormir, ...
<% End If
 
   Response.Write("<BR><BR>")
   Randomize
   i = Int(Rnd * 4)
   Select Case i
     Case 0
                Response.Write("Hace buen día, ¿ verdad ?")
     Case 1
       Response.Write("Me parece que hoy lloverá.")
     Case 2
       Response.Write("Esta tarde llueve seguro.")
     Case 3
       Response.Write("Que sol mas espléndido.")
   End Select
%>
  </b></p>
</body>
</html>
```

## Ejemplo 5

Obtener los datos de un formulario introducidos por un usuario y los muestra en una página. La página que solicita los datos es (fichero form.html):

```asp
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Formulario de entrada</title>
</head>

<body>
  <form name="formulario" action="recogida.asp" method="post">
		nombre: <input type="text" name="nombre" size="20"><br>
		apellidos: <input type="text" name="apellidos" size="40"><br>
		sexo:<br>
		h <input type="radio" name="sexo" value="varon"> m <input type="radio" name="sexo" value="hembra">
			<br><br>
		<input type="submit" value="enviar">
	</form>
</body>
</html>
```

Página asp que recoje los datos y los procesa (recogida.asp)

```asp
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Procesado de datos</title>
</head>

<body>
  su nombre es <% = request.form("nombre") %>
	<br>
	sus apellidos son <% = request.form("apellidos") %>
	<br>
	su sexo es <% = request.form("sexo") %>
</body>
</html>
```
