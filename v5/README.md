# Entrevistas de investigación 4.6

Actualización de 4.5 para retomar las partes al regresar de WhatsApp.

## Instalar

Reemplaza `index.html`, `app.js`, `sw.js` y `manifest.webmanifest` en la misma carpeta publicada. Conserva `icons/` y todos los demás archivos. No borres datos del sitio ni cambies de dirección o navegador para acceder a las entrevistas existentes. Cierra y vuelve a abrir la página después de publicar y comprueba el encabezado 4.6.

## Retomar la parte 2 que falta

1. Abre la entrevista y pulsa Preparar WAV en el bloque 1.
2. Debajo del bloque aparecerán todas sus partes, cada una con Compartir, Descargar y Confirmar enviada.
3. Si la parte 1 ya llegó al chat, pulsa Confirmar enviada en esa parte. No necesitas compartirla otra vez.
4. Pulsa Compartir parte 2. Regresa a la página tras enviarla y confirma su entrega.

Las confirmaciones se guardan en IndexedDB. Si el teléfono recarga la página, abre la entrevista y vuelve a preparar el WAV: las partes confirmadas se reconocen y las demás siguen pendientes. Los WAV preparados se regeneran en memoria; no se guardan copias adicionales en la base de entrevistas.

Enviar todos prepara el primer bloque pendiente y muestra sus partes. Al confirmar todas sus partes, prepara el bloque siguiente. No se abren menús de compartir automáticamente; cada archivo se comparte con un toque del usuario.

Cada parte permanece debajo de su bloque; el diálogo temporal de 4.5 se eliminó. El límite sigue siendo 15 MB decimales por parte. Descargar WAV guarda el bloque completo. Reiniciar marcas de envío borra también las confirmaciones por parte.

## Verificación

Se comprobó la sintaxis y la conservación del avance después de serializar y recuperar el estado: confirmar la primera parte no marca todo el bloque; preparar de nuevo conserva esa confirmación; la segunda continúa pendiente; el bloque se marca solo al confirmar ambas. La división WAV conserva la lógica validada de 4.5. Falta comprobar el comportamiento real en el teléfono con WhatsApp.
