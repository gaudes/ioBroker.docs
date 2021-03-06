---
translatedFrom: en
translatedWarning: Wenn Sie dieses Dokument bearbeiten möchten, löschen Sie bitte das Feld "translationsFrom". Andernfalls wird dieses Dokument automatisch erneut übersetzt
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/de/adapterref/iobroker.phantomjs/README.md
title: ioBroker.phantomjs
hash: wB5lseMg/xyHiORnWgccglXroLLv8OmrbnMbjHmG0Y4=
---
![Logo](../../../en/adapterref/iobroker.phantomjs/admin/phantomjs.png)

![Anzahl der Installationen](http://iobroker.live/badges/phantomjs-stable.svg)
![NPM-Version](http://img.shields.io/npm/v/iobroker.phantomjs.svg)
![Downloads](https://img.shields.io/npm/dm/iobroker.phantomjs.svg)
![Tests](https://travis-ci.org/ioBroker/ioBroker.phantomjs.svg?branch=master)
![NPM](https://nodei.co/npm/iobroker.phantomjs.png?downloads=true)

# IoBroker.phantomjs
Mit diesem Adapter können Sie Screenshots von Webseiten (z. B. Flot) erstellen und als PNG-Datei speichern oder über einen internen WEB-Server freigeben.

Der Benutzer kann diese Datei später per E-Mail oder per Telegramm oder was auch immer senden.

Langsame Webclients können die Diagramme anzeigen, wenn die Diagramme automatisch alle x Minuten generiert werden.

## Voraussetzung
Es wird ein vorgefertigtes Paket von Phantomjs verwendet. Wenn für Ihr System der Prebuild nicht vorhanden ist, können Sie diesen Adapter nicht verwenden.
Auf einigen Linux-Systemen ist die zusätzliche Bibliothek "libfontconfig" erforderlich. Es kann wie folgt installiert werden:

```
sudo apt-get install libfontconfig
```

## Fehler installieren
Wenn Sie einen Fehler wie erhalten

```
Unexpected platform or architecture: linux/armIt seems there is no binary available for your platform/architecture Try to install PhantomJS globally
```

Während der Installation tut es uns dann leid. Sie müssen googeln und suchen, wie Phantomjs global auf Ihrem System installiert werden können.

## Verwendung
Es gibt zwei Möglichkeiten, Bilder zu erstellen.

### Über Staaten
Durch die Erstellung der Instanz für Staaten werden erstellt:

- **Dateiname** - Dateiname, in dem das Bild gespeichert wird. Wenn der Pfad nicht absolut ist, ist er relativ zu `` `... / iobroker / node_modules / iobroker.phantomjs```.
- **Breite** - Breite des Bildes. Standardwert 800px.
- **Höhe** - Höhe des Bildes. Standardwert 600px.
- **Paging** - Format der PDF-Seite. Der Dateiname muss mit ".pdf" enden.
- **renderTime** - Intervall in ms, um zu warten, bis die Seite gerendert wird.
- **online** - Auf Anfrage muss das URL-Bild auf den internen Webserver hochgeladen werden. Der Zugriff kann dann über http:// ip: 8082 / state / phantomjs.0.pictures.filename_png erfolgen
- **clipTop** - obere Position des Clip-Rechtecks. Standardwert 0px.
- **clipLeft** - linke Position des Clip-Rechtecks. Standardwert 0px.
- **clipWidth** - Breite des Clip-Rechtecks. Der Standardwert entspricht der Breite. Achtung, dieser Wert wird jedes Mal durch die Änderung der Breite überschrieben.
- **clipHeight** - Höhenposition des Cliprechtecks. Der Standardwert entspricht der Höhe. Achtung, dieser Wert wird jedes Mal durch die Höhenänderung überschrieben.
- **scrollTop** - Bildlauf nach oben. Standardwert 0px.
- **scrollLeft** - Bildlauf nach links. Standardwert 0px.

Nachdem der URL-Status geschrieben wurde, versucht der Adapter, das Bild zu erstellen, und ändert beim Erstellen das Bestätigungsflag des Status **url** in true.

### Über Nachrichten
Mit dem Skriptcode wie folgt:

```
sendTo('phantomjs.0', 'send', {
    url:                    'http://localhost:8082/flot/index.html?l%5B0%5D%5Bid%5D=system.adapter.admin.0.memHeapTotal&l%5B0%5D%5Boffset%5D=0&l%5B0%5D%5Bart%5D=average&l%5B0%5D%5Bcolor%5D=%23FF0000&l%5B0%5D%5Bthickness%5D=3&l%5B0%5D%5Bshadowsize%5D=3&timeArt=relative&relativeEnd=now&range=10&live=false&aggregateType=step&aggregateSpan=300&hoverDetail=false&useComma=false&zoom=false',
    output:                 'picture.png',  // default value
    width:                  800,            // default value
    height:                 600,            // default value
    timeout:                2000,           // default value
    zoom:                   1,              // default value

    'clip-top':             0,              // default value
    'clip-left':            0,              // default value
    'clip-width':           800,            // default value is equal to width
    'clip-height':          600,            // default value is equal to height
    'scroll-top':           0,              // default value
    'scroll-left':          0,              // default value

    online:                 false           // default value
}, function (result) {
    if (result.error) {
        console.error(JSON.stringify(result.error));
    }
    if (result.stderr) {
        console.error(result.stderr);
    }
    if (result.stdout) {
        console.log(result.stdout);
    }
    console.log(result.output);
});
```

Sie können einen Screenshot einer URL erstellen. Nur das Feld **url** ist obligatorisch, alle anderen sind optional und werden aus den aktuellen Einstellungen ausgefüllt.

### PDF-Generierung
```
sendTo('phantomjs.0', 'send', {
    url:                    'http://localhost:8082/flot/index.html?l%5B0%5D%5Bid%5D=system.adapter.admin.0.memHeapTotal&l%5B0%5D%5Boffset%5D=0&l%5B0%5D%5Bart%5D=average&l%5B0%5D%5Bcolor%5D=%23FF0000&l%5B0%5D%5Bthickness%5D=3&l%5B0%5D%5Bshadowsize%5D=3&timeArt=relative&relativeEnd=now&range=10&live=false&aggregateType=step&aggregateSpan=300&hoverDetail=false&useComma=false&zoom=false',
    output:                 'document.pdf',

    'paper-margin':         '0cm',          // paper-margin or paper-margin-top/paper-margin-left
    'paper-margin-top':     0,
    'paper-margin-left':    0,

    // only one of
    // 1.
    'paper-format':         'A4',           // 'A3', 'A4', 'A5', 'Legal', 'Letter', 'Tabloid': 'paper-format' should be used with 'paper-orientation'
    'paper-orientation':    'portrait',     // 'portrait', 'landscape'

    // 2.
    'paper-width':          200,            // '5in',   '10cm': 'paper-width' should be used 'paper-height'
    'paper-height':         300,            // '7.5in', '20cm'

    timeout:                2000            // default value
}, function (result) {
    if (result.error) {
        console.error(JSON.stringify(result.error));
    }
    if (result.stderr) {
        console.error(result.stderr);
    }
    if (result.stdout) {
        console.log(result.stdout);
    }
    console.log(result.output);
});
```

Unterstützte Maßeinheiten sind: 'mm', 'cm', 'in', 'px'. Keine Einheit bedeutet "px".

Sie können mehr über phantomJS [Hier](http://phantomjs.org/api/webpage/property/paper-size.html) lesen.

## Changelog

### 1.1.2 (2020-07-28)
* (Apollon77) added ssl handling to ignore self signed ssl certificates

### 1.1.1 (2020-07-27)
* (Apollon77) libfontconfig automatically installed

### 1.0.2 (2020-07-24)
* (Apollon77) Add config to automatically install libfontconfig when js-controller 3+ is used
* (Apollon77) Add ignore-ssl-errors=true to parameters to prevent error with self signed ssl certs
* (Apollon77) Adjust state description to not confuse witha static port number :-)

### 1.0.1 (2018-05-04)
* (bluefox) Problem with page size fixed

### 1.0.0 (2018-02-19)
* (bluefox) clipping support
* (bluefox) IMPORTANT: paging is replaces by 'paper-xxx' options.

### 0.1.3 (2017-09-24)
* (bluefox) add pdf support

### 0.1.2 (2016-04-30)
* (bluefox) change package name from phantomjs to phantomjs-prebuilt

### 0.1.0 (2016-04-30)
* (bluefox) add renderTime
* (bluefox) add upload to local web-server

### 0.0.1 (2016-04-28)
* (bluefox) initial commit

## License
Copyright 2016-2020 bluefox <dogafox@gmail.com>.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an 
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific 
language governing permissions and limitations under the License.