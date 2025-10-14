---
title: Automatische Skalierung
description: Erfahren Sie, wie Adobe Commerce in der Cloud-Infrastruktur skaliert werden kann, um Ressourcenanforderungen zu erfüllen.
feature: Cloud, Auto Scaling
topic: Architecture
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Automatische Skalierung

Die automatische Skalierung fügt der Cloud-Infrastruktur automatisch Ressourcen hinzu oder entfernt diese, um eine optimale Leistung und angemessene Kosten zu gewährleisten. Diese Funktion ist derzeit nur für Projekte verfügbar, die mit einer [skalierten Architektur“ konfiguriert &#x200B;](scaled-architecture.md).

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
>Mit Ihren Abfragen können Sie ein [New Relic-Dashboard erstellen](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

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

Um die automatische Skalierung für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt zu aktivieren oder zu deaktivieren [&#x200B; Sie ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket). Wählen Sie die folgenden Gründe im Ticket aus:

- **Kontaktgrund**: Infrastrukturänderungsanfrage
- **Adobe Commerce-Infrastruktur-Kontaktgrund**: Andere Infrastrukturänderungsanfrage

>[!IMPORTANT]
>
>Die Funktion zur automatischen Skalierung erfasst unerwartete Ereignisse. Selbst wenn die automatische Skalierung aktiviert ist, empfiehlt Adobe, mit dem [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) fortzufahren, falls Sie ein bevorstehendes Ereignis erwarten.

### Belastungstests

Adobe ermöglicht zunächst die automatische Skalierung in Ihrem Cloud-Projekt _Staging_-Cluster. Nachdem Sie Belastungstests in Ihrer Umgebung durchgeführt und abgeschlossen haben, ermöglicht Adobe dann die automatische Skalierung auf Ihrem Produktions-Cluster. Anleitungen für Belastungstests finden Sie unter [Leistungstests](../launch/checklist.md#performance-testing).

### IP-Zulassungsliste

Nach der Aktivierung der automatischen Skalierung stammt der ausgehende Webknoten-Traffic von den IP-Adressen der Service-Knoten. Wenn Sie eine Zulassungsliste mit einem Service eines Drittanbieters verwenden, der nicht mit Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt geliefert wurde, überprüfen Sie die IP-Adressen in der Service-Zulassungsliste des Drittanbieters.

Beispiel:

- Wenn die Zulassungsliste die IP-Adressen für Ihre Service-Knoten (1, 2 und 3) enthält, ist keine Aktion erforderlich.
- Enthält die Zulassungsliste die IP-Adressen für Ihre Dienstknoten (1, 2 und 3) und Webknoten (4, 5 und 6) - in diesem Fall alle sechs Knoten - dann ist keine Aktion erforderlich.
- Wenn die Zulassungsliste auf die Zulassungsliste setzte die IP-Adressen _nur_ für Ihre Web-Knoten (4, 5 und 6) enthält, müssen Sie die Datei aktualisieren, um die IP-Adressen für die Service-Knoten einzuschließen.
