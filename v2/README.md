# Entrevistas de tesina — versión 3

**Contenido real del ZIP:** index.html (interfaz), app.js (grabación y gestión), sw.js (acceso sin conexión), manifest.webmanifest, icons/ y backend/ (servidor opcional de transcripción).

El archivo `entrevistas_tesina_v3.html` se entrega por separado como alternativa de un solo archivo. Para usar el micrófono desde el celular, publícalo mediante HTTPS. La versión del ZIP añade instalación y caché para consulta sin conexión.

Aplicación para grabar entrevistas largas en el navegador móvil. Los bloques de audio, las transcripciones y la matriz se guardan en **IndexedDB del dispositivo**. No se suben audios al repositorio. Al transcribir un bloque, el usuario lo envía expresamente al servidor configurado, que lo remite al proveedor de transcripción.

## Publicar la interfaz en GitHub Pages

1. Crea un repositorio, por ejemplo `entrevistas-tesina`.
2. Sube a la **raíz** del repositorio `index.html`, `app.js`, `sw.js`, `manifest.webmanifest` y la carpeta `icons/`. No subas credenciales ni archivos `.env`.
3. En **Settings → Pages**, publica la rama `main` y la carpeta `/ (root)`.
4. Abre `https://TUUSUARIO.github.io/entrevistas-tesina/` desde el móvil; acepta el micrófono y ejecuta la prueba de cinco segundos.
5. Usa «Agregar a pantalla de inicio» si el navegador lo ofrece. Se incluyen iconos PNG para instalación y un icono SVG para el navegador.

La aplicación puede abrirse sin red después de instalarse y completar la primera carga, pero debes verificarlo en tu teléfono antes de salir a campo. Mantén la pantalla encendida: ningún sitio web puede garantizar grabación continua en segundo plano o con el teléfono bloqueado. Al cerrar la pestaña puede perderse el bloque que aún no se guardó. Los bloques son grabaciones independientes; al rotarlos puede existir un breve salto de audio. Prueba previamente una sesión de 60 a 90 minutos con ese modelo de teléfono y navegador.

## Servidor opcional de transcripción

GitHub Pages no ejecuta el servidor. Despliega `backend/` en un servicio Node.js con HTTPS, Node 20 o posterior, instalación `npm install` e inicio `npm start`. Define estas variables **en el servidor**, usando `.env.example` solo como guía:

- `OPENAI_API_KEY`: clave de la API (nunca en GitHub ni en HTML).
- `APP_ACCESS_TOKEN`: clave aleatoria larga para impedir uso público de tu servidor.
- `ALLOWED_ORIGIN`: origen exacto de Pages, por ejemplo `https://TUUSUARIO.github.io` (sin ruta ni barra final).
- `PORT`: el puerto asignado por el alojamiento.

En la pestaña **Transcripción**, escribe la URL HTTPS del servidor y la clave de acceso. La URL se guarda en ese navegador; la clave de acceso se mantiene solo en memoria de la pestaña. Pulsa **Transcribir** en cada bloque. La transcripción utiliza `gpt-4o-transcribe-diarize` y devuelve etiquetas de hablante genéricas; **no sabe** cuál es el entrevistador y cuál el entrevistado. Puedes corregir manualmente la etiqueta de cada intervención en pantalla. Verifica y corrige la atribución antes de usar citas en la tesina. La separación de hablantes puede variar entre bloques.

## Copias y límites

Cada bloque es reproducible y descargable por separado. Descarga todos los bloques al terminar y verifica que se abren. «Descargar audios individualmente» puede activar el bloqueo de descargas múltiples del navegador; usa los botones de cada bloque si sucede. La ficha JSON incluye metadatos y transcripciones, **no** incorpora los audios. Exporta TXT, documento `.doc` compatible con Word y CSV de la matriz. Guarda los audios fuera del teléfono conforme al consentimiento y al protocolo de investigación. No borres los datos del sitio, no uses modo privado y no cambies de navegador a mitad del proyecto.

La recuperación de una sesión interrumpida conserva los bloques confirmados y permite crear otros nuevos en la misma sesión. No puede recuperar audio que todavía estaba en la memoria del grabador cuando se cerró la página. La aplicación no ha sido certificada para dispositivos clínicos ni para conservar una cadena formal de custodia.
