# Vista previa de la cámara del complemento Cordova
<a href="https://badge.fury.io/js/cordova-plugin-camera-preview" target="_blank"><img height="21" style='border:0px;height:21px;' border='0' src="https://badge.fury.io/js/cordova-plugin-camera-preview.svg" alt="Versión NPM"></a>
<a href='https://www.npmjs.org/package/cordova-plugin-camera-preview' target='_blank'><img height='21' style='border:0px;height:21px;' src='https://img.shields.io/npm/dt/cordova-plugin-camera-preview.svg?label=NPM+Descargas' border='0' alt='NPM Descargas' /></a>

Complemento de Cordova que permite la interacción de la cámara desde Javascript y HTML

**Los comunicados se mantienen actualizados cuando corresponde. Sin embargo, este complemento está en constante desarrollo. Como tal, se recomienda usar el maestro para tener siempre las últimas correcciones y características.**

**Las relaciones públicas son muy apreciadas.**

# Características

<ul>
  <li>Iniciar una vista previa de la cámara desde el código HTML</li>
  <li>Hacer fotos e instantáneas</li>
  <li>Mantener la interactividad HTML</li>
  <li>Arrastre el cuadro de vista previa</li>
  <li>Establecer el efecto de color de la cámara</li>
  <li>Envíe el cuadro de vista previa al reverso del contenido HTML</li>
  <li>Establezca una posición personalizada para el cuadro de vista previa de la cámara</li>
  <li>Establezca un tamaño personalizado para el cuadro de vista previa</li>
  <li>Establezca un alfa personalizado para el cuadro de vista previa</li>
  <li>Configure el modo de enfoque, el zoom, los efectos de color, el modo de exposición, el modo de balance de blancos y la compensación de exposición</li>
  <li>Toca para enfocar</li>
  <li>Grabar vídeos</li>
</ul>

# Instalación

Utilice cualquiera de los métodos de instalación enumerados a continuación según el marco que utilice.

Para instalar la versión maestra con las últimas correcciones y características

```
Complemento cordova agregar https://github.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview.git

Complemento cordova iónico agregar https://github.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview.git

meteoro agregar cordova:cordova-plugin-camera-preview@https://github.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview.git#[latest_commit_id]

<especificación del complemento="https://github.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview.git" source="git" />
```

o si desea utilizar la última versión publicada en npm

```
Complemento cordova agregar cordova-plugin-camera-preview

Complemento iónico de cordova agregar cordova-plugin-camera-preview

meteorito añadir cordova:cordova-plugin-camera-preview@X.X.X

<gap:plugin name="cordova-plugin-camera-preview" />
```

#### peculiaridades de iOS
1. No es posible usar la cámara web de su computadora durante la prueba en el simulador, debe probar el dispositivo.
2. Si está desarrollando para iOS 10+, también debe agregar lo siguiente a su config.xml

```xml
<plataforma del archivo de configuración="ios" target="*-Info.plist" parent="NSCameraUsageDescription" overwrite="true">
  <string>Permitir que la aplicación use tu cámara</string>
</archivo de configuración>

<!-- o para Phonegap -->

<brecha:plataforma del archivo de configuración="ios" target="*-Info.plist" parent="NSCameraUsageDescription" overwrite="true">
  <string>Permitir que la aplicación use tu cámara</string>
</gap:archivo de configuración>
```

#### peculiaridades de Android

