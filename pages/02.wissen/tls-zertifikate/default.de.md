---
title: TLS-Zertifikate
visible: true
---

# TLS-Zertifikate (digitale Zertifikate)

Wir hatten bereits das asymmetrische Kryptosystem behandelt. Wir können damit zwar Nachrichten für einen bestimmten Empfänger verschlüsselt versenden, dieser kann auch etwas digital unterschreiben, aber wir kennen noch gar nicht die Identität dieser Person. Jeder kann beliebig Schlüsselpaare erzeugen und verteilen. Jeder potentielle Absender sollte sich den öffentlischen Schlüssel persönlich überreichen lassen. Dadurch kann er die reale Person zuordnen. Das ist aber sehr umständlich, denn das muss mit jedem Kommunikationspartner durchgeführt werden. Wir brauchen daher noch einen Identitätsnachweis, so eine Art Personalausweis.

Diesen Ausweis stellt eine Zertifizierungsstellen aus, die sog. _Certificate Authority (CA)_. Möchte ich diesen Ausweis haben, stelle ich eine Anfrage bzw. Antrag, den sog. _Certificate Signing Request (CSR)_, an diese Einrichtung. Diese Anfrage ist ein Formular, auf dem die Identität erfasst wird. Es kann bspw. die Firmenanschrift zur Identifikation eines Unternehmens oder die Wohnanschrift zur Identifikation einer Person enthalten. Auch eine E-Mail-Adresse oder Web-Adresse kann angegeben werden. Um das Zertifikat sinnvoll verwenden zu können, muss auch die eigene Unterschrift erfasst werden. Bezogen auf das Beispiel des [Asymmetrischen Kryptosystems](../asymmetrisches-kryptosystem) ist das die Angabe der Farbe gelb für unseren öffentlichen Schlüssel.

Die Zertifizierungsstelle überprüft nun die Angaben. Sie schickt einen Brief mit einem Passwort an die Postanschrift. Ein anderes Passwort wird per E-Mail zugesandt. Um die Web-Adresse zu prüfen, muss der Antragsteller dort eine TAN veröffentlichen, die er selbst im Antrag angegeben hat oder von der Zertfifizierungsstelle mitgeteilt bekam. Diese TAN prüft die Zertifizierungsstelle. Nun weiß sie, ob alle Angaben korrekt sind. Sind diese korrekt, stellt die Zertifierungsstelle ein Zertifkat mit den gleichen Angaben wie auf der Anfrage aus und unterschreibt dieses mit dem privaten Schlüssel, den nur die Zertifizierungsstelle kennt.


