---
title: Integrationen - Übersicht
description: Erfahren Sie mehr über Integrationsoptionen von Drittanbietern für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Integrationen - Übersicht

Integrationen sind für die Verwendung externer Services nützlich, z. B. für Git-Hosting oder Slack-Bots, und für die Pflege Ihrer aktuellen Entwicklungsprozesse, z. B. die Verwendung der Pull-Request-Funktion für die Code-Überprüfung in GitHub. Sie können Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt die folgenden Integrationen hinzufügen:

![Integrationen](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**So fügen Sie eine Integration mithilfe der Cloud-CLI hinzu**:

Der folgende Befehl beginnt mit interaktiven Eingabeaufforderungen, um den Typ und die Optionen für die neue Integration auszuwählen.

```bash
magento-cloud integration:add
```

**Auflisten der für Ihr Projekt konfigurierten Integrationen**:

```bash
magento-cloud integration:list
```

Beispielantwort:

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB Konsole]

**So fügen Sie eine Integration mithilfe der[!DNL Cloud Console]** hinzu:

1. Klicken _in &quot;_&quot; auf **[!UICONTROL Integrations]**.

1. Klicken Sie auf einen Integrationstyp oder klicken Sie auf **[!UICONTROL Add integration]**.

1. Führen Sie die Schritte zur Auswahl des Integrationstyps und zur Konfiguration schrittweise durch.

1. Nach dem Hinzufügen der Integration wird sie in der Liste in der Ansicht Integrationen angezeigt.

>[!ENDTABS]

## Commerce-Webhooks

Sie können Commerce-Webhooks in Ihrem Cloud-Projekt mit der globalen [ENABLE_WEBHOOKS“ &#x200B;](../environment/variables-global.md#enable_webhooks). Commerce-Webhooks senden als Reaktion auf Commerce-generierte Ereignisse Anfragen an einen externen Server. Im [_Webhooks-Handbuch_](https://developer.adobe.com/commerce/extensibility/webhooks) wird diese Funktion ausführlich beschrieben.

## Allgemeine Webhooks

Sie können Cloud-Infrastruktur- und Repository-Ereignisse mithilfe einer benutzerdefinierten Webhook-Integration erfassen und melden, um JSON-Nachrichten an eine _Webhook_-URL zu `POST`.

**Verwenden Sie die folgende Syntax, um eine Webhook-URL hinzuzufügen**:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` () - Geben Sie den Integrationstyp `webhook` an.
- `url` - Geben Sie die Webhook-URL an, die JSON-Nachrichten empfangen kann.

Die Beispielantwort zeigt eine Reihe von Eingabeaufforderungen, die eine Möglichkeit bieten, die Integration anzupassen. Bei Verwendung der Standardantwort (leer) werden Nachrichten zu allen Ereignissen in allen Umgebungen in einem Projekt gesendet.

Sie können die Integration anpassen, um bestimmte [Ereignisse“ zu melden](#events-to-report) z. B. Code an eine Verzweigung zu senden. Sie können beispielsweise das `environment.push`-Ereignis angeben, um eine Nachricht zu senden, wenn ein Benutzer Code an eine Verzweigung sendet:

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Sie können Ereignisse in einem `pending`, `in_progress` oder `complete` Status melden:

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

Und Sie können _Nachrichten ein_ oder _ausschließen_ für bestimmte Umgebungen:

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

Nach Abschluss der Integration erhalten Sie eine Zusammenfassung der Werte:

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Vorhandene Integration aktualisieren

Sie können eine vorhandene Integration aktualisieren. Ändern Sie beispielsweise den Status wie folgt von `complete` auf `pending`:

```bash
magento-cloud integration:update --states=pending <int-id>
```

Beispielantwort:

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Zu meldende Ereignisse

| Ereignis | Beschreibung |
| ----- | :-----------|
| `environment.access.add` | Einem Benutzer wurde Zugriff auf die Umgebung gewährt |
| `environment.access.remove` | Ein Benutzer wurde aus der Umgebung entfernt |
| `environment.activate` | Eine Verzweigung wurde mit einer Umgebung „aktiviert“ |
| `environment.backup` | Ein Benutzer hat einen Snapshot ausgelöst |
| `environment.branch` | Eine Verzweigung wurde mithilfe der Verwaltungskonsole erstellt |
| `environment.deactivate` | Eine Verzweigung wurde „deaktiviert“. Der Code ist noch vorhanden, aber die Umgebung wurde zerstört |
| `environment.delete` | Eine Verzweigung wurde gelöscht |
| `environment.initialize` | Die `master` Verzweigung des Projekts, die mit einem ersten Commit initialisiert wurde |
| `environment.merge` | Eine aktive Verzweigung wurde mithilfe der Verwaltungskonsole oder API zusammengeführt |
| `environment.push` | Ein Benutzer hat Code an eine Verzweigung gesendet |
| `environment.restore` | Ein Benutzer hat einen Schnappschuss wiederhergestellt |
| `environment.route.create` | Eine Route wurde über die Verwaltungskonsole erstellt |
| `environment.route.delete` | Eine Route wurde über die Verwaltungskonsole gelöscht |
| `environment.route.update` | Eine Route wurde über die Verwaltungskonsole geändert |
| `environment.subscription.update` | Die Größe der `master` wurde geändert, da sich das Abonnement geändert hat. Es gibt jedoch keine Inhaltsänderungen |
| `environment.synchronize` | In einer Umgebung wurden Daten oder Code aus der übergeordneten Umgebung kopiert |
| `environment.update.http_access` | HTTP-Zugriffsregeln für eine Umgebung wurden geändert |
| `environment.update.restrict_robots` | Die Funktion für blockübergreifende Roboter wurde aktiviert oder deaktiviert |
| `environment.update.smtp` | Das Senden von E-Mails wurde für eine Umgebung aktiviert oder deaktiviert |
| `environment.variable.create` | Eine Variable wurde erstellt |
| `environment.variable.delete` | Eine Variable wurde gelöscht |
| `environment.variable.update` | Eine Variable wurde geändert |
| `project.domain.create` | Eine Domain wurde erstellt und dem Projekt hinzugefügt |
| `project.domain.delete` | Eine dem Projekt zugeordnete Domain wurde entfernt |
| `project.domain.update` | Eine dem Projekt zugeordnete Domain wurde aktualisiert. |
