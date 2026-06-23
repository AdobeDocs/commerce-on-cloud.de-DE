---
title: Automatische Skalierung
description: Erfahren Sie, wie Adobe Commerce in der Cloud-Infrastruktur skaliert werden kann, um Ressourcenanforderungen zu erfüllen.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 605
ht-degree: 0%

---

# Automatische Skalierung

Die automatische Skalierung fügt der Cloud-Infrastruktur automatisch Ressourcen hinzu oder entfernt diese, um eine optimale Leistung und angemessene Kosten zu gewährleisten. Diese Funktion ist derzeit nur für Projekte verfügbar, die mit einer [skalierten Architektur“ konfiguriert ](scaled-architecture.md).

## Webserver-Knoten

Die [Web-Ebene](scaled-architecture.md#web-tier) kann skaliert werden, um einen Anstieg bei Prozessanfragen und höheren Traffic-Anforderungen zu berücksichtigen. Derzeit wird die Funktion zur automatischen Skalierung nur horizontal skaliert, indem Webserverknoten hinzugefügt oder entfernt werden.

Eine automatische Skalierung tritt auf, wenn die Nutzung und der Traffic von CPU einen vordefinierten Schwellenwert erreichen:

- **Knoten hinzugefügt** - CPUs/Kerne aller aktiven Web-Knoten haben eine Kapazität von 75 % für eine Minute, und der Traffic steigt in 5 aufeinander folgenden Minuten um 20 %.
- **Knoten entfernt** - CPUs/Kerne aller aktiven Web-Knoten werden 20 Minuten lang zu 60 % geladen. Knoten werden in der Reihenfolge entfernt, in der sie hinzugefügt wurden.

Die Mindest- und Höchstschwellen werden auf der Grundlage der vertraglich vereinbarten Ressourcengrenzen jedes Händlers festgelegt, wodurch das Risiko einer unendlichen Skalierung verringert wird.

## Überwachen von Schwellenwerten mit New Relic

Mit dem [New Relic-Service](../monitor/new-relic-service.md) können Sie bestimmte Schwellenwerte überwachen, z. B. die Host-Anzahl und die CPU-Nutzung. Die folgenden New Relic-Abfragen verwenden eine Variablennotation nur zu `cluster-id`.

>[!TIP]
>
>Eine Referenz zum Erstellen von Abfragen finden Sie unter [NRQL-Syntax, -Klauseln und -Funktionen](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) in der _New Relic_-Dokumentation.
>Verwenden Sie Ihre Abfragen, um ein [New Relic-Dashboard zu ](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Host-Anzahl

Die folgende New Relic-Beispielabfrage zeigt die Host-Anzahl innerhalb der Umgebung:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Im folgenden Screenshot bezieht sich **Angezeigte APM-Hosts** auf die Anzahl der Hosts mit Transaktionen, die während des ausgewählten Zeitraums protokolliert wurden.

![New Relic-Host-Anzahl](../../assets/new-relic/host-count.png)

### Verwendung von CPU

Die folgende New Relic-Beispielabfrage zeigt die Verwendung von CPU für Web-Knoten:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Nutzung von New Relic Web Nodes CPU](../../assets/new-relic/web-node-cpu-usage.png)

## Automatische Skalierung aktivieren

Um die automatische Skalierung für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt zu aktivieren oder zu deaktivieren [ Sie ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Wählen Sie die folgenden Gründe im Ticket aus:

- **Kontaktgrund**: Infrastrukturänderungsanfrage
- **Adobe Commerce-Infrastruktur-Kontaktgrund**: Andere Infrastrukturänderungsanfrage

>[!IMPORTANT]
>
>Die Funktion zur automatischen Skalierung erfasst unerwartete Ereignisse. Selbst wenn die automatische Skalierung aktiviert ist, empfiehlt Adobe, mit dem [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) fortzufahren, wenn Sie ein bevorstehendes Ereignis erwarten.

### Belastungstests

Adobe ermöglicht zunächst die automatische Skalierung in Ihrem Cloud-Projekt _Staging_-Cluster. Nachdem Sie Lasttests in Ihrer Umgebung durchgeführt und abgeschlossen haben, aktiviert Adobe die automatische Skalierung auf Ihrem Produktions-Cluster. Anleitungen für Belastungstests finden Sie unter [Leistungstests](../launch/checklist.md#performance-testing).

### IP-Zulassungsliste

Nach der Aktivierung der automatischen Skalierung stammt der ausgehende Webknoten-Traffic von den IP-Adressen der Service-Knoten. Wenn Sie eine Zulassungsliste mit einem Service eines Drittanbieters verwenden, der nicht mit Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt geliefert wurde, überprüfen Sie die IP-Adressen in der Service-Zulassungsliste des Drittanbieters.

Beispiel:

- Wenn die Zulassungsliste die IP-Adressen für Ihre Service-Knoten (1, 2 und 3) enthält, ist keine Aktion erforderlich.
- Enthält die Zulassungsliste die IP-Adressen für Ihre Dienstknoten (1, 2 und 3) und Webknoten (4, 5 und 6) - in diesem Fall alle sechs Knoten - dann ist keine Aktion erforderlich.
- Wenn die Zulassungsliste die IP-Adressen _nur_ für Ihre Web-Knoten (4, 5 und 6) enthält, müssen Sie die Datei aktualisieren, um die IP-Adressen für die Service-Knoten einzuschließen.

