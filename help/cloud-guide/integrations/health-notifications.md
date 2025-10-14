---
title: Statusbenachrichtigungen
description: Erfahren Sie, wie Sie Slack-, E-Mail- und PagerDuty-Benachrichtigungen für die Speicherplatznutzung in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt konfigurieren.
feature: Cloud, Observability, Integration
exl-id: 5a7f37e9-e8f9-4b6b-b628-60dcaa60cc64
source-git-commit: c3c708656e3d79c0893d1c02e60dcdf2ad8d7c7c
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Statusbenachrichtigungen

Adobe Commerce on Cloud Infrastructure überwacht die Speicherplatznutzung auf allen Anwendungen und Services in Ihrer Starter-Umgebung oder Ihrer Pro-Integrationsumgebung. Ein Datenbankdatenträger, dem der Speicherplatz ausgeht, kann zu Datenbeschädigungen führen. Die Konsistenzprüfung erfolgt alle 5 Minuten und kann Sie per E-Mail oder über einen anderen externen Service benachrichtigen. Es gibt drei Warnungen bei geringem Festplattenspeicher für Konsistenzbenachrichtigungen:

- **Warnung** - Der verfügbare Speicherplatz sinkt unter 20 %
- **Kritisch** - Der verfügbare Speicherplatz sinkt auf unter 10 %
- **Alles löschen** - Nach einem Ereignis auf niedriger Festplatte wird verfügbarer Speicherplatz auf mehr als 20 % zurückgegeben

>[!NOTE]
>
>In Pro-Produktionsumgebungen können Sie die Warnmeldungsrichtlinie Managed Warnhinweise for Adobe Commerce für New Relic verwenden, um den Festplattenspeicher zu überwachen. Siehe [Überwachen der Leistung mit verwalteten &#x200B;](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)&quot;.

## E-Mail-Benachrichtigungen

Die E-Mail-Integration für den Systemzustand erfordert eine Ursprungsadresse und mindestens eine Empfängeradresse. Sie können dieselbe E-Mail-Adresse für die `from-address` und die `recipients` verwenden. Im folgenden Beispiel wird eine E-Mail-Integration mit zwei Empfängern registriert:

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slack-Kanalbenachrichtigungen

Slack ist ein externer Service, der interaktive Apps namens Bots verwendet, um Nachrichten in einem Chatraum zu posten. Bevor Sie Statusbenachrichtigungen in Slack erhalten können, müssen Sie einen benutzerdefinierten [Bot-Benutzer](https://api.slack.com/bot-users) für Ihre Slack-Gruppe erstellen. Slack Nachdem Sie den Bot-Benutzer für Ihren Kanal oder Ihre Kanäle konfiguriert haben, speichern Sie das von [&#x200B; bereitgestellte &#x200B;](https://api.slack.com/docs/token-types#bot)Bot-Token“, um Ihre Integration zu registrieren. Im folgenden Beispiel werden Integritätsbenachrichtigungen in einem Slack-Kanal registriert:

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuty-Benachrichtigungen

PagerDuty ist ein externer Dienst, der Team-Mitglieder auf Abruf über wichtige Probleme informieren kann. Bevor Sie Statusbenachrichtigungen in PagerDuty erhalten können, müssen Sie eine [PagerDuty-Integration](https://developer.pagerduty.com/v2/docs/integrating) erstellen, die die Ereignis-API Version 2 verwendet. Verwenden Sie den Integrationsschlüssel oder _Routing-Schlüssel_, um Ihre Integration zu registrieren. Im folgenden Beispiel werden Benachrichtigungen für PagerDuty mithilfe eines Routing-Schlüssels registriert:

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```

## Protokollverwaltung

Um den verfügbaren Speicherplatz zu erhöhen, können Sie Protokolldateien aus Ihrer Umgebung abschneiden oder entfernen. Wenn „LogRotate“ aktiviert ist, laden Sie zunächst eine Sicherungskopie der Protokolle herunter und entfernen Sie diese:

```bash
rm -rf some-log-file.log.gz
```

Alternativ können Sie einzelne Protokolldateien abschneiden, um ihre Größe zu reduzieren. Ein detailliertes Beispiel zum Abschneiden von Protokolldateien finden Sie im Video-Tutorial zum Abschneiden von Protokolldateien{target="_blank"}.
