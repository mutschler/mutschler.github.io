---
layout: post
title: plexivity
date: 2014-10-06 14:21:26.000000000 +02:00
---
Vor einiger Zeit habe ich mich entschlossen von [XBMC](http://xbmc.org/) auf [Plex](https://plex.tv/) umzusteigen. Hauptsächlich weil ich die getrennte Server/Client Umgebung ziemlich gut finde. Der Plex Media Server läuft direkt auf meiner DS1513+ und sendet die Infos an mehrere Clients (MacBook, Mac Mini, iPhone, iPad und Raspberry) so dass ich ohne Probleme im Schlafzimmer auf dem MacBook ein Film anfangen und im Wohnzimmer über den Rasperry mit [RasPlex](http://www.rasplex.com/) zu ende sehen kann.

Plex bietet auserdem die Möglichkeit über ein Webinterface die Media Daten direkt im Browser wieder zu geben, was relativ nützlich ist wenn man mal für längere Zeit nicht zuhause ist und oder die Letzen Urlaubs Videos/Bilder mit Freunden und Familie teilen will.

Nun bin ich jedoch ein kleiner Statistik "Freak" und da muss ich ehrlich zugeben fehlt mir persöhnlich bei Plex noch ein bisschen was. Es gibt zwar eine Seite welche die Aktuellen wiedergaben anzeigt, das wars dann aber auch schon so ziemlich. Ich würde allerdings gerne (direkt) wissen wenn jemand etwas von meinem Plex Server streamt, alleine schon aus dem Grund weil dadurch sowohl meine Internet Leitung als auch die Leistung meiner DS beeinträchtigt wird.

Eine kurze Suche im Internet hat mich auf [plexWatch](https://github.com/ljunkie/plexWatch/) gebracht, welches ich recht interessant fand, allerdings dank einiger abhängigkeiten nicht ohne weiteres (stichwort debian chroot) auf der DS läuft.Also habe ich mich dazu entschlossen meine eigenen Lösung zu schreiben: plexivity (Plex-activity)

![plexivity Login Screen](/content/images/2014/Oct/plexivity.jpg)

plexivity ist eine Flask App die in einem gewissen Intervall das Plex Web API abfragt und bei laufender Wiedergabe eine Push Nachricht über Pushover, Boxcar, Pushbullet oder per E-Mail sendet.

![plexivity Statistik](/content/images/2014/Oct/plexivity_stats_01.png)
Desweiteren gibt es einen Verlauf, Statistiken, Benutzer auflistung und Charts der angesehenen Medien.

plexivity befindet sich momentan noch in der Entwicklung ein Link folgt ;)
