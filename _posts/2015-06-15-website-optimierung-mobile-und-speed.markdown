---
layout: post
title: 'Website Optimierung: Mobilgeräte und Geschwindikeit'
date: 2015-06-15 17:42:19.000000000 +02:00
---
Ein immer wieder kehrendes Thema heutzutage ist die Optimierung von Websites für Mobile Endgeräte. Ich habe mir schon seit langem angewöhnt neue Projekte direkt auf einem Framework (in meinem Fall meist [Bootstrap](http://getbootstrap.com/) von twitter) auf zu bauen, da das im nachhinein viel Arbeit erstpart.

Zusätzlich haben sich im laufe der Zeit einige "helper Klassen" angesammelt, die natürlich auch häufiger bentzt werden (die bekannteste dürfte wohl der clearfix sein).

Da die verbeitung von Mobilen Endgeräten immer mehr zunimmt und sich auch das Nutzungsverhalten entsprechend ändert, sollte man allerdings auch auf die Größe/Ladezeit wert legen.

Bootstrap setzt seit Version 3 auf den Mobile First ansatz, welchen ich auch bei den meisten Projekten bevorzuge. Mobile Geräte werden eben immer wichtiger.

Ein weiterer wichtiger Punkt (für mich) ist die Geschwindigkeit der Website, sowohl auf Mobil Geräten als auch auf Desktop geräten, was immer schwerer zu lösen wird (unter anderem auch dank Retina und immer höher werdeneder Auflösung). Wenn ich eine Website gestalte und dort Bilder in Desktop auflösung unterbringe (evlt. sogar Retina) der Nutzer aber von Unterwegs mit einem Smartphone zugreift, kann das sehr schnell zu Unmut führen, da sich die Ladezeiten schnell in die Länge ziehen.

Natürlich bezieht sich das nicht nur auf Bilder und Grafiken sondern auf __jeden__ inhalt der Website (HTML, CSS, JavaScript, etc). Die Minimierung solcher Inhalte wird meist vernachlässigt, kann aber einiges an Geschwindigkeitsboost bringen und spart nebenbei noch Daten.

Ich selbst nutze für die Mehrheit meiner Projekte [grunt](http://gruntjs.com/) (oder einen anderen taskrunner). Dort definiere ich mir entsprechende Aufgaben, die dann der reihe nach ausgefürt werden und die Website optimieren.

Ich gehe hier kurz auf die Folgenden Themen ein:

* [Bilder & Grafiken Optimieren](#bilder)
* [CSS, JS und HTML Minimieren und Zusammenfügen](#css)
* [CDN Nutzen](#cdn)
* [Browser Caching Nutzen](#cache)
* [GZip bzw. Deflate Aktivieren](#deflate)
* [JavaScript am Seiten ende einbinden](#js)


### Bilder und Grafiken Optimieren<a id="bilder"></a>
Dazu nutze ich [jpegoptim](http://freecode.com/projects/jpegoptim/) mit `--strip-all --all-progressive` für JPG's und [OptiPNG](http://optipng.sourceforge.net/) für PNG's. Diese Tools optimieren die Dateigröße ohne Beeinträchtigung der Bildqualität.

Leider löst das nicht das Problem der verschiedenen Auflösungen. Es wird immer noch auf jedem Gerät das selbe Bild geladen und angezeigt. Dafür nutze ich verschiedene Lösungen, je nach Anwendungsfall:

1. Für dynamische Seiten mit Redaktionssystem [Adaptive Images](http://adaptive-images.com/) ein Server Seitiges PHP Script das unterschiedliche Bildgrößen in abhängigkeit der Auflösung bzw. Browser Weite generiert und liefert.

2. Für "statische" Seiten, also reines HTML und/oder Seiten bei denen wenig Bilder vorhanden sind und/oder Lösung 1 nicht funktioniert: [HiSRC](https://github.com/teleject/hisrc) ein jQuery Plugin das bilder abhängig von der Auflösung austauscht.

3. [Picturefill](https://github.com/scottjehl/picturefill) ein JavaScript polifyll des [HTML Picture Elements](https://html.spec.whatwg.org/multipage/embedded-content.html#embedded-content) auch für ältere Browser.

### CSS, JS, HTML Minimierung <a id="css"></a>
Da jedes KB zählt (ja auch leerzeichen "kosten" was), sollte man natürlich auch seine Scripte und Styles minimieren. Und wenn wir gerade dabei sind, vorzugsweiße auch direkt zusammenführen damit weniger HTTP Aufrufe zum Laden gemacht werden müssten, was wiederum die Ladezeit verkürzt.

Auch dafür habe ich einen eigenen Task in Grunt angelegt. Zuerst werden die entsprechenden Dateien zu einer Datei zusammengeführt (main.css und main.js) und anschließend minimiert.

Bei HTML Dateien werden lösche ich kommentare und leerzeichen/formatierungen diese sind für den Browser unnötig.

### CDN Nutzen <a id="cdn"></a>
CDN steht für Content Delivery Network. Vorweg ist zu sagen, dass dies meiner Meinung nach nur Sinn macht, wenn mit Besuchern aus Unterschiedlichen Geografischen Regionen zu rechnen ist. Dabei kann ein CDN enorme Vorteile haben, da nicht alle Daten vom eigenen Server sondern vom dem End-Nutzer am nächsten liegenden Server geladen werden. Außerdem kann es sein dass die Datei bereits beim User im Cache hinterlegt ist, sofern er vorher eine andere Seite besucht hat, die das selbe CDN nutzt.

Zu guterletzt hat auch das wieder auswirkungen auf die Geschwindigkeit und vor allem auf den Daten Verkehr des eigenen Servers.


### Browser Caching nutzen <a id="cache"></a>
So weit möglich sollte auf jeden Fall das Browser eigenen Caching genutzt werden, das hat beim ersten Besuch zwar kaum auswirkungen, macht sich dafür aber bei jedem weiteren Seitenaufruf bemerkbar. Dazu sollten auf jeden Fall die entsprechenden Header auf Serverseite gesetzt werden (Expires).

Ich nutze für CSS/JS meist eine Ablaufzeit von 1 Jahr und für Bilder/Schriften 1 Monat. Sollte sich eine Resource doch mal vor dem Geplanten ablauf Datum ändern, gibt es immer noch die Möglichkeit die aktuallisierung per URL-Fingerprinting (umbenennen der Datei) zu ändern.

### GZip & Deflate aktivieren <a id="deflate"></a>
Viele Webserver können heutzutage Daten vor dem Senden im gzip-Format komprimieren. Dadurch können diese Resourcen schneller heruntergeladen werden. Häufig ist das schon durch eine einfache Zeile in einer `.htaccess` möglich.


### JavaScript am Seitenende <a id="js"></a>
Der Browser läd eine Website bekanntlich von oben nach unten, daher sollten alle nicht zwingend nötigen Scripte und auch Stylesheets am Seitenende eingebunden werden. Scripte und Styles für Bereiche die erst nach dem Scrollen (below the fold) sichtbar sind werden somit erst geladen, wenn der User bereits einen Teil der Website angezeigt bekommt.

### Sonstiges
Ich persöhnlich versuche auch wo immer möglich Bilder progressive zu hinterlegen, so wird dem Benutzer bereits von Anfang an ein Bild gezeigt, welches nach und nach Schärfer wird.

Auch wenn mit HTTP 1.1 [Presistent Connections](https://en.wikipedia.org/wiki/HTTP_persistent_connection) eigentlich der Standart ist, sollte man sichergehen das dies auch Unterstütz wird
