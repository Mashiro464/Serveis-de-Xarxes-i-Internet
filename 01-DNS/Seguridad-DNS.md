# Seguridad en DNS

## 1. Introducción

El protocolo DNS fue diseñado en los primeros años de Internet, cuando la seguridad no era una prioridad. Como consecuencia, no incorpora mecanismos nativos de autenticación ni cifrado, lo que lo hace vulnerable a distintos tipos de ataques.

El servicio DNS utiliza principalmente el protocolo UDP en el puerto 53. Al no establecer una conexión previa ni verificar la identidad del remitente, puede ser susceptible a falsificación de respuestas si no se implementan medidas adicionales de protección.

---

## 2. Principales amenazas en DNS

El sistema DNS puede verse afectado por distintos tipos de ataques orientados a redirigir el tráfico o manipular la resolución de nombres.

### 2.1 Pharming

El pharming es una técnica que redirige a los usuarios desde un sitio web legítimo hacia uno fraudulento, incluso cuando la URL introducida es correcta.

El objetivo suele ser:

- Robo de credenciales
- Obtención de datos bancarios
- Distribución de malware

### 2.2 DNS Hijacking (Secuestro DNS)

El DNS hijacking consiste en redirigir el tráfico DNS hacia un servidor controlado por un atacante.

Existen tres modalidades principales:

#### 2.2.1 Secuestro local

Se produce cuando el archivo `hosts` del sistema es modificado por malware.

En Linux:
```
/etc/hosts
```


En Windows:
```
C:\Windows\System32\drivers\etc\hosts
```

#### 2.2.2 Secuestro del router

El atacante modifica la configuración DNS del router, afectando a todos los dispositivos conectados a la red.

Suele producirse cuando:

- El router mantiene credenciales por defecto
- El firmware no está actualizado
- La red WiFi tiene baja seguridad

#### 2.2.3 Secuestro del servidor DNS

Ataque dirigido a servidores DNS en Internet.  
Es menos frecuente debido a las medidas de seguridad implementadas en infraestructuras críticas.

### 2.3 DNS Spoofing o Cache Poisoning

Consiste en introducir información falsa en la caché DNS de un resolver.

El atacante envía una respuesta falsificada antes de que llegue la respuesta legítima del servidor autoritativo. Si el resolver acepta la respuesta falsa, esta se almacena en caché hasta que expire el TTL.

---

## 3. Vulnerabilidad del uso de UDP

El protocolo DNS utiliza principalmente UDP, que:

- No establece conexión previa
- No verifica la identidad del remitente
- Permite la falsificación de paquetes

Esto facilita ataques de suplantación si no existen mecanismos adicionales de seguridad.

---

## 4. Medidas de protección

Para reducir los riesgos asociados al DNS se recomienda:

- Cambiar las credenciales por defecto del router
- Mantener actualizado el firmware del router
- Utilizar servidores DNS seguros
- Implementar DNS sobre HTTPS (DoH)
- Implementar DNS sobre TLS (DoT)
- Instalar soluciones antimalware
- Revisar periódicamente el archivo `hosts`

---

## 5. DNSSEC

### 5.1 ¿Qué es DNSSEC?

DNSSEC (Domain Name System Security Extensions) es un conjunto de extensiones que añade autenticación e integridad al sistema DNS mediante el uso de criptografía de clave pública.

No cifra la información, pero garantiza que los datos no han sido modificados y que provienen del servidor legítimo.

### 5.2 Funcionamiento

DNSSEC introduce nuevos tipos de registros:

- RRSIG → Firma digital del registro
- DNSKEY → Clave pública
- DS → Delegation Signer
- NSEC / NSEC3 → Negación autenticada de existencia

El sistema crea una cadena de confianza desde la raíz hasta el dominio consultado.

Si alguna firma no coincide, la respuesta se considera inválida y se descarta.

### 5.3 Limitaciones

- Implementación compleja
- No todos los dominios lo utilizan
- No cifra el tráfico, únicamente valida autenticidad e integridad

---

## 6. Conclusión

La seguridad en DNS es un aspecto fundamental en la infraestructura de red. Aunque el protocolo original presenta vulnerabilidades inherentes a su diseño, la aplicación de buenas prácticas y la implementación de tecnologías como DNSSEC permiten mitigar significativamente los riesgos.

Una correcta configuración y mantenimiento del sistema DNS es esencial para garantizar la integridad, autenticidad y disponibilidad de los servicios en red.
