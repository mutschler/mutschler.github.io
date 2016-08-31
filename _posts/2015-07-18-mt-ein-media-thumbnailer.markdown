---
layout: post
title: 'mt: ein Media Thumbnailer'
date: 2015-07-18 23:00:10.000000000 +02:00
---
Ich war schon länger auf der Suche nach einem Tool dass mir Kontaktabzüge von Videos machen kann. Da schwirren ja bereits so einige rum, aber wenn man das ganze automatisiert und ohne GUI laufen lassen will, wirds dann schon deutlich schwieriger.

# Anforderungen

Ich hatte eigentlich hauptsächlich 3 Anforderungen:

1. Sollte sowohl unter linux als auch auf meinem Mac laufen
2. Geschwindigkeit
3. mögliche Automatisierung/scripting

Bisher habe ich dafür immer [vcs](http://p.outlyer.net/vcs/) verwendet, das braucht aber einige abhängigkeiten (ImageMagick, ffmpeg) und arbeitet nur mit der GNU version von getopt welche am Mac nach installiert werden muss.

# Lösung

Da ich schon seit längerem mit dem Gedanken spiele [go](http://golang.com) zu lernen war das die Möglichkeit endlich damit anzufangen.

Herausgekommen ist [mt](https://bitbucket.org/raphaelmutschler/mt/) ein kleines tool das bis auf libavcodec keinerlei abhängigkeiten an 3rd Party Libraries stellt und relativ schnell und einfach auf alle möglichen Plattformen verteilt werden kann. Für meine DiskStation nutze ich ein static binary welches libavcodec bereits mit beinhaltet.

# Funktionen

Anfänglich konnte mt wirklich nur mit sehr reduziertem funktionsumfang aufwarten, mittlerweile habe ich diesen aber um einiges erweitert:

* Kontaktabzüge oder einzel Bilder
* Anzahl, Größe und Abstand der einzelnen Screenshots kann bestimmt werden
* Farben für Header und Hintergrund können definiert werden
* möglichkeit ein Wasserzeichen einzubinden
* Logo oder ähnliches kann in den Header integriert werden
* Schrift und Schriftgröße kann geändert werden
* SW und Invert Filter für Bilder

Im laufe der Zeit wird da sicher noch die ein oder andere Funktion dazu kommen.
