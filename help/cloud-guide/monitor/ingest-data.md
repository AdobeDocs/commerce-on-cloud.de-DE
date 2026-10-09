---
title: Datenaufnahme
description: Erfahren Sie, wie Sie die Datenaufnahme in Commerce in New Relic anzeigen und verwalten.
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: 'https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 4239b8a6-e74f-567d-a7a5-b98b9ead0ea4
    internal-label: Observability
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
    internal-label: Data management
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%
---
# Datenaufnahme

New Relic ist auf umfangreiche Daten angewiesen, um eine effektive Überwachung und Analyse zu ermöglichen. Große Datensätze können jedoch die rechtzeitige Bereitstellung von Ergebnissen, Leistung und Compliance beeinträchtigen. Dieses Thema enthält einige Anleitungen zum Verwalten der Datenaufnahme und Strategien zur Verfeinerung Ihrer Daten, sodass sie am effektivsten sind.

New Relic bietet eine _Daten-Management_-Ansicht, die Ihre Plannutzung nach Datenquelle zusammenfasst.

**So zeigen Sie Ihre aufgenommenen Daten und Quellen an**:

1. Klicken Sie im New Relic-Benutzermenü auf **[!UICONTROL Manage your data]**.
1. Klicken Sie in der Liste _Administration_ auf **[!UICONTROL Data management]**.

   ![Daten-Management](../../assets/new-relic/data-ingestion.png)

   Auf der Registerkarte **[!UICONTROL Data ingestion]** werden die Daten angezeigt, die für den jeweiligen Tag und die Datenquelle aufgenommen wurden.
   Die Registerkarte Datenaufbewahrung zeigt an und steuert, wie lange Daten gespeichert werden.

1. Wählen Sie die Registerkarte **[!UICONTROL Limits]** aus und sehen Sie sich die Beschränkungen für Ihr Konto an.

Zu den Datenquellen für Adobe Commerce gehören:

- **APM-Ereignisse** - Ereignisdaten, die in Diagrammen und Dashboards verwendet werden
- **Infrastruktur** - Prozess- und Host-Metriken wie CPU, Speicher, Netzwerke
- **Logging** - Protokolle für CDN, APM und Anwendungs-Server

Protokolldaten tragen zu einem großen Teil zur Aufnahme bei. Erfahren Sie[&#x200B; wie Sie Protokolldaten anzeigen und analysieren &#x200B;](log-management.md#view-and-analyze-log-data) mit Ihrem Adobe-Support-Mitarbeiter eine Strategie für die Datenaufnahme und -speicherung erarbeiten. Weitere Informationen zum [Verwalten der Datenaufnahme](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) finden Sie in der _Dokumentation zu New Relic_.
