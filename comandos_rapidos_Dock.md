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

### Crear un Contenedor solo usando Docker Build ###
**Escribir un Dockerfile**
**Crea un archivo llamado Dockerfile y añade el siguiente contenido como ejemplo:**

Dockerfile
Copiar código:
```shell
# Usar una imagen base de Ubuntu
FROM ubuntu:latest

# Instalar actualizaciones y paquetes necesarios
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

# Copiar el código de la aplicación en el contenedor
COPY . /app

# Definir el directorio de trabajo
WORKDIR /app

# Instalar las dependencias de la aplicación
RUN pip3 install -r requirements.txt

# Especificar el comando para ejecutar la aplicación
CMD ["python3", "app.py"]
```

**Construir la imagen**

```shell
docker build -t <nombre_imagen> .
```

**Ejecutar un contenedor desde la nueva imagen**

```shell
docker run -d -p 5000:5000 <nombre_imagen>
```

### Docker Swarm ###


![image-1](https://github.com/user-attachments/assets/8d17cbaf-1e26-4337-9a3f-a7267d28d19c)



### Docker compose crear conjunto de contendores interconectados ###

**docker-compose.yml** 


```shell
version: "3.9"

services:

    redis:
        image: redis:alpine
        networks:
            - frontend
    db:
        image: postgres: 15-alpine
        environment:
            POSTGRES_USER: "postgres"
            POSTGRES_PASSWORD: "postgres"
    volumes:
        - db-data:/var/lib/postgresql/data 
    networks:
        - backend
    vote:
        image: dockersamples/examplevotingapp_vote 
        ports:
            - 5000:80
        networks:
            - frontend
        deploy:
            replicas: 2
    result:
        image: dockersamples/examplevotingapp_result 
        ports:
            - 5001:80
        networks:
            - backend
    worker:
        image: dockersamples/examplevotingapp_worker 
        networks:
            - frontend
            - backend
        deploy:
            replicas: 2
    networks:
        frontend:
        backend:
    volumes:
        db-data:
```

**Ejecutar archivo yml**

```shell
docker-compose -f {compose file name} up
```

**Directorios de docker de todo**

```shell
cd /var/lib/docker
```

**Login de Docker privado** 

```shell
docker login private-registry.io
```

**Doker  private images**

```shell
docker image tag my-image localhost:5000/my-imgae
```

**Doker push private images**

```shell
docker push localhost:5000/my-image
```

**Doker pull private images**

```shell
docker pull localhost:5000/my-image
```

**Docker remote engine en otra sistema**

```shell
docker -H=10.20.123.2:2375 run nginx
```

**Docker procesos en maquina**

```shell
docker exec 55335642def ps -eaf 
```

```shell
ps -eaf | grep docker-java-home
```

**Docker info**

```shell
docker info | more
```

**Muestra el historial de una imagen y muestra información sobre cada capa que la compone**

```shell
docker history 05
```

**Muestra el real almacenamiento de las imagenes** 

```shell
docker system  df -v
```

### Redes de docker ###

**Mostrar redes** 

```shell
docker network ls
```

**Inspeccionar red de ls**

```shell
docker network inspect bridge
```

**Inspeccionar red de un contenedor o imagen**

```shell
docker inspect mysql:5.6
```

**Docker utiliza su propio swervidor de DNS por eso encuentra las propiedades de mysql:5.6**
**docker bastante completo con parametros**

```shell
docker run -d --network=wp-mysql-network -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --name webapp --link mysql-db:mysql-db -d kodekloud/simple-webapp- mysql
```



