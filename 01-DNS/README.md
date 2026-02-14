# DNS – Domain Name System

## 1. Introducción

El Domain Name System (DNS) es un sistema distribuido y jerárquico encargado de traducir nombres de dominio en direcciones IP.

Cuando un usuario introduce una URL en el navegador (por ejemplo, www.ifp.es), el sistema DNS traduce ese nombre en una dirección IP (por ejemplo, 213.192.253.78), permitiendo que el equipo cliente se comunique con el servidor correspondiente.

El DNS pertenece a la capa de aplicación del modelo OSI y utiliza el puerto 53, principalmente sobre el protocolo UDP, aunque también puede emplear TCP.

---

## 2. Funcionamiento del DNS

El proceso de resolución de nombres sigue una estructura jerárquica:

1. El usuario introduce un dominio en el navegador.
2. El resolver recursivo comprueba si tiene la respuesta en caché.
3. Si no la tiene, consulta a un servidor raíz.
4. El servidor raíz indica el servidor TLD correspondiente (.com, .es, etc.).
5. El servidor TLD indica el servidor autoritativo del dominio.
6. El servidor autoritativo devuelve la dirección IP.
7. El resolver entrega la IP al navegador.
8. El navegador establece la conexión HTTP/HTTPS con el servidor.

Este proceso permite una resolución eficiente y distribuida a escala global.

---

## 3. Arquitectura del DNS

El sistema DNS está compuesto por:

- Espacio de nombres jerárquico
- Resource Records (RR)
- Servidores autoritativos
- Solucionadores recursivos
- Protocolo de consulta-respuesta

### 3.1 DNS Autoritativo

Contiene los registros oficiales del dominio y responde con la información definitiva.

### 3.2 DNS Recursivo

Actúa como intermediario entre el cliente y los servidores autoritativos. Puede almacenar respuestas en caché para mejorar el rendimiento.

---

## 4. Espacio de Nombres y FQDN

El DNS utiliza una estructura jerárquica en forma de árbol cuya raíz se representa con un punto (.).

Ejemplo de FQDN:

**www.ejemplo.com.**

- www → host
- ejemplo → dominio de segundo nivel
- .com → dominio de nivel superior (TLD)
- . → raíz

---

## 5. Registros DNS (Resource Records)

La unidad básica del DNS es el Resource Record (RR).

Estructura general:

**<NAME> <TTL> <CLASS> <TYPE> <RDATA>**

- NAME → nombre del dominio
- TTL → tiempo de vida en caché (en segundos)
- CLASS → normalmente IN (Internet)
- TYPE → tipo de registro (A, AAAA, MX, CNAME…)
- RDATA → valor asociado (IP, alias, servidor…)

### 5.1 Tipos de registros más comunes

**A**  
Asocia un dominio a una dirección IPv4.

**AAAA**  
Asocia un dominio a una dirección IPv6.

**CNAME**  
Alias de otro dominio. Siempre apunta a otro nombre, nunca directamente a una IP.

Ejemplo:
blog.smx.es → CNAME → smx.es

**MX**  
Define los servidores de correo del dominio.

**NS**  
Indica los servidores de nombres del dominio.

**SOA**  
Define el servidor autoritativo principal y los datos administrativos de la zona.

---

## 6. Instalación y Configuración de un Servidor DNS (BIND9)

### 6.1 Instalación en Ubuntu / Debian


Actualizar el sistema:
```
sudo apt update
```


Instalar BIND9:
```
sudo apt install bind9 bind9utils bind9-doc
```


Verificar estado del servicio:
```
sudo systemctl status bind9
```


---

### 6.2 Configuración de zona directa

Editar el archivo:
```
sudo nano /etc/bind/named.conf.local
```


Añadir la siguiente configuración:
```
zone "midominio.local" {
    type master;
    file "/etc/bind/db.midominio.local";
};

```


Crear el archivo de zona:
```
sudo nano /etc/bind/db.midominio.local
```


Ejemplo de contenido del archivo de zona:
```
$TTL 604800
@   IN  SOA ns.midominio.local. admin.midominio.local. (
        2         ; Serial
        604800    ; Refresh
        86400     ; Retry
        2419200   ; Expire
        604800 )  ; Negative Cache TTL

```


Reiniciar el servicio:
```
sudo systemctl restart bind9
```


---

### 6.3 Comprobación

Desde el cliente:
```
nslookup www.midominio.local
```
```
dig www.midominio.local
```


---