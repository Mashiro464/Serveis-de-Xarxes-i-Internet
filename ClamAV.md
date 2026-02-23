# 🛡️ Mi Servidor de Correo Seguro (Reto Postfix + ClamAV)

Este proyecto es el resultado de mi práctica configurando un servidor de
correo en Ubuntu.\
La idea era no solo que el servidor enviara y recibiera correos, sino
que fuera capaz de detectar virus y borrarlos antes de que llegaran a mi
bandeja de entrada.

------------------------------------------------------------------------

## 🚀 ¿Qué es lo que he montado?

He usado **Postfix** (el que mueve los correos) y lo he conectado con
**ClamAV** (el antivirus).\
La clave de todo es el **Milter**, que hace que Postfix le pase el
correo al antivirus *"en el aire"* para que lo revise en tiempo real.

------------------------------------------------------------------------

## 🧪 La prueba de fuego: ¿Funciona?

Para no usar un virus real, utilicé el test **EICAR**.\
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
<img width="688" height="744" alt="image" src="https://github.com/user-attachments/assets/9d5a53ea-75e8-4666-8ff5-ab4a794f212a" />
-   `milter_default_action = tempfail` → Si el antivirus falla, el
    correo no entra (modo seguro).
-   `smtpd_milters` → Dirección y puerto donde escucha ClamAV.


------------------------------------------------------------------------

### 2️⃣ ClamAV (`/etc/clamav/clamav-milter.conf`)

Configuración importante:

    OnInfected Reject
    MilterSocket inet:7357@127.0.0.1

-   `OnInfected Reject` → Si hay virus, se rechaza directamente.
-   `MilterSocket inet:7357@127.0.0.1` → Comunicación por puerto TCP
    para evitar problemas de permisos (AppArmor).
<img width="688" height="744" alt="image" src="https://github.com/user-attachments/assets/9d5a53ea-75e8-4666-8ff5-ab4a794f212a" />
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
