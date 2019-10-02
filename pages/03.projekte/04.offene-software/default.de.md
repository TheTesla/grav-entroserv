---
title: 'Open Source Software'
content:
    items: '@self.children'
    leading: '0'
    columns: '2'
    limit: '5'
    order:
        by: date
        dir: desc
    show_date: '0'
    pagination: '1'
    url_taxonomy_filters: '1'
---

# Open Source Software

Es gibt ein sehr umfangreiches Angebot an freier Software. Oft existieren mehrere Lösungen für ein Problem. Bspw. existieren die Groupware/CRM-Ösungen tine20, Sogo, vTiger, Kopano, Groupoffice etc. Doch egal welche der Programm man verwenden möchte, erfordert die Konfiguration umfangreiches Fachwissen. Das Kernproblem liegt in der Verknüpfung mit den externen Komponenten, z.~B. einem Mailserver.

Hier werden deshalb Projekte präsentiert, die dieses Schnittstellenproblem lösen sollen. 

## Cryptdomainmgr

Dieses Programm erneuert kryptographisches Schlüsselmaterial für Web- und E-Mail-Dienste automatisch. Es verbindet die CA, welche TLS-Zertifikate austellt, den eigentlichen Web- oder E-Mail-Server, welcher die Zertifikate und DKIM-Schlüssel nutzt und das DNS, welches den öffentlichten DKIM-Schlüssel zur Signaturprüfung sowie den TLSA-Hashwert zur Prüfung des Zertifikates bereithält. 

Mehr dazu [hier](cryptdomainmgr)

## Ansible-tine

Eine fertige Groupware-Lösung, bei der auf Anhieb alles sofort funktioniert, gibt es sicherlich als Software-as-a-Service-Angebot. Allerdings handelt es sich dabei oft um Komplettpakete, die kaum erweiterbar sind. Auch freien Komplettpaketen fehlt oft eine sinnvolle Grundkonfiguration. Bis ein vollständiges System steht entstehen oft hohe Kosten. Dieses Projekt soll daher eine sinnvolle Grundkonfiguration der Komponenten Groupware (tine20), Mailserver (postfix, dovecot), Mailingliste (mailman), Spamfilter (rspamd), CMS (grav) und Domainverwaltung (Cryptdomainmgr) bereitstellen. Zusätzlich stellt das Paket Autoconfig von Thunderbird und Autodiscover für Android-Geräte bereit.

Mehr dazu [hier](ansible-tine)