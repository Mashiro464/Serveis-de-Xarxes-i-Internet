# 🛡️ Mi Servidor de Correo Seguro (Reto Postfix + ClamAV)

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

Este proyecto es el resultado de mi práctica configurando un servidor de
correo en Ubuntu.
La idea era no solo que el servidor enviara y recibiera correos, sino
que fuera capaz de detectar virus y borrarlos antes de que llegaran a mi
bandeja de entrada.

------------------------------------------------------------------------

## 🚀 ¿Qué es lo que he montado?

He usado **Postfix** (el que mueve los correos) y lo he conectado con
**ClamAV** (el antivirus).
La clave de todo es el **Milter**, que hace que Postfix le pase el
correo al antivirus *"en el aire"* para que lo revise en tiempo real.

------------------------------------------------------------------------

## 🧪 La prueba de fuego: ¿Funciona?

Para no usar un virus real, utilicé el test **EICAR**.
Es un texto que no hace nada malo, pero que todos los antivirus
reconocen como amenaza para poder hacer pruebas.

### 🔥 Comando utilizado (Swaks)

``` bash
swaks --to conesa@ifp-GDC       --server 10.10.10.10       --body 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*'
```
<img width="901" height="574" alt="image" src="https://github.com/user-attachments/assets/1ce2127f-a8f2-46f4-8a28-4f65979acf8c" />

### ✅ Resultado (¡Éxito!)

El servidor respondió con:

    <- 550 5.7.1 Command rejected

Esto significa que el antivirus detectó la amenaza y el servidor rechazó
el correo antes de que entrara.

------------------------------------------------------------------------

## 🛠️ Lo que tuve que configurar

### 1️⃣ Postfix (`/etc/postfix/main.cf`)

Añadí líneas para decirle a Postfix que consulte al antivirus antes de
aceptar correos:

``` conf
milter_default_action = tempfail
smtpd_milters = inet:127.0.0.1:7357
```


-   `milter_default_action = tempfail` → Si el antivirus falla, el
    correo no entra (modo seguro).
-   `smtpd_milters` → Dirección y puerto donde escucha ClamAV.

<img width="688" height="744" alt="image" src="https://github.com/user-attachments/assets/9d5a53ea-75e8-4666-8ff5-ab4a794f212a" />
------------------------------------------------------------------------

### 2️⃣ ClamAV (`/etc/clamav/clamav-milter.conf`)

Configuración importante:

    OnInfected Reject
    MilterSocket inet:7357@127.0.0.1

-   `OnInfected Reject` → Si hay virus, se rechaza directamente.
-   `MilterSocket inet:7357@127.0.0.1` → Comunicación por puerto TCP
    para evitar problemas de permisos (AppArmor).
<img width="588" height="410" alt="image" src="https://github.com/user-attachments/assets/dbd08c7b-0674-4fd4-82fc-ecf3e9aa6f64" />
------------------------------------------------------------------------

## 🔍 Cosas que he aprendido

-   **Escaneo en tiempo real** → El virus no llega a guardarse en disco.
-   **No solo existen virus** → Para SPAM se usan herramientas como
    SpamAssassin o Rspamd.
-   **SPF y DKIM** → Son "sellos digitales" que validan que el servidor
    es legítimo.
-   **Fail2Ban** → Protege contra ataques de fuerza bruta.

------------------------------------------------------------------------

## 📋 Problemas que tuve (Troubleshooting)

  ------------------------------------------------------------------------
  Problema                  Causa                Solución
  ------------------------- -------------------- -------------------------
  Error 451                 Postfix no podía     Revisar permisos o usar
                            hablar con ClamAV    socket TCP

  Correo aceptado (250 OK)  OnInfected estaba en Cambiar a Reject
                            cuarentena           
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 📈 Conclusión

Montar un servidor de correo es relativamente sencillo, pero hacerlo
seguro es el verdadero reto.

El uso de un **Milter** permite proteger el sistema en tiempo real y
aplicar una política estricta:\
👉 Si el servidor no está seguro de que el correo está limpio,
simplemente lo rechaza.

# 📋 Preguntas y Respuestas -- Postfix + ClamAV

------------------------------------------------------------------------

## 1️⃣ ¿Qué es ClamAV y qué detecta?

Es un antivirus de código abierto para Linux.\
Detecta virus, troyanos y malware en los archivos adjuntos (como el test
EICAR que usamos).

------------------------------------------------------------------------

## 2️⃣ ¿Por qué no se integra directo y qué usa de puente?

Porque hablan "idiomas" distintos (SMTP vs Escaneo).\
Usan un puente llamado **Milter** (en este caso, *clamav-milter*).

------------------------------------------------------------------------

## 3️⃣ ¿Qué hace clamav-milter?

Actúa como un filtro en tiempo real: recibe el correo de Postfix, lo
pasa al antivirus y le da el veredicto antes de que el correo se guarde.

------------------------------------------------------------------------

## 4️⃣ ¿Qué pasa si detecta un virus?

Según nuestra configuración (`OnInfected Reject`), el servidor rechaza
el correo inmediatamente y devuelve un error **550**, impidiendo que el
virus entre.

------------------------------------------------------------------------

## 5️⃣ ¿Qué directivas se usan en Postfix?

-   `smtpd_milters` → Indica dónde está el antivirus.
-   `milter_default_action` → Define qué hacer si el antivirus falla
    (configurado en `tempfail` para bloquear por seguridad).

------------------------------------------------------------------------

## 6️⃣ ¿Limitaciones y complementos?

ClamAV es limitado detectando **Spam** y **Phishing**.\
Se complementa con:

-   **SpamAssassin / Rspamd** → Para detección de spam.
-   **SPF / DKIM** → Para evitar suplantación de identidad (spoofing).
