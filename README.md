
# Docker

## Documentacion

[Documentacion De Docker](https://docs.docker.com/engine/)

## Instalacion

[Instalacion](https://www.docker.com/products/docker-desktop)

## Imagenes completas

[Imagenes](https://hub.docker.com/search?q=&type=image)

## Problemas que resuelve docker

### Problemas al construir:

* Dependencias de desarrollo
* Versiones de entornos de ejecución
* Equivalencia de entornos de desarrollo
* Equivalencia de entornos de producción
* Versiones / compatibilidad

### Problemas al Distribuir:

* Generaciones del build diferentes
* Acceso a servidores de producción
* Ejecución nativa vs la distribuida
* Serverless

### Problemas al Ejecutar:

* Dependencias de aplicación
* Compatibilidad de sistema operativo
* Disponibilidad de servicios externos
* Recursos de hardware
* Docker permite:
* Construir, distribuir y correr tu código en cualquier lado


## Problemas que poseen las maquinas virtuales con un software completo 

**Virtualization**: es una imagen o archivo que contiene información dentro. Por lo general son pesadas de administración costosa y son lentas.

**Pesadas**:
En el orden de los GB
Muchas VMs en el mismo host suelen repetirse en lo que contienen
Administración costosa:
Una VM tiene que ser administrada como cualquier otra computadora: patching, update, etc
Hay que administrar la seguridad interna entre apps

**Lentas**:
Correr nuestro código en una VM implica no solo arrancar aplicaciones, sino también esperar el boot de la VM en sí.


## Ventajas de docker

**Containarization**: un estándar para llevar algo dentro. Agrupadores de procesos.

**Versátiles**:
En el orden de los MB
Tienen todas las dependencias que necesitan para funcionar correctamente.
Funcionan igual en todos lados.

**Eficientes**:
Comparten archivos simultáneos con otros contenedores.
Solo se ejecutan procesos, no un SS.OO completo.

**Aislados**:
Lo que pasa en el container queda en el container.
No pueden alterar su entorno de ejecución (a menos que explícitamente se indique lo contrario)


## Representacion Grafica del uso

![VM usa mas recursos que el docker ya que solo ejecuta procesos sin el S.O como tal](https://blogs.bmc.com/wp-content/uploads/2018/07/containers-vs-virtual-machines.jpg)

## Arquitectura Docker

![Arquitectura](https://ualmtorres.github.io/SeminarioDockerPresentacion/images/DockerEngine.png)

##  Comandos Utiles

[Commands of Docker Basic](https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf)

## Contenedores

Los contenedores son el **concepto fundamental** al hablar de docker. Un contenedor es una entidad lógica, una agrupación de procesos que se ejecutan de forma nativa como cualquier otra aplicación en la máquina host.

Un contenedor ejecuta sus procesos de forma nativa

* Es una agrupación de procesos, uno o más procesos.
* Es una entidad lógica, no tiene el limite estricto de las máquinas virtuales, emulación del sistema operativo simulado por otra más abajo.
* Ejecuta sus procesos de forma nativa.
* Los procesos que se ejecutan adentro de los contenedores ven su universo como el contenedor lo define, no pueden ver mas allá del contenedor, a pesar de estar corriendo en una maquina más grande.
* No tienen forma de consumir más recursos que los que se les permite. Si esta restringido a menos memoria ram, es la única que pueden usar.
* A fines prácticos los podemos imaginar cómo maquinas virtuales, pero NO lo son.
* Cuando un contenedor es ejecutado, el daemon de docker le dice, a partir de acá para arriba este disco es tuyo, pero no puedes subir mas arriba.
* Docker hace que los procesos adentro de un contenedor este aislados del resto del sistema, no le permite ver más allá.
* Cada contenedor tiene un ID único, también tiene un nombre.

# Comandos

## Listado

### Lista de contenedores activos

``
	docker ps
``

### Lista de imagenes

` docker images `

### Lista de contenedor activos/inactivos con detalles

``
	docker ps -a
``

### Obtener una lista de ids en base a los contenedores

``
	docker ps -aq
``

### Obtener una lista de ids en base a las imagenes

` docker images -q `

## Inspeccionar

### Ver toda la informacion

``
	docker inspect nombre_contenedor
``

### Aplicar un filtro para la informacion

``
	docker inspect -f {{ .Propiedad}}
``

Ejemplo

> docker inspect -f {{json .Config}}

## Logs

### Ver los registros que se tienen para un contenedor

``
	docker logs container_name
``

## Borrar contenedor

### Eliminar contenedor inactivo

``
	docker rm nombre_contenedor
``

### Borrar todos los contenedores que esten inactivos

``
	docker container prune
``

### Eliminar todos los contenedores esten apagados/activos

`docker rm -f $(docker ps -aq)`

### Detener el contenedor y eliminarlo

``
	docker rm -f MyContainer
``

## Borrar imagenes

### Borrar todas las imagenes inactivas

``
	docker image prune
``


### Borrar imagen especifica 

` docker rmi nombreImagen`

### Borrar imagen especifica forzadamente 

` docker rmi -f nombreImagen`

## Cambiar el nombre o alias de un contenedor

### Renombrar el alias

``
	docker rename nombre nuevoNombre
``

Ejemplo



CONTAINER ID |   	IMAGE 	|		COMMAND 	|	CREATED		|	STATUS	|	PORTS 	|		NAMES
---|	---	|	---|		--- | 	---	|		---	|	---
EDC89CFD5EB4|	HELLO-WORLD	|	"/HELLO"|		2 HOURS AGO| 	EXITED(0)	|			|	eager_carson

> docker rename eager_carson hola_mundo 

## Correr docker

### Correr directamente a la terminal y de manera interactiva

` docker run -ti ubuntu `

### Correr un contenedor

` docker run nombre `

### Correr un contenedor con un nombre personalizado

` docker run --name hello-there hello-world`

> hello-world es una imagen asi que solo tendrias que asignarle la imagen que deseas y darle otro nombre customizado que quieras :)

### Correr un contenedor en segundo plano

` docker run nombre -d `

### Correr contenedor en un puerto especifico y no entrar directamente en la ejecucion del mismo

`docker run -p 80:80 -d nombreImagen`

> 80 => Puerto de la maquina donde se corre docker
> > 80 => Puerto del contenedor

### Parar un contenedor corriendo

`docker container stop nombreContenedor`

### Reiniciar un contenedor

`docker container restart nombreContenedor`

### Correr un proceso en un contenedor corriendo

`docker exec -it nombreContenedor bash`

> i => Interactive
>> t => Terminal
>>> Bash => Procces

### Correr un contenedor con un command manual

`docker run --name alwaysup -d ubuntu tail -f /dev/null `

> luego de ubuntu viene a ser nuestro command de lanzamiento para el contenedor

### Obtener el process id de un contenedor

`docker inspect --format '{{.State.Pid}}' alwaysup`

### Ver logs de un contenedor

`docker logs nombreContenedor`

### Seguimiento de logs a un contenedor

`docker logs -f nombreContenedor`

### Ver las ultimas 10 entradas de logs a un contenedor docker rm -f $(docker ps -aq)

`docker logs --tail 10 -f nombreContenedor`

### Construir una imagen en base a un dockerfile y indicar (.) donde existe el archivo para poder crearlo

`docker build -t nameImage .`

### Corre una imagen por detras (detach) en el puerto 80 (puedes cambiarlo al que necesites)

`docker run -d -p 8080:80 nameImage`