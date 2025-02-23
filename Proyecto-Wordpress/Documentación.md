La idea de este manual es configurar un servidor web apache montado en una instancia EC2 Debian (o Ubuntu) con un sistema de almacenamiento 
en EFS y la base de datos que esté gestionada por RDS en una subred privada.


________________________________________________________________________________________________________________________________________________________________________________

CREACIÓN Y CONFIGURACIÓN DE EC2, EFS Y RDS 

Lo primero que hay que hacer es crear un VPC con dos subredes públicas y dos subredes privadas.
![image](https://github.com/user-attachments/assets/e76702e5-bc65-4deb-97f9-41e7a6e1aae5)

Para hacerlo, en el buscador colocamos VPC 

![image](https://github.com/user-attachments/assets/a5b844dd-2134-40b2-8d59-a6ac970aa27d)

Pulsamos en Create VPC 

![image](https://github.com/user-attachments/assets/7acd0e81-48d9-4dda-a3f0-9df32b51d0be)

Con las siguientes especificaciones:

![image](https://github.com/user-attachments/assets/9e42d0dd-f958-4675-b5ed-d7d3255df5da)

![image](https://github.com/user-attachments/assets/3a363c3d-187d-471e-9d6f-741b952fd91c)

Ahora hay que crear las subredes públicas, me dirijo a Virtual Private Clouds - Subnets y la creamos con las siguientes especificaciones:

![image](https://github.com/user-attachments/assets/18f5dd50-841e-4231-ade1-0ca1985d1b09)

![image](https://github.com/user-attachments/assets/a1e39a43-5fbe-4038-9c27-32a826ffb0ee)


Hacemos lo mismo con la otra subred pública:

![image](https://github.com/user-attachments/assets/a19691fb-7427-40a6-bfe5-82ff7ebcd3fa)

Para las privadas, lo mismo

![image](https://github.com/user-attachments/assets/2f003bbe-51b1-45df-b3d5-355db974b269)

![image](https://github.com/user-attachments/assets/77fa184a-83b0-462d-a694-0c0605332ac4)

El siguiente paso es crear una puerta de enlace en Internet Gateways

![image](https://github.com/user-attachments/assets/300250d6-da44-4e6a-ad04-93ca2d1604c9)

![image](https://github.com/user-attachments/assets/009e75ad-0768-4583-a1ee-d58a883d9451)

Ahora la conecto a la VPC en Actions - Attach VPC 

![image](https://github.com/user-attachments/assets/502e3881-de67-4ea4-8b82-58b25debfb51)

![image](https://github.com/user-attachments/assets/87266976-53ca-4aa2-a000-d9342c888d48)

El siguiente paso es configurar las tablas de enrutamiento en Route Tables

![image](https://github.com/user-attachments/assets/a53c2e9d-0f35-45ec-be06-bed2f9b6006a)

Renombramos nuestro enrutamiento para una mejor organización 


![image](https://github.com/user-attachments/assets/26b9d6d9-7404-40f2-849c-8823de17cd62)

Pulsamos en Edit Routes

![image](https://github.com/user-attachments/assets/8633aa53-afab-45f4-a296-0aa016af4a89)

Busco mi gateway de Wordpress

![image](https://github.com/user-attachments/assets/885d92fb-bb3a-4169-8d56-796762475943)

![image](https://github.com/user-attachments/assets/6e596de8-b526-41a9-b005-dad7cdd1d809)

Vamos a subnet associations y agrego las subredes asociadas públicas

![image](https://github.com/user-attachments/assets/3371196a-7b54-406c-b090-73a3b8fe30c2)

Para las privadas haremos un proceso parecido creando otra Tabla de Rutas

![image](https://github.com/user-attachments/assets/cf80f919-b67f-4f89-9002-80141eea8c98)

Y metemos las rutas

![image](https://github.com/user-attachments/assets/96202011-2eaf-47dc-b080-6c4dc033d7ba)

Y tenemos todo listo para crear la instancia. Nos dirigimos a EC2 y Launch Instance

![image](https://github.com/user-attachments/assets/267bfc71-6281-4009-9177-ded468af6356)

Lo haremos con las siguientes especificaciones:

![image](https://github.com/user-attachments/assets/6ff64d4c-e986-4d8c-b15e-a22b321ee36f)

![image](https://github.com/user-attachments/assets/0f839c9a-e85c-42bb-9194-ec34e3e01366)

![image](https://github.com/user-attachments/assets/595f451a-c4f5-47b2-854e-834f680d8125)

![image](https://github.com/user-attachments/assets/6ab78eb1-afea-4c77-9463-10c0f5577566)

![image](https://github.com/user-attachments/assets/26a7598b-b2a0-4de1-81d1-b6ca1e4d18d8)

Y aquí el resumen de la Instancia

![image](https://github.com/user-attachments/assets/49b33940-208d-4f07-a495-9028421797ac)

Una vez creada, nos conectamos en Connect

![image](https://github.com/user-attachments/assets/f4e27243-ec22-40d7-8549-19fa29666744)

![image](https://github.com/user-attachments/assets/c656d89d-3746-4ec0-a8f9-88c6958166e0)

![image](https://github.com/user-attachments/assets/e9581e7a-8a5b-42e1-8222-bb6b2d2c8a3d)

________________________________________________________________________________________________________________________________________________________________________________

INSTALACIÓN PREVIA

Empezamos con sudo apt update

![image](https://github.com/user-attachments/assets/f069b689-dc0a-41c4-ad8b-e451cabd8833)

Seguimos con sudo apt updade -y

![image](https://github.com/user-attachments/assets/15eea12a-db62-43ca-b567-ae98dbe1ebfb)

Instalamos el servidor apache con sudo apt install apache2 -y

![image](https://github.com/user-attachments/assets/d24a7c62-38ce-4835-bf87-5921987ebee5)

iniciamos el servidor apache:sudo systemctl start apache2

![image](https://github.com/user-attachments/assets/a59d19ab-7512-442e-9d10-cb2af0914ccc)

Activo apache con sudo systemctl enable apache2 

![image](https://github.com/user-attachments/assets/7c6a92d5-6067-474f-9441-720a64a1e3ec)

Ahora instalo php con sudo apt install php libapache2-mod-php y sigo con php-mysql -y



![image](https://github.com/user-attachments/assets/2ce8a26a-69e5-46c5-b3c3-0d0f58afee1e)


Reinicio apache con sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/ae16c302-918f-4cbf-8565-cadb6e76f2aa)

Verificamos la instalacion creando un archivo .php:echo "" | sudo tee /var/www/html/info.php

![image](https://github.com/user-attachments/assets/a728f28d-4782-4703-8a77-69c4f67924d8)

Ahora accedo al servidor para comprobar que funciona todo correctamente en el siguiente enlace de mi instancia 44.192.130.154/info.php

![image](https://github.com/user-attachments/assets/9dc7ad9b-8f48-4508-a56d-a9f81be07147)

Ahora voy a eliminar el archivo para obtener una mayor seguridad con sudo rm /var/www/html/info.php

![image](https://github.com/user-attachments/assets/ae5c51f8-5843-4835-9ef7-01057b593403)

Ahora accedo mediante el mismo enlace y vemos que ya no existe

![image](https://github.com/user-attachments/assets/f60b91fb-7966-435f-9c7e-f8459c8df49f)

________________________________________________________________________________________________________________________________________________________________________________

CREACIÓN DE BASE DE DATOS

El primer paso es buscar RDS en el buscador de AWS

![image](https://github.com/user-attachments/assets/469d5b2d-b4c6-4f23-8494-ab407e7e1623)

Creo una Database

![image](https://github.com/user-attachments/assets/26f058e7-d571-4efa-8ce8-70e1bf1a9ecc)

Con las siguientes especificaciones: 

![image](https://github.com/user-attachments/assets/58d55562-628f-47f0-96e2-029977c16da7)

![image](https://github.com/user-attachments/assets/5ba9fc83-3510-4796-bbdc-ec7d272d5953)

![image](https://github.com/user-attachments/assets/13f2590c-f355-4d08-a85b-f6fac5ab8be9)

![image](https://github.com/user-attachments/assets/0eb94a4c-d020-40f6-8248-0937aa4a2093)

![image](https://github.com/user-attachments/assets/84dd064f-1907-47dd-854f-9fc749c72f32)

![image](https://github.com/user-attachments/assets/fcfd1848-dbea-4dc1-a8ea-a621e2801633)

![image](https://github.com/user-attachments/assets/af82f32d-8766-4e45-b938-38d4c6c09204)

![image](https://github.com/user-attachments/assets/400a9f43-4abb-4d15-adf3-04a9f52f18f4)

![image](https://github.com/user-attachments/assets/b9f2f1c8-441f-450f-ba15-73bf8969fcc6)

Con todas estas especificaciones, creo la base de datos.

El siguiente paso es la creación de un sistema de archivos con el almacenamiento EFS para EC2, en el buscador coloco EFS

![image](https://github.com/user-attachments/assets/860ecf1c-7630-452e-bbf4-66bd77431dce)

Y creo uno con las siguientes especificaciones;

![image](https://github.com/user-attachments/assets/5da11a78-7ab8-4060-bdbd-59957445f7f8)

Ahora me dirijo a mi instancia de AWS y es necesario la instalación del paquete nfs-common con sudo apt install nfs-common

![image](https://github.com/user-attachments/assets/b166c7f0-570e-4cc8-8f34-f8f639afddb2)

El siguiente paso es 


montar el EFS en la instancia con el siguiente comando: sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 
fs-099a50b9aaa57f35a.efs.us-east-1.amazonaws.com:/ /mnt/efs

![image](https://github.com/user-attachments/assets/acce349c-6d81-40eb-a188-30e5453a1212)

Para comprobar que todo está correcto, coloco ls -ld /mnt/efs

![image](https://github.com/user-attachments/assets/83e8a955-c144-4fe3-becf-a8a105d5a6ce)

Sigo con ls /mnt/efs y por último df -T

![image](https://github.com/user-attachments/assets/41928b6a-64b8-4b5e-88ba-c62d3a3e3129)

________________________________________________________________________________________________________________________________________________________________________________

INSTALACIÓN DE WORDPRESS

Para instalar Wordpress, simplemente colocamos cd /var/www/html sudo wget http://wordpress.org/latest.tar.gz 

![image](https://github.com/user-attachments/assets/b50f2b86-8bb6-469e-a7d0-a1e5930c3d8b)

sudo tar -xzf latest.tar.gz

![image](https://github.com/user-attachments/assets/9ed950a3-6dde-4c24-aed0-cd68e8e9610f)

Seguimos con la configuración de la base de datos que se usará para WordPress con sudo nano wp-config.php 
define( 'DB_NAME', 'db_wordpress' );
define( 'DB_USER', 'Miguel' );
define( 'DB_PASSWORD', 'Miguelitoh' );
define( 'DB_HOST', 'db-wordpress.cit7omfkhpgw.us-east-1.rds.amazonaws.com' );

![image](https://github.com/user-attachments/assets/cf0e5da6-0135-4487-8649-3347f5a66995)



Ahora vamos a comprobar que todo está correcto con mysql -u root -h db-wordpress.cit7omfkhpgw.us-east-1.rds.amazonaws.com -p que es el nombre de mi DNS
y la contraseña que he puesto anteriormente en mi db.

![image](https://github.com/user-attachments/assets/2aa2766b-3bad-43ce-ac21-8d9983143d00)

Paso a paso vamos colocando los siguientes pasos
CREATE DATABASE db_wordpress; 
CREATE USER 'Miguel'@'%' IDENTIFIED BY 'Miguelitoh'; 
GRANT ALL PRIVILEGES ON wordpress.* TO 'Miguel'@'%';
FLUSH PRIVILEGES;

![image](https://github.com/user-attachments/assets/6df688f4-9fa2-4e6d-964a-8e2ce3d27511)

Y estaría listo, simplemente llamamos al servidor web en el navegador de mi IP pública (http://44.192.130.154/wordpress)

![image](https://github.com/user-attachments/assets/1a792e03-8d98-4b13-aed0-517801f71ce5)

Una vez hecho, seguimos instalando Wordpress

![image](https://github.com/user-attachments/assets/03616d9d-8729-4a87-b9d5-44f6f2accd07)

Una vez terminada la instalación de WordPress, pues iniciamos sesión de manera normal y vemos que todo está correcto.

![image](https://github.com/user-attachments/assets/7e48a78c-c6cd-4037-b6a3-d2dfe297dd4f)

![image](https://github.com/user-attachments/assets/1d3348c3-146c-4e9a-abaf-85abb8c84c71)




Proyecto Realizado por Miguel Ángel Grande Sánchez







































































1- La primera parte del proyecto es crear la instancia con una AMI dentro de la subred pública 1.

![image](https://github.com/user-attachments/assets/8b65752c-3938-41fd-8cbc-35203ef3279c)
