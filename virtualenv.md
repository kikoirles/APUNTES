---
title: "Apuntes de Entornos Virtuales Vitualenv"
tags: ""
---

### Virtualenv Windows ###

Virtualenv es una herramienta usada para crear un entorno Python aislado. Este entorno tiene sus propios directorios de instalación que no comparten bibliotecas con otros entornos virtualenv o las bibliotecas instaladas globalmente en el servidor

Para empezar la instalación realizamos lo siguiente:

```shell
pip install virtualenv
```

**ver modulos instalados**
```shell
pip freeze
```

creamos una carpeta en el escritorio que va a contener los entornos virtuales
luego las subcarpetas proyecto 1 y 2
Mis entornos > Proyecto 1
Mis ebtornos > Proyecto 2

**Nos movemos al Proyecto 1**

```shell
cd Desktop\Mis entornos\Proyecto 1
```

**Siguientemente vamos a crear un entorno virtual con nombre proyecto1**

```shell
virtualenv proyecto1
```

**Activar entorno virtual**
```shell
.\proyecto1\Scripts\activate
```
Cambiara a (proyecto1) 

Ver de nuevo modulos instalados

```shell
pip freeze
```

No no muestrara nada porque no hay ningun modulo instalado en el entorno proyecto1

```shell
pip install pyjokes
```

```shell
pip freeze
```

**desactivar el entornos**

```shell
deactivate
```



### Virtualenv LINUX ###

Primero, asegúrate de tener pip instalado. Luego, puedes instalar virtualenv utilizando el siguiente comando:
       
```shell
pip install virtualenv
```

**1. Ver módulos instalados globalmente**
Para ver los módulos instalados globalmente, puedes usar:
                    
```shell
pip freeze
```

**2. Crear directorios para los entornos virtuales**
Supongamos que quieres crear una carpeta en el escritorio que contenga tus entornos virtuales. Primero, crea la carpeta principal y las subcarpetas para tus proyectos:

```shell
mkdir -p ~/Escritorio/Mis_entornos/Proyecto1
mkdir -p ~/Escritorio/Mis_entornos/Proyecto2
```

**3. Crear un entorno virtual**
Muévete a la carpeta del proyecto donde deseas crear el entorno virtual:

```shell
cd ~/Escritorio/Mis_entornos/Proyecto1
```
Luego, crea el entorno virtual con el nombre que prefieras, por ejemplo, proyecto1:

```shell
virtualenv proyecto1
```

**4. Activar el entorno virtual**
Para activar el entorno virtual, usa el siguiente comando:

```shell
source proyecto1/bin/activate
```

Una vez activado, verás que el prompt de tu terminal cambia para indicar que estás dentro del entorno virtual, por ejemplo, (proyecto1).

**5. Ver módulos instalados en el entorno virtual**
Ahora, si ejecutas:

```shell
pip freeze
```

No se mostrará nada porque aún no has instalado ningún módulo en el entorno virtual.

**6. Instalar un módulo (ejemplo: pyjokes)**
Para instalar un módulo dentro del entorno virtual, usa:

```shell
pip install pyjokes
```

**7. Ver módulos instalados en el entorno virtual**
Después de la instalación, puedes verificar los módulos instalados nuevamente:

```shell
pip freeze
```

Deberías ver que solo pyjokes está listado.

**8. Desactivar el entorno virtual**
Para salir del entorno virtual y volver al entorno global de Python, usa:

```shell
deactivate
```

Estos son los pasos básicos para configurar y utilizar entornos virtuales en Linux usando virtualenv. Mantener tus proyectos en entornos virtuales separados ayuda a evitar conflictos de dependencias y facilita la gestión de diferentes versiones de paquetes en distintos proyectos.
