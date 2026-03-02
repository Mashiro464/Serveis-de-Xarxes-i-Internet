# 📧 Guía de Administración de Servicios de Correo

## Conceptos, Herramientas y Seguridad

------------------------------------------------------------------------

## 1️⃣ Fundamentos de los Servidores de Correo

Un servidor de correo es una aplicación que gestiona el ciclo de vida de
un mensaje electrónico. En el ámbito empresarial, es una herramienta de
importancia estratégica.

Sus funciones principales se dividen en tres pilares:

-   **Recepción**: Capta mensajes de otros usuarios.
-   **Distribución**: Envía correos hacia otros servidores de Internet.
-   **Gestión**: Entrega los mensajes a los buzones de los destinatarios
    correspondientes.

------------------------------------------------------------------------

## 2️⃣ Protocolos de Comunicación

  ------------------------------------------------------------------------
  Protocolo   Nombre Completo Puertos (Normal / Seguro) Función Principal
  ----------- --------------- ------------------------- ------------------
  SMTP        Simple Mail     25 / 587                  Envío de correos
              Transfer                                  entre servidores
              Protocol                                  

  IMAP        Internet        143 / 993                 Consulta. Los
              Message Access                            mensajes siguen en
              Protocol                                  el servidor

  POP3        Post Office     110 / 995                 Descarga. El
              Protocol v3                               correo suele
                                                        borrarse tras
                                                        bajarlo
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 3️⃣ Arquitectura del Sistema (MTA, MDA, MUA)

### 📮 MTA (Mail Transfer Agent)

La "Oficina de Correos". Recoge y entrega los mensajes.\
Ejemplos: Postfix, Exim, Sendmail.

### 📬 MDA (Mail Delivery Agent)

El "Buzón". Almacena el correo hasta que el usuario lo solicita.\
Ejemplos: Dovecot, Maildrop.

### 💻 MUA (Mail User Agent)

El "Cliente". Programa final que usa el usuario.\
Ejemplos: Thunderbird, Outlook, Mailutils (CLI).

------------------------------------------------------------------------

## 4️⃣ Herramientas de Administración y Configuración

### 📦 Postfix --- El MTA Principal

**Instalación:**

``` bash
sudo apt install postfix
```

**Archivo clave:**

    /etc/postfix/main.cf

**Gestión del servicio:**

``` bash
sudo systemctl status postfix
```

------------------------------------------------------------------------

### 📦 Dovecot --- El MDA y Servidor IMAP/POP3

**Instalación:**

``` bash
sudo apt install dovecot-imapd dovecot-pop3d
```

Permite configurar cuotas de disco para que los usuarios no saturen el
servidor.

------------------------------------------------------------------------

### 🔧 Mailutils --- La Navaja Suiza

``` bash
echo "Cuerpo del mensaje" | mail -s "Asunto de prueba" usuario@ejemplo.com
```

------------------------------------------------------------------------

## 5️⃣ Diagnóstico y Pruebas con Telnet

Comandos básicos en una sesión Telnet:

-   `EHLO`
-   `MAIL FROM:`
-   `RCPT TO:`
-   `DATA` (terminar con un punto `.` en línea nueva)
-   `QUIT`

------------------------------------------------------------------------

## 6️⃣ Seguridad y Filtrado de Contenido

### 🛡️ ClamAV (Antivirus)

``` bash
freshclam
clamscan
```

### 🚫 SpamAssassin (Filtro Anti-Spam)

``` bash
sa-update
```

Si supera el umbral (ej. 5.0), marca el correo como `[SPAM]`.

------------------------------------------------------------------------

## 7️⃣ Importancia del DNS (Registros MX)

``` bash
host -t MX nombre-dominio.com
```

