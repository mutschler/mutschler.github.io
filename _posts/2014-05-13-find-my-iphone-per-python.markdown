---
layout: post
title: Find my iPhone per python
date: 2014-05-13 19:40:52.000000000 +02:00
---
Ich bastel ab und zu immer mal wieder an meinem Raspberry rum. Zuletzt habe ich mir einen Funk transmitter gekauft, welcher in Zukunft einige Lampen in meiner Wohung steuern wird. Funk Thermostate sind auch schon verbaut und werden wohl das nächste Projekt werden.

Ich habe schon vor längerer Zeit angefangen mit der Idee zu spielen möglichst vieles zu Automatisieren bzw per PC und/oder Smartphone zu steuern. Der erste Schritt den ich in dieser Richtung unternommen habe, damals noch an einem alten Mac Mini, war wenn ich von der Arbeit nach Hause komme automatisch eine entsprechende Playlist in iTunes zu starten sowie meine einige meiner Lieblingsseiten/Feeds nach News zu durchsuchen die für mich Interessant sind.

Dazu habe ich mir damals einen haufen Skripte zusammen gebaut und war lange auf der Suche nach einer geeigneten Schnittstelle für die Automatische erkennung wann ich zuhause bin und wann nicht.

Ich hab mit Latitude von Google experimentiert ebenso wie mit einigen anderen Location Diensten deren Namen ich schon längst wieder vergessen habe.

Bis mir einviel, dass Apple mit Find my iPhone ja eigentlich schon alles bietet was man braucht, WENN es doch nur ein API gäbe. Leider konnte ich dazu nichts finden was funktioniert hätte, daher habe ich mich kurzerhand hingesetzt und meine eigene Library dafür gebastelt.

## FindMyI
FindMyI ist mein ansatz per python zugriff auf den iCloud Lokalisierungsdienst des eigenen iDevices zu bekommen.

{% highlight python %}
from findmyi import FMI

finder = FMI( "yourmail@me.com", "PASSWORD" )
finder.updateDevice()

for deviceNum in range(len(finder.devices)):
    print finder.devices[deviceNum]
{% endhighlight %}

gibt in etwa das hier zurück:
{% highlight json %}
{
	'displayName': u'iPad',
	'name': u'Huntsman',
	'battery': 0.0,
	'model': u'ThirdGen-white',
	'id': u'DEVICEIDGOESHERe',
	'latitude': 52.1348086086,
	'longitude': 6.20540518,
	'accuracy': 100.0,
	'positionType': u'Wifi',
	'updateTime': 1375958135394
}
{% endhighlight %}

per latitude / longitude und etwas geoCoding kann dann relativ schnell auch die Position bestimmt werden. Bei mir war das ganze damals so eingestellt das sobald ich mich in einem Radius von 20 Metern um meine Adresse befand das Script weitere Scripte aufgerufen hat.

Später wurde das ganze dann erweitert um entsprechende Einstellungen wenn ich weiter als 10km weg bin etc.

In Zukunft werde ich das ganze aber wohl nicht mehr für die "bin ich Zuhause" Funktionalität nutzen (dazu ist es einfach zu ungenau) dafür aber z.B. für einige andere Sachen:

* wenn ich über einen Längeren Zeitraum nicht im Umkreis von 500 Metern von zuhause war könnte man die Heizung noch weiter herunter fahren etc.
* Ich bin bereits seit 5 Tagen nicht zuhause gewesen, dann wird Abends mal für einige Stunden das Licht angeschaltet (um den eindruck zu erwecken ich wäre Zuhause)

und so weiter und so fort... eigentlich ist so ziemlich alles möglich was nur entfernt mir meiner Position zu tun hat.

Wens Interessiert, das ganze hat noch einige weitere optionen wie zb Senden einer Nachricht, Sperren und/oder fern löschen des Gerätes... Auch wenn ich letzteres nie ausprobiert habe.

Den Code gibts auf [Bitbucket](https://bitbucket.org/raphaelmutschler/fmi)

oder per pip / easy_install (https://pypi.python.org/pypi/findmyi/0.2)

{% highlight plaintext %}
pip install findmyi
{% endhighlight %}
