ACTIVIDADES MOODLE SOBRE DOCKER - MIGUEL ÁNGEL GRANDE SÁNCHEZ
______________________________________________________________________________________________________________________

1- PRÁCTICA 1

![image](https://github.com/user-attachments/assets/8980ff0e-3bb4-45f2-ade8-907ae196fe61)

Lo primero que hay que hacer es desinstalar los paquetes que puedan dar problemas a futuro:

for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

![image](https://github.com/user-attachments/assets/4ebed60a-7fa5-47d3-9754-581cf12936ec)

Lo siguientes es hacer el set-up del repositorio de docker con los siguientes comandos:

sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

Y añado el repositio en el apt

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

![image](https://github.com/user-attachments/assets/e0931c6a-0244-46a0-bc5b-da8c38cfc061)

Lo siguiente es instalar los paquetes de Docker con

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

![image](https://github.com/user-attachments/assets/9d0d1dd7-e431-4cfd-83ac-4512260fd22d)

Por último compruebo que todo esté correcto con:

sudo docker run hello-world

![image](https://github.com/user-attachments/assets/19a7e1e9-3106-4f59-8169-cf5d6bab5319)

______________________________________________________________________________________________________________________

2- PRÁCTICA 2

![image](https://github.com/user-attachments/assets/26dcedae-bb82-467a-af5a-b8e06a27778f)

Para crear una imagen hello-world simplemente y como he mostrado antes con el comando

sudo docker run hello-world

![image](https://github.com/user-attachments/assets/4723ded6-2eba-4ee4-81e6-7061825885e5)

Para mostrar las imágenes creadas, se utiliza:

sudo docker images

![image](https://github.com/user-attachments/assets/589560a1-f06a-44b0-b357-0df108a43857)

Para mostrar los contenedores en docker se realiza con:

sudo docker ps

![image](https://github.com/user-attachments/assets/c94c5f37-ed41-4eca-8a70-9b6fb3eee975)

Y para verlos todos docker ps -a

![image](https://github.com/user-attachments/assets/49dd3d8d-8659-4dda-bfdd-c787e5a1fe19)

Lo siguiente que voy a crear y editar es un dockerfile con:

mkdir mi_proyecto
cd mi_proyecto
nano Dockerfile

![image](https://github.com/user-attachments/assets/9428d1e8-c928-4864-b9b7-c71626ecceac)

Y coloco algo básico dentro del Dockerfile

# Indica la imagen base
FROM ubuntu:latest

# Actualiza paquetes e instala, por ejemplo, curl
RUN apt-get update && apt-get install -y curl

# Mensaje o comando que se ejecutará por defecto
CMD ["echo", "Hola desde mi contenedor!"]


![image](https://github.com/user-attachments/assets/7134b561-a067-4db4-8708-81cce167e80c)

Para crear mi contenedor, colocamos el siguiente comando en el terminar:

docker build -t proyecto .

![image](https://github.com/user-attachments/assets/fc56c7fa-6251-44a6-9a80-d4cd32ad7be8)

Y compruebo con:

docker images

![image](https://github.com/user-attachments/assets/20c87273-0961-4f65-a4cf-33a3f302bd1d)

Para correr la imagen se usa

docker run proyecto

![image](https://github.com/user-attachments/assets/2fe2a344-7e32-4264-bfb1-62a779b0f963)

Me creo una cuenta en hub.docker.com

![image](https://github.com/user-attachments/assets/f3b7f2c0-9275-4501-8d0d-4cb0e072bdce)

Para subir mi imagen a Docker Hub, primero debo hacer login:

docker login

![image](https://github.com/user-attachments/assets/f0d2dfca-4091-40b7-a2ee-296815d9f2fa)

![image](https://github.com/user-attachments/assets/37bb6c87-7e24-4a44-ad33-d0e2e89ff3c2)

Después, es importante que la imagen tenga la etiqueta que incluya mi usuario de Docker Hub. Por ejemplo, si mi usuario es xmigue28, etiqueto mi imagen así:

docker tag proyecto xmigue28/proyecto:latest

![image](https://github.com/user-attachments/assets/00fc72a7-6c82-45cf-89ce-dc0f808e2c42)

Y por último, subo el contenedor a Docker Hub

docker push xmigue28/proyecto:latest




























