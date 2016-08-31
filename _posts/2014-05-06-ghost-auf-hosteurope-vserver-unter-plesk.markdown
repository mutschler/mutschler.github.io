---
layout: post
title: Ghost auf Hosteurope vServer unter Plesk
date: 2014-05-06 20:25:38.000000000 +02:00
tags: [ghost, plesk, hosteurope]
---
Nach langem rum gefummel ist es mir nun endlich doch noch gelungen [Ghost](http://ghost.org) auf meinem Hosteurope vServer zum laufen zu bringen...

Dazu war allerdings einiges an try and error nötig, hier eine kurze zusammenfassung/anleitung wie es dann schlussendlich doch noch geklappt hat. Wie man sieht, hab ich das ganze als subdomain unter blog.raphaelmutschler.de laufen, die domain dazu wurde natürlich vorher in Plesk angelegt.

## Node.js installieren
als erstes muss mal node.js installiert werden und genau da gehts schon los... ein `apt-get install nodejs` gibt nämlich lediglich die meldung "Couldn't find package nodejs" aus. Das war wohl nichts. Bleiben zwei möglichkeiten:

1. Nodejs source code runterladen und auf dem server selbst compilieren
2. ein repository adden welches node packages hat (da hosteurope das wohl nicht mit liefert...)

Ich habe mich für den zweiten schritt entscheiden, aus dem einfachen Grund weils schneller geht, man lebt ja schließlich nicht ewig ;)

{% highlight bash %}
sudo apt-get update
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
{% endhighlight %}



noch schnell mit `node -v` und `npm -v` die versionen gecheckt und weiter gehts!

## Ghost installieren

nachdem nun alles soweit vorbereitet ist kann auch Ghost installiert werden (ich hab das ganze ins user verzeichnis installiert, es kann aber genauso gut das www dir benutzt werden), das hat sogar relativ gut geklappt:

{% highlight bash %}
curl -L https://ghost.org/zip/ghost-latest.zip -o ghost.zip
unzip -uo ghost.zip -d ghost
cd ghost
npm install --production
{% endhighlight %}

noch schnell die `config.example.js` nach `config.js` kopiert und entsprechend angepasst. Dabei ist zu erwähnen dass ich die dort eine feste IP eingetragen habe (sollte aber mit 0.0.0.0 auch funktionieren) meine config datei sieht jedenfalls so aus:

{% highlight markup %}
production: {
    url: 'http://blog.raphaelmutschler.de',
    mail: {},
    database: {
        client: 'sqlite3',
        connection: {
            filename: path.join(__dirname, '/content/data/ghost.db')
        },
        debug: false
    },
    server: {
        host: 'MEINEIP',
        port: 'XXXX'
    }
},
{% endhighlight %}

`npm start --production` in die console gehauen und das ding startet. YEAH! Denkste... Ghost ist nun aber immerhin schon einmal unter `MEINEIP:XXXX` zu erreichen... also weiter gehts.

## vHost einrichten
Hier wirds etwas tricky da Plesk absolut wiederlich ist sobald man etwas selbst ändern und/oder einstellen/installieren will was nicht von haus aus mit kommt und/oder über Plesk gemacht werden kann. Zum Glück gibt es die möglichkeit eine vhost.conf in die Plesk konfiguration des vHosts rein laden zu lassen. Nur wo muss die vhost.conf hin?

die File struktur ist ja in etwa so:

{% highlight markup %}
/var/www/vhosts/
  |- raphaelmutschler.de
    |- etc
    |- conf
    |- httpdocs
    |- subdomains
      |- blog.raphaelmutschler.de
        |- conf
        |- httpdocs
{% endhighlight %}

nun liegen die vHost config files aber nicht unter  `/var/www/vhosts/raphaelmutschler.de/subdomains/blog.raphaelmutschler.de/conf/`
sondern in `/var/www/vhosts/blog.raphaelmutschler.de/conf/`

also im zuletzt genannten Verzeichniss eine `vhost.conf` erstellen und mit inhalt füllen:

{% highlight apacheconf %}
ProxyRequests off
ProxyPass / http://MEINEIP:XXXX/
ProxyPassReverse / http://MEINEIP:XXXX/
{% endhighlight %}

unter normalen umständen wäre das ganze noch in einem `<VirtualHost *:80>` verschachtelt und auch `ServerName` und `ServerAlias` würden angegeben werden. Da das ganze aber später in eine von Plesk verwaltete config includiert wird, würde das im nächsten Punkt nur für unnötige Fehler sorgen.

Nun muss Plesk noch mitgeteilt werden, dass wir manuell an der config rum gebastelt haben und er sich die Daten doch bitte neu laden soll:
`/usr/local/psa/admin/bin/httpdmng --reconfigure-domain DOMAINNAME`
leider bringt im anschluss der Aufruf der URL blog.raphaelmutschler.de lediglich einen Fehler (403):
> Forbidden You don't have permission to access / on this server.


## Module aktiveren

Der Fehler der beim Aufruf der URL angezeigt wird kommt daher, dass das Apache Modul proxy nicht geladen ist:

{% highlight apacheconf %}
a2enmod proxy
a2enmod proxy_http
{% endhighlight %}

das zweite Modul am besten direkt mit aktivieren das spart einem die lästige Fehler suche wenn aus dem 403er ein 500er Fehler wird...

nun muss auch noch die config datei des Prox Mods bearbeitet werden: `/etc/apache2/mods-enabled/proxy.conf`

{% highlight apacheconf %}
<IfModule mod_proxy.c>
  ProxyRequests Off

  <Proxy *>
    AddDefaultCharset off
    Order deny, allow
    Allow from all
  </Proxy>

  ProxyVia On
</IfModule>  
{% endhighlight %}
Eigentlich muss nur aus `Deny from all` ein `Allow from all` gemacht werden...

diesmal reicht auch ein `service apache2 restart` aus um den apache mit den soeben aktivieren modulen neu zu starten und siehe da: es geht!

## forever and ever and ever...
Doof ist nur dass das ganze sofort wieder abschmiert sobald man die ssh verbindung zum server trennt... dafür gibt es 2 möglichkeiten:

1. das ganze per screen session laufen lassen
2. per npm forever installieren und das nutzen

Der erste Fall ist relativ schnell gehandhabt `screen npm start --production` und schon wars das... Wichtig ist lediglich das `--production` nicht zu vergessen, da das ganze sonst im development mode läuft.

Variante 2 besteht auch lediglich aus zwei zeilen code und ist mir persöhnlich etwas lieber:

{% highlight bash %}
sudo npm install forever -g
NODE_ENV=production forever start index.js
{% endhighlight %}

Dabei wird `--production` automatisch über `NODE_ENV` gesetzt und das ganze im hintergrund ausgeführt.

Wichtig dabei ist dass im home verzeichnis des nutzers der ordner `.forever` existiert und dort auch schreib rechte für den benutzer vorhanden sind.


Alles in allem hab ich etwas mehr als 45 Minuten daran gesessen wobei die meiste Zeit dafür drauf ging Fehlerquellen zu finden und zu beheben. Ich hoffe dass ich dem ein oder anderen damit ein paar Minuten schenken konnte.
