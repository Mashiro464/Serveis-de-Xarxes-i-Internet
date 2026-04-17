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

## Ver formatos disponibles

Antes de descargar siempre es bueno saber que opciones ofrece el servidor

```bash
yt-dlp -F https://www.youtube.com/watch?v=Aq5WXmQQooo
```

<img width="1846" height="655" alt="image" src="https://github.com/user-attachments/assets/fc656046-cf26-4016-a2a0-2e8b3a4fab26" />

## Descargar el video

Descargar el video con la mejor calidad de video y audio combinados

```Bash
yt-dlp -f "bv+ba/b" https://www.youtube.com/watch?v=Aq5WXmQQooo -o "rick_video.mp4"
```

<img width="998" height="333" alt="image" src="https://github.com/user-attachments/assets/e79277df-a809-41d4-b2f0-c997556aff48" />

#### Convertir a MP4

```Bash
ffmpeg -i video_origen.webm -vcodec libx264 -acodec aac video_final.mp4
```

<img width="1839" height="804" alt="image" src="https://github.com/user-attachments/assets/a33aa22b-c517-4c3b-bcb1-96276d73f0c7" />

#### Pasar el audio

```Bash
ffmpeg -i video_origen.webm -vn -acodec libmp3lame -ab 192k audio_final.mp3
```

<img width="1011" height="782" alt="image" src="https://github.com/user-attachments/assets/a90f6278-0e0f-4aa3-a22b-5db97d1ee06f" />

#### Verificacion

Una vez se finalizen los comandos se comprueba que todo esta en su lugar

<img width="591" height="114" alt="image" src="https://github.com/user-attachments/assets/1a7a295c-2dd0-49e9-b9e3-21804e3899ed" />


## Trancodificacion de Video


### Convertir a MKV ussando codec H.264

```Bash
ffmpeg -i video_final.mp4 -vcodec libx264 video_264.mkv
```

<img width="1015" height="778" alt="image" src="https://github.com/user-attachments/assets/7eba2d64-a2b4-48c3-887c-e79e4ee41b8b" />


### Convertir a MKV ussando codec H.265

```Bash
ffmpeg -i video_final.mp4 -vcodec libx265 video_265.mkv
```

<img width="1006" height="777" alt="image" src="https://github.com/user-attachments/assets/b5a321b0-cfc2-49b4-b421-77c1e6066fd0" />

- Diferencia técnica: El H.265 es mas eficiente que ell H.264. El archivo de H.265 suele ser de menor tamaño pero tendra la misma calidad visual

#### Comprobacion 

Aqui se me olvido hacerlo por eso es la misma captura que abajo

<img width="623" height="82" alt="image" src="https://github.com/user-attachments/assets/54d39b65-a69c-4622-903b-abff2992f1e6" />


## Modificación de Canales de Audio

### Audio en MP3

Aqui usamos `copy` para el video y solo cambiamos el formato del audio

```Bash
ffmpeg -i video_final.mp4 -vcodec copy -acodec mp3 h264_mp3.mkv
```

<img width="1000" height="762" alt="image" src="https://github.com/user-attachments/assets/71f66e15-bc53-49a9-9b27-d717e7be64f3" />


### Audion en AAC

```Bash
ffmpeg -i video_final.mp4 -vcodec copy -acodec aac h264_aac.mkv
```

<img width="1003" height="775" alt="image" src="https://github.com/user-attachments/assets/49922fc9-dd59-4cfd-8665-6f244dc0a926" />


### Audio en Vorbis

```Bash
ffmpeg -i video_final.mp4 -vcodec copy -acodec libvorbis h264_vorbis.mkv
```

<img width="1009" height="775" alt="image" src="https://github.com/user-attachments/assets/c8dd15e4-8f94-473b-abb8-fb99402a3c93" />

#### Compbrobacion

<img width="623" height="82" alt="image" src="https://github.com/user-attachments/assets/4ea19b33-a3ec-43d7-a363-0219431af905" />

## Ajuste de Bitrate

Aqui voy a forzar la calidad de 2.5 Mbps para el video y 192 kbps para el audio

```Bash
ffmpeg -i video_final.mp4 -b:v 2500k -b:a 192k video_bitrate.mp4
```

<img width="1009" height="774" alt="image" src="https://github.com/user-attachments/assets/36bf296d-ec8e-42fb-acf8-1084467c5b38" />

## Recortes de Fragmentos

Voy a recortar el video de los segundos 5 al 8

### Usando -t 3

aqui dices donde quieres empezar a hacer el recorte y dices cuanto ha de durar

```Bash
ffmpeg -i video_final.mp4 -ss 5 -t 3 video_recorte_v1.mp4
```

<img width="1012" height="578" alt="image" src="https://github.com/user-attachments/assets/7280f0e0-a43f-47e9-b13c-eb0734d9b3d2" />


### Usando - to 8

Aqui decimos donde queires que empieze y donde tiene que terminar

```Bash
ffmpeg -i video_final.mp4 -ss 00:00:05 -to 00:00:08 video_recorte_v2.mp4
```

<img width="1008" height="580" alt="image" src="https://github.com/user-attachments/assets/8d51726c-9c22-4bdf-ab21-93d17856e24e" />

#### Archivos creados

<img width="692" height="102" alt="image" src="https://github.com/user-attachments/assets/96eb5e28-e9d0-4ae4-b2a4-3b1320d186dd" />










