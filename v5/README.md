# Entrevistas de investigación 4.5

Actualización sobre los cuatro archivos suministrados de la versión 4.4.

## Instalar en el sitio existente

Reemplaza en la misma carpeta de GitHub Pages `index.html`, `app.js`, `sw.js` y `manifest.webmanifest` con los incluidos. Conserva tu carpeta `icons/` y los demás archivos del sitio. Usa la misma dirección y el mismo navegador para seguir accediendo a las entrevistas guardadas. No borres los datos del sitio. Después de publicar, cierra y vuelve a abrir la aplicación y comprueba que el encabezado muestre 4.5.

## Compartir audios grandes

1. Abre la entrevista y pulsa **Preparar WAV**.
2. Pulsa **Compartir WAV**. Si supera 15 millones de bytes, se preparan partes WAV independientes, cada una con su propia cabecera y de hasta 15 MB decimales.
3. En cada parte, pulsa **Compartir esta parte**, selecciona WhatsApp y envía el archivo.
4. Al regresar, confirma **Sí, llegó al chat** solo cuando compruebes la entrega. El programa continúa con la siguiente parte.

**Enviar todos por WhatsApp** sigue el mismo flujo para los bloques pendientes. No marca un bloque como enviado si falla la preparación, falla el compartir o cancelas una parte. Si cancelas después de enviar algunas partes de un bloque, el bloque sigue pendiente: el reintento comienza desde su primera parte y puede duplicar las ya enviadas. Las marcas antiguas de la versión 4.4 se conservan; usa Reiniciar marcas de envío si esa versión marcó como enviado un bloque que falló.

**Descargar WAV** conserva el archivo completo, incluso si supera 15 MB. En el diálogo puedes descargar una parte y adjuntarla como documento si el dispositivo no permite compartir desde el navegador.

Los nombres `_parte_01_de_02.wav` indican el orden dentro del bloque; conserva todas las partes y sus nombres. La división no corta ni descarta muestras de audio. El umbral de 15 MB es una medida para evitar el problema observado en ese teléfono, no una garantía sobre todas las versiones de WhatsApp.

## Comprobaciones

Se verificó la sintaxis de JavaScript, las cabeceras WAV y el tamaño de las partes para audios sintéticos de hasta 32 MB. La suma de los datos PCM de las partes coincide exactamente con el original. Falta comprobar el envío real desde el teléfono y WhatsApp. El WAV local continúa siendo mono de 16 kHz y 16 bits, y la conversión mantiene el límite previo de bloques de hasta unos diez minutos.
