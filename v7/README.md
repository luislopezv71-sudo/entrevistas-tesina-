# Entrevistas de investigación 4.7

Actualización de 4.5 para retomar las partes al regresar de WhatsApp.

## Instalar

Reemplaza `index.html`, `app.js`, `sw.js` y `manifest.webmanifest` en la misma carpeta publicada. Conserva `icons/` y todos los demás archivos. No borres datos del sitio ni cambies de dirección o navegador para acceder a las entrevistas existentes. Cierra y vuelve a abrir la página después de publicar y comprueba el encabezado 4.7.

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

## Cerrar una sesión interrumpida sin grabar

1. Antes del cierre, respalda los bloques originales si aún no tienes una copia.
2. Abre la entrevista interrumpida.
3. En la sección Grabación, pulsa **Cerrar sesión interrumpida**.
4. Lee y acepta la confirmación. El micrófono no se activa ni se crea otro bloque.
5. Espera el mensaje de cierre. En la lista aparecerá **Cerrada tras interrupción**.
6. Repite el procedimiento para cada entrevista que necesite cierre.

El botón solo aparece para la sesión interrumpida cargada. El cierre se deshabilita durante grabación, guardado o envío. Se guardan la fecha de cierre y el motivo `closed_after_interruption`; la duración existente y los demás campos se conservan. Las notas y el guion visibles en pantalla se guardan con el cierre. Los bloques de audio permanecen en la misma base de datos y siguen disponibles para escuchar, descargar y compartir.

El estado cerrado no confirma que se haya grabado toda la entrevista. Verifica el final del último bloque y cualquier tramo faltante. El audio que nunca se guardó no se recupera al cerrar la ficha.

Si falla el guardado, la aplicación indica el error y conserva la sesión como interrumpida. No reanudes el micrófono para forzar el cierre.

Se verificó que la operación solo escribe en la ficha de sesión, preserva campos existentes y deja el estado sin cambiar ante un error de almacenamiento. Falta comprobar el botón en el dispositivo real.
