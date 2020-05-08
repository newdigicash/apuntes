# Comandos de docker
Comandos más usados de docker

Hay un [tutorial][urlTutorial] que explica cómo usar los comandos. 
Mientras que [este tutorial][urlTutoDockerfile] detalla cómo configurar el archivo dockerfile para apache.

Aquí hay otro [tutorial para configurar Node][urlTutoNode].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.2

## 2. Observación

El [formato de run][urlRun] es `docker run opciones imagen comando argumentos`.  
El comando _commit_ guarda localmente la imagen con todo lo que este instalado y configurado, 
para luego arrancar y exponer los puertos.

## 3. Contenido 
### 3.1 Comandos
Cmd | Descripción
:-- | :--
`docker pull id-magen` | descarga imagen desde dockerhub
`docker images` | muestra imagenes descargadas o guardadas
`docker run id-magen` | corre una imagen
`docker ps -a` | lista todos contenedores
`docker container ls -all` | lista todos contenedores
`docker build --tag "nombre-contenedor" .` | para crear nueva imagen
`docker build -t nombre ruta-dockfile` | para crear nueva imagen con dockerfile
`docker commit -m "msg" -a "usr" id-contenedor id-usuario/nombre-imagen` | guarda estado de imagen local
`docker run --interactive --tty ubuntu bash` | ejecuta _ubuntu_ modo bash
`docker run -it ubuntu` | ejecuta _ubuntu_ interactive y en terminal
`docker run --detach --publish 80:80 --name nombre nginx` | ejecuta _nginx_ en el 80
`docker run -d -p 8085:80 mi-imagen` | arrancar el server *mi-imagen*
`docker start id-contenedor` | arranca el contenedor
`docker stop id-contenedor` | detiene el contenedor
`docker attach id-contenedor` | ingresa al contenedor
`docker cp archivo id-contenedor:ruta` | [Copia][urlTutoCp] _archivo_ a carpeta del contenedor
`docker container stop nombre-contenedor` | detiene el contenedor por nombre
`docker container rm nombre-contenedor` | borra contenedor por nombre
`docker rmi id-imagen` | borra la imagen
`docker rm id-contenedor` | borra el contenedor
`docker login -u id-usuario` | inicia sesion en el hub
`docker push id-usuario/nombre-imagen` | sube imagen a docker hub

### 3.2 Uso de comandos
Descarga la imagen desde hub
~~~
docker pull hello-world
~~~

Ejecuta la imagen sin parámetros
~~~
docker run hello-world
~~~
Descarga desde el hub y ejecuta ***ubuntu*** 
~~~
docker run -it ubuntu:latest
~~~

Ejecuta ***nginx*** en el 80 y lo nombra ***webserver***
~~~
docker run --detach --publish 80:80 --name webserver nginx
~~~

Arranca ***nginx*** y expone el puerto 80
~~~
docker run -d -p 80:80 id-imagen nginx -g 'daemon off;'
~~~

Arranca ***apache*** y expone el 80 y 443
~~~
docker run -d -p 80:80 -p 443:433 id-imagen apachectl -D FOREGROUND
~~~

Para depurar apache y usar la terminal
~~~
docker run -i -t -p 8080:80 id-imagen /bin/bash
apachectl start
~~~

### 3.3 Uso de Dockerfile
En [alpine][urlAlpine], instala el JRE , copia *HelloWorld.class* al path y ejecuta en la terminal `java HelloWorld`
~~~
FROM alpine:latest
ADD HelloWorld.class HelloWorld.class
RUN apk --update add openjdk8-jre
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "HelloWorld"]
~~~

En ubuntu, instala apache, expone el puerto 80 y arranca apache
~~~
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y apache2
expose 80
CMD /usr/sbin/apache2ctl -D FOREGROUND
~~~

En ubuntu, instala apache, expone el 80 y 443 y arranca apache
~~~
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y apache2
expose 80
expose 443
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
~~~

## 4. Fuentes
Doc comando run <https://docs.docker.com/engine/reference/run/>

Doc dockerfile <https://docs.docker.com/engine/reference/builder/>

Dock comando cp <https://docs.docker.com/engine/reference/commandline/cp/>

Tutorial de docker <https://www.youtube.com/playlist?list=PLCTD_CpMeEKTj_n9XY0vz9n6Asi-g0kRg>

[//]: # (referencias citadas)
[urlRun]: https://docs.docker.com/engine/reference/commandline/run/
[urlTutoNode]: https://docs.docker.com/get-started/part2/
[urlTutoDockerfile]: https://linuxconfig.org/how-to-build-a-docker-image-using-a-dockerfile
[urlTutorial]: https://www.youtube.com/playlist?list=PLCTD_CpMeEKTj_n9XY0vz9n6Asi-g0kRg
[urlAlpine]: https://alpinelinux.org/
[urlTutoCp]: https://www.shellhacks.com/docker-cp-command-copy-file-to-from-container
