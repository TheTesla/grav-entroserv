---
title: Ansible-tine
routable: true
visible: true
routes:
    aliases:
        - /de/offene-software/ansible-tine
        - test
---

# Ansible-tine

| Projektdaten |
| --------- | ------- |
| Quellcode | https://github.com/TheTesla/ansible-tine |
| Vortrag | [Ansible-Projekt: Mailserver, Mailingliste, CMS. Groupware](https://media.ccc.de/v/eh19-130-ansible-projekt-fr-mailserver-mailingliste-cms-und-groupware-mit-nutzerverwaltung) | 

Wer kennt das nicht? Man möchte einen Webauftritt bereitstellen, welcher neben einer Webseite mit Content-Management-System auch noch ein E-Mail-Postfach inklusive Spamfilter bietet. Eine Mailingliste ist auch noch wünschenswert. Die E-Mail-Postfächer sollten per Webinterface erreichbar sein und ein E-Mail-Programm sollte alle Einstellungen selbst herausfinden.

Dieses Ansible-Projekt erlaubt das parametrische Deployment des beschriebenen Systems auf eine oder mehreren Domains. DNS-Einträge inkl. CAA, Letsencrypt-Zertifikate, TLSA und DKIM werden über den Cryptdomainmgr automatisch gehandhabt. Die Zertifikats-Erneuerung nutzt DNS-01-Authentication. Es wird ein SSL-Labs-Score von 100 in allen vier Kategorien erreicht. SPF, ADSP, und DMARC werden unterstützt. Thunderbird-Autoconfig und Microsoft-ActiveSync-Autodiscover für Android-Smartphones funktionieren problemlos. Neben Kontakt- und Kalendersynchronisation über ActiveSync werden CardDav und CalDav unterstützt. Neue Nutzer und Gruppen können über tine20 angelegt werden. Sie werden in einer mysql-Datenbank gespeichert. Jeder Benutzer bekommt ein E-Mail-Postfach sowie eine OpenID. Der Spamfilter rspamd bietet neben einer Weboberfläche, DKIM-Signierung und -Prüfung auch Lernen von Spam und Ham bei der Markierung der Mails im Thunderbird.

Mailinglisten über mailman und das Flatfile-CMS Grav runden das Projekt ab.

Teste die Software auf einen V-Server von netcup:

[5 € Netcup-Gutschein](https://www.netcup.de/bestellen/gutschein_einloesen.php?gutschein=36nc15480723169)
