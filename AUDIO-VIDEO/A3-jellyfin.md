### Resumen Jellyfin

Jellyfin es un servidor de medios de código abierto y gratuito que permite organizar, gestionar y transmitir contenido multimedia (películas, series, música) a diferentes dispositivos. A diferencia de otras plataformas, no tiene cuotas de suscripción ni funciones bloqueadas, ofreciendo total privacidad al usuario.


### Proceso de instalacion y Configuracion

Se tienen que crear la estructura de carpetas en la VM para organizar los datos de configuracion, caché y los archivos multimedia.

```Bash
mkdir config cache media
```

<img width="374" height="63" alt="image" src="https://github.com/user-attachments/assets/4fb82026-85e3-4ec3-be64-a69a57f384b5" />


### Despliege con Docker Compose

Con Docker Compose podemos hacer que jellyfin corra de forma aislada

```Bash
sudo docker-compose up -d
```

<img width="742" height="347" alt="image" src="https://github.com/user-attachments/assets/a2a82327-bc84-4d98-8081-3e96b3a0b1b3" />

## Configuración del Servidor

Aqui definimos el nombre del servidor

<img width="1009" height="626" alt="image" src="https://github.com/user-attachments/assets/cfa39b0e-6677-4775-9cfc-7aaa60297028" />

El usuario y la contraseña para acceder a el

<img width="1001" height="749" alt="image" src="https://github.com/user-attachments/assets/e50d5dd0-ad38-4428-944e-55cfaccd3447" />

El tipo de archivo que querremos guardar en esa carpeta

<img width="848" height="228" alt="image" src="https://github.com/user-attachments/assets/1fceb22c-fd34-4fbf-9c72-1d59a72c6816" />

Donde estaran los archivos

<img width="839" height="186" alt="image" src="https://github.com/user-attachments/assets/ce55356e-003d-4cdf-ad17-363ca870b051" />

Descargamos una cancion para tener una muestra

<img width="1003" height="128" alt="image" src="https://github.com/user-attachments/assets/31502a7b-cf3f-4556-89d5-28e7727b0fd6" />

Para poder conectarnos con otros dispositivos

<img width="917" height="261" alt="image" src="https://github.com/user-attachments/assets/5f06005b-64fb-46b1-ac78-087ae769b171" />

Aqui se puede ver como esta en la maquina virtual y en el navegador de mi ordenador abierto

<img width="1721" height="821" alt="image" src="https://github.com/user-attachments/assets/9c733810-a8e2-443f-ba23-74bc75e3b375" />



### Demostracion Movil

El video esta en la carpeta


## Comparativa Jellyfin contra Plex

| Característica | Jellyfin | Plex |
| :--- | :--- | :--- |
| **Licencia** | Código Abierto (Open Source) | Propietario |
| **Coste** | 100% Gratis | Freemium (Plex Pass necesario) |
| **Privacidad** | Alta (Datos locales) | Recopila datos de uso |
| **Transcodificación** | Gratuita por hardware | Requiere pago (Plex Pass) |






