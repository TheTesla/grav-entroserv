---
title: Cryptdomainmgr
media_order: cdmlogo.svg
process:
    markdown: true
    twig: false
visible: true
widget:
    areas:
        -
            location: default
            widgets:
                -
                    load: /widgets/_testwidget
                    enabled: true
    options:
        key: val
content:
    items: '@self.modular'
    order:
        by: ''
        dir: ''
---

![](cdmlogo.svg)

| Projektdaten |
| ------------- | --------------------------------------------------------------- |
| Quellcode | [https://github.com/TheTesla/cryptdomainmgr](https://github.com/TheTesla/cryptdomainmgr) |
| Paket        | [https://pypi.org/project/cryptdomainmgr/](https://pypi.org/project/cryptdomainmgr/) |
| Lizenz       | AGPLv3 |

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

Nun fehlt nur noch das eigentliche Programm:

```bash
python2 -m pip install cryptdomainmgr
```

## Nutzung

Die gesamte Konfiguration erfolgt über Dateien im ini-Format. Die Einträge können über mehrere Dateien verteilt werden. Bei Konflikten gilt immer der Eintrag der letzten Datei.

Für den Zugriff auf die API des Domainhosters müssen Zugangsdaten angegeben werden. Zumindest das Passwort sollte in einer extra Datei liegen. Bspw. die Datein _/etc/cryptdomainmgr/credentials.conf_:

```
[domain]
handler = dnsuptools/inwx
user = myUsename
passwd = myPassword1234
```

Diese Angaben gelten grundsätzlich für alle verwalteten Domains. Abweichende Daten auch für jede Domain einzeln hinterlegt werden.

Jetzt kann die Konfiguration für die erste Domain angelegt werden, bspw. die Datei _/etc/cryptdomainmgr/mydomains.conf_:

```
[domain:test1234.entroserv.de]
ip4 = auto
ip6 = auto
```

Die Minimalkonfiguration ist fertig. Sie hinterlegt die IPv4- und die IPv6-Adresse, über welche der Server extern erreichbar ist, automatisch im DNS. Die IP-Adressen werden durch Aufruf von [ip4.icanhazip.com](http://ip4.icanhazip.com) bzw. [ip6.icanhazip.com](http://ip6.icanhazip.com) ermittelt. Die Ermittlung der IP-Adressen und das Eintragen im DNS wird durch Aufruf von: 

```bash
python2 -m cryptdomainmgr --update /etc/cryptdomainmgr/*.conf 
```

einmal ausgeführt.

Alternativ kann der Inhalt der Konfiguration auch direkt als Argument übergeben werden:

```bash
python2 -m cryptdomainmgr --update /etc/cryptdomainmgr/inwxcred.conf --config-content $'[domain:test1234.entroserv.de] \n handler=dnsuptools/inwx \n ip4=auto'
```

Wichtig ist das ``$``-Zeichen, damit die ``\n`` als Zeilenumbruch interpretiert werden. In diesem Beispiel liegen die Zugangsdaten für _inwx_ in ``/etc/cryptdomainmgr/inwxcred.conf``.

Die Log-Ausgabe des Programms, sollte, wenn alles richtig funktioniert, ungefähr so aussehen:

```
[2019-02-03 16:07:22,496]    INFO      Interpreting config sections
[2019-02-03 16:07:22,496]    INFO        - cdm
[2019-02-03 16:07:22,497]    INFO        - domain
[2019-02-03 16:07:22,634]    INFO      Running phase: update
[2019-02-03 16:07:22,634]    INFO      Create resource records for section "test1234.entroserv.de"
[2019-02-03 16:07:24,144]    INFO      add (exists) A for test1234.entroserv.de : 92.60.36.246
```

Es wird sogar angezeigt, dass der Eintrag in identischer From schon vorhanden ist. Beim erstmaligen Ausführen steht dort: ``add (new) ...``.

Sollte bereits ein alter, nicht mehr gültiger, Eintrag vorhanden sein, welcher nun geändert werden soll, wird auch der Löschvorgang protokolliert:

```
[2019-02-03 16:24:04,441]    INFO      Interpreting config sections
[2019-02-03 16:24:04,442]    INFO        - cdm
[2019-02-03 16:24:04,442]    INFO        - domain
[2019-02-03 16:24:04,598]    INFO      Running phase: update
[2019-02-03 16:24:04,599]    INFO      Create resource records for section "test1234.entroserv.de"
[2019-02-03 16:24:07,241]    INFO      add (new) A for test1234.entroserv.de : 92.60.36.246
[2019-02-03 16:24:08,273]    INFO      delete A for test1234.entroserv.de : 1.2.3.4
```
Mehrere Einträge können mit Komma getrennt angegeben werden:

```bash
python2 -m cryptdomainmgr --update /etc/cryptdomainmgr/inwxcred.conf --config-content $'[domain:test1234.entroserv.de] \n handler=dnsuptools/inwx \n ip4=1.2.3.4,5.6.7.8,auto'
```

Die Einträge werden einzeln angelegt. Als letztes werden wieder alte Einträge entfernt:

```
[2019-02-03 16:35:18,958]    INFO      Interpreting config sections
[2019-02-03 16:35:18,958]    INFO        - cdm
[2019-02-03 16:35:18,959]    INFO        - domain
[2019-02-03 16:35:19,108]    INFO      Running phase: update
[2019-02-03 16:35:19,108]    INFO      Create resource records for section "test1234.entroserv.de"
[2019-02-03 16:35:20,947]    INFO      add (exists) A for test1234.entroserv.de : 1.2.3.4
[2019-02-03 16:35:21,774]    INFO      add (new) A for test1234.entroserv.de : 5.6.7.8
[2019-02-03 16:35:22,878]    INFO      add (new) A for test1234.entroserv.de : 92.60.36.246
[2019-02-03 16:35:24,237]    INFO      delete A for test1234.entroserv.de : 6.7.8.9
```


Es ist auch möglich einen Eintrag einfach nur zu löschen bzw. sicherzustellen, dass für diese Domain kein A-Record hinterlegt ist:

```bash
python2 -m cryptdomainmgr --update /etc/cryptdomainmgr/inwxcred.conf --config-content $'[domain:test1234.entroserv.de] \n handler=dnsuptools/inwx \n ip4=none'
```