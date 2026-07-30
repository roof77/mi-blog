---
title: Notas sobre ffmpeg
date: 2026-07-30T13:10:00+01:00
tags:
- ffmpeg
showHero: true
heroStyle: big
pinned: false
created: 2026-07-30T13:10:31.325685878+00:00


---

# # Apuntes sobre ffmpeg

Toda la documentación de ffmpeg [aquí](https://ffmpeg.org/ffmpeg-all.html).

## Como descartar trozos erroneos de un video

### Codificando con h264

```bash
ffmpeg -err_detect ignore_err -fflags +genpts+igndts+discardcorrupt \
  -i entrada.mp4 -c:v libx264 -c:a aac -crf 18 -preset medium salida.mp4
```

Explicaciones:

* ```-err_detect ignore_err```: Esto dice a ffmpeg que ignore los errores al decodificar y se los salte en lugar de parar.
* ```-fflags +genpts+igndts+discardcorrupt```:
  * `genpts`: Generar el PTS desaparecido si hay DTS.
  * `igndts`: Ignorar el DTS si hay PTS. En caso de que haya PTS, el valor de DTS se establece a NOPTS. ESto se ignora cuando se pone la flag  `nofillin`.
  * `discardcorrupt`: Descarta los paquetes corruptos
  * ¿Que son los PTS y los DTS?
    * PTS (Presentation time stamp): Indica el momento en que el fotograma se debe mostrar en pantalla.
    * DTS (Decoding time stamp): El momento en que el fotograma debe decodificarse, que no siempre coincide con el orden en que se va a mostrar
* `-i`: indica el fichero de entrada.
* `-c:v`: indica el codec de video a usar al recodificar. En este caso se usa el libx264
* `-c:a`:igual que para el video, pero para el audio
* [`-crf`](https://trac.ffmpeg.org/wiki/Encode/H.264): Constant rate factor. Para la calidad de video. 18 se considera "visualmente sin pérdidas"
* [`-preset`](https://trac.ffmpeg.org/wiki/Encode/H.264):  Para el equilibro entre calidad/tiempo de codificación
* Documentación [ffmpeg](https://ffmpeg.org/ffmpeg-formats.html)

### Codificando con h265

```bash
ffmpeg -err_detect ignore_err -fflags +genpts+igndts+discardcorrupt \
  -i entrada.mp4 -c:v libx265 -tag:v hvc1 -c:a aac -crf 28 -preset medium salida.mp4
```

Explicaciones de las diferencias con el anterior:

* `-c:v libx265`: Usa el codificador HEVC (H.265) en vez de H.264
* `-tag:v hvc1`: Para que no de problemas en dispositivos apple.
* `-crf 28`: El equivalente en H.265 al -crf 18 en H.264

