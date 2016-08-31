---
layout: post
title: MediaThumber (mt) update
date: 2015-08-18 14:22:42.000000000 +02:00
---
Es ist genau einen Monat her, dass ich hier über MediaThumber (mt) geschrieben habe. In der zwischenzeit hab ich dem Teil ein paar neue funktionen und Bug-Fixes spendiert unter anderem:

* zu schwarze Bilder werden nicht mehr gespeichert
* alle verfügbaren cores werden benutzt (performance verbesserung)
* Eine Option um den die Codecs, Bitrate und FPS in den Header zu schreiben
* 2 neue Filter (cross processing und film strip)
* option für eigene ausgabe pfade (1.0.3-dev)
* option um einen Start- und Endzeitpunkt anzugeben (1.0.3-dev)
* DroidSans als Standart Schrift
* PreCompiled Binarie files (für 1.0.2)

### Optionen:

* **--bg-content="0,0,0"** Hintergrundfarbe des Contents (r,g,b)
* **--bg-header="0,0,0"** Hintergundfarbe des Headers (r,g,b)
* **--columns=2** Anzahl an Spalten in denen die Bilder angeordnet werden
* **--disable-timestamps=false** Timestamps auf den einzelnen Bildern ausschalten
* **--fg-header="255,255,255"** Schrift Farbe des Headers (r,g,b)
* **--filter="none"** Filter die auf die einzelnen Bilder angewendet werden sollen (mehrere mit Komma getrannt sind möglich)
* **--filters** eine Auflistung der verfügbaren Filter
* **--font** Pfad zu einer `ttf` Font-Datei um diese zu Verwenden
* **--font-size=12** Schriftgöße in pt
* **--from="00:00:00"** Startpunkt für Screenshots im Format HH:MM:SS
* **--to="00:00:00"** Endpunkt für Screenshots im Format HH:MM:SS
* **--header="true"** Header an Screenshots anhängen (true/false)
* **--header-image=""** Pfad zu einem Bild welches in den Header Integriert werden soll
* **--header-meta="false"** Codec, FPS und Bitrate in den Header schreiben (true/false)
* **--numcaps=4** Anzahl der Screenshots die gemacht werden sollen
* **--output="{% raw %}{{.Path}}{{.Name}}{% endraw %}.jpg"** Speicher Pfad
* **--padding=10** Abstand zwischen den einzelnen Bildern
* **--single-images="false"** Einzelbilder statt Kontaktabzug (true/false)
* **--skip-blank="false"** Leere/Schwarze Bilder überspringen (true/false)
* **--verbose** Loggt auch DEBUG informationen
* **--version** Version anzeigen
* **--width=400** Breite eines einzelnen Bildes in px

Diese Liste (auf englisch) kann natürlich auch mit `mt --help` angezeigt werden.

Hier noch mal der Link dazu:
https://github.com/mutschler/mt/

ab Version 1.0.2 gibt es auch fertige (linux) binaries zum download:
https://github.com/mutschler/mt/releases
