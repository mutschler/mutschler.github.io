---
layout: post
title: Uberspace Backup auf Synology DiskStation
date: 2014-05-06 21:12:29.000000000 +02:00
---

**UPDATE:** Eine überarbeitete Version des Backup-Scripts, gibt's [hier]({% link _posts/2014-05-08-backup-script-uberarbeitet.markdown %})

**ACHTUNG** Ab DSM Version 6, können sich nur noch Benutzer per SSH anmelden, die der Bentuzergruppe `admin` angehören! Vielen Dank an [Klaus](http://blog.raphaelmutschler.de/uberspace-backup-auf-synology-diskstation#comment-3158262553) für den Hinweis.

Vor einiger Zeit habe ich mir einen [uberspace](https://uberspace.de) Account gegönnt, hauptsächlich als testing Server für diversere kleinere Projekte. Heute wollte ich mich mal daran machen die Backups für meinen Account einzurichten. Es gibt zwar bereits eine – meiner Meinung nach – sehr gute integrierte Backup lösung bei der man sich um nichts kümmern muss, aber ich wollte dennoch auch ein lokales Backup auf meiner DiskStation haben.

Da ich bereits einen Backup-User habe (für andere Server etc.) habe ich mich dazu entschieden diesen auch hier zu verwenden. Theoretisch könnte dazu auch der admin account verwendet werden... aber da bin ich immer etwas... übervorsichtig.

Die vorbereitungen sind eigentlich recht einfach:

Per ssh auf die DiskStation verbinden, einen Key erstellen und copieren

{% highlight plaintext %}
ssh backup@DISKSTATION_IP
ssh-keygen -t rsa -C "meine@mail.de"
cat ~/.ssh/id_rsa.pub
{% endhighlight %}

den angezeigten Key dann dem überspace account hinzufügen und man sollte ohne probleme eine verbindung zum uberspace server herstellen können:

{% highlight plaintext %}
ssh user@server.uberspace.de
{% endhighlight %}

das hat schon mal funktioniert, als nächstes habe ich ein neues Verzeichnis auf meiner Backup Partition angelegt in welches ich in zukunft die Dateien sichern will...

{% highlight plaintext %}
mkdir /volume1/Backups/server.uberspace.de/
{% endhighlight %}

dann habe ich mir ein kleines script zusammen gebaut, welches sich per ssh zum uberspace server verbindet und das ganze dann per rsync in das ausgewählte verzeichnis sichert.

Dabei habe ich mich dazu entschieden auch jeweils den aktuellsten mysqldumt mit zu sichern, sicher ist sicher... wer weis vielleicht will ich das ganze ja wirklich mal als eine art "offline mirror" betreiben, dazu ist es auf jeden Fall nicht verkehrt auch die Datenbanken mit zu sichern...

{% highlight bash %}
#!/bin/sh
USER="username"
SERVER="server.uberspace.de"
PORT="22"
SSHID="/volume1/homes/backup/.ssh/id_rsa"
SOURCE="/var/www/virtual/username/html :/mysqlbackup/latest/username"
TARGET="/volume1/Backups/server.uberspace.de/"
LOG="/volume1/Backups/server.uberspace.de/backup.log"

/usr/syno/bin/rsync -avz --delete --progress -e "ssh -p $PORT -i $SSHID" $USER@$SERVER:$SOURCE $TARGET >> $LOG 2>&1
{% endhighlight %}

bei `SOURCE` habe ich 2 Verzeichnisse angegeben (trennung der Verzeichnisse durch `:`) welche ich sichern möchte, das ist zum einen der html ordner (enthält die website) und der symlink zum letzten mysqlbackup

Zum Schluss noch im DSM eingeloggt und unter `Systemsteuerung > Aufgabenplaner` einen Task angelegt und das wars auch schon.

Der Grund wieso ich das Backup von der DS aus ziehe und nicht vom uberspace server aus auf die DS schiebe, ist der selbe aus dem uberspace die eigenen Backups auch in dieser Art durchführt... sollte jemals durch irgend welche umstände irgend jemand zugriff auf meinen uberspace account haben, ist mein Backup sicher. Von uberspace aus gibt es darauf keinerlei zugriff!
