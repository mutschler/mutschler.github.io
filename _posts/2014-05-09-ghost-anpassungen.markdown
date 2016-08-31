---
layout: post
title: 'Ghost Anpassungen #1'
date: 2014-05-09 14:16:51.000000000 +02:00
---
Nach dem der neue Blog nun einige Tage läuft, kann ich nur sagen ich bin begeistert von Ghost. Naja, jedenfalls wenn man mal darüber hinweg sieht dass ich node.js absolut nicht ausstehen kann.

Das Front- sowie Backend ist super minimalistisch gehalten was mir sehr gut gefällt. Allerdings kommen dann doch so ein zwei sachen die man gerne noch hätte... Prinzipiell geht alles und das meiste davon auch relativ einfach, wenn man weiß wo man schauen/etwas ändern muss.

## Kommentare
Ghost bietet _kein_ eigenes Kommentarsystem, was ich persöhnlich ziemlich gut finde, es gibt bereits genug lösungen dafür, wieso also dauernd das Rad neu erfinden, statt einfach eine bereits vorhandene Lösung für sich zu nutzen.

In meinem Fall ist das __Disquse__ und die Installation ist super einfach und innerhalb von wenigen Sekunden erledigt, eine ausführliche Anleitung dazu findet man auf der [Hilfe Seite von Disquse](http://help.disqus.com/customer/portal/articles/1454924-ghost-installation-instructions)


## Tracking
Die Meisten Wordpress Themes bieten dafür mittlerweile eine Option in die der Code eingetragen werden kann, bei Ghost muss das (noch) selbst im Template erledigt werden, aber auch hier ist das ganze wieder in wenigen Sekunden erledigt.

`/ghostdir/content/themes/themename/default.hbs` öffnen und den Tracking Code einfach vor `</body>` einfügen.
