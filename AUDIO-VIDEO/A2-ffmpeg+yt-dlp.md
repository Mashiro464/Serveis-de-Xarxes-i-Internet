# Parte Escrita


## Busca información acerca de los siguientes protocolos en cuanto a: funcionalidades principales, latencia, red, seguridad y compatibilidad?

### RTMP (Real-Time Messaging Protocol)

- Funcionalidad: Protocolo desarrollado por Adobe para transmisión en tiempo real.
- Latencia: Baja ideal para emisión en directo.
- Red: Basado en TCP.
- Seguridad: Puede usar RTMPS).
- Compatibilidad: Muy usado en YouTube, Twitch, pero en desuso para reproducción en navegadores modernos.

### HLS (HTTP Live Streaming)

- Funcionalidad: Protocolo de Apple que divide el vídeo en pequeños segmentos HTTP.
- Latencia: Media-alta aunque existe Low-Latency HLS.
- Red: HTTP.
- Seguridad: HTTPS, DRM integrado.
- Compatibilidad: Muy alta navegadores, móviles, smart TVs.

### RTSP (Real-Time Streaming Protocol)

- Funcionalidad: Control de streaming en tiempo real.
- Latencia: Baja.
- Red: TCP o UDP.
- Seguridad: Limitada.
- Compatibilidad: Amplia en dispositivos profesionales, limitada en navegadores.

### SRT (Secure Reliable Transport)

- Funcionalidad: Protocolo moderno para transmisión segura y fiable en redes inestables.
- Latencia: Baja y configurable.
- Red: UDP.
- Seguridad: Cifrado AES integrado.
- Compatibilidad: En crecimiento usado en broadcast profesional.










































# Parte Practica

### Instalamos FFmpeg

```bash
sudo apt update && sudo apt install -y ffmpeg
```

### Descargar YT-DLP

```bash
sudo wget https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -O /usr/local/bin/yt-dlp
```

### Permisos de ejecución
```bash
sudo chmod a+rx /usr/local/bin/yt-dlp
```
<img width="884" height="783" alt="image" src="https://github.com/user-attachments/assets/63775b80-ce33-4e8a-836a-17a70e03842c" />


## Paso 1 Ver formatos disponibles

Antes de descargar siempre es bueno saber que opciones ofrece el servidor

```bash
yt-dlp -F https://www.youtube.com/watch?v=Aq5WXmQQooo
```














