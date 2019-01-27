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

Dieses Programm erneuert kryptographisches Schlüsselmaterial für Web- und E-Mail-Dienste automatisch. Es verbindet die CA, welche TLS-Zertifikate austellt, den eigentlich Web- oder E-Mail-Server, welcher die Zertifikate und DKIM-Schlüssel nutzt und das DNS, welches den öffentlichten DKIM-Schlüssel zur Signaturprüfung sowie den TLSA-Hashwert zur Prüfung des Zertifikates bereithält. 

Mehr dazu [hier](cryptdomainmgr)