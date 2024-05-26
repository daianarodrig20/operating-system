# TP9 - Docker

### 1. ¿Qué es Docker? ¿Explique su Arquitectura?

Docker es un servicio que permite ejecutar pequeños contenedores de software sobre el núcleo de nuestro sistema operativo, por lo que es una opción más rápida y liviana que el uso de máquinas virtuales. Docker utiliza características de aislamiento de recursos del kernel Linux, tales como cgroups y espacios de nombres (namespaces) para permitir que "contenedores" independientes se ejecuten dentro de una sola instancia de Linux, evitando la sobrecarga de iniciar y mantener máquinas virtuales.

La arquitectura Docker es una arquitectura cliente-servidor, donde el cliente habla con el servidor (que es un proceso daemon) mediante un API para poder gestionar el ciclo de vida de los contenedores y así poder construir, ejecutar y distribuir los contenedores.

### 2. Instalación de Docker en la VM (local o AWS)

#### Paso 1: Desinstalar versiones anteriores con el siguiente comando:

```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```
#### Paso 2: Configurar el repositorio de Docker

1. Permitir utilizar un repositorio sobre HTTPS:
``` sh
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```    
    
2. Agregar la clave GPG oficial de Docker:
``` sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3. Configurar el repositorio stable:

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Paso 3: Instalación de Docker Engine
Instalar la última versión de Docker Engine y del contenedor:

```sh
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Verificar que Docker Engine esté instalado correctamente:

```sh
sudo docker run hello-world
```


### Crear un contenedor con Apache
Crear un contenedor que contenga una imagen de un sistema operativo a elección e instalar Apache que escuche en el puerto 8080 del host.
Copiar el contenido de la carpeta default de Apache desde el host.
Verificar su funcionamiento.

```sh
> docker pull ubuntu
> docker run -it ubuntu
> apt install apache2
> docker ps
> docker commit reverent_payne ubuntu
> docker run -p 8080:80 --name mi_apache -td ubuntu
> docker ps -a
> docker exec -it <CONTAINER-ID> bash
```

Verificar en localhost:8080

### Crear un contenedor con MySQL o MariaDB
Crear un contenedor con MySQL o MariaDB que tenga un volumen del tipo Host para persistir los datos.
```sh
> docker pull mysql
> docker volume prune
> docker volume create mysql-db-data
> docker volume ls
> docker run -p 33060:3306 --name mysql-db -r MYSQL_ROOT_PASSWORD=admin --nount src=,ysql-db-data,dst=/var/lib/mysql -d mysql
```

### Generar datos en la base creada en el contenedor anterior
``` sh
sudo docker exec it myMysql mysql -p
```

### Borrar y recrear el contenedor
Borrar el contenedor anterior sin borrar el volumen.
Crear uno nuevo usando ese volumen y verificar que no se perdieron los datos.

``` sh
# docker rm -f mysql-db
# docker run -p 33060:3306 --name mysql-db -e MYSQL_ROOT_PASSWORD=<<>> -mount src=mysql-db-data,dst=/var/lib/mysql -d mysql
# docker exec .it mysql-db mysql -p
```

### Crear una red y asignarla a los contenedores anteriores
Crear una red y asignarla a los contenedores anteriores.
Verificar la interconexión entre ambos.
Bridge: La red estándar que usarán todos los contenedores.

```sh
docker network create --driver bridge miRed
```


### Crear un contenedor con WordPress con datos persistentes
Descargar la última versión de WordPress:
```sh
docker pull wordpress
```

Verificar que se creó el directorio y dentro de ese directorio, crear un Dockerfile:

```sh
mkdir wordpress
cd wordpress
touch Dockerfile
```

Editar el archivo Dockerfile con el siguiente contenido:

```sh
FROM wordpress:latest
```

Crear un archivo docker-compose.yml que iniciará nuestro servidor web y una instancia de MySQL por separado:
yaml

```sh
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
```

Generar el contenedor:
```sh
docker-compose up -d
```

De esta manera, el sistema construirá las imágenes necesarias e iniciará los contenedores web junto con la base de datos del proyecto.
5