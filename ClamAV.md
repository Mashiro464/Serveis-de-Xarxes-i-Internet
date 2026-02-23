# 🛡️ Servidor de Correo Seguro con Postfix y ClamAV (Lab)

Este proyecto documenta la implementación y securización de un servidor de correo basado en **Postfix** sobre Ubuntu, integrando un sistema de escaneo de malware en tiempo real mediante **ClamAV-Milter**.

## 🚀 Escenario de la Prueba
Para verificar la seguridad del servidor, se ha utilizado la herramienta `swaks` para enviar un mensaje que contiene la firma de virus **EICAR**. El objetivo es que el servidor identifique la amenaza y rechace la conexión antes de que el correo sea procesado.

### Resultado Exitoso
Al realizar el test, el servidor responde con un código de rechazo, confirmando que el filtro funciona correctamente:
`<- 550 5.7.1 Command rejected`

---

## 🛠️ Configuración del Sistema

### 1. Postfix (`/etc/postfix/main.cf`)
Se configuró Postfix para actuar como un gateway que consulta al antivirus mediante un socket:
* **milter_default_action = tempfail**: Si el antivirus no está disponible, el correo se bloquea temporalmente (fail-closed).
* **smtpd_milters**: Se redirige el flujo de datos al puerto o socket del escáner.

### 2. ClamAV Milter (`/etc/clamav/clamav-milter.conf`)
Los parámetros clave para garantizar el bloqueo fueron:
* `OnInfected Reject`: Ordena el rechazo inmediato del mensaje si se detecta un virus.
* `MilterSocket`: Configurado para permitir la comunicación fluida con Postfix.
* `OnFail Defer`: Protege el sistema en caso de caída del servicio de escaneo.

---

## 🔍 Investigación de Seguridad (Hardening)

Como parte de la investigación de seguridad en servidores de correo, se han considerado los siguientes puntos para un entorno de producción:

1.  **Detección en tiempo real (SMTP-level scanning)**:
    El escaneo ocurre durante la fase `DATA` de la sesión SMTP. Esto evita que el malware llegue a tocar el sistema de archivos del usuario final, cortando la amenaza en el perímetro.

2.  **Arquitectura de Red**:
    En este laboratorio se ha optimizado la comunicación mediante Sockets. Para entornos de alta carga, se recomienda la migración a **Sockets TCP** para separar el servicio de antivirus en una máquina dedicada (Scalability).

3.  **Capas de Defensa Adicionales**:
    * **SpamAssassin / Rspamd**: Para filtrar no solo virus, sino también phishing y correo basura basado en reputación.
    * **Implementación de SPF, DKIM y DMARC**: Vital para prevenir el *spoofing* y asegurar que los correos enviados desde nuestro dominio `ifp-GDC` sean legítimos.
    * **Fail2Ban**: Implementado para bloquear ataques de fuerza bruta contra el puerto 25 (SMTP) y el puerto 587 (Submission).
<img width="901" height="574" alt="image" src="https://github.com/user-attachments/assets/1ce2127f-a8f2-46f4-8a28-4f65979acf8c" />

---

## 📈 Conclusión
La integración de ClamAV con Postfix a través de la interfaz Milter proporciona una defensa robusta y eficiente. El éxito de este laboratorio demuestra que una configuración correcta de los parámetros de rechazo (`OnInfected Reject`) es fundamental para mantener la integridad de los buzones de correo.

## 🛠️ Herramienta de Auditoría: Swaks (Swiss Army Knife for SMTP)

Para las pruebas de penetración y verificación de seguridad, se ha utilizado **Swaks**, una herramienta de línea de comandos extremadamente flexible para probar servidores SMTP.

### ¿Por qué Swaks?
A diferencia de un cliente de correo convencional (como Outlook o Thunderbird), Swaks permite:
* **Forzar el cuerpo del mensaje**: Introducir manualmente la cadena EICAR sin que un antivirus local lo bloquee antes de enviarlo.
* **Simulación de protocolos**: Probar diferentes etapas de la negociación SMTP (EHLO, MAIL FROM, RCPT TO, DATA).
* **Depuración (Debug)**: Ver las respuestas exactas del servidor (códigos 250, 451, 550) en tiempo real.

### Comando utilizado en el laboratorio:
```bash
swaks --to conesa@ifp-GDC \
      --server 10.10.10.10 \
      --body 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' \
      --header "Subject: Test de Seguridad Antivirus"```
