## DHCP en Ubuntu — Instalación, Configuración y Seguridad

## 1. Introducción

DHCP (Dynamic Host Configuration Protocol) es un protocolo que permite asignar automáticamente direcciones IP y otros parámetros de red a los equipos de una red.

Funciona sobre un servidor central (firewall, router, servidor o estación de trabajo) que asigna direcciones IP a los clientes.

DHCP proporciona automáticamente:

- Dirección IP única
- Máscara de subred
- Puerta de enlace predeterminada
- Servidores DNS
- Configuración proxy (WPAD)

---

## 2. Puertos que utiliza DHCP

Según IANA y el RFC 2131:

- IPv4 → UDP 67 (servidor) y UDP 68 (cliente)
- IPv6 → UDP 546 y 547

### Puerto UDP 67
El servidor escucha en este puerto para recibir solicitudes DHCP.

### Puerto UDP 68
El cliente utiliza este puerto para enviar solicitudes y recibir respuestas.

La comunicación se realiza en la **capa 4 del modelo OSI** utilizando **UDP**.

---

## 3. Instalación del servidor DHCP en Ubuntu

```
sudo apt-get install isc-dhcp-server
```

---

## 4. Gestión del servicio

```
sudo service isc-dhcp-server status
sudo service isc-dhcp-server start
sudo service isc-dhcp-server restart
sudo service isc-dhcp-server stop
```
Antes de iniciarlo:

El servidor debe tener IP fija

Debe existir al menos una subred configurada

---

## 5. Configuración mediante Webmin

Webmin permite administrar el servidor DHCP mediante interfaz web.

Instalación

Opción 1:
```
sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl \
libpam-runtime libio-pty-perl apt-show-versions python

wget http://prdownloads.sourceforge.net/webadmin/webmin_1.590_all.deb
sudo dpkg -i webmin_1.590_all.deb
```

Opción 2 (repositorio):
```
sudo nano /etc/apt/sources.list
```

Añadir:
```
deb http://download.webmin.com/download/repository sarge contrib
```

```
wget http://www.webmin.com/jcameron-key.asc
sudo apt-key add jcameron-key.asc
sudo apt update
sudo apt install webmin
```

Acceso:
```
https://IP_SERVIDOR:10000
```

---

## 6. Configuración manual — /etc/dhcp/dhcpd.conf

Archivo principal:
```
/etc/dhcp/dhcpd.conf
```

Después de modificarlo:
```
sudo service isc-dhcp-server restart
```

---

## 7. Declaraciones principales
Subnet
```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8;
    default-lease-time 600;
    max-lease-time 7200;
}
```

Host (Reserva IP)
```
host equipo1 {
    hardware ethernet 00:11:22:33:44:55;
    fixed-address 192.168.1.10;
}
```

Shared-network

Agrupa varias subredes dentro de una misma red física.

Group

Permite agrupar hosts o subredes con parámetros comunes.

---

## 8. Parámetros importantes
```
authoritative;
default-lease-time 600;
max-lease-time 7200;
option subnet-mask 255.255.255.0;
option routers 192.168.1.1;
option broadcast-address 192.168.1.255;
option domain-name "empresa.local";
option domain-name-servers 8.8.8.8, 8.8.4.4;
```

---

## 9. Cliente DHCP en Ubuntu

Archivo configuración cliente:
```
/etc/dhcp/dhclient.conf
```

Archivo de concesiones:
```
/var/lib/dhcp/dhclient.leases
```

---

## 10.Comando dhclient

Solicitar IP:
```
sudo dhclient eth0
```

Liberar IP:
```
sudo dhclient -r eth0
```

Usar archivo configuración distinto:
```
sudo dhclient -cf /etc/dhcp/clientedhcp.conf
```

---

## 11. Agente de retransmisión DHCP (Relay)

Instalación:
```
sudo apt-get install isc-dhcp-relay
```

Configuración:
```
/etc/default/isc-dhcp-relay
```

Ejemplo:
```
SERVERS="192.168.56.34"
INTERFACES="eth1 eth2"
OPTIONS=""
```

Si existen distintas subredes, puede ser necesario añadir rutas:
```
up ip route add 192.168.200.0/24 via 192.168.100.202
```

---

## 12. Proceso DORA

DHCP DISCOVER

DHCP OFFER

DHCP REQUEST

DHCP ACK

Si falla → DHCP NAK

---

## 13. APIPA

Si no se encuentra servidor DHCP, el sistema asigna automáticamente una IP privada (169.254.x.x).

---

## 14. Métodos de asignación
Manual

Administrador asigna IP asociada a MAC.

Automática

IP fija basada en MAC registrada en el servidor.

Dinámica

IP temporal con tiempo de concesión (lease time).

---

## 15. Seguridad en DHCP

DHCP no es un protocolo autenticado.

Amenazas

Ataques DoS por agotamiento de IP

Servidor DHCP falso

Manipulación de DNS dinámico

Entrega de configuraciones maliciosas

Medidas de seguridad

Usar ACL

Monitorizar logs

Usar VLANs

IP estática en servidores críticos

Mantener servidor actualizado

Usar DHCPv6 con autenticación cuando sea posible