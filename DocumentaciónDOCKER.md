ACTIVIDADES MOODLE SOBRE DOCKER - MIGUEL ÁNGEL GRANDE SÁNCHEZ
______________________________________________________________________________________________________________________

1- PRÁCTICA 1

![image](https://github.com/user-attachments/assets/8980ff0e-3bb4-45f2-ade8-907ae196fe61)

Lo primero que hay que hacer es desinstalar los paquetes que puedan dar problemas a futuro:

for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

![image](https://github.com/user-attachments/assets/4ebed60a-7fa5-47d3-9754-581cf12936ec)

Lo siguientes es hacer el set-up del repositorio de docker con los siguientes comandos:

```
sudo apt-get update
sudo apt-get install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

```

Y añado el repositio en el apt

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && sudo apt-get update
```

![image](https://github.com/user-attachments/assets/e0931c6a-0244-46a0-bc5b-da8c38cfc061)

Lo siguiente es instalar los paquetes de Docker con
```bash

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

![image](https://github.com/user-attachments/assets/9d0d1dd7-e431-4cfd-83ac-4512260fd22d)

Por último compruebo que todo esté correcto con:
```bash

sudo docker run hello-world

```

![image](https://github.com/user-attachments/assets/19a7e1e9-3106-4f59-8169-cf5d6bab5319)

______________________________________________________________________________________________________________________

2- PRÁCTICA 2

![image](https://github.com/user-attachments/assets/26dcedae-bb82-467a-af5a-b8e06a27778f)

Para crear una imagen hello-world simplemente y como he mostrado antes con el comando
```bash

sudo docker run hello-world
```

![image](https://github.com/user-attachments/assets/4723ded6-2eba-4ee4-81e6-7061825885e5)

Para mostrar las imágenes creadas, se utiliza:
```bash


sudo docker images
```

![image](https://github.com/user-attachments/assets/589560a1-f06a-44b0-b357-0df108a43857)

Para mostrar los contenedores en docker se realiza con:
```bash

sudo docker ps
```

![image](https://github.com/user-attachments/assets/c94c5f37-ed41-4eca-8a70-9b6fb3eee975)

Y para verlos todos docker ps -a


![image](https://github.com/user-attachments/assets/49dd3d8d-8659-4dda-bfdd-c787e5a1fe19)

Lo siguiente que voy a crear y editar es un dockerfile con:
```

mkdir mi_proyecto
cd mi_proyecto
nano Dockerfile
```

![image](https://github.com/user-attachments/assets/9428d1e8-c928-4864-b9b7-c71626ecceac)

Y coloco algo básico dentro del Dockerfile
```

# Indica la imagen base
FROM ubuntu:latest

# Actualiza paquetes e instala, por ejemplo, curl
RUN apt-get update && apt-get install -y curl

# Mensaje o comando que se ejecutará por defecto
CMD ["echo", "Hola desde mi contenedor!"]
```


![image](https://github.com/user-attachments/assets/7134b561-a067-4db4-8708-81cce167e80c)

Para crear mi contenedor, colocamos el siguiente comando en el terminar:
```

docker build -t proyecto .
```

![image](https://github.com/user-attachments/assets/fc56c7fa-6251-44a6-9a80-d4cd32ad7be8)

Y compruebo con:
```

docker images
```

![image](https://github.com/user-attachments/assets/20c87273-0961-4f65-a4cf-33a3f302bd1d)

Para correr la imagen se usa
```

docker run proyecto
```

![image](https://github.com/user-attachments/assets/2fe2a344-7e32-4264-bfb1-62a779b0f963)

Me creo una cuenta en hub.docker.com

![image](https://github.com/user-attachments/assets/f3b7f2c0-9275-4501-8d0d-4cb0e072bdce)

Creo el repositorio proyecto

![image](https://github.com/user-attachments/assets/5749195b-cf73-42f3-961a-a2562abc695c)


Para subir mi imagen a Docker Hub, primero debo hacer login:
```

docker login
```

![image](https://github.com/user-attachments/assets/f0d2dfca-4091-40b7-a2ee-296815d9f2fa)

![image](https://github.com/user-attachments/assets/37bb6c87-7e24-4a44-ad33-d0e2e89ff3c2)

Después, es importante que la imagen tenga la etiqueta que incluya mi usuario de Docker Hub. Por ejemplo, si mi usuario es xmigue28, etiqueto mi imagen así:

```docker tag proyecto xmigue28/proyecto:latest```

![image](https://github.com/user-attachments/assets/00fc72a7-6c82-45cf-89ce-dc0f808e2c42)

Y por último, subo el contenedor a Docker Hub

```docker push xmigue28/proyecto:latest```

![image](https://github.com/user-attachments/assets/4c2c6c7a-9fe7-4ce5-9c6f-81a00b7c22e4)

Ahora veo en mi repositorio si se ha subido:

![image](https://github.com/user-attachments/assets/fa2dea0e-a148-4fd7-8d70-d52006827a0d)


3- PRÁCTICA 3

Para descargar la imagen de ubuntu hay que hacer lo siguiente:

```docker pull ubuntu```

![image](https://github.com/user-attachments/assets/73c7c2af-0b8f-49fc-9636-8d509291ded5)

Lo mismo para hello-world:

```docker pull hello-world```

![image](https://github.com/user-attachments/assets/2f880c0a-3300-445d-a2c1-7e449ea6e602)

Y por último con la imagen de Ngix:

```docker pull nginx```

![image](https://github.com/user-attachments/assets/554adb8a-f51f-4372-bd8e-46d17350eb5c)

Y para ver el listado es:

```docker images```

![image](https://github.com/user-attachments/assets/1ced71f3-011f-4dd2-b9c0-b6118ed82e02)

Para ejecutar el contenedor y darle un nombre, debe de hacerse de esta manera: 

```docker run --name myhello1 hello-world```

![image](https://github.com/user-attachments/assets/98c7c929-3cdf-4604-a58d-dfd8f357f3ef)

Igual para el 2:

```docker run --name myhello1 hello-world```

![image](https://github.com/user-attachments/assets/0698a4f8-7f40-4292-a2fe-382af44baceb)


Lo mismo para el 3:

```docker run --name myhello1 hello-world```

![image](https://github.com/user-attachments/assets/ab82ee6b-1408-40fb-9fe9-3c22176aaca9)

Para ver todos los contenedores:

```sudo docker -ps a```

![image](https://github.com/user-attachments/assets/e5bda61d-a533-4f9a-ac36-6d7e386e53b2)

Para parar los contenedores se utiliza:

```
docker stop myhello1
docker stop myhello2
docker stop myhello3
```

![image](https://github.com/user-attachments/assets/cb807944-24fc-4b06-9a7f-c9ad449f03b4)

Para borrar los contenedores se ejecuta:
```
docker rm myhello1
docker rm myhello1
docker rm myhello1
```
![image](https://github.com/user-attachments/assets/9f9dd5d3-0b9d-4e49-91dd-289161f8ce1e)

Ahora los muestro con:
```
sudo docker -ps a
```
![image](https://github.com/user-attachments/assets/06f9648c-7f17-436b-a01d-8bddd6c94ed1)

Para borrar TODOS los contenedores:
```
docker rm -f $(docker ps -aq)
```
![image](https://github.com/user-attachments/assets/ce390fcb-d9f7-4ca5-afdd-b7e495bf2abe)

______________________________________________________________________________________________________________________

4. PRÁCTICA 4

Voy a coger los ejemplos 1, 2 y 3 para hacer esta tarea.

EJEMPLO 1 - Despliegue de la aplicación Guestbook.

La aplicación guestbook por defecto utiliza el nombre redis para conectarse a la base de datos, por lo tanto debemos nombrar al contenedor redis con ese nombre para que tengamos una resolución de nombres adecuada.

Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que ip va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:
```
docker network create red_guestbook
```
![image](https://github.com/user-attachments/assets/53360535-1aed-4242-a2e1-2ea833e56106)

Para ejecutar los contenedores:
```
docker run -d --name redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes
```
![image](https://github.com/user-attachments/assets/35d7fbe4-6713-4846-aeac-cb7a2664c67f)
```
docker run -d -p 80:5000 --name guestbook --network red_guestbook iesgn/guestbook
```
![image](https://github.com/user-attachments/assets/eaeaf834-9f76-442d-9624-64af10041e36)

Para ver la aplicación de GuestBook, simplemente colocamos localhost en el navegador:

![image](https://github.com/user-attachments/assets/a42e1569-248c-4c39-bbe1-f8df03fca032)

EJEMPLO 2 - Despliegue de la aplicación Temperaturas

El microservicio frontend se conecta a backend usando el nombre temperaturas-backend. Por lo tanto el contenedor con el micorservicio backend tendrá ese nombre para disponer de una resolución de nombres adecuada en el dns.

Vamos a crear una red para conectar los dos contenedores:
```
docker network create red_temperaturas
```
![image](https://github.com/user-attachments/assets/c697daa7-cdf8-43df-ab6e-3c8715b81258)

Para ejecutar los contenedores:
```
docker run -d --name temperaturas-backend --network red_temperaturas iesgn/temperaturas_backend
```
![image](https://github.com/user-attachments/assets/bcc0b124-ecf0-449a-8b6b-e8f649ff56b7)
```
docker run -d -p 80:3000 --name temperaturas-frontend --network red_temperaturas iesgn/temperaturas_frontend
```
![image](https://github.com/user-attachments/assets/62d4350f-2436-4fe2-8dc6-6519b143da3e)

Luego me meto en localhost y aparecerá la app

![image](https://github.com/user-attachments/assets/dc13473a-c11f-43d8-814e-9946bb481562)

EJEMPLO 3 - Despliegue de Wordpress + mariadb

Para la instalación de WordPress necesitamos dos contenedores: la base de datos (imagen mariadb) y el servidor web con la aplicación (imagen wordpress). Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que ip va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:
```
docker network create red_wp
```
![image](https://github.com/user-attachments/assets/dd7c2468-ce79-465d-830b-e16fc5db03fc)

Siguiendo la documentación de la imagen mariadb y la imagen wordpress podemos ejecutar los siguientes comandos para crear los dos contenedores:
```
docker run -d --name servidor_mysql \
                --network red_wp \
                -v /opt/mysql_wp:/var/lib/mysql \
                -e MYSQL_DATABASE=bd_wp \
                -e MYSQL_USER=user_wp \
                -e MYSQL_PASSWORD=asdasd \
                -e MYSQL_ROOT_PASSWORD=asdasd \
                mariadb
```
![image](https://github.com/user-attachments/assets/be35a43f-5d3f-40f8-929d-0a0f469827da)
```
docker run -d --name servidor_wp \
                --network red_wp \
                -v /opt/wordpress:/var/www/html/wp-content \
                -e WORDPRESS_DB_HOST=servidor_mysql \
                -e WORDPRESS_DB_USER=user_wp \
                -e WORDPRESS_DB_PASSWORD=asdasd \
                -e WORDPRESS_DB_NAME=bd_wp \
                -p 80:80 \
                wordpress
```
![image](https://github.com/user-attachments/assets/c5de5d1f-42f9-4d28-b795-e389a89d4919)

Y muestro todos los contenedores con:
```
docker ps
```
![image](https://github.com/user-attachments/assets/2403ef8e-8675-48e4-a489-7c6c7035b19a)

Luego voy a localhost y ya estaría terminado el punto.

![image](https://github.com/user-attachments/assets/eb7c312b-7a26-4b05-bbba-d184feeca089)

______________________________________________________________________________________________________________________

5. PRÁCTICA 5

En esta práctica cogeré los 3 ejemplos.

EJEMPLO 1 - Despliegue de la aplicación guestbook

Creo mi propio docker-compose.yml con las siguientes líneas: 
```
version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    environment:
      REDIS_SERVER: redis
    ports:
      - 8080:5000
  db:
    container_name: redis
    image: redis
    restart: always
    command: redis-server --appendonly yes
    volumes:
      - redis:/data
volumes:
  redis:
```
![image](https://github.com/user-attachments/assets/3c5bc2d1-380b-4f86-91fe-5a1813985788)

Creo el escenario con:
```
docker compose up -d
```
![image](https://github.com/user-attachments/assets/baa39726-2fbb-478d-8a80-0c77f539f336)

Para ver los contenedores subidos se utiliza:
```
docker compose ps
```
![image](https://github.com/user-attachments/assets/695d77af-8d9c-4620-bb20-015bbd5f9556)
































