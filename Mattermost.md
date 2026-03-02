# Guía de Instalación de Mattermost en Ubuntu Server

------------------------------------------------------------------------
**Servidor:** Ubuntu 22.04 LTS\
**IP Local:** 192.168.109.60\
**Puerto:** 8065
# 🔐 Configuración de SSH (Puerto 22)

## 📌 Descripción

En este proyecto utilizamos **SSH (Secure Shell)** para la
administración remota del servidor.

El acceso se realiza mediante el **puerto 22**, que es el puerto
estándar del protocolo SSH.\
La conexión se realiza desde Windows usando **MobaXterm (MobaXtreme)**.

------------------------------------------------------------------------

## 🎯 Objetivos

-   Permitir acceso remoto seguro al servidor.\
-   Administrar el sistema desde otro equipo.\

------------------------------------------------------------------------

## 🌐 Conexión desde MobaXterm

1.  Abrir **MobaXterm**.\
2.  Ir a **Session → SSH**.\
3.  Introducir:
    -   IP del servidor\
    -   Puerto: **22**\
    -   Usuario\
4.  Conectar.

------------------------------------------------------------------------

## 🔒 Seguridad

-   Se utiliza el **puerto 22 (por defecto en SSH)**.\
-   Se recomienda usar contraseñas seguras y limitar accesos
    innecesarios.

------------------------------------------------------------------------

## 📁 Resultado

El servidor puede administrarse de forma remota mediante **SSH en el
puerto 22**, utilizando **MobaXterm**, garantizando una conexión segura
y cifrada.

------------------------------------------------------------------------

## 1. Requisitos del Sistema

-   **SO:** Ubuntu Server (Instalación mínima)
-   **Base de Datos:** PostgreSQL v14+
-   **Red:** Puerto 8065 abierto para tráfico TCP

------------------------------------------------------------------------

## 2. Instalación de la Base de Datos (PostgreSQL)

Se instaló PostgreSQL para gestionar el almacenamiento de mensajes y
usuarios.

### Instalación de paquetes

``` bash
sudo apt install postgresql postgresql-contrib -y
```

### Configuración de base de datos y usuario

Acceder a la consola:

``` bash
sudo -u postgres psql
```

Ejecutar:

``` sql
CREATE DATABASE mattermost;
CREATE USER mmuser WITH PASSWORD '***';
GRANT ALL PRIVILEGES ON DATABASE mattermost TO mmuser;
ALTER DATABASE mattermost OWNER TO mmuser;
```

------------------------------------------------------------------------

## 3. Instalación de Mattermost Server

Se utilizó el repositorio oficial PPA para facilitar las actualizaciones
automáticas.

### Configuración del repositorio

Importar llave y configurar:

``` bash
curl -o- https://deb.packages.mattermost.com/repo-setup.sh | sudo bash -s mattermost
```

### Instalación

``` bash
sudo apt update && sudo apt install mattermost -y
```

------------------------------------------------------------------------

## 4. Configuración de la Aplicación

Archivo de configuración:

    /opt/mattermost/config/config.json

### Parámetros modificados

**SqlSettings (DataSource):**

    postgres://mmuser:password123@localhost:5432/mattermost?sslmode=disable&connect_timeout=10

**ServiceSettings (SiteURL):**

    http://192.168.109.60:8065

------------------------------------------------------------------------

## 5. Gestión del Servicio

``` bash
sudo systemctl start mattermost
sudo systemctl enable mattermost
sudo systemctl restart mattermost
```

------------------------------------------------------------------------

## 6. Configuración de Usuarios (Pruebas)

-   **Administrador:** Creado mediante el asistente web inicial.
-   **Usuario de prueba:**
    -   Nombre: pato
    -   Email: pato@gmail.com
    -   Invitación generada mediante el *Invite Link* del equipo.

------------------------------------------------------------------------

## 7. Troubleshooting

### Firewall

``` bash
sudo ufw allow 8065/tcp
```

### WebSockets

La **SiteURL** en la consola de administración debe coincidir
exactamente con la IP y el puerto usados en el navegador.

### Consultas a la Base de Datos

``` sql
SELECT username, email FROM users;
```
