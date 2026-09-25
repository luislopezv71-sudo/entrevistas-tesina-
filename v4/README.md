# Entrevistas de tesina · versión 4: PHP + transcripción offline

La grabadora web mantiene el audio en IndexedDB del navegador. Esta versión añade dos maneras de transcribir **bloques ya grabados**:

| Modalidad | Qué hace | Requiere |
| --- | --- | --- |
| En línea | Envía al endpoint PHP el bloque elegido; PHP lo remite a OpenAI y devuelve el texto a la página. | Hosting PHP con cURL, fileinfo, HTTPS y acceso saliente; clave de API y conexión. |
| Offline | Transcribe los audios descargados en una computadora Windows y genera JSON importables por bloque. | Python y modelo multilingüe descargados previamente. No necesita conexión durante la transcripción. |

**Ni la página PHP ni el programa offline guardan audios en `confianzaquesana.org`.** El servidor PHP usa el archivo temporal que gestiona PHP y lo reenvía a la API. Debes conservar los originales en el celular y descargarlos para crear tu respaldo. La transcripción automática puede contener errores; confirma las citas escuchando el audio. La separación automática de hablantes no está incluida en el modo offline.

## Estructura del paquete

```text
web/                          → subir el contenido a GitHub Pages, por ejemplo /v4/
  index.html, app.js, sw.js, manifest.webmanifest, icons/
php-api/
  public_html/entrevistas-api/transcribe.php → subir al hosting público
  private/config.example.php  → copiar como config.php FUERA de public_html
offline/
  PREPARAR_WINDOWS.bat          → instalación inicial con internet
  INICIAR_WINDOWS.bat           → transcripción local posterior
  preparar_modelo.py, transcribir.py, requirements.txt
```

## A. Publicar la interfaz web

1. En el repositorio GitHub Pages crea una carpeta `v4` y sube **el contenido** de `web/` a ella. No subas `php-api/private/`, claves, audios ni el modelo de la computadora al repositorio.
2. Abre `https://luislopezv71-sudo.github.io/entrevistas-tesina-/v4/` y prueba el micrófono y la descarga de un bloque corto.
3. Abre tus sesiones existentes en el **mismo navegador y dispositivo**. La base IndexedDB utiliza el mismo origen y nombre que la versión anterior, pero descarga los audios originales antes de depender de una actualización.
4. Recarga la página una vez tras publicar para que se instale el service worker de v4. Si ves contenido antiguo, cierra la pestaña y abre de nuevo la URL `/v4/`.

La aplicación necesita HTTPS para grabar en el móvil. Una página web no puede garantizar grabación continua con el celular bloqueado. Las transcripciones se almacenan en el mismo navegador junto a los bloques; exporta TXT, Word y la ficha JSON para respaldarlas.

## B. Instalar transcripción en línea por PHP

El código PHP requiere **PHP 8.1 o posterior**, extensiones `curl` y `fileinfo`, salida HTTPS a `api.openai.com`, y límites de subida mayores al tamaño de tus bloques. Si el hosting restringe conexiones salientes, la transcripción en línea no funcionará allí.

1. Desde el administrador de archivos o FTP del hosting, sube `php-api/public_html/entrevistas-api/transcribe.php` a:

   ```text
   /home/TU_USUARIO/public_html/entrevistas-api/transcribe.php
   ```

2. Crea una carpeta **fuera** de `public_html`:

   ```text
   /home/TU_USUARIO/entrevistas-private/
   ```

   Copia `php-api/private/config.example.php` como `config.php` en esa carpeta. El archivo PHP busca exactamente `~/entrevistas-private/config.php`, calculado desde la ubicación anterior. Si tu hosting tiene otra estructura, ajusta `$configPath` en `transcribe.php` a la ruta privada correcta.

3. En `config.php`, cambia `openai_api_key` por tu clave privada de API y `app_access_token` por una clave aleatoria distinta, de **al menos 32 caracteres**. Puedes generar esta última desde la terminal con `openssl rand -hex 32`; también sirve un generador de contraseñas fiable. Mantén `allowed_origin` exactamente en `https://luislopezv71-sudo.github.io` **sin la ruta del repositorio**. No coloques credenciales en GitHub ni en `public_html`.
4. En el panel PHP configura, por ejemplo, `upload_max_filesize=25M`, `post_max_size=26M` y `max_execution_time=180`. Algunas cuentas no permiten todos esos valores; si el proveedor impone un límite menor, usa bloques de 1 o 5 minutos. El código rechaza archivos mayores de 24 MiB.
5. En la pestaña **Transcripción** de la web, pon la URL completa:

   ```text
   https://confianzaquesana.org/entrevistas-api/transcribe.php
   ```

   Escribe el `app_access_token` en **Clave de acceso PHP**. La URL se recuerda en ese navegador; la clave de acceso solo vive en esa pestaña. Pulsa **Transcribir en línea** en un bloque de prueba y verifica el resultado.