1. Al usar el complemento para dispositivos más antiguos, la vista previa de la cámara tomará el foco dentro de la aplicación una vez inicializada. Para evitar que la aplicación se cierre cuando un usuario presiona el botón Atrás, el evento para la vista de la cámara está deshabilitado. Si aún desea que el usuario navegue, puede agregar un oyente para el evento de retroceso para la vista previa (consulte <code>[onBackButton](#onBackButton)</code>)

# Methods

### startCamera(options, [successCallback, errorCallback])

Inicia la instancia de vista previa de la cámara.
<br>

<strong>Opciones:</strong>
Todas las opciones indicadas son opcionales y tendrán valores predeterminados aquí

* `x` - Por defecto es 0
* `y` - Por defecto es 0
* `width` - Por defecto es window.screen.width
* `altura` - Por defecto es window.screen.height
* `camera`: consulta <code>[CAMERA_DIRECTION](#camera_Settings.CameraDirection)</code>: el valor predeterminado es la cámara frontal
* `toBack` - Predeterminado en falso - Establézcalo en verdadero si desea que su html esté delante de su vista previa
* `tapPhoto`: el valor predeterminado es verdadero: no funciona si toBack está configurado en falso, en cuyo caso se usa el método takePicture
* `tapFocus`: el valor predeterminado es falso: permite al usuario tocar para enfocar, cuando la vista está en primer plano
* `previewDrag` - Predeterminado en falso - No funciona si toBack está configurado en falso
* `storeToFile`: el valor predeterminado es falso: captura imágenes en un archivo y devuelve la ruta del archivo en lugar de devolver los datos codificados en base64.
* `disableExifHeaderStripping`: el valor predeterminado es falso. **Solo Android**: deshabilite la rotación automática de la imagen y deje que el navegador se ocupe de ella (siga leyendo sobre cómo lograrlo)

```javascript
dejar opciones = {
  x: 0,
  y: 0,
  ancho: ventana.pantalla.ancho,
  altura: ventana.pantalla.altura,
  cámara: CameraPreview.CAMERA_DIRECTION.BACK,
  hacia atrás: falso,
  toqueFoto: cierto,
  tapFocus: falso,
  vista previaArrastrar: falso,
  almacenar en archivo: falso,
  deshabilitarExifHeaderStripping: falso
};

CameraPreview.startCamera(opciones);
```

Al establecer `toBack` en verdadero, recuerda agregar el estilo a continuación en el HTML o el elemento del cuerpo de tu aplicación:

```css
html, cuerpo, .ion-app, .ion-content {
  color de fondo: transparente;
}
```

Cuando tanto `tapFocus` como `tapPhoto` son verdaderos, la cámara enfocará y tomará una foto tan pronto como la cámara termine de enfocar.

Si captura imágenes grandes en Android, puede notar que el rendimiento es deficiente; en esos casos, puede establecer `disableExifHeaderStripping` en verdadero y, en su lugar, solo agregue Javascript/HTML adicional para obtener una visualización adecuada de sus imágenes capturadas sin arriesgar la velocidad de su aplicación.

Al capturar imágenes grandes, es posible que desee almacenarlas en un archivo en lugar de codificarlas en base64, ya que la codificación, al menos en Android, es muy costosa. Con la función `storeToFile` habilitada, el complemento capturará la imagen en un archivo temporal dentro del caché temporal de la aplicación (el mismo lugar donde Cordova extraerá sus activos). Este método se usa mejor con `disableExifHeaderStripping` para obtener el mejor rendimiento posible.

### detener la cámara ([devolución de llamada exitosa, devolución de llamada de error])

<info>Detiene la instancia de vista previa de la cámara.</info><br/>

```javascript
CameraPreview.stopCamera();
```

### switchCamera([successCallback, errorCallback])

<info>Cambia entre la cámara trasera y la cámara delantera, si está disponible.</info><br/>

```javascript
CameraPreview.switchCamera();
```

### mostrar ([devolución de llamada exitosa, devolución de llamada de error])

<info>Muestra el cuadro de vista previa de la cámara.</info><br/>

```javascript
CameraPreview.show();
```

### ocultar ([devolución de llamada exitosa, devolución de llamada de error])

<info>Ocultar el cuadro de vista previa de la cámara.</info><br/>

```javascript
CameraPreview.hide();
```

### TakePicture(opciones, devolución de llamada exitosa, [devolución de llamada de error])

<info>Toma la foto. Si el ancho y la altura no se especifican o son 0, se usarán los valores predeterminados. Si se especifican el ancho y el alto, elegirá un tamaño de foto admitido que sea el más cercano al ancho y el alto especificados y tenga la relación de aspecto más cercana a la vista previa. El argumento `calidad` por defecto es `85` y especifica el valor de calidad/compresión: `0=compresión máxima`, `100=calidad máxima`.</info><br/>

```javascript
CameraPreview.takePicture({ancho:640, alto:640, calidad: 85}, function(base64PictureData|filePath) {
  /*
    si la opción storeToFile es falsa (el valor predeterminado), se devuelve base64PictureData.
    base64PictureData es una imagen jpeg codificada en base64. Utilice estos datos para almacenarlos en un archivo o cargarlos.
    Depende de usted descubrir la mejor manera de guardarlo en el disco o lo que sea para su aplicación.
  */

  /*
    si la opción storeToFile se establece en true, se devuelve una ruta de archivo. Tenga en cuenta que el archivo
    se almacena en almacenamiento temporal, por lo que debe moverlo a una ubicación permanente si
    no quiero que el sistema operativo lo elimine arbitrariamente.
  */

  // Un ejemplo simple es si lo va a usar dentro de un atributo HTML img src, entonces haría lo siguiente:
  imageSrcData = 'datos:imagen/jpeg;base64,' + base64PictureData;
  $('img#mi-img').attr('src', imageSrcData);
});

// O si desea utilizar las opciones predeterminadas.

CameraPreview.takePicture(función(base64PictureData){
  /* código aquí */
});
```

### tomar instantánea (opciones, devolución de llamada exitosa, [devolución de llamada de error])

<info>Tome una instantánea de la vista previa de la cámara. La imagen tendrá el mismo tamaño que el especificado en las opciones de `startCamera`. El argumento `calidad` por defecto es `85` y especifica el valor de calidad/compresión: `0=compresión máxima`, `100=calidad máxima`.</info><br/>

```javascript
CameraPreview.takeSnapshot({calidad: 85}, función(base64PictureData){
  /*
    base64PictureData es una imagen jpeg codificada en base64. Utilice estos datos para almacenarlos en un archivo o cargarlos.
  */

  // Un ejemplo simple es si lo va a usar dentro de un atributo HTML img src, entonces haría lo siguiente:
  imageSrcData = 'datos:imagen/jpeg;base64,' +base64PictureData;
  $('img#mi-img').attr('src', imageSrcData);
});
```

### getSupportedFocusModes(cb, [errorCallback])

<info>Obtenga modos de enfoque admitidos por el dispositivo de cámara actualmente iniciado. Devuelve una matriz que contiene los modos de enfoque admitidos. Consulte <code>[FOCUS_MODE](#camera_Settings.FocusMode)</code> para conocer los posibles valores que se pueden devolver.</info><br/>

```javascript
CameraPreview.getSupportedFocusModes(function(focusModes){
  focusModes.forEach(function(focusMode) {
    console.log(focusMode + ', ');
  });
});
```

### setFocusMode(focusMode, [successCallback, errorCallback])

<info>Establece el modo de enfoque para el dispositivo de la cámara actualmente iniciado.</info><br/>
* `focusMode` - <código>[FOCUS_MODE](#camera_Settings.FocusMode)</code>

```javascript
CameraPreview.setFocusMode(CameraPreview.FOCUS_MODE.CONTINUOUS_PICTURE);
```

### getFocusMode(cb, [errorCallback])

<info>Obtenga el modo de enfoque para el dispositivo de cámara actualmente iniciado. Devuelve una cadena que representa el modo de enfoque actual.</info>Consulte <code>[FOCUS_MODE](#camera_Settings.FocusMode)</code> para conocer los posibles valores que se pueden devolver.</info><br/>

```javascript
CameraPreview.getFocusMode(function(currentFocusMode){
  console.log(currentFocusMode);
});
```

### getSupportedFlashModes(cb, [errorCallback])

<info>Obtenga los modos de flash admitidos por el dispositivo de la cámara iniciado actualmente. Devuelve una matriz que contiene los modos flash admitidos. Consulte <code>[FLASH_MODE](#camera_Settings.FlashMode)</code> para conocer los posibles valores que se pueden devolver</info><br/>

```javascript
CameraPreview.getSupportedFlashModes(function(flashModes){
  flashModes.forEach(function(flashMode) {
    console.log(flashMode + ', ');
  });
});
```

### setFlashMode(flashMode, [successCallback, errorCallback])

<info>Establece el modo de flash. Consulte <code>[FLASH_MODE](#camera_Settings.FlashMode)</code> para obtener detalles sobre los posibles valores para flashMode.</info><br/>

```javascript
CameraPreview.setFlashMode(CameraPreview.FLASH_MODE.ON);
```

### getFlashMode(cb, [errorCallback])

<info>Obtenga el modo de flash para el dispositivo de la cámara actualmente iniciado. Devuelve una cadena que representa el modo de flash actual.</info>Consulte <code>[FLASH_MODE](#camera_Settings.FlashMode)</code> para conocer los posibles valores que se pueden devolver</info><br/>

```javascript
CameraPreview.getFlashMode(function(currentFlashMode){
  console.log(currentFlashMode);
});
```

### getHorizontalFOV(cb, [errorCallback])

<info>Obtenga el FOV horizontal para el dispositivo de cámara actualmente iniciado. Devuelve una cadena de un flotador que es el FOV de la cámara en Grados. </info><br/>

```javascript
CameraPreview.getHorizontalFOV(function(getHorizontalFOV){
  console.log(getHorizontalFOV);
});
```

### getSupportedColorEffects(cb, [errorCallback])

*Actualmente, esta característica es solo para Android. Un PR para el soporte de iOS sería felizmente aceptado *

<info>Obtenga los modos de color admitidos por el dispositivo de cámara actualmente iniciado. Devuelve una matriz que contiene efectos de color admitidos (cadenas). Consulte <code>[COLOR_EFFECT](#camera_Settings.ColorEffect)</code> para conocer los posibles valores que se pueden devolver.</info><br/>

```javascript
CameraPreview.getSupportedColorEffects(function(colorEffects){
  colorEffects.forEach(function(color) {
    console.log(color + ', ');
  });
});
```


### setColorEffect(colorEffect, [successCallback, errorCallback])

<info>Establecer el efecto de color. Consulte <code>[COLOR_EFFECT](#camera_Settings.ColorEffect)</code> para obtener detalles sobre los valores posibles para colorEffect.</info><br/>

```javascript
CameraPreview.setColorEffect(CameraPreview.COLOR_EFFECT.NEGATIVE);
```

### setZoom(zoomMultiplier, [successCallback, errorCallback])

<info>Establezca el nivel de zoom para el dispositivo de cámara actualmente iniciado. La opción zoomMultipler acepta un número entero. El nivel de zoom está inicialmente en 1</info><br/>

```javascript
CameraPreview.setZoom(2);
```

### getZoom(cb, [errorCallback])

<info>Obtenga el nivel de zoom actual para el dispositivo de cámara actualmente iniciado. Devuelve un número entero que representa el nivel de zoom actual.</info><br/>

```javascript
CameraPreview.getZoom(function(currentZoom){
  console.log(currentZoom);
});
```

### getMaxZoom(cb, [errorCallback])

<info>Obtenga el nivel de zoom máximo para el dispositivo de cámara actualmente iniciado. Devuelve un número entero que representa el nivel de zoom mínimo.</info><br/>

```javascript
CameraPreview.getMaxZoom(function(maxZoom){
  console.log(maxZoom);
});
```

### getSupportedWhiteBalanceModes(cb, [errorCallback])

<info>Devuelve una matriz con los modos de balance de blancos admitidos para el dispositivo de cámara actualmente iniciado. Consulte <code>[WHITE_BALANCE_MODE](#camera_Settings.WhiteBalanceMode)</code> para obtener detalles sobre los posibles valores devueltos.</info><br/>

```javascript
CameraPreview.getSupportedWhiteBalanceModes(function(whiteBalanceModes){
  console.log(whiteBalanceModes);
});
```

### getWhiteBalanceMode(cb, [errorCallback])

<info>Obtenga el modo de balance de blancos actual del dispositivo de la cámara actualmente iniciado. Consulte <code>[WHITE_BALANCE_MODE](#camera_Settings.WhiteBalanceMode)</code> para obtener detalles sobre los posibles valores devueltos.</info><br/>

```javascript
CameraPreview.getWhiteBalanceMode(función(whiteBalanceMode){
  consola.log(modoequilibrioblanco);
});
```
### setWhiteBalanceMode(whiteBalanceMode, [successCallback, errorCallback])

<info>Establezca el modo de balance de blancos para el dispositivo de cámara actualmente iniciado. Consulte <code>[WHITE_BALANCE_MODE](#camera_Settings.WhiteBalanceMode)</code> para obtener detalles sobre los valores posibles para whiteBalanceMode.</info><br/>

```javascript
CameraPreview.setWhiteBalanceMode(CameraPreview.WHITE_BALANCE_MODE.CLOUDY_DAYLIGHT);
```

### getExposureModes(cb, [errorCallback])

<info>Devuelve una matriz con modos de exposición admitidos para el dispositivo de cámara actualmente iniciado. Consulte <code>[EXPOSURE_MODE](#camera_Settings.ExposureMode)</code> para obtener detalles sobre los posibles valores devueltos.</info><br/>

```javascript
CameraPreview.getExposureModes(function(exposureModes){
  console.log(modos de exposición);
});
```

### getExposureMode(cb, [errorCallback])

<info>Obtenga el modo de exposición actual del dispositivo de la cámara actualmente iniciado. Consulte <code>[EXPOSURE_MODE](#camera_Settings.ExposureMode)</code> para obtener detalles sobre los posibles valores devueltos.</info><br/>

```javascript
CameraPreview.getExposureMode(function(exposureMode){
  consola.log(modoexposición);
});
```
### establecer el modo de exposición (modo de exposición, [devolución de llamada exitosa, devolución de llamada de error])

<info>Establece el modo de exposición para el dispositivo de la cámara actualmente iniciado. Consulte <code>[EXPOSURE_MODE](#camera_Settings.ExposureMode)</code> para obtener detalles sobre los valores posibles para el modo de exposición.</info><br/>

```javascript
CameraPreview.setExposureMode(CameraPreview.EXPOSURE_MODE.CONTINUO);
```
### getExposureCompensationRange(cb, [errorCallback])

<info>Obtenga la compensación de exposición mínima y máxima para el dispositivo de cámara iniciado actualmente. Devuelve un objeto que contiene números enteros mínimo y máximo.</info><br/>

```javascript
CameraPreview.getExposureCompensationRange(función(expoxureRange){
  console.log("min: " + rango de exposición.min);
  console.log("max: " + rango de exposición.max);
});
```
### getExposureCompensation(cb, [errorCallback])

<info>Obtenga la compensación de exposición actual para el dispositivo de cámara actualmente iniciado. Devuelve un número entero que representa la compensación de exposición actual.</info><br/>

```javascript
CameraPreview.getExposureCompensation(function(expoxureCompensation){
  console.log(exposiciónCompensación);
});
```
### setExposureCompensation(exposureCompensation, [successCallback, errorCallback])

<info>Establezca la compensación de exposición para el dispositivo de cámara iniciado actualmente. exposiciónCompensación acepta un número entero. si la compensación de exposición es menor que la compensación de exposición mínima, se establece al mínimo. si la compensación de exposición es mayor que la compensación de exposición máxima, se establece al máximo. (consulte getExposureCompensationRange() para obtener la compensación de exposición mínima y máxima).</info><br/>

```javascript
CameraPreview.setExposureCompensation(-2);
CameraPreview.setExposureCompensation(3);
```

### setPreviewSize([dimensiones, exitCallback, errorCallback])

<info>Cambiar el tamaño de la ventana de vista previa.</info><br/>

```javascript
CameraPreview.setPreviewSize({ancho: ventana.pantalla.ancho, alto: ventana.pantalla.alto});
```

### getSupportedPictureSizes(cb, [errorCallback])

```javascript
CameraPreview.getSupportedPictureSizes(función(dimensiones){
  // tenga en cuenta que la versión vertical, ancho y alto intercambiados, de estas dimensiones también son compatibles
  dimensiones.forEach(función(dimensión) {
    console.log(dimensión.ancho + 'x' + dimensión.altura);
  });
});
```

### getCameraCharacteristics(cb, [errorCallback])

*Actualmente, esta característica es solo para Android. Un PR para el soporte de iOS sería felizmente aceptado *

<info>Conoce las características de todas las cámaras disponibles. Devuelve un objeto JSON que representa las características de todas las cámaras disponibles.</info><br/>

```javascript
CameraPreview.getCameraCharacteristics(función(características){
  consola.log(características);
});
```

Características del ejemplo:

```
{
  "CAMERA_CHARACTERISTICS": [
    {
      "INFO_SUPPORTED_HARDWARE_LEVEL": 1,
      "LENS_FACING": 1,
      "SENSOR_INFO_PHYSICAL_SIZE_WIDTH": 5.644999980926514,
      "SENSOR_INFO_PHYSICAL_SIZE_HEIGHT": 4.434999942779541,
      "SENSOR_INFO_PIXEL_ARRAY_SIZE_WIDTH": 4032,
      "SENSOR_INFO_PIXEL_ARRAY_SIZE_HEIGHT": 3024,
      "LENS_INFO_AVAILABLE_FOCAL_LENGTHS": [
        {
          "FOCAL_LENGTH": 4.199999809265137
        }
      ]
    },

    {
      "INFO_SUPPORTED_HARDWARE_LEVEL": 0,
      "LENS_FACING": 0,
      "SENSOR_INFO_PHYSICAL_SIZE_WIDTH": 3.494999885559082,
      "SENSOR_INFO_PHYSICAL_SIZE_HEIGHT": 2.625999927520752,
      "SENSOR_INFO_PIXEL_ARRAY_SIZE_WIDTH": 2608,
      "SENSOR_INFO_PIXEL_ARRAY_SIZE_HEIGHT": 1960,
      "LENS_INFO_AVAILABLE_FOCAL_LENGTHS": [
        {
          "FOCAL_LENGTH": 2.0999999046325684
        }
      ]
    }
  ]
}
```

### tapToFocus(xPoint, yPoint, [successCallback, errorCallback])

<info>Establece un punto de enfoque específico. Tenga en cuenta que esto supone que la cámara está en pantalla completa.</info><br/>

```javascript
let xPunto = evento.x;
let yPunto = evento.y
CameraPreview.tapToFocus(xPoint, yPoint);
```

### onBackButton(devolución de llamada exitosa, [devolución de llamada de error])

<info>Evento de devolución de llamada para tocar el botón Atrás</info><br/>

```javascript
CameraPreview.onBackButton(función() {
  console.log('Botón Atrás presionado');
});
```

### getBlob(url, [devolución de llamada exitosa, devolución de llamada de error])

Cuando trabaje con archivos locales, es posible que desee mostrarlos en ciertos contenedores como lienzo,
dado que file:// no siempre es un tipo de URL válido, primero debe convertirlo explícitamente a
una gota, antes de empujarla más hacia el lado de la pantalla. La función getBlob hará el
conversión adecuada para usted, y si tiene éxito, pasará el contenido en su función de devolución de llamada como
primer argumento.

```javascript

function mostrarImagen(contenido) {
  var ctx = $("lienzo").getContext('2d');

  img.onload = función(){
    ctx.drawImage(img, 0, 0)
  }

  img.src = URL.createObjectURL(blob);
}

función tomarFoto() {
  CameraPreview.takePicture({ancho: aplicación.dimensión.ancho, altura: aplicación.dimensión.altura}, función (datos){
    if (cordova.platformId === 'android') {
      CameraPreview.getBlob('archivo://' + datos, función(imagen) {
        mostrarImagen(imagen);
      });
    } demás {
      mostrarImagen('datos:imagen/jpeg;base64,' + datos);
    }
  });
}

```

### startRecordVideo(options, cb, [errorCallback])

*Actualmente, esta característica es solo para Android. Un PR para el soporte de iOS sería felizmente aceptado *

<info>Comience a grabar video en el caché.</info><br/>

```javascript
var opciones = {
   dirección de la cámara: CameraPreview.CAMERA_DIRECTION.BACK,
   ancho: (ventana.pantalla.ancho / 2),
   altura: (ventana.pantalla.altura / 2),
   calidad: 60,
   con Flash: falso
}

CameraPreview.startRecordVideo(opciones, función(filePath){
   consola.log(ruta del archivo)
});
```

### detenerGrabarVideo(cb, [errorCallback])

*Actualmente, esta característica es solo para Android. Un PR para el soporte de iOS sería felizmente aceptado *

<info>Detener la grabación de video y devolver la ruta del archivo de video</info><br/>

```javascript
CameraPreview.stopRecordVideo(función(filePath) {
   consola.log(ruta del archivo);
});
```

# Ajustes

<a name="Configuración_de_la_cámara.ModoEnfoque"></a>

### FOCUS_MODE

<info>Focus mode settings:</info><br/>

| Name | Type | Default | Note |
| --- | --- | --- | --- |
| FIXED | string | fixed |  |
| AUTO | string | auto |  |
| CONTINUOUS | string | continuous | IOS Only |
| CONTINUOUS_PICTURE | string | continuous-picture | Android Only |
| CONTINUOUS_VIDEO | string | continuous-video | Android Only |
| EDOF | string | edof | Android Only |
| INFINITY | string | infinity | Android Only |
| MACRO | string | macro | Android Only |

<a name="camera_Settings.FlashMode"></a>

### FLASH_MODE

<info>Flash mode settings:</info><br/>

| Name | Type | Default | Note |
| --- | --- | --- | --- |
| OFF | string | off |  |
| ON | string | on |  |
| AUTO | string | auto |  |
| RED_EYE | string | red-eye | Android Only |
| TORCH | string | torch |  |

<a name="camera_Settings.CameraDirection"></a>

### CAMERA_DIRECTION

<info>Configuración de la dirección de la cámara:</info><br/>

| Name | Type | Default |
| --- | --- | --- |
| BACK | string | back |
| FRONT | string | front |

<a name="camera_Settings.ColorEffect"></a>

### COLOR_EFFECT

<info>Configuración de efectos de color:</info><br/>

| Name | Type | Default | Note |
| --- | --- | --- | --- |
| AQUA | string | aqua | Android Only |
| BLACKBOARD | string | blackboard | Android Only |
| MONO | string | mono | |
| NEGATIVE | string | negative | |
| NONE | string | none | |
| POSTERIZE | string | posterize | |
| SEPIA | string | sepia | |
| SOLARIZE | string | solarize | Android Only |
| WHITEBOARD | string | whiteboard | Android Only |

<a name="camera_Settings.ExposureMode"></a>

### EXPOSURE_MODE

<info>Configuración del modo de exposición:</info><br/>

| Name | Type | Default | Note |
| --- | --- | --- | --- |
| AUTO | string | auto | IOS Only |
| CONTINUOUS | string | continuous | |
| CUSTOM | string | custom | |
| LOCK | string | lock | IOS Only |

Nota: Use AUTO para permitir que el dispositivo ajuste automáticamente la exposición una vez y luego cambie el modo de exposición a BLOQUEO.

<a name="camera_Settings.WhiteBalanceMode"></a>

### WHITE_BALANCE_MODE

<info>Configuración del modo de balance de blancos:</info><br/>

| Name | Type | Default | Note |
| --- | --- | --- | --- |
| LOCK | string | lock | |
| AUTO | string | auto | |
| CONTINUOUS | string | continuous | IOS Only |
| INCANDESCENT | string | incandescent | |
| CLOUDY_DAYLIGHT | string | cloudy-daylight | |
| DAYLIGHT | string | daylight | |
| FLUORESCENT | string | fluorescent | |
| SHADE | string | shade | |
| TWILIGHT | string | twilight | |
| WARM_FLUORESCENT | string | warm-fluorescent | |

# Sample App

<a href="https://github.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview-sample-app">cordova-plugin-camera-preview-sample-app</a> para un ejemplo completo de Cordova en funcionamiento para las plataformas Android e iOS.

# capturas de pantalla

<img src="https://raw.githubusercontent.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview/master/img/android-1.png"/> <img hspace="20" src="https://raw.githubusercontent.com/cordova-plugin-camera-preview/cordova-plugin-camera-preview/master/img/android-2.png"/>

# Créditos

Mantenido por [Weston Ganger](https://westonganger.com) - [@westonganger](https://github.com/westonganger)

Creado por Marcel Barbosa Pinto [@mbppower](https://github.com/mbppower)
