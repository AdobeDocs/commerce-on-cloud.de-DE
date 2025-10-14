---
title: Einrichten des ActiveMQ-Service
description: Erfahren Sie, wie Sie den ActiveMQ Artemis-Service aktivieren, um Nachrichtenwarteschlangen für Adobe Commerce in der Cloud-Infrastruktur zu verwalten.
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Einrichten [!DNL ActiveMQ] Services

Das [Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=de) ist ein System in Adobe Commerce, das es einem [Modul](https://experienceleague.adobe.com/de/docs/commerce-operations/implementation-playbook/glossary#module) ermöglicht, Nachrichten in Warteschlangen zu veröffentlichen. Außerdem werden die Verbraucher definiert, die die Nachrichten asynchron erhalten.

Der MQF kann [ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) als Messaging-Broker verwenden, der eine skalierbare Plattform für das Senden und Empfangen von Nachrichten bietet. Sie enthält auch einen Mechanismus zum Speichern nicht zugestellter Nachrichten. [!DNL ActiveMQ Artemis] unterstützt das STOMP-Protokoll (Streaming Text Oriented Messaging Protocol) für Messaging.

[!DNL ActiveMQ Artemis] ist als Alternative zu RabbitMQ für die Verwaltung von Nachrichtenwarteschlangen verfügbar. Dies ist besonders nützlich, wenn Sie für ActiveMQ spezifische Funktionen benötigen oder das STOMP-Protokoll verwenden möchten.

>[!IMPORTANT]
>
>Wenn Sie einen vorhandenen Message Broker-Dienst wie [!DNL ActiveMQ] bevorzugen, anstatt sich bei der Erstellung auf die Cloud-Infrastruktur von Adobe Commerce zu verlassen, verwenden Sie die Umgebungsvariable [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) , um ihn mit Ihrer Site zu verbinden.

{{service-instruction}}

**So aktivieren Sie ActiveMQ Artemis**:

1. Fügen Sie den erforderlichen Namen, den erforderlichen Typ und den erforderlichen Datenträgerwert (in MB) zusammen mit der installierten ActiveMQ-Version zur `.magento/services.yaml` hinzu.

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. Konfigurieren Sie die Beziehungen in der `.magento.app.yaml`.

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

## Verbinden mit ActiveMQ zum Debuggen

Zum Debuggen können Sie eine direkte Verbindung zu einer Service-Instanz auf eine der folgenden Arten herstellen:

- Verbinden über Ihre lokale Entwicklungsumgebung
- Verbindung mit der Anwendung herstellen
- Verbinden von Ihrer PHP-Anwendung

### Verbinden über Ihre lokale Entwicklungsumgebung

1. Melden Sie sich bei der `magento-cloud` CLI an und führen Sie folgendes Projekt aus:

   ```bash
   magento-cloud login
   ```

1. Überprüfen Sie die Umgebung, in der ActiveMQ Artemis installiert und konfiguriert ist.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH verwenden, um eine Verbindung zur Cloud-Umgebung herzustellen:

   ```bash
   magento-cloud ssh
   ```

1. Rufen Sie die ActiveMQ-Verbindungsdetails und Anmeldedaten aus der [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)-Variablen ab:

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   oder

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Suchen Sie in der Antwort die ActiveMQ-Informationen. Der Name der Beziehung hängt davon ab, wie Sie ihn in Ihrer `.magento.app.yaml`-Datei konfiguriert haben. Wenn Sie beispielsweise `activemq-artemis` als Beziehungsnamen verwendet haben:

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Aktivieren der lokalen Port-Weiterleitung an ActiveMQ (wenn sich Ihr Projekt in einer anderen Region wie z. B. US-3, EU-5 oder AP-3 befindet, ersetzen Sie ``us-3``/``eu-5``/``ap-3`` durch ``us``)

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Ein Beispiel für den Zugriff auf die ActiveMQ Artemis-Web-Konsole unter `http://localhost:8161`:

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis verwendet Port 61616 für STOMP-Messaging und Port 8161 für die Web-Konsole.

1. Solange die Sitzung geöffnet ist, können Sie auf die ActiveMQ Artemis-Web-Konsole unter `http://localhost:8161` zugreifen, indem Sie den Benutzernamen und das Kennwort aus der Variablen MAGENTO_CLOUD_RELATIONSHIPS verwenden.

### Verbindung mit der Anwendung herstellen

Um eine Verbindung zu ActiveMQ herzustellen, das in einer Anwendung ausgeführt wird, installieren Sie eine STOMP-Client-Bibliothek in Ihrer PHP-Anwendung.

### Verbinden von Ihrer PHP-Anwendung

Um ActiveMQ mit Ihrer PHP-Anwendung zu verbinden, fügen Sie Ihrer Quellstruktur eine PHP STOMP-Bibliothek hinzu. Adobe Commerce verwendet das STOMP-Protokoll für die ActiveMQ-Kommunikation. Die Konfiguration wird während der Bereitstellung automatisch eingerichtet, wenn ActiveMQ Artemis als konfigurierter Service erkannt wird.

## Protokollunterstützung

ActiveMQ Artemis in Adobe Commerce verwendet in der Cloud-Infrastruktur das STOMP-Protokoll (Streaming Text Oriented Messaging Protocol):

- **STOMP**: Das für Warteschlangenvorgänge verwendete Messaging-Protokoll (Port 61616)
- **Web-Konsole**: Verwaltungsschnittstelle, auf die über HTTP zugegriffen werden kann (Port 8161)

## Unterschiede zu RabbitMQ

Während sowohl [!DNL ActiveMQ Artemis] als auch [!DNL RabbitMQ] als Nachrichtenvermittler für Adobe Commerce dienen, gibt es einige Unterschiede:

- **Protokoll**: ActiveMQ Artemis verwendet das STOMP-Protokoll, während RabbitMQ AMQP verwendet
- **Konfiguration**: Wenn ActiveMQ Artemis konfiguriert ist, verwendet Adobe Commerce automatisch das STOMP-Protokoll
- **Priorität**: Wenn sowohl ActiveMQ als auch RabbitMQ konfiguriert sind, hat ActiveMQ Vorrang für STOMP-basierte Vorgänge, während AMQP-Vorgänge RabbitMQ verwenden
- **Web-Konsole**: ActiveMQ bietet eine Web-basierte Verwaltungskonsole zur Überwachung von Warteschlangen und Nachrichten

## Warteschlangenkonfiguration

Wenn ActiveMQ Artemis als Dienst konfiguriert ist, konfiguriert Adobe Commerce automatisch das Warteschlangensystem für die Verwendung des STOMP-Protokolls. Die Konfiguration wird während der Bereitstellung in die `app/etc/env.php`-Datei geschrieben:

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

Sie können diese Konfiguration bei Bedarf mit der Umgebungsvariablen [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) überschreiben.

