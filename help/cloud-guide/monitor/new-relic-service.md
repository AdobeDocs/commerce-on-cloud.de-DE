---
title: New Relic-Service
description: Erfahren Sie mehr über den New Relic-Service, der mit Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt verfügbar ist.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Übersicht über den New Relic-Service

Alle Adobe Commerce-Projekte für Cloud-Infrastrukturen umfassen den Zugriff auf den New Relic-Service, der die Leistungsüberwachung und die Untersuchung von Ereignissen der [!DNL Commerce]-Anwendung und Cloud-Infrastruktur unterstützt.

Die folgenden New Relic-Funktionen sind für die Verwendung mit Produktions- und Staging-Umgebungen verfügbar:

- [New Relic APM](#new-relic-apm) (Pro und Starter)
- [New Relic Infrastructure](#new-relic-infrastructure) (nur Pro)
- [New Relic-Protokollverwaltung](#new-relic-log-management) (nur Pro)

>[!INFO]
>
>Andere New Relic-Funktionen sind in Adobe Commerce-Projekten nicht verfügbar.

## NEW RELIC APM

[New Relic for Application Performance Management (APM)](https://docs.newrelic.com/introduction-apm/) ist ein Softwareanalyseprodukt, mit dem Sie Anwendungsinteraktionen analysieren und verbessern können. New Relic APM ist für alle Adobe Commerce in Cloud-Infrastrukturprojekten verfügbar und bietet die folgenden Funktionen:

- **Konzentration auf bestimmte Transaktionen** - Markieren und überwachen Sie aktiv wichtige Kundenaktionen auf Ihrer Site, z. B. das Hinzufügen zum Warenkorb, das Auschecken oder die Verarbeitung einer Zahlung.
- **Überwachung von Datenbankabfragen** - Suchen und Überwachen von Datenbankabfragen, die die Leistung beeinträchtigen.
- **App Map** - Alle Anwendungsabhängigkeiten auf Ihrer Site, Erweiterungen und externen Services anzeigen.
- **[!DNL Apdex]Scores** - Evaluieren Sie die Leistung und erstellen Sie Warnhinweise, die Probleme identifizieren und Sie benachrichtigen, wenn sie auftreten, z. B. die Performance der Site, die durch einen Flash-Verkauf oder ein Web-Ereignis beeinträchtigt wird. Siehe [Apdex-Score](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Verwaltete Warnhinweise für Adobe Commerce**-Verwenden Sie diese Warnhinweisrichtlinie für New Relic, um die Anwendungs- und Infrastrukturleistung auf der Grundlage von Best Practices der Branche zu überwachen. Siehe [Überwachen der Leistung mit der Warnmeldungsrichtlinie „Verwaltete Warnhinweise für Adobe Commerce&quot;](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Bereitstellungen verfolgen** - Überwachen Sie Bereitstellungsereignisse und analysieren Sie die Auswirkungen der Bereitstellung auf die Gesamtleistung. Siehe [Verfolgen von Bereitstellungen](track-deployments.md).

Ihr Adobe Commerce on Cloud Infrastructure-Projekt enthält die Software für den New Relic APM-Service zusammen mit einem Lizenzschlüssel. Sie müssen keine zusätzliche Software erwerben oder installieren.

## New Relic-Infrastruktur

Zu den Pro-Projekten gehört der Service [New Relic Infrastructure (NRI](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/), der sich automatisch mit den Anwendungsdaten und Leistungsanalysen verbindet, um eine dynamische Serverüberwachung zu ermöglichen. Dieser Service ist für Pro-Produktions- und Staging-Umgebungen verfügbar.

## New Relic-Protokollverwaltung

Alle Cloud-Infrastrukturprojekte umfassen [New Relic-Protokollverwaltung](log-management.md). Der Service ist so vorkonfiguriert, dass er alle Protokolldaten aus Ihren Staging- und Produktionsumgebungen aggregiert und in einem zentralen Dashboard für die Protokollverwaltung anzeigt.
