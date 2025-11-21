Plugin Bluebird para Cordova/Capacitor
============

> **Nota:** Este es un fork de [https://github.com/BlueFletch/cordova-bluebird-api](https://github.com/BlueFletch/cordova-bluebird-api). Se encuentra en proceso de migración a Capacitor. Por ahora, solo se ha actualizado para hacerlo compatible con versiones de SDK de Android > 34.

Este es un plugin de Cordova/Phonegap para interactuar con los lectores de códigos de barras y lectores de bandas magnéticas de los dispositivos robustos BlueBird. Ha sido probado en un BP30 y próximamente se probará en un BP70.

=============

Este plugin es compatible con plugman. Para instalarlo, ejecuta lo siguiente desde la línea de comandos de tu proyecto:
```$ cordova plugin add https://github.com/BlueFletch/cordova-bluebird-api.git```


==============

<h3>Modo de uso:</h3>
Debes registrar un *callback* que será llamado cuando ocurra un evento de lectura exitoso en el escáner o en el lector de banda magnética.

```
function loadBarcode(barcode) {
	console.log("barcode read : " + barcode);
	//TODO: handle barcode read
}
function handleMagstripeTracks(tracks) {
	console.log("read magstripe value : " + JSON.stringify(tracks));
   
	//read track 1
	var track1 = tracks[0].split('^');
	if (track1.length == 1)  {
		window.magstripe.read(function(){});
		return;
	}
	
	var cc = {
		number : track1[0].substr(1),
		name : track1[1].trim(),
		expr : '20' + track1[2].substr(0,2) + '-' + track1[2].substr(2,2)
	};
	console.log("read credit card " + JSON.stringify(cc));
	//TODO: handle magstripe
}
document.addEventListener("deviceready", function(){ 
	if (window.bluebirdBarcodeScanner) {
		console.log("initializing barcode scanner")
		bluebirdBarcodeScanner.register(loadBarcode, function (argument) {
			console.log("failed to register barcode scanner");
		});
	}
	 
	if (window.bluebirdMagstripe) {
		console.log("initializing magstripe")
		bluebirdMagstripe.register(handleMagstripeTracks, function (argument) {
				console.log("failed to register for mag stripe reader");
		});	
	}
}, false);
```

=============
<h3>More API options:</h3>

<h5>Magstripe:</h5>
* You need to "activate" the reader, by calling `bluebirdMagstripe.read()` when you need to read a credit card (for instance, on a checkout page).  A succcessful read will call your registration callback.

<h5>Scanner:</h5>
* You can wire a soft button to the barcode scanner by calling `bluebirdBarcodeScanner.softScanOn()`
* Turn off the scanner manually using: `bluebirdBarcodeScanner.softScanOff()`

==============
Copyright 2014 BlueFletch Mobile

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
