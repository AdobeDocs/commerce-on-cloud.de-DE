---
title: Protokoll-Handler
description: Erfahren Sie, wie Sie Protokoll-Handler für Adobe Commerce in der Cloud-Infrastruktur konfigurieren.
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Protokoll-Handler

Sie können Protokoll-Handler konfigurieren, um Nachrichten an einen Remote-Protokollierungsserver zu senden. Ein Protokoll-Handler überträgt Build- und Bereitstellungsprotokolle auf andere Systeme, ähnlich wie Sie Protokolle per Push an Slack und E-Mail senden. Sie können einen _syslog_-Handler aktivieren, der sich ideal für die Protokollierung von Meldungen im Zusammenhang mit Hardware eignet, oder einen Graulog Extended Log Format (GELF)-Handler, der sich ideal für die Protokollierung von Meldungen aus Software-Anwendungen eignet.

Im folgenden Beispiel werden diese beiden Handler konfiguriert, indem die Konfiguration zur `.magento.env.yaml`-Datei hinzugefügt wird. Die Werte für die minimale Protokollierungsebene (`min_level`) finden Sie unter [Protokollierungsebenen](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Protokollebenen

Protokollebenen bestimmen den Detaillierungsgrad von Benachrichtigungsinhalten. Die folgenden Protokollebenen-Kategorien umfassen alle darunter liegenden Protokollebenen. Beispielsweise umfasst eine `debug` Protokollierungsebene die Protokollierung auf jeder Ebene, während eine `alert` nur Warnhinweise und Notfälle anzeigt.

- **debug** - Detaillierte Debugging-Informationen
- **info** - Interessante Ereignisse wie ein Benutzerlogin oder SQL-Protokoll
- **Hinweis** - Normale, aber wichtige Ereignisse
- **Warnung** - Ausnahmefälle, bei denen es sich nicht um Fehler handelt, wie z. B. die Verwendung einer veralteten API oder die schlechte Verwendung einer API
- **error** - Laufzeitfehler, die keine sofortige Aktion erfordern
- **Kritisch** - Kritische Bedingungen, z. B. eine nicht verfügbare Anwendungskomponente oder eine unerwartete Ausnahme
- **Warnhinweis** - Sofortmaßnahmen erforderlich - wie z. B. Ausfall einer Website oder Nichtverfügbarkeit der Datenbank -, die Trigger von SMS-Warnhinweisen informieren
- **Notfall** - System ist nicht verwendbar
