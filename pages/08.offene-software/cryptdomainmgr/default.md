---
title: Cryptdomainmgr
media_order: cdmlogo.svg
visible: true
---

![](cdmlogo.svg)

_Cryptdomainmgr_ ist ein Python-Programm, welches TLS-Zertifikate, TLSA-Domaineinträge und DKIM-Schlüssel automatisch erneuert.

Das Verfahren "DNS-Based Authentication of Named Entities" (DANE) koppelt TLS-Zertifikate über Hashwerte (TLSA-Einträge) an die DNS-Zone. Im Falle einer kompromittierten Zertifizierungsstelle (CA) kann über den DNS-Eintrag die Richtigkeit des Zertifikates nachgewiesen werden. An unberechtigte Dritte ausgestellte Zertifikate werden entlarvt. Die DNS-Zone darf jedoch nicht kompromittiert sein und sollte per DNSSEC abgesichert werden.

Um den Versand betrügerischer E-Mails zu erschweren, signieren Mailserver E-Mails mit mit einem DKIM-Schlüssel. Die Signatur kann über den DKIM-Eintrag in der Absenderdomain validiert werden. Das Fälschen der Absenderdomain wird dadurch entdeckt.

Die asymmetrischen Schlüsselpaare für die DKIM-Signatur und TLS-Zertifikate müssen regelmäßig erneuert werden, um den Angriffszeitraum bei gebrochenen Schlüsseln gering zu halten.

_Cryptdomainmgr_ erneuert die Schlüssel sowie DH-Parameter automatsich lückenlos ohne Downtime in drei Phasen: Prepare, Rollover, Cleanup. Auch negative Caching stellt dadurch kein Problem dar.

## Aufbau

Der _Cryptdomainmgr_ wird auf dem System installiert, auf welchem der Web- oder Mailserver-Dienst läuft. Softwaremodule kommunizieren mit den externen Komponenten. Momentan können TLS-Zertifikate nur von _Let's Encrypt_ bezogen werden. Der DNS-Anbieter _Internetworx_ stellt eine API bereit, über welche die DNS-Einträge aktualisiert werden können. Zum Erzeugen der DKIM-Schüssel und zum Signieren wird _rspamd_ verwendet. Die Zertifikate werden den Mailserver-Diensten _postfix_ und _dovecot_ sowie dem Webserver _apache2_ zur Verfügung gestellt.

Um weitere DNS-Server, Zertifikatsanbieter und Dienste zu unterstützen kann _Crypdomainmgr_, ohne Änderungen am Hauptprogramm vornehmen zu müssen, mit entsprechenden Softwaremodulen erweitert werden. 

![](cdmStruktur.svg)

## Installation

Auf _Debian 9_ oder _Ubuntu 18.04_ sind mehrere Installationsschritte erforderlich. Die Software wird über _pypi_ bereitgestellt. Da der zugehörige Paketmanager _pip_ keine Abhängigkeiten über _apt_ nachinstalliert, müssen diese zuerst manuell installiert werden:

```bash
apt install -y libcurl4-openssl-dev libssl-dev
```

Um Zertifikate bei _Let's Encrypt_ abzuholen wird _curl_ benötigt:

```bash
apt install -y curl
```

Für die Erzeugung der DKIM-Schlüssel und zur DKIM-Signierung muss noch _rspamd_ installiert werden:

```bash
apt install -y lsb-release wget # optional
CODENAME=`lsb_release -c -s`
wget -O- https://rspamd.com/apt-stable/gpg.key | apt-key add -
echo "deb [arch=amd64] http://rspamd.com/apt-stable/ $CODENAME main" > /etc/apt/sources.list.d/rspamd.list
echo "deb-src [arch=amd64] http://rspamd.com/apt-stable/ $CODENAME main" >> /etc/apt/sources.list.d/rspamd.list
apt update
apt install -y rspamd
```
