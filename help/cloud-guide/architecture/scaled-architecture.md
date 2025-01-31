---
title: Skalierte Architektur
description: Erfahren Sie mehr über die Split-Tier-Architektur und deren Skalierung zur Erfüllung der Anforderungen.
feature: Cloud, Auto Scaling, Iaas, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Skalierte Architektur

Die Cloud-Infrastruktur kann entsprechend Ihren Ressourcenanforderungen skaliert werden, um eine höhere Effizienz zu erzielen. Die Adobe Commerce on Cloud-Infrastruktur überwacht Ihre Programme und kann die Kapazität anpassen, um eine kontinuierliche, vorhersehbare Leistung aufrechtzuerhalten. Die Konvertierung in diese Architektur trägt dazu bei, Probleme wie Latenz oder große Traffic-Spitzen zu minimieren.

>[!NOTE]
>
>Die skalierte Architektur ist für Adobe Commerce auf Cloud-Infrastrukturkonten mit dem Pro 48-Cluster oder höher verfügbar.

## Architektur mit mehreren Ebenen

Früher bestand die Pro-Architektur aus drei Knoten, von denen jeder einen vollständigen Technologie-Stack enthielt. Jetzt gibt es eine skalierbare Infrastruktur, die eine mehrstufige Architektur mit mindestens sechs Knoten bietet: drei Knoten für die zentrale Datenbank und Services und drei Knoten für den Webserver. Diese Split-Tier-Architektur bietet die Möglichkeit, Ebenen unabhängig zu skalieren, um ein optimales Leistungsgleichgewicht zu erzielen.

### Service-Ebene

Es gibt drei Service-Knoten für Datenspeicherung, Cache und Services: **OpenSearch** oder **Elasticsearch**, **MariaDB**, **Redis** und mehr. Wenn sich die Servicestufe der Kapazität nähert, besteht die einzige Möglichkeit zur Skalierung darin, die Servergröße zu erhöhen, z. B. die Steigerung der Leistung und des Speichers von CPU. Die Kapazität ist auf die Größe des verfügbaren Knotens beschränkt. Da der Datenbank-Cluster für hohe Verfügbarkeit ausgelegt ist, können Sie mit den verwendeten Technologien nicht horizontal zuverlässig skalieren.

![Skalierung der Service-Ebene](../../assets/scaling-service.png)

Betrachten wir ein Beispiel, bei dem der Service-Knoten-Instanztyp _m5.2xlarge_ mit 32 GB RAM ist. Ein Dienst wie die Datenbank beansprucht eine beträchtliche Menge an Arbeitsspeicher (30 GB). Durch die Skalierung auf die nächste verfügbare Instanzgröße _m5.4xlarge_ steht 64 GB RAM zur Verfügung, wodurch der Arbeitsspeicher verdoppelt und die wachsenden Anforderungen der Datenbank erfüllt werden können.

Sie können die Leistung der Service-Ebene weiter optimieren, indem Sie Traffic basierend auf dem Knotentyp weiterleiten. Standardmäßig ist der Datenbankknoten vom Web-Traffic getrennt. Beispielsweise können Sie Webtraffic auf dem Datenbankknoten bereitstellen.

### Web-Stufe

Es gibt drei Web-Knoten für die Verarbeitung von Anfragen und Webtraffic: **php-fpm** und **NGINX**. Zusätzlich zur vertikalen Skalierung durch Erhöhung der Leistung und des Speichers kann die Web-Ebene horizontal skaliert werden, indem Webserver zu einem vorhandenen Cluster hinzugefügt werden, wenn sie auf PHP-Ebene eingeschränkt werden. Unter [Automatische Skalierung](autoscaling.md) erfahren Sie, wie die Web-Knoten automatisch skaliert werden.

![Web-Stufen-Skalierung](../../assets/scaling-web.png)

Dies ergänzt die vertikale Skalierung, die von der Service-Ebene bereitgestellt wird. Während die Service-Ebene in Größe und Leistung skaliert wird, um einer wachsenden Datenbank- und Service-Nutzung Rechnung zu tragen, skaliert die Web-Ebene in Größe, Leistung und Instanzen, um einer Zunahme von Prozessanfragen und höheren Traffic-Anforderungen gerecht zu werden.

