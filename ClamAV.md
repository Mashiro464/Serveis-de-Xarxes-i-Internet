# 🛡️ Servidor de Correo Seguro: Postfix + ClamAV (Lab)

Este repositorio contiene la documentación técnica, archivos de
configuración y pruebas de concepto para la implementación de un sistema
de correo seguro sobre Ubuntu Server 22.04/24.04.

------------------------------------------------------------------------

## 📖 Introducción

El objetivo de este proyecto es mitigar la entrada de software malicioso
en una infraestructura corporativa utilizando una arquitectura de
filtrado **Milter (Mail Filter)**.

En lugar de escanear el buzón una vez recibido el correo, el sistema
intercepta la conexión durante la sesión SMTP, analizando el contenido
en memoria y rechazando amenazas proactivamente.

------------------------------------------------------------------------

## 🚀 Escenario de la Prueba: El Test EICAR

Para validar la eficacia del sistema, se ha utilizado el archivo **EICAR
(European Institute for Computer Antivirus Research)**.\
No es un virus real, sino una cadena de texto estándar que todos los
antivirus del mundo deben identificar como una amenaza crítica.

### 🔎 Análisis del Flujo de Bloqueo

1.  **Handshake**: El cliente inicia la sesión.
2.  **Transmisión de Datos**: El cliente envía la cadena EICAR mediante
    el comando `DATA`.
3.  **Intercepción Milter**: Postfix pausa la entrega y envía el flujo
    de datos a ClamAV a través de un socket.
4.  **Veredicto**: ClamAV identifica la firma y devuelve una señal de
    *Reject*.
5.  **Respuesta SMTP**: Postfix corta la conexión con el código
    `550 5.7.1 Command rejected`.

------------------------------------------------------------------------

## 🛠️ Configuración Profunda del Sistema

### 1️⃣ Postfix (`/etc/postfix/main.cf`)

Se aplicaron directivas de endurecimiento para asegurar que ningún
correo ignore el escáner:

``` conf
# Configuración Milter
smtpd_milters = inet:127.0.0.1:7357
non_smtpd_milters = inet:127.0.0.1:7357

# Acción por defecto: Fail-Closed
# Si el milter cae, no se aceptan correos (Seguridad Máxima)
milter_default_action = tempfail

# Protocolo de comunicación Milter
milter_protocol = 6
```

------------------------------------------------------------------------

### 2️⃣ ClamAV Milter (`/etc/clamav/clamav-milter.conf`)

Configuración clave del motor de escaneo:

-   `OnInfected Reject` → El servidor no pone en cuarentena, rechaza
    directamente la conexión.
-   `MilterSocket inet:7357@127.0.0.1` → Uso de socket TCP para evitar
    problemas de permisos (AppArmor).

------------------------------------------------------------------------

## 🛠️ Auditoría con Swaks (Swiss Army Knife for SMTP)

Swaks permite comunicarse directamente con el servidor SMTP para
realizar pruebas controladas.

### 🔥 Simulación de Ataque

``` bash
swaks --to conesa@ifp-GDC       --server 10.10.10.10       --body 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*'
```

------------------------------------------------------------------------

## 📊 Tabla de Códigos Obtenidos

  Fase          Código      Resultado
  ------------- ----------- -----------------------------
  Conexión      220         Servidor listo
  Remitente     250 2.1.0   Dirección aceptada
  Envío Virus   550 5.7.1   Bloqueo exitoso por malware

------------------------------------------------------------------------

## 🔍 Investigación de Hardening y Escalabilidad

En un entorno de producción real, este laboratorio se expandiría con:

### A️⃣ Capa de Reputación (Rspamd)

-   **Greylisting** → Retrasa correos de servidores desconocidos.
-   **Bayes** → Aprende patrones de spam.

### B️⃣ Autenticación de Dominio (SPF, DKIM, DMARC)

-   **SPF** → Lista de IPs autorizadas.
-   **DKIM** → Firma criptográfica del mensaje.
-   **DMARC** → Política de acción si fallan SPF o DKIM.

------------------------------------------------------------------------

## 📋 Guía de Resolución de Problemas (Troubleshooting)

  ----------------------------------------------------------------------------
  Problema                   Causa               Solución
  -------------------------- ------------------- -----------------------------
  Error 451 (Service         Permisos en socket  Migración a socket TCP (7357)
  Unavailable)               Unix                

  Correo aceptado (250 OK)   OnInfected en       Cambiar a Reject
                             Quarantine          

  Logs                       Falta de monitoreo  Usar
                                                 `tail -f /var/log/mail.log` o
                                                 `journalctl`
  ----------------------------------------------------------------------------
<img width="901" height="574" alt="image" src="https://github.com/user-attachments/assets/1ce2127f-a8f2-46f4-8a28-4f65979acf8c" />
------------------------------------------------------------------------

## 📈 Conclusión

La implementación demuestra que la seguridad perimetral del correo
depende de la correcta orquestación entre el **MTA (Postfix)** y el
**Filtro (ClamAV)**.

La configuración **fail-closed** garantiza que, ante cualquier fallo del
sistema, la prioridad siempre sea la protección de la infraestructura.
