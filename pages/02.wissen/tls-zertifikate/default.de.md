---
title: TLS-Zertifikate
visible: true
---

# TLS-Zertifikate (digitale Zertifikate)

Wir hatten bereits das asymmetrische Kryptosystem behandelt. Wir können damit zwar Nachrichten für einen bestimmten Empfänger verschlüsselt versenden, dieser kann auch etwas digital unterschreiben, aber wir kennen noch gar nicht die Identität dieser Person. Jeder kann beliebig Schlüsselpaare erzeugen und verteilen. Jeder potentielle Absender sollte sich den öffentlischen Schlüssel persönlich überreichen lassen. Dadurch kann er die reale Person zuordnen. Das ist aber sehr umständlich, denn das muss mit jedem Kommunikationspartner durchgeführt werden. Wir brauchen daher noch einen Identitätsnachweis, so eine Art Personalausweis.

Diesen Ausweis stellt eine Zertifizierungsstellen aus, die sog. _Certificate Authority (CA)_. Möchte ich diesen Ausweis haben, stelle ich eine Anfrage, den sog. _Certificate Signing Request (CSR)_, an diese Einrichtung. Diese Anfrage ist ein Formular, auf dem die Identität erfasst wird. Es kann bspw. die Firmenanschrift zur Identifikation eines Unternehmens oder die Wohnanschrift zur Identifikation einer Person enthalten. Auch eine E-Mail-Adresse oder Web-Adresse kann angegeben werden. Um das Zertifikat sinnvoll verwenden zu können, muss auch die eigene Unterschrift erfasst werden. Bezogen auf das Beispiel des [Asymmetrischen Kryptosystems](asymmetrisches-kryptosystem) ist das die Angabe der Farbe gelb für unseren öffentlichen Schlüssel.