Betrachten wir ein Beispiel, bei dem der Web-Knoten-Instanztyp _C5.2xlarge mit acht CPUs und 16 GB RAM_ ist. Die Anzahl der Anfragen an die Website ist stark gestiegen. Sie können einen C5.2xlarge-Knoten hinzufügen, um den Anstieg der php-fpm-Prozesse zu bewältigen, oder Sie können jeden Instanztyp in _C5.4xlarge mit 16 CPU und 32 GB RAM_ ändern. Durch Hinzufügen eines Knotens wird das Risiko unzureichender Spitzenkapazitäten reduziert.

## Projektstruktur

Pro-Projekte mit der skalierten Architektur verfügen über mindestens sechs Knoten.

- 3 Web-Knoten c5.2xlarge (8 CPU, 16 GB RAM)

- 3 Serviceknoten m5.2xlarge (8 CPU, 32 GB RAM)

Jedes Projekt ist jedoch einzigartig und erfordert eine Leistungsüberwachung, um das Ressourcenmanagement ordnungsgemäß zu analysieren. Jedes Konto enthält den [New Relic-Service](../monitor/new-relic-service.md) der sich automatisch mit den Anwendungsdaten und Leistungsanalysen verbindet, um eine dynamische Serverüberwachung zu ermöglichen. Insbesondere können Sie den New Relic-Service verwenden, um die CPU- und RAM-Auslastung zu überwachen und festzustellen, welche Knoten zusätzliche Ressourcen benötigen. Wenn eine Ressource ihre Kapazität erreicht oder Sie eine Leistungsminderung feststellen, die auf der Analyse basiert, können Sie eine Anfrage zur Skalierung Ihrer Infrastruktur erstellen, um den Bedarf zu decken.

### SSH-Zugriff

Bestimmte Dateien und Protokolle, z. B. das `/app/<project-id>/var/log`, werden nicht zwischen Knoten freigegeben. Jeder Knoten verfügt über einen eindeutigen SSH-Zugriff. Sie können die `magento-cloud` CLI nicht verwenden, um sich beim Service oder bei den Web-Knoten anzumelden. Die Knotenadressen finden Sie jedoch in der SSH-Zugriffsliste in Ihrem [!DNL Cloud Console].

```bash
ssh <node>.<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
```

- `node` 1 bis 3: Adressen für den Zugriff auf die Service-Knoten

- `node` 4 bis _n_ - Adressen für den Zugriff auf die Web-Knoten

>[!TIP]
>
>Nach der Anmeldung können Sie die Server-ID und die Rolle bestätigen: Service-Knoten verwenden die _einheitliche_ Rolle und Web-Knoten verwenden die _Web_-Rolle.

Die Beispielantwort bei der Anmeldung bei einem **Service-Knoten** enthält die _einheitliche_ Rolle:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:unified.

project-id@server-id:~$
```

Die Beispielantwort bei der Anmeldung bei einem **Web-Knoten** enthält die Rolle _Web_:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:web.

project-id@server-id:~$
```

### Speicherorte protokollieren

Die Protokollspeicherorte variieren je nach Knoten geringfügig. Ein Datenbankprotokoll, z. B. das **MySQL-Fehlerprotokoll**, ist beispielsweise auf einem Dienstknoten (`/var/log/mysql/mysql-error.log`) verfügbar, nicht jedoch auf einem Webknoten.

Jedes Pro-Konto enthält den [New Relic Logs-Service](../monitor/new-relic-service.md), der sich automatisch mit Protokolldaten aus der Anwendung verbindet, um eine dynamische Protokollverwaltung zu ermöglichen. In der Anwendung &quot;New Relic-Protokolle“ werden aggregierte Protokolldaten aus allen Knoten angezeigt, sodass Sie Leistungsprobleme auf bestimmten Knoten über ein einziges Dashboard beheben können.
