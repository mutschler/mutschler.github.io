---
layout: post
title: Backup Script überarbeitet
date: 2014-05-08 15:16:30.000000000 +02:00
---
Vor einigen Tagen habe ich über das [Backup meines uberspace Acounts](http://blog.raphaelmutschler.de/uberspace-backup-auf-synology-diskstation/) gesprochen, ich habe mich dazu entschieden das Backup Script noch einmal etwas zu überarbeiten.

Ich habe einige Zusätzliche Variablen eingeführt, automatisches erstellen des Backup Ordners (falls nicht vorhanden), Speicherplatz Check sowie Backup Rotation und symlink des aktuellsten Backups auf den `latest` Ordner.

Backups werden nun in Ordnern mit dem ISO-8601 Datum gesichert (2014-05-08) außerdem werden die Backups nun Inkrementell vorgenommen, so dass ich bis zu 14 Tage zurück gehen kann, sollte das nötig sein, dabei werden symlinks auf Dateien gesetzt welche sich nicht geändert haben um Platz zu sparen.

Das neue Skript erzeugt nun Backups in dieser Form:

{% highlight shell %}
ls -la /volume1/Backup/server.uberspace.de/
  2014-05-08
  2014-05-07
  2014-05-06
  backup.log
  latest -> /volume1/Backup/server.uberspace.de/2014-05-08
{% endhighlight %}

## Die Variablen:
Ich denke die Server Settings sollten weitgehend klar sein...

`SERVER` wird nun auch für den lokalen/backup Ordner Namen verwendet  
`DAY0` dabei handelt es sich um das AKTUELLE Datum im ISO-8601 Format  
`DAY1` ist das Datum vom vorheringen Tag (für die symlinks)  
`DAY_MAY` Datum des ältesten Backups (wird gelöscht wenn vorhanden)  
`RSYNC` der link zum rsync binary, kann an unterschiedlichen orten sein  
`GB_FREE` Wie viel freier Speicherplatz mindestens vorhanden sein muss um das Backup zu starten (in GB)

## Das neue Script

{% highlight bash %}
#!/bin/sh

#server settings
USER="username"
SERVER="server.uberspace.de"
PORT="22"
SSHID="/volume1/homes/backup/.ssh/id_rsa"
SOURCE="/var/www/virtual/username/html :/mysqlbackup/latest/username"

#binarys
RSYNC="/usr/syno/bin/rsync"
AWK="/usr/bin/awk"
LN="/bin/ln"
DATE="/opt/bin/date"

#lokale variablen
#Heutiges Datum im ISO-8601 Format
DAY0=`$DATE -I`
#Datum von gestern
DAY1=`$DATE -I -d "1 day ago"`
#maximales backup
DAY_MAX=`$DATE -I -d "14 days ago"`
BACKUP_DRIVE="/volume1/Backup"
BACKUP_FOLDER="$BACKUP_DRIVE/$SERVER"
TARGET="$BACKUP_FOLDER/$DAY0"
LNK="$BACKUP_FOLDER/$DAY1"
LOG="$BACKUP_FOLDER/backup.log"
#mindestens x GB frei
GB_FREE="100"


#check ob genug noch platz auf der platte ist
HDMINFREE=$(($GB_FREE * 1024 * 1024))
KBISFREE=$(df $BACKUP_DRIVE | tail -n1 | $AWK '{print $4}')
if [ $KBISFREE -lt $HDMINFREE ] ; then
	echo "Fatal: Not enough space left for rsyncing backups!" >> $LOG
	exit
fi

#schau ob backup folder vorhanden ist wenn nicht wird er angelegt
if [ ! -e $BACKUP_FOLDER ]; then
	mkdir $BACKUP_FOLDER
fi

$RSYNC -avz --delete --progress --link-dest="$LNK" -e "ssh -p $PORT -i $SSHID" $USER@$SERVER:$SOURCE $TARGET >> $LOG 2>&1

#symlink latest auf heutiges datum
if [ -d $BACKUP_FOLDER/$DAY0 ]; then
	$LN -sfn $TARGET $BACKUP_FOLDER/latest
fi

#loeschen des aeltesten Backups
if [ -d $BACKUP_FOLDER/$DAY_MAX ]; then
	rm -rf $BACKUP_FOLDER/$DAY_MAX
fi
{% endhighlight %}
