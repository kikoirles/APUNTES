---
title: "Comandos_Básicos_de_Docker"
tags: ""
---


# Comandos Básicos de Docker


**Iniciar Docker**  

```shell
sudo systemctl start docker
```

**Comprobar el estado de Docker**  

```shell  
sudo systemctl status docker
```

**Listar imágenes locales**  


```shell
docker images
```

**Descargar una imagen desde Docker Hub**  

```shell
docker pull <nombre_imagen>  
```

**Listar contenedores en ejecución**

```shell 
docker ps  
```

**Listar todos los contenedores (incluidos los detenidos)**

```shell  
docker ps -a  
```

**Ejecutar un contenedor**

```shell 
docker run <nombre_imagen>  
```

**Ejecutar un contenedor en modo interactivo**

```shell   
docker run -it <nombre_imagen> /bin/bash
```  

**Detener un contenedor** 

```shell 
docker stop <id_contenedor>  
```

**Eliminar uncontenedor**

```shell 
docker rm <id_contenedor>
``` 

**Eliminar una imagen**

```shell  
docker rmi <id_imagen>  
```

### Comandos Avanzados de Docker ### 
**Ejecutar un contenedor en segundo plano (detached mode)**  

```shell
docker run -d <nombre_imagen>  
```

**Nombrar un contenedor**

```shell 
docker run --name <nombre_contenedor> <nombre_imagen>
```

**Nombrar un contenedor** 
**puerto y restart**

```shell
docker run -d -p 5000:5000 --restart=always –name my-registry my registry2
```

**Nombrar contenedor y linkear a redis**

```shell
Docker run -d –name=clickcounter –link redis:redis -p 8085:5000 kodekloud/click-counter
``` 

**Mapear puertos entre el host y el contenedor**  

```shell
docker run -p <puerto_host>:<puerto_contenedor> <nombre_imagen>  
```
**Docker mysql contraseña**

```shell
docker run -v /opt/data:/var/lib/mysql -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
```

**Docker mysql contraseña**

```shell
docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
```

**Montar volúmenes**

```shell  
docker run -v <ruta_host>:<ruta_contenedor> <nombre_imagen>  
```

**Inspeccionar un contenedor**

```shell  
docker inspect <id_contenedor> 
``` 

**Ver los logs de un contenedor**

```shell
un contenedor** docker logs <id_contenedor>
```  

**Reiniciar un contenedor**

```shell  
docker restart <id_contenedor>  
```

**Ver el uso de recursos de los contenedores**

```shell 
docker stats  
```

**Conectar a un contenedor en ejecución**

```shell
docker exec -it <id_contenedor> /bin/bash  
```

***Ejecutar un contenedor y mostrar el contenido de /etc/release**

```shell  
docker run python:3.6 cat /etc/*release  
```

**Ejemplo de inspección detallada de un contenedor por ID y buscar APP**  

```shell
docker inspect <id_contenedor> | grep APP  
```

### Docker links ###
**Forma de realizar enlaces entre distintas maquinas en docker**

```shell
docker run -d –name-vote -p 5000:80 –link redis:redis voting-app
```

![image](https://github.com/user-attachments/assets/a5e55f7b-5b11-4079-ba6f-f5be8a0d96e2)




