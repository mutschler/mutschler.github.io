---
layout: post
title: 11 Monate TVDates – eine Zusammenfassung
date: 2015-07-02 23:30:03.000000000 +02:00
---
Schon 11 Monate ist es mittlerweile her dass TVDates offiziell in die BETA gestartet ist.

Uhrsprünglich als kleines, privates Übungsprojekt für Smarty und PHP gedacht, hat sich daraus schnell mehr entwickelt. Unter anderem auch dank der ersten 2 Tester die direkt Ihr Interesse angemeldet haben, das ganze auch weitergehend nutzen zu können.

Seit den ersten Schritten ist einiges geschehen: Es gab ein neues Theme, eine neue URL, diverse Verbesserungen am Backend und einige Optimierungen der Performance. Nach knapp 11 Monaten ist es nun mal an der Zeit ein kleines Fazit zu ziehen und ein bisschen mit den Zahlen zu spielen :)

### Die Idee
Angefangen hat alles, wie bereits erwähnt, als kleines Übungsprojekt. Ich dachte mir damals: Wäre doch klasse wenn es eine deutsche Alternative zu thetvdb.com gäbe. Und so wurde TVDates gebohren. Da ich thetvdb.com in einigen Projekten selbst nutze und in eben diesen Projekten auch gerne TVDates nutzen würde stand schnell fest: Die API __muss__ kompatibel sein! Jedenfalls teilweiße.

### Umsetzung
Anfangs noch mit einer lokalen MAMP Installtion wollte ich dann doch relativ schnell auf von außen erreichbaren Web Space umsteigen. Den anfang machte der Server auf dem auch dieser Blog läuft. Bis ich dann auf [uberspace](http://uberspace.de) gestoßen bin.

Ich habe mich bewusst gegen irgend welche PHP-Routing-Frameworks entschieden, nen bisschen was lernen/selbst tun wollte ich ja schließlich auch noch. Lediglich Smarty half mir bei der Umsetzung der Templates.

Frontend und Admin Bereich laufen komplett auf PHP mit eigenen Smarty templates und Routen, der anfängliche sha1 Login wurde durch salting und Login-Token-Rotation abgesichert, zusätzlich gibt's die Option der Zwei-Faktor-Authentifizierung (2FA) per Authy. Das Theme hat sich auch deutlich verändert und an der Performance hat sich ordentlich was getan, nicht zuletzt dank Caching der Files.

### Zahlen und Fakten

- 600GB Traffic
- 8GB HDD Ausnutzung (davon 6,5GB Bilder und 1,5GB API-Cache)
- 38MB Datenbank
- 542 Serien
- 34.000 Episoden
- 57.388.273 Hits
- 40 Benutzer (Registriert)

Der Monat mit den meisten Klicks war der März 2015 mit 13.541.043  die 85GB Traffic generiert haben.
Der Monat mit dem meisten Traffic war der Mai 2015 mit 92GB bei "gerademal" 8.837.179 Hits.

Das hat unter anderem dazu geführt dass ich die Bilder nun zustätzlich zum caching noch mit `jpegoptim` Optimiere und so sank der Traffic im Juni auf 60GB bei 6.819.885 im Vergleich sind das ~8KB Pro Aufruz zu vorher ~18KB.

### Wie geht es weiter?
Darüber denke ich öfter nach. Seit ich mir die Zahlen angeschaut habe erst recht. Wenn diese Zahlen schon in der BETA Zustande kommen, wie sieht das dann erst aus wenn das Offizell Online geht? Fest steht, sollte es so sein, muss noch einiges getan werden. Es sind noch einige Zeilen Code zu optimieren, ich muss mich defintiv nach einem eigenen vServer umsehen (das uberspace limit von 10GB Speicherplatz und 100GB Traffic hab ich ja jetzt schon fast geknackt) was wiederum viel Zeit und Kosten verursacht.

Eine Überlegung wäre, die API-Keys von thetvdb.com nicht mehr zu aktzeptieren sondern lediglich die eigenen, denn Knapp 45.000.000
der Hits wurden unter verwendung eines tvdb Keys getätigt. Das würde aber auch nur mittelfristig etwas ändern, da dann eben bei TVDates die Keys generiert werden. Ein API-Limit wäre denkbar, jedoch müsste ich mir auch darüber ausreichend Gedanken machen (wie viele hits in welchem Zeitraum, was passiert wenn das Limit überschritten wurde)

In naher Zukunft ist also erst mal nichts in dieser Richtung geplant. Auf längere Sicht wird sich eine Umstellung (welcher Art auch immer) nicht verhindern lassen, wenn das Projekt weitergeführt werden soll.
