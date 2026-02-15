# FTP (File Transfer Protocol)

## 📌 Descripción
Configuración de un servidor FTP para la transferencia de archivos en red utilizando vsftpd en Linux.

---

## 🎯 Objetivos
- Instalar un servidor FTP
- Configurar usuarios locales
- Permitir acceso local y remoto
- Verificar el funcionamiento del servicio

---

## 🛠 Instalación

### 1️⃣ Actualizar repositorios
```
sudo apt update
```
### 2️⃣ Instalar vsftpd
```
sudo apt install vsftpd -y
```
### 3️⃣ Comprobar estado del servicio
```
sudo systemctl status vsftpd
```
### ⚙ Configuración básica
Editar el archivo de configuración:

```
sudo nano /etc/vsftpd.conf
```
Asegurarse de que estén activadas las siguientes opciones:
```
conf

anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
```
### Guardar cambios y reiniciar el servicio:
```
sudo systemctl restart vsftpd
```
### 👤 Crear usuario FTP
```
sudo adduser usuarioftp
```
### Asignar permisos al directorio:

```
sudo chmod 755 /home/usuarioftp
```
### 🔥 Configuración del Firewall
Permitir FTP en el puerto 21:

```
sudo ufw allow 21/tcpvv
sudo ufw reload
```
### 🧪 Prueba de conexión
Desde otro equipo o cliente FTP:

Servidor: IP_del_servidor

Usuario: usuarioftp

Contraseña: ********

Puerto: 21

### 📌 Puertos utilizados
Puerto	Función
21	Canal de control
20	Canal de datos (modo activo)

### ✅ Conclusión
Se ha instalado y configurado correctamente un servidor FTP utilizando vsftpd, permitiendo la transferencia de archivos de manera controlada mediante usuarios locales.