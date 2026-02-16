# NGINX

## 📌 Descripción
NGINX es un servidor web de alto rendimiento que también puede funcionar como proxy inverso y balanceador de carga.  
Está diseñado con una arquitectura asíncrona basada en eventos, lo que le permite manejar miles de conexiones simultáneamente con bajo consumo de memoria.

---

## 🎯 Objetivos
- Instalar NGINX en Ubuntu
- Configurar el servicio
- Crear un servidor web funcional
- Configurar un Virtual Host
- Entender el funcionamiento como Reverse Proxy

---

## 🛠 Instalación

#### Actualizar repositorios:
```
sudo apt update
```

#### Instalar NGINX:
```
sudo apt install nginx -y
```

#### Comprobar estado del servicio:
```
sudo systemctl status nginx
```

#### Iniciar el servicio (si no está activo):
```
sudo systemctl start nginx
```

#### Habilitar inicio automático:
```
sudo systemctl enable nginx
```

#### Comprobar funcionamiento desde el navegador:
```
http://localhost
```

## 📂 Estructura principal
#### Archivo de configuración principal:
```
/etc/nginx/nginx.conf
```
#### Carpeta de sitios disponibles:
```
/etc/nginx/sites-available/
```
#### Carpeta de sitios habilitados:
```
/etc/nginx/sites-enabled/
```
#### Directorio web por defecto:
```
/var/www/html
```
## ⚙ Parámetros importantes
### worker_processes
Define el número de procesos worker.
Recomendado: igual al número de cores del servidor.
```
grep processor /proc/cpuinfo | wc -l
```
### worker_connections
Número máximo de conexiones simultáneas por worker.

Ejemplo:
```
nginx
Copiar código
events {
    worker_connections 1024;
}
```

### keepalive_timeout
Tiempo que mantiene abierta una conexión persistente.
```
keepalive_timeout 65;
```

### gzip
Activa compresión para mejorar rendimiento:
```
gzip on;
```

## 🌐 Crear Virtual Host
#### Crear directorio del sitio:
```
sudo mkdir -p /var/www/misitio
```
#### Asignar permisos:
```
sudo chown -R $USER:$USER /var/www/misitio
```
#### Crear archivo de configuración:
```
sudo nano /etc/nginx/sites-available/misitio
```
#### Contenido del archivo:
```
server {
    listen 80;
    server_name misitio.local;

    root /var/www/misitio;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
#### Activar el sitio:
```
sudo ln -s /etc/nginx/sites-available/misitio /etc/nginx/sites-enabled/
```
#### Comprobar configuración:
```
sudo nginx -t
```

#### Reiniciar NGINX:
```
sudo systemctl restart nginx
```

## 🔁 NGINX como Reverse Proxy
Ejemplo para redirigir a una aplicación en el puerto 3000:
```
server {
    listen 80;
    server_name app.local;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```