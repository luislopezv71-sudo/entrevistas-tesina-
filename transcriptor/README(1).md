# Transcriptor de entrevistas para computadora · versión 1

Aplicación de Windows en Python y Tkinter para transcribir, en una sola operación, los bloques descargados de una entrevista realizada con la grabadora web. Acepta archivos `.webm`, `.m4a`, `.mp4`, `.ogg`, `.mp3` y `.wav`; reconoce nombres terminados en `_001`, `_002` y los de la versión 1.1 terminados en `_seg_001`, `_seg_002`. También admite un audio completo recuperado de la versión 1 terminado en `_v1_COMPLETO`, en una carpeta por separado.

La transcripción se hace **en la computadora** con faster-whisper y el modelo multilingüe `small`. Una primera preparación descarga dependencias y modelo con internet. Después, el programa lee un modelo local y no necesita conectarse para transcribir. Los audios y textos no se suben a un servidor desde esta herramienta. La instalación y descarga del modelo sí usan internet. faster-whisper decodifica audio con PyAV, por lo que su instalación normal no requiere instalar FFmpeg por separado.

## Archivos

```text
INICIAR_WINDOWS.bat       abre la interfaz
PREPARAR_WINDOWS.bat      instala dependencias y descarga el modelo por primera vez
app.py                   ventana de selección y avance
core.py                  orden, validación, transcripción y exportaciones
preparar_modelo.py       descarga el modelo small
requirements.txt         dependencia faster-whisper
```

## Preparar la computadora

1. Instala Python 3 en Windows (3.10 a 3.13) desde [python.org](https://www.python.org/downloads/). En el instalador activa **Add Python to PATH** y verifica en CMD `py -3 --version`. Se recomienda una computadora con al menos **8 GB de RAM** para el modelo `small`; el consumo real depende del equipo y la duración de los audios.
2. Extrae **toda** la carpeta del ZIP en la computadora. No abras el `.bat` directamente dentro del ZIP.
3. Con internet, ejecuta `PREPARAR_WINDOWS.bat`. Crea un entorno `.venv`, instala `faster-whisper` y descarga el modelo a `modelos/small`. Puede tardar y requiere espacio libre. Si falla, copia el mensaje de la ventana.
4. Después de que el modelo esté listo, puedes desconectar internet. Ejecuta `INICIAR_WINDOWS.bat` para abrir la aplicación.

La aplicación **no utiliza Node.js** ni necesita un servidor PHP para la transcripción local. No incluye el modelo en el ZIP.

## Preparar audios descargados

Crea una carpeta **por entrevista**. Descarga cada bloque desde la aplicación web y colócalo allí sin modificar su nombre. Por ejemplo:

```text
E01/
  Tesina_E01_seg_001.webm
  Tesina_E01_seg_002.webm
  Tesina_E01_seg_003.webm
```

También sirven bloques como `Tesina_E01_001.m4a`, `Tesina_E01_002.m4a` o WAV como `Tesina_E01_seg_001.wav`. Si tienes el original y el WAV del mismo bloque, la aplicación elige **una sola copia** y te indica cuál eligió. Por defecto prioriza el original para que el JSON por bloque conserve el nombre y tamaño necesarios para importarlo en la versión 4; puedes activar **Preferir WAV** si el original no se decodifica.

Los audios de distintas entrevistas deben ir en carpetas distintas, incluso si comparten código. Si falta un número de bloque, la aplicación se detiene y te avisa. Puedes aceptar explícitamente transcribir lo disponible, pero el resultado quedará incompleto. Antes de transcribir, escucha el primer y último bloque; si un archivo descargado no se reproduce, la transcripción de ese bloque podría fallar.

**Grabaciones de la versión 1 antigua:** si solo tienes el audio completo recuperado (`_v1_COMPLETO`), ponlo solo en su carpeta y comprueba antes que se reproduce. Se transcribirá como un único archivo; no mezcles ese completo con otros bloques.

## Transcribir y revisar

1. En la ventana, elige **Carpeta de audios** (una entrevista) y una **Carpeta donde guardar resultados**.
2. Pulsa **Revisar bloques** para ver orden, faltantes y copias WAV. Luego pulsa **Transcribir entrevista**.
3. Se crea una carpeta nueva por ejecución; los resultados anteriores no se sobrescriben:

   - `transcripcion_completa.txt`: texto legible por bloques;
   - `transcripcion_completa.csv`: matriz con bloque, archivo, tiempos y texto;
   - `transcripcion_completa.srt`: subtítulos con tiempos globales aproximados;
   - `Tesina_E01_seg_001.json`, etc.: texto de cada audio y metadatos para importar en la versión 4 cuando coincida exactamente con el archivo original;
   - `resumen.json`: lista de bloques y sus duraciones estimadas.

El tiempo global es **aproximado**: suma la duración del audio de cada archivo, sin medir los silencios entre el cierre de un bloque y el inicio del siguiente. Cada JSON tiene los tiempos relativos a su bloque. Los hablantes se muestran como **Sin identificar**; revisa y corrige citas, nombres, cifras y hablantes escuchando los audios. El modelo puede omitir o alterar palabras.

El botón **Detener tras el bloque actual** evita iniciar el siguiente. Si un bloque falla o detienes la operación, conserva los JSON ya escritos en la carpeta parcial. Una ejecución posterior crea otra carpeta y vuelve a procesar los bloques.

## Notas para la versión 4 web

Para importar el JSON de un bloque en la pestaña Audio de la web, transcribe **el mismo archivo original** descargado desde ese bloque. La web comprueba su nombre y tamaño; si transcribes una copia WAV de un original WebM/M4A, el JSON no coincidirá para importación automática. Puedes seguir usando el TXT o CSV en la computadora.

La herramienta no accede a IndexedDB del celular: primero descarga cada bloque y cópialo a la PC. Haz un respaldo de los originales antes de borrar datos del navegador. Mantén los audios y resultados según el consentimiento de las personas entrevistadas.

## Referencia técnica

- [faster-whisper, repositorio y guía de instalación](https://github.com/SYSTRAN/faster-whisper).
