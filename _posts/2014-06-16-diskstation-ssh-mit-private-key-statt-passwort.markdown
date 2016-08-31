---
layout: post
title: 'DiskStation: ssh mit Private Key statt Passwort'
date: 2014-06-16 13:20:44.000000000 +02:00
---
Standart mäsig ist der ssh Zugriff auf die DiskStation nur mit Password möglich, wenn man allerdings den SSH zugriff in einigen Scripten benutz und/oder den Zugang allgemeine einfach öfters benötigt, wird das dauernde eintippen des Passworts relativ schnell lästig.

Abhilfe schaft die verbindung per Private Key.

## Vorbereitung

Zuerst müssen die entsprechenden Keys generiert werden, das geht unter Linux/OS X relativ einfach ([Anleitung für Windows](http://kb.siteground.com/how_to_generate_an_ssh_key_on_windows_using_putty/))  per terminal und dem befehl `ssh-keygen`.

{% highlight bash %}
	ssh-keygen -t rsa -C "your_email@example.com"
{% endhighlight %}

danach den Anweisungen folgen und warten bis der Schlüssel generiert wurde.

Wenn alles geklappt hat den Key per

{% highlight bash %}
	cat ~/.ssh/id_rsa.pub | pbcopy
{% endhighlight %}

anzeigen lassen und in die Zwischenablage kopieren.

## Key hinzufügen

Als nächstes muss der Key auf die DiskStation übertragen werden. Dazu wie gewohnt eine ssh Verbindung mit dem gewählten Nutzer erstellen.

Falls nicht bereits vorhanden muss nun der `.ssh` ordner sowie die `authorized_keys` Datei angelegt werden:

{% highlight bash %}
mkdir ~/.ssh
chmod 0700 ~/.ssh
touch ~/.ssh/authorized_keys
chown -R EUER_BENUTZER ~/.ssh
chmod 0600 ~/.ssh/authorized_keys
{% endhighlight %}

nun muss der zuvor kopierte Key in die `authorized_keys` eingetragen werden

{% highlight bash %}
	nano ~/.ssh/authorized_keys
{% endhighlight %}

und den Key einfügen.


## ssh neustarten

Als letzten Schritt muss SSH neu gestartet werden, das geht entweder über DSM oder per terminal mit

{% highlight bash %}
	/usr/syno/etc.defaults/rc.d/S95sshd.sh restart
{% endhighlight %}

nun sollte beim erneuten verbinden mit der DiskStation eine verbindung ohne eingabe eines Passworts möglich sein.
