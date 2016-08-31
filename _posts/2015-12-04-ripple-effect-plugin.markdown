---
layout: post
title: ripple effect plugin
date: 2015-12-04 12:27:45.000000000 +01:00
author: Ruffy
---
In letzter Zeit ist der "Ripple Effect" von Google's Material Design Guidelines immer öfter in der Kategorie "können wir das nicht auch schnell einbauen" aufgetaucht.

Also dachte ich mir, schreib ich doch direkt ein kleines Plugin. Ja, ich weiß es gibt schon zu genüge Plugins welche den gewünschten Effekt erziehlen, aber die meisten waren dann doch etwas zu "advanced" für meinen Anwendungsfall.

Ich wollte ein einfaches Plugin, das mit möglichst wenig aufwand und vorallem Quellcode den gewünschten Effekt erzieht. So dass JEDER (auch der End-Kunde) dieses Einfach verwenden kann.

Herausgekommen ist "ripple" ein 1.400 Zeichen bzw 8kb großes Plugin das einfach nur eingebunden werden muss und anhand von klassen den gewünschten effekt auf jedes HTML Element anwendbar macht.

Dazu gibt es eine [Doku und Demo](http://dev.raphaelmutschler.de/ripple) so wie die dazugehörige [Bitbucket Repo](https://bitbucket.org/raphaelmutschler/ripple) außerdem ist das ganze einfach per `bower install ripples` installierbar.

"ripple" kommt mit zwei vorgefertigten klassen `ripple` welche den Effekt in weiß darstellt so wie `ripple-dark` als schwarze alternative. Weitere Farb kombinationen sind einfach selbst hinzuzufügen indem die klasse `.ripple .ink` mit einer anderen Hintergrundfarbe überschrieben wird.

Bsp:

{% highlight css %}
.btn-success.ripple .ink {
    background: lime;
}
{% endhighlight %}
