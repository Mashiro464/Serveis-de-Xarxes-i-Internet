# 🦆 Proyecto Chat Privado: Servidor Ejabberd (Pato)

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

Este repositorio documenta la instalación, configuración y despliegue de
un servidor de mensajería instantánea basado en el protocolo **XMPP**
utilizando **Ejabberd**. El proyecto permite la comunicación segura
entre clientes en una red local.

------------------------------------------------------------------------

## 📍 Detalles de la Infraestructura

  Rol                    Hostname            Dirección IP
  ---------------------- ------------------- ----------------
  **Servidor de Chat**   `pato.pato.local`   `10.10.10.10`
  **Cliente de Chat**    `cliente-pato`      `10.10.10.105`

------------------------------------------------------------------------

## 🚀 Guía de Instalación Paso a Paso

### 1️⃣ Configuración del Servidor (`10.10.10.10`)

Instalación del servicio:

``` bash
sudo apt update && sudo apt install ejabberd -y
```

Editar archivo de configuración:

📄 `/etc/ejabberd/ejabberd.yml`

``` yaml
hosts:
  - "pato.pato.local"

acl:
  admin:
    user:
      - "admin@pato.pato.local"
```
<img width="711" height="586" alt="image" src="https://github.com/user-attachments/assets/94208b38-43a6-4890-909d-d069b9284211" />

Reiniciar servicio:

``` bash
sudo systemctl restart ejabberd
```

------------------------------------------------------------------------

### 2️⃣ Gestión de Usuarios

``` bash
# Registrar cuenta de administrador
sudo ejabberdctl register admin pato.pato.local ******

# Registrar cuenta de usuario estándar
sudo ejabberdctl register punky pato.pato.local ******
```
<img width="563" height="64" alt="image" src="https://github.com/user-attachments/assets/bf042fe4-423e-4429-8caa-d55856090d16" />

------------------------------------------------------------------------

### 3️⃣ Configuración del Cliente (`10.10.10.105`)

Editar archivo:

``` bash
sudo nano /etc/hosts
```

Añadir:

    10.10.10.10    pato.pato.local
<img width="406" height="35" alt="image" src="https://github.com/user-attachments/assets/59b31407-b20a-4765-bd2c-bc6c14902f6b" />

#### Configuración en Pidgin

-   Protocolo: XMPP\
-   Usuario: `admin` o `punky`\
-   Dominio: `pato.pato.local`\
-   Servidor de conexión: `10.10.10.10`\
-   Seguridad: "Usar cifrado si está disponible"

------------------------------------------------------------------------

## 🛠️ Troubleshooting

  Error                 Solución
  --------------------- ----------------------------------------------
  DNS lookup failed     Usar IP 10.10.10.10 en servidor de conexión
  Error SSL             Configurar "Usar cifrado si está disponible"
  Acceso denegado web   Usar JID completo `admin@pato.pato.local`

------------------------------------------------------------------------

## 🌐 Panel de Administración Web

-   URL: http://10.10.10.10:5280/admin\
-   Usuario: `admin@pato.pato.local`

<img width="423" height="426" alt="image" src="https://github.com/user-attachments/assets/c7cfdff4-01c8-47db-ae11-4077ed3e63e7" />

<img width="425" height="429" alt="image" src="https://github.com/user-attachments/assets/584cd1d4-8ad3-4b1f-bd57-58a5895252e8" />

