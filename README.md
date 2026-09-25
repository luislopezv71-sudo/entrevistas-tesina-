# Entrevistas de Investigación — corrección 1.1

Archivo principal: `index.html` en la raíz del repositorio. El archivo HTML anterior contiene la misma aplicación; este paquete ya tiene el nombre necesario para GitHub Pages.

## Qué se corrigió

En la versión 1, `MediaRecorder.start(intervalo)` entregaba fragmentos de una misma grabación. Algunos fragmentos **no son archivos reproducibles de manera independiente**. La versión 1.1 crea una grabación nueva por bloque y la cierra antes de guardarla: el navegador genera un archivo completo para cada bloque. Hay un breve intervalo entre el cierre de un bloque y el inicio del siguiente. Comprueba los audios durante una prueba antes de una entrevista real.

- Los bloques nuevos tienen controles **Escuchar**, **Descargar**, **Compartir** y **Descargar WAV**.
- **Compartir** abre el menú del teléfono; elige WhatsApp si aparece. Requiere HTTPS y soporte del navegador para compartir archivos.
- El grabador prefiere `audio/mp4` cuando el navegador puede grabarlo; si no, usa `audio/webm` u `audio/ogg`. Cambiar solo la extensión del archivo no convierte el formato. WhatsApp podría tratar algunos WebM como documentos, sin reproducirlos dentro del chat. **Descargar WAV** convierte un bloque nuevo a audio PCM mono de 16 kHz en el teléfono; ocupa aproximadamente 1.9 MB por minuto y puede compartirse desde Archivos como documento. Algunas apps móviles pueden seguir sin mostrar un reproductor integrado.
- Los enlaces de descarga tardan un minuto en revocarse, para dar tiempo al sistema móvil a guardar archivos grandes.
- Se abre la base de datos en su versión 2 para convivir con otras versiones de la aplicación en el mismo dominio.

## Cómo recuperar grabaciones antiguas de la versión 1

1. Abre **esta versión 1.1 en el mismo navegador, perfil y celular** donde grabaste. No borres datos del sitio.
2. En **Sesiones recuperables**, pulsa **Ver** en la entrevista antigua.
3. Pulsa **Recuperar audio completo de V1**. El programa une los fragmentos almacenados **en orden**, sin recodificarlos. Descarga el archivo y comprueba que se reproduce hasta el final.
4. También puedes pulsar **Compartir audio completo de V1**. Si WhatsApp no lo admite como audio, elige la opción **Documento** desde la aplicación de archivos del teléfono.
5. Conserva los fragmentos originales hasta comprobar el archivo recuperado. Si algún fragmento nunca llegó a almacenarse, no podrá recuperarse.

Los fragmentos antiguos individualmente descargados pueden continuar sin abrir: la recuperación necesita todos los fragmentos que permanezcan en la base de datos del celular. Unirlos no cambia el códec. Si el archivo completo se reproduce en el navegador pero no en WhatsApp, conviértelo en una computadora a MP3 o M4A con una herramienta de audio y escucha el resultado antes de enviarlo. **La versión 1.1 no convierte audio a MP3.**

## Estructura para la raíz del repositorio

```text
index.html             → aplicación 1.1 (reemplaza el index.html anterior)
README.md              → documentación actualizada (reemplaza el README.md anterior)
verificar_grabaciones.html → consulta local de todas las sesiones y bloques
v2/                   → conservar sin cambios
v4/                   → conservar sin cambios
```

La aplicación usa el mismo origen de GitHub Pages y la misma base local `tesina_interviews_db`; subir estos dos archivos no borra por sí mismo las entrevistas del dispositivo. Antes de cambios en el navegador, descarga y comprueba tus audios.

## Comprobar sesiones que no aparecen

La interfaz anterior mostraba solo las **20 sesiones más recientes**. Se eliminó ese límite; ahora lista todas las sesiones que continúen guardadas en IndexedDB.

Para inspeccionarlas sin escribir ni borrar datos, sube `verificar_grabaciones.html` junto a `index.html` **en la raíz** del repositorio y, desde el **mismo celular, navegador y perfil** donde grabaste, abre:

`https://luislopezv71-sudo.github.io/entrevistas-tesina-/verificar_grabaciones.html`

Pulsa **Buscar sesiones y audios**. La página enumera todas las entrevistas, bloques, audios sin ficha y permite escuchar o descargar los bloques encontrados. Los audios nunca salen del dispositivo por esta comprobación. Si una ficha indica más bloques que los encontrados, los restantes no constan en esa base local; revisa la carpeta Descargas y tus respaldos. Si no aparece nada, revisa el navegador y perfil originales y que no se hayan borrado los datos del sitio. No restablezcas ni desinstales el navegador durante la búsqueda. Subir archivos al repositorio no transfiere los audios del celular a GitHub.

## Publicación y prueba rápida

1. Extrae el ZIP y sube **`index.html`, `README.md` y `verificar_grabaciones.html` directamente a la raíz** de `luislopezv71-sudo/entrevistas-tesina-`, reemplazando los archivos existentes con esos nombres. Conserva las carpetas `v2/` y `v4/`. En GitHub, comprueba **Settings → Pages → Build and deployment: Deploy from a branch → main / (root)**. Abre `https://luislopezv71-sudo.github.io/entrevistas-tesina-/` en tu celular. La ruta `/v1/` no existe en este repositorio.
2. Autoriza el micrófono y graba una entrevista de prueba con segmentos de **1 minuto** durante algo más de dos minutos.
3. Finaliza y prueba **Escuchar** en cada bloque. Descarga y reproduce cada bloque desde la aplicación de archivos del celular.
4. Prueba **Compartir** con un bloque corto. Si el navegador usa `.m4a`, WhatsApp puede aceptarlo como audio; si usa `.webm`, quizá deba enviarse como documento o convertirse a otro formato.
5. Si el archivo original no abre en el reproductor del teléfono, usa **Descargar WAV**, comprueba que abre y adjúntalo desde WhatsApp como **Documento**. La conversión de bloques largos puede consumir memoria del celular.
6. Solo después de estas verificaciones realiza una entrevista importante. Mantén el teléfono con espacio libre, batería suficiente y pantalla activa: la captura web puede suspenderse si el sistema bloquea la página.

La confirmación de consentimiento de la aplicación no sustituye los formatos o políticas de tu estudio. Al compartir archivos por WhatsApp, el contenido pasa a las personas o servicios elegidos; hazlo solo si el consentimiento y protocolo de resguardo lo permiten.
