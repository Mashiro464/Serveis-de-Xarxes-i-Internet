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
estándar del protocolo SSH.
La conexión se realiza desde Windows usando **MobaXterm (MobaXtreme)**.

------------------------------------------------------------------------

## 🎯 Objetivos

-   Permitir acceso remoto seguro al servidor.
-   Administrar el sistema desde otro equipo.

------------------------------------------------------------------------

## 🌐 Conexión desde MobaXterm

1.  Abrir **MobaXterm**.
2.  Ir a **Session → SSH**.
3.  Introducir:
    -   IP del servidor
    -   Puerto: **22**
    -   Usuario
4.  Conectar.

------------------------------------------------------------------------

## 🔒 Seguridad

-   Se utiliza el **puerto 22 (por defecto en SSH)**.
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
sudo systemctl status ejabberd
```
Como quedo el servicio

<img width="708" height="333" alt="image" src="https://github.com/user-attachments/assets/3cc106ec-5e37-40c3-bb96-d0439beb2e96" />

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

1️⃣ Instalación de Pidgin
```
sudo apt install pidgin
```

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

------------------------------------------------------------------------

<img width="423" height="426" alt="image" src="https://github.com/user-attachments/assets/c7cfdff4-01c8-47db-ae11-4077ed3e63e7" />

<img width="425" height="429" alt="image" src="https://github.com/user-attachments/assets/584cd1d4-8ad3-4b1f-bd57-58a5895252e8" />

------------------------------------------------------------------------

# Arquitectura XMPP con Ejabberd

## ¿Qué puertos se utilizan y para qué?

En esta arquitectura XMPP con Ejabberd, intervienen principalmente los
siguientes puertos:

-   **Puerto 5222 (TCP):** Puerto estándar para comunicación
    Cliente-Servidor (C2S).\
    Lo utiliza Pidgin para conectarse a Ejabberd, autenticarse y
    enviar/recibir mensajes en tiempo real.

-   **Puerto 5280 (TCP):** Puerto del Panel de Administración Web.\
    Permite acceder desde el navegador a:\
    http://IP_SERVIDOR:5280/admin para gestionar usuarios y
    estadísticas.

------------------------------------------------------------------------

## ¿Qué aspectos de la configuración son importantes?

### 1️⃣ Definición del Dominio Lógico

En /etc/ejabberd/ejabberd.yml es fundamental definir correctamente el
apartado hosts\
(ejemplo: "gunter" o "pato.pato.local").

XMPP enruta los mensajes basándose en el dominio, no solo en la IP.

### 2️⃣ Resolución DNS Local (/etc/hosts)

Como no existe un DNS real en la red virtual, es necesario modificar el
archivo /etc/hosts\
en el cliente para asociar la IP del servidor con su nombre de dominio.

### 3️⃣ Uso del JID Completo

Es obligatorio usar el formato completo:

usuario@dominio

Ejemplo: usuario1@gunter

Usar solo el nombre de usuario provoca errores de autenticación.

### 4️⃣ Ajustes de Seguridad en el Cliente

En Pidgin se debe configurar la seguridad como:

"Usar cifrado si está disponible"

Esto es necesario porque Ejabberd suele usar certificados autofirmados
que pueden ser rechazados si se exige cifrado estricto.

------------------------------------------------------------------------

## ¿Qué incidencias técnicas se presentaron?

### Error: DNS lookup failed: non-existing domain

Problema:\
Pidgin intentaba resolver el dominio usando DNS públicos de Internet.

Solución:\
Se configuró directamente la IP del servidor en el campo\
"Servidor de conexión" en las opciones avanzadas de la cuenta.

------------------------------------------------------------------------

### Error: Necesita cifrado, pero no está disponible en este servidor

Problema:\
Pidgin intentaba establecer conexión TLS obligatoria.

Solución:\
Se cambió la opción de seguridad a\
"Usar cifrado si está disponible" en la pestaña Avanzado.

------------------------------------------------------------------------

### Acceso denegado al Panel Web

Problema:\
Se intentaba acceder usando solo el nombre corto del administrador (ej.
admin).

Solución:\
Se introdujo el JID completo en el login:

admin@dominio
