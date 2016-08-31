---
layout: post
title: Zwei Faktor Authentifizierung mit Authy
date: 2014-07-18 17:58:16.000000000 +02:00
---
Ich habe schon länger mit dem Gedanken gespielt bei einigen meiner Projekte auch eine [Zwei-Faktor-Authentifizierung](http://de.wikipedia.org/wiki/Zwei-Faktor-Authentifizierung) – auch bekannt als 2FA oder "One Time Password" (OTP)  – anzubieten. Denn jeder zusätzliche Schritt gibt einem doch irgendwie wieder etwas mehr das Gefühl der Sicherheit.

Das ganze ist vorallem daran gescheitert dass ich keine Lösung gefunden habe die mir zugesagt hat, klar es gibt Google Authentificator aber Googles Hauptgeschäft sind immer noch Suchenanfragen und Werbung und man muss ja nicht noch mehr Informationen dort ablegen. Auch selbst etwas in der Art zu schreiben wäre eine Lösung denn das System auf dem die Meisten Anbieter aufbauen ist Open-Source, aber das kostet zum einen viel Zeit zum anderen müsste man dann auch eine App entwickeln die der User installier und/oder den Code per SMS verschicken. Und das Mobilfunknetzt ist ja bekanntlich auch nicht gerade das Sicherste, ganz davon abgesehen dass dann ein Provider für das versenden von SMS her müsste etc.

Irgendwann habe ich dann [Authy](https://www.authy.com/) gefunden. Die Umsetzung sieht super aus, und es wurde auch einges an Logik eingebaut um die Zeiten auf dem Gerät und Server synchron zu halten auch wenn man gerade mal keine internet Verbindung am Handy hat, was vorallem dann wichtig werden kann wenn man sich im Ausland aufhält.

Dank zahlreicher API-Wrapper ist das ganze super leicht in bereits bestehende Websites/Apps zu integrieren und der Kostenlose Service mit 1000 Auths pro Monat reicht für den Anfag locker aus.

Die Mobile-App ist auch ziemlich schick und unterstütz neben Authys eigenem Protokoll auch den Google Authenticator welcher von zahlreichen anderen Anbietern benutzt wird, was für den User den Vorteil hat, dass er nur eine App braucht. Ein großer Vorteil von Authy im vergleich zu Google Authenticator und anderen Anbietern die ich gefunden habe, ist die Möglichkeit seine Auth-Keys beim verlust des Telefons wieder herstellen zu können. Außerdem werden mehrere Geräte unterstütz (ich habe die App zb auf meinem iPad und iPhone installiert) und es gibt eine [App für OSX](https://www.authy.com/thefuture) die über Bluetooth die Tokens vom Gerät empfangen kann so wie eine [Chrome App](https://www.authy.com/products/authy-for-pc) für alle Plattformen.

In meinem Test war es mir möglich in nicht ganz 15 Minuten meine bestehenden Login Funktionen anzupassen und das Authy API zu integrieren.

Es gibt auch eine [tolle Website](http://twofactorauth.org/) die auflistet welche Anbieter bereits 2FA unterstützen, einige der (für mich) wichtigsten sind: [Facebook](https://www.facebook.com/settings?tab=security&section=code_generator&view), [GMail](https://support.google.com/accounts/answer/180744?hl=de), [Github](https://github.com/blog/1614-two-factor-authentication) und [Dropbox](https://www.dropbox.com/help/363/de)
