---
title: New Relic-Protokollverwaltung
description: Erfahren Sie, wie Sie das New Relic-Protokoll verwenden
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# New Relic-Protokollverwaltung

Alle Cloud-Infrastrukturprojekte umfassen [New Relic-Protokollverwaltung](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). Der Service ist so vorkonfiguriert, dass er alle Protokolldaten aus Ihren Staging- und Produktionsumgebungen aggregiert und in einem zentralen Dashboard für die Protokollverwaltung anzeigt.

Die aggregierten Daten enthalten Informationen aus den folgenden Protokollen:

- Alle `ece-tools`- und Anwendungsprotokolle aus dem `~/var/log`
- Protokolle für Cloud-Services aus dem `var/log/platform/<project-ID>`
- Fastly CDN und WAF

Wenn Ihr Projekt mit New Relic verbunden ist, können Sie den New Relic-Protokolldienst verwenden, um Aufgaben wie die folgenden auszuführen:

- Verwenden von New Relic-Abfragen zum Suchen aggregierter Protokolldaten
- Visualisieren von Protokolldaten über die New Relic-Protokollanwendung
- Erstellen benutzerdefinierter Diagramme, Dashboards und Warnhinweise
- Fehlerbehebung bei Leistungsproblemen über ein einziges Dashboard

## Anzeigen und Analysieren von Protokolldaten

Verwenden Sie die Anwendung &quot;New Relic Logs“, um die aggregierten Protokolldaten zu durchsuchen und Fehler in Anwendungen, Infrastrukturen, CDN und WAF zu beheben. Sie können Diagramme, Dashboards und Warnhinweise mithilfe von Protokolldaten erstellen, die von New Relic APM- und Infrastructure-Services erfasst wurden.

**So verwenden Sie die New Relic-Protokollanwendung**:

1. Melden Sie sich bei Ihrem [New Relic-Konto an](https://login.newrelic.com/login).

1. Wählen **Protokolle** aus dem Navigationsmenü des Explorers aus.

1. Vergewissern Sie sich, dass Ihr Konto oben in der Ansicht _Alle Protokolle_ ausgewählt ist.

1. Wählen Sie einen Zeitbereich für die Protokollabfrage aus.

1. Um die Protokolldaten der Infrastruktur für Cloud-Services (Protokolle aus `~/var/log/`) zu überprüfen, geben Sie die Abfragezeichenfolge `has: "filePath"` in das Feld _Nach Protokollen suchen_ ein. Klicken Sie dann auf **[!UICONTROL Query logs]**.

   Die Namen der Protokolldateien werden in der Spalte `filePath` gespeichert, wobei vollständige Pfade zur Protokolldatei angegeben werden.

   ![Cloud-Projekt New Relic Service-Protokolldaten](../../assets/new-relic/var-log-query.png)

1. Um Fastly-Protokolldaten zu überprüfen, geben Sie die Abfragezeichenfolge `has: "client_ip"` in das Feld _Nach Protokollen suchen_ ein. Klicken Sie dann auf **[!UICONTROL Query logs]**.

1. Um die Fastly-Protokollergebnisse nach Länder-Code zu filtern, klicken Sie auf **[!UICONTROL Add column]** und wählen Sie dann **[!UICONTROL geo_country_code]** aus.

   ![Cloud-Projekt New Relic CDN-Protokollattributfilter](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>Sie können die Abfrageansicht im Dropdown-Menü _Gespeicherte Ansichten_ speichern. Klicken Sie auf **[!UICONTROL Create new]**, geben Sie einen Namen ein, wählen Sie Optionen aus und klicken Sie auf **[!UICONTROL Save view]**.
>
>Siehe [Erste Schritte mit der Protokollverwaltung](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) und [Einführung in die New Relic-Abfragesprache](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) auf der _New Relic Docs_-Website.
