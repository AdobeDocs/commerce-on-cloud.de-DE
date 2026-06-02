---
title: Einrichten von Benachrichtigungen
description: Erfahren Sie, wie Sie Benachrichtigungen für Adobe Commerce in Cloud-Infrastrukturumgebungen konfigurieren.
feature: Cloud, Configuration, Logs
exl-id: dfbe1084-ad30-4489-af2d-d6f6b5eae1c4
TQID: https://experienceleague.adobe.com/YWCv3iFJDvmDTCSaB9cWGmR0WVAEd4ckJdd5MFb8XOo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 413
ht-degree: 0%

---

# Einrichten von Benachrichtigungen

Standardmäßig schreibt Adobe Commerce in der Cloud-Infrastruktur Erstellungs- und Bereitstellungsaktionen in die `app/var/log/cloud.log` im Stammverzeichnis der Adobe Commerce-Anwendung. Optional können Sie Protokolle an ein Messaging-System wie Slack und E-Mail senden, um Echtzeitbenachrichtigungen zu erhalten.

Sie können beispielsweise eine Slack-Nachricht senden, um eine Gruppe von Personen zu benachrichtigen, wenn eine Bereitstellung fehlschlägt, und eine Untersuchung der Fehlerursache einleiten.

## Benachrichtigungen planen

Beachten Sie Folgendes, bevor Sie Benachrichtigungen konfigurieren:

- Welche Art von Benachrichtigungen möchten Sie erhalten (Slack-Nachrichten, E-Mail, beides)?
- Wie viele Details sollen in den Protokollen angezeigt werden?
- Wo sollen Benachrichtigungen eingerichtet werden (Integration, Staging, Produktion)?

Beispielsweise können Sie bei der ersten Entwicklung E-Mail-Benachrichtigungen bevorzugen, die detaillierte Protokolle über Ihre Integrationsumgebung enthalten, um Probleme vor der Bereitstellung in der Staging-Umgebung zu beheben. Wenn Sie für die Bereitstellung in der Staging- oder Produktionsumgebung bereit sind, können Sie eine Slack-Nachricht bevorzugen, die weniger detaillierte Informationen enthält.

>[!NOTE]
>
>Die Konfigurationsdatei, die zum Einrichten von Benachrichtigungen verwendet wird, befindet sich im Stammverzeichnis Ihres Projektverzeichnisses. Daher wird sie angewendet, wenn Sie Änderungen in eine beliebige Umgebung pushen. Wenn Sie Benachrichtigungen pro Umgebung anpassen möchten, müssen Sie die Konfigurationsdatei ändern, bevor Sie sie in diese Umgebung pushen.

## Konfigurieren von Benachrichtigungen

So konfigurieren Sie Benachrichtigungen:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.
1. Fügen Sie in der `.magento.env.yaml`-Datei in Ihrem Projektstammverzeichnis Ihre Messaging-Systemeinstellungen hinzu, einschließlich bevorzugter Benachrichtigungs- [Protokollebenen](log-handlers.md#log-levels).

   Verwenden Sie beispielsweise Folgendes, um sowohl die Slack- _auch_-E-Mail-Konfigurationen zu konfigurieren:

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce in der Cloud-Infrastruktur sendet nur während der Bereitstellungsphase E-Mails.

1. Übergeben Sie Ihre Änderungen und übertragen Sie sie auf den Remote-Server.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Beispiel für eine Slack-Konfiguration

Das folgende Beispiel zeigt eine reine Slack-Konfiguration:

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` - Ihr Slack [Benutzer-Token](https://api.slack.com/docs/token-types#user). Ihr Benutzer-Token autorisiert Adobe Commerce in der Cloud-Infrastruktur zum Senden von Nachrichten.
- `channel` - Name des Slack-Kanals, über den Adobe Commerce in der Cloud-Infrastruktur Benachrichtigungen sendet.
- `username` - Benutzername, den Adobe Commerce in der Cloud-Infrastruktur verwendet, um Benachrichtigungen in Slack zu senden.
- `min_level` - Minimale Protokollebene für Benachrichtigungsinhalte Es wird empfohlen, `info` zu verwenden.

### Beispiel einer E-Mail-Konfiguration

Das folgende Beispiel zeigt eine reine E-Mail-Konfiguration:

>[!NOTE]
>
>Adobe Commerce in der Cloud-Infrastruktur sendet nur während der Bereitstellungsphase E-Mails.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to`: Die E-Mail-Adresse Adobe Commerce in der Cloud-Infrastruktur sendet Benachrichtigungsnachrichten.
- `from` - E-Mail-Adresse für den Versand von Benachrichtigungsinhalten an Empfänger.
- `subject` - Beschreibung der E-Mail.
- `min_level` - Minimale Protokollebene für Benachrichtigungsinhalte Es wird empfohlen, `notice` oder `warning` zu verwenden.