6. Si el dominio publica bajo otra carpeta o subdominio, usa la URL HTTPS real del archivo `transcribe.php`. El navegador envía peticiones CORS desde GitHub Pages; el servidor solo acepta el origen configurado y exige el token.

### Modelo y hablantes

El valor inicial `gpt-transcribe` devuelve texto del bloque, que la página muestra como **Sin identificar** y con marca inicial del bloque. Si necesitas etiquetas automáticas y tiempos por intervención, cambia el modelo del `config.php` a `gpt-4o-transcribe-diarize`. Esta variante está anunciada para retirarse el **26 de febrero de 2027**; revisa la documentación de la API antes de esa fecha. En cualquier modelo, confirma manualmente el hablante antes de citar.

La API tiene un límite de 25 MB por archivo; este servidor fija 24 MiB para dejar margen. La cuenta de API puede tener cargos de uso independientes de tu plan ChatGPT.

## C. Instalar y usar la transcripción offline en Windows

1. Con internet, instala Python 3.10 o posterior. En Windows verifica en la consola que funciona `py --version`.
2. Extrae la carpeta `offline/` del ZIP a una ruta local. Con internet ejecuta **PREPARAR_WINDOWS.bat** una sola vez. Este instala `faster-whisper` y descarga el modelo multilingüe `small` dentro de `offline/modelos/small/`. El modelo no va incluido en el ZIP y requiere espacio en disco.
3. Desconecta el equipo de internet para comprobar la operación offline. Desde la web descarga **cada bloque** de una entrevista y comprueba que abre; copia todos los audios de esa entrevista a una carpeta sin audios de otra entrevista. Conserva sus nombres originales (`Proyecto_E01_001.webm`, `Proyecto_E01_002.webm`, etc.).
4. Arrastra esa carpeta de audios sobre **INICIAR_WINDOWS.bat** o ejecuta:

   ```bat
   py transcribir.py "C:\Entrevistas\E01" --salida "C:\Entrevistas\E01_resultados"
   ```

   El script usa **solo** `offline/modelos/small/` (o la carpeta indicada con `--modelo-local`) y no solicita red. Si el modelo falta, muestra un error; nunca descarga el modelo durante esta fase. Para una segunda entrevista usa otra carpeta de salida.
5. La carpeta de resultados contendrá `Proyecto_E01_001.json`, `Proyecto_E01_002.json` y, cuando hay varios bloques, `transcripcion_consolidada.txt`. El programa no sobrescribe JSON ya existentes.
6. En la web abre la sesión correspondiente. En la pestaña **Audio**, pulsa **Importar JSON offline** en cada bloque y elige el JSON del mismo número. La página comprueba el **nombre y tamaño** del audio antes de importar. Luego revisa el texto en la pestaña **Transcripción** y exporta TXT o Word desde allí.

La transcripción offline funciona después de grabar y descargar; no transcribe en tiempo real dentro del celular. Consume CPU y memoria del equipo. El texto no identifica automáticamente entrevistador y entrevistado; puedes corregir esas etiquetas en la página. Conserva el audio original y las transcripciones finales en un respaldo conforme al consentimiento del estudio.

## Solución de problemas

| Mensaje o síntoma | Revisa |
| --- | --- |
| No permite usar micrófono | HTTPS, permisos del navegador y prueba de 5 segundos. |
| PHP responde 403 | `allowed_origin` debe ser solo `https://luislopezv71-sudo.github.io`. |
| PHP responde 401 | `app_access_token` escrito en la web y en `config.php` debe coincidir. |
| PHP responde 503 | Ruta a `config.php`, extensión cURL/fileinfo o configuración incompleta. |
| PHP responde 413 o subida vacía | Límites PHP y duración del bloque. |
| PHP responde 502 | Acceso saliente a la API, clave API, cuenta, modelo y límites del hosting. |
| Error de CORS | URL HTTPS, origen exacto, o protección adicional del hosting que responde antes de PHP. |
| Falta el modelo offline | Ejecuta `PREPARAR_WINDOWS.bat` con internet antes de ir a campo. |
| JSON offline no coincide | Usa el audio descargado desde ese mismo bloque, con nombre y bytes originales. |

## Referencias técnicas

- [OpenAI: transcripción de archivos](https://developers.openai.com/api/docs/guides/speech-to-text)
- [OpenAI: modelos retirados](https://developers.openai.com/api/docs/deprecations)
- [Proyecto faster-whisper](https://github.com/SYSTRAN/faster-whisper)
