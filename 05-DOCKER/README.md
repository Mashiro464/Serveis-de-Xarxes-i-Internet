# 📑 Actividad: Despliegue de Aplicaciones con Docker (M0375)

Este documento recoge el desarrollo de la actividad práctica de Docker
correspondiente al módulo M0375 del ciclo ASIX. Se han trabajado
aspectos relacionados con la instalación del entorno, la gestión de
imágenes, redes, almacenamiento persistente y la orquestación de
servicios mediante Docker Compose.

------------------------------------------------------------------------

## 1. Configuración del Entorno en Ubuntu

Para la realización de la actividad se ha configurado Docker en un
sistema Ubuntu.

### Instalación

Se han utilizado los repositorios oficiales para instalar la versión
estable de:

-   docker-ce
-   docker-compose-plugin

### Post-instalación

Para poder ejecutar comandos Docker sin necesidad de usar sudo, se ha
añadido el usuario al grupo docker:

``` bash
sudo usermod -aG docker $USER
```

------------------------------------------------------------------------

## 2. Gestión de Imágenes y Redes

### Creación de Imágenes Propias

Se ha utilizado el comando docker commit para guardar el estado de un
contenedor tras realizar modificaciones manuales.

``` bash
docker commit -m "Apache con mod_apache" -a "TuNombre" [ID_CONTENEDOR] mi_apache:v1
```

### Arquitectura de Red

Se han utilizado:

-   Bridge (por defecto)
-   Red personalizada (Custom Network)

``` bash
docker network create redlocal
docker run -d --network=redlocal --name web1 nginx
```

------------------------------------------------------------------------

## 3. Almacenamiento y Volúmenes

Se han aplicado dos métodos principales:

### Docker Volumes

Gestionados directamente por Docker.

### Bind Mounts

Mapeo de un directorio del host al contenedor.

``` bash
docker run -d -p 80:80 -v $(pwd)/html:/usr/share/nginx/html nginx
```

Los datos almacenados en volúmenes permanecen aunque el contenedor sea
eliminado.

------------------------------------------------------------------------

## 4. Orquestación con Docker Compose

Ejemplo de archivo docker-compose.yml:

``` yaml
version: '3.8'

services:
  nginx:
    image: nginx
    container_name: miAppNginx
    ports:
      - "89:80"
    networks:
      - appnet

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: "tu_password"
    networks:
      - appnet

networks:
  appnet:
    driver: bridge
```

Comando para iniciar el stack:

``` bash
docker compose up -d
```

------------------------------------------------------------------------

## 5. Cuestionario Teórico

### ¿Qué son los contenedores?

Entornos aislados que empaquetan una aplicación y sus dependencias
compartiendo el kernel del sistema operativo host.

### Diferencia Imagen vs Contenedor

-   Imagen: plantilla inmutable.
-   Contenedor: instancia en ejecución de una imagen.

### Diferencia Docker vs LXC

-   Docker: orientado a aplicaciones.
-   LXC: orientado a contenedores de sistema completo.

### Ventajas

-   Portabilidad
-   Eficiencia
-   Escalabilidad
-   Aislamiento

### Tipos de Servicios

-   Microservicios
-   Bases de datos
-   Servidores web
-   Entornos de desarrollo
-   CI/CD

------------------------------------------------------------------------

## 6. Comandos Utilizados

  Comando                                         Función
  ----------------------------------------------- --------------------------------
  docker inspect \[ID\]                           Ver configuración detallada
  docker network connect \[red\] \[contenedor\]   Conectar a otra red
  docker compose up -d                            Levantar servicios
  docker system prune                             Limpiar recursos no utilizados
