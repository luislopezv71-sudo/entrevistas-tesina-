# Entrevistas de Investigación — versión 1

Aplicación web para registrar entrevistas de tesina desde un celular o una computadora. Graba audio en segmentos, conserva las sesiones en el navegador y permite descargar los archivos para su respaldo. Esta primera versión funciona con un único archivo HTML y no necesita instalar paquetes.

> **Versión 1:** grabación y organización local. La transcripción automática no está incluida.

## Funciones

- Ficha de proyecto, entrevistador y código del entrevistado.
- Confirmación de que el entrevistado autorizó la grabación.
- Grabación con controles para iniciar, pausar, reanudar y finalizar.
- Segmentos de audio configurables cada 1, 5 o 10 minutos; el valor inicial es 5 minutos.
- Indicador aproximado del nivel del micrófono y contador de tiempo.
- Notas del investigador y marcadores de momentos importantes.
- Consulta de las sesiones guardadas en el mismo navegador.
- Descarga de cada segmento o solicitud de descarga de todos los segmentos.
- Exportación de una ficha JSON con datos de la entrevista, notas, marcadores y lista de segmentos. **El JSON no contiene los audios.**

## Tecnologías y etiquetas

| Componente | Uso en la versión 1 |
| --- | --- |
| HTML5 y CSS3 | Interfaz adaptable a pantallas móviles. |
| JavaScript | Controles de grabación y gestión de sesiones. |
| MediaDevices y MediaRecorder | Acceso al micrófono y grabación del audio. |
| IndexedDB | Almacenamiento local de sesiones y segmentos. |
| Web Audio API | Indicador aproximado de entrada del micrófono. |
| Screen Wake Lock API | Solicitud de mantener la pantalla activa cuando el navegador lo permite. |

**Lenguajes:** HTML, CSS y JavaScript. **Dependencias:** ninguna. **Servidor propio:** no requerido para la versión 1.

**Descripción sugerida para GitHub:** Grabadora web de entrevistas académicas con segmentos de audio, respaldo local en IndexedDB y exportación de metadatos.

**Topics sugeridos para GitHub:** `entrevistas`, `tesina`, `investigacion`, `grabadora`, `audio`, `html`, `css`, `javascript`, `mediarecorder`, `indexeddb`, `github-pages`.

**Etiqueta de versión sugerida:** `v1.0.0`.

## Archivos del repositorio

```text
README.md
entrevistas_tesina_v1.html
```

El código de la aplicación está dentro de `entrevistas_tesina_v1.html`: incluye el estilo CSS y el JavaScript, por lo que no requiere carpetas adicionales.

## Publicación en GitHub Pages

1. Crea un repositorio y sube `README.md` y `entrevistas_tesina_v1.html` a su raíz.
2. En la configuración del repositorio, abre **Settings → Pages** y selecciona la publicación desde la rama principal y la carpeta raíz.
3. Abre la URL publicada y añade `/entrevistas_tesina_v1.html` al final. Por ejemplo: `https://USUARIO.github.io/REPOSITORIO/entrevistas_tesina_v1.html`.
4. Desde el celular, acepta el permiso del micrófono y haz una grabación breve de prueba. Descarga y reproduce el archivo resultante antes de realizar una entrevista real.

GitHub Pages sirve el archivo HTML mediante HTTPS. También se puede usar otro alojamiento HTTPS compatible con archivos estáticos. Abrir el archivo directamente desde el almacenamiento del celular puede impedir el acceso al micrófono, según el navegador.

## Uso durante una entrevista

1. Escribe el nombre del proyecto, el entrevistador y un código para el entrevistado. Evita registrar el nombre completo del participante si el protocolo requiere anonimato.
2. Elige la duración de los segmentos y confirma que la persona autorizó la grabación.
3. Pulsa **Iniciar entrevista** y comprueba que el indicador del micrófono responde.
4. Usa **Marcar momento importante** y **Guardar notas** para documentar hallazgos e incidencias.
5. Pulsa **Finalizar** y espera a que se guarde el último segmento.
6. Descarga los audios y la ficha JSON. Comprueba que los archivos existen y se reproducen fuera del navegador.

## Almacenamiento y límites

Los audios y las fichas se guardan en **IndexedDB del navegador y dispositivo usados**. Publicar el HTML en GitHub Pages no sube las grabaciones al repositorio. Cada navegador conserva sus propios datos: no aparecerán automáticamente al cambiar de teléfono, navegador o perfil.

Mantén el teléfono con batería suficiente y, en lo posible, la pantalla activa. El bloqueo de pantalla, la suspensión del navegador, el cierre de la pestaña o la falta de espacio pueden interrumpir la captura. El último segmento que todavía no haya sido entregado y guardado puede perderse. Los intervalos seleccionados son objetivos de entrega del navegador y pueden retrasarse.

El botón **Descargar todos** solicita varias descargas; algunos navegadores pueden bloquearlas. Si ocurre, usa **Descargar** en cada segmento. La ficha JSON sirve para documentar la sesión, pero **no sustituye el respaldo de los audios**. No borres los datos del sitio antes de verificar tus copias.

Esta versión muestra sesiones anteriores y sus segmentos guardados; **no continúa automáticamente una grabación interrumpida**. Para entrevistas de una hora o más, realiza primero una prueba completa con el celular y navegador que usarás en campo.

## Privacidad y consentimiento

La casilla de autorización registra la confirmación hecha por quien entrevista; no sustituye el consentimiento informado ni el protocolo de resguardo de datos del proyecto. Conserva audios y copias de seguridad de acuerdo con las reglas de tu institución y limita el acceso a las personas autorizadas.

## Alcance de la versión 1

No incluye transcripción automática, identificación de hablantes, sincronización entre dispositivos ni copia de seguridad en la nube. El repositorio contiene la aplicación web; los datos de entrevistas deben resguardarse por separado.
