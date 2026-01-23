---
title: Einrichten des RabbitMQ-Service
description: Erfahren Sie, wie Sie den RabbitMQ-Service aktivieren, um Nachrichtenwarteschlangen für Adobe Commerce in der Cloud-Infrastruktur zu verwalten.
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 258fe6de7a8b80cb3403f1ce04d0bf2e299f68ae
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Einrichten [!DNL RabbitMQ] Services

Das [Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=de) ist ein System in Adobe Commerce, das es einem [Modul](https://experienceleague.adobe.com/de/docs/commerce-operations/implementation-playbook/glossary#module) ermöglicht, Nachrichten in Warteschlangen zu veröffentlichen. Außerdem werden die Verbraucher definiert, die die Nachrichten asynchron erhalten.

Der MQF verwendet [RabbitMQ](https://www.rabbitmq.com/) als Messaging-Broker, der eine skalierbare Plattform für das Senden und Empfangen von Nachrichten bietet. Sie enthält auch einen Mechanismus zum Speichern nicht zugestellter Nachrichten. [!DNL RabbitMQ] basiert auf der Spezifikation des Advanced Message Queuing Protocol (AMQP) 0.9.1.

>[!NOTE]
>
>Adobe Commerce in Cloud-Infrastrukturen unterstützt auch [ActiveMQ Artemis](activemq.md) als alternativen Nachrichtenwarteschlangendienst unter Verwendung des STOMP-Protokolls.

>[!IMPORTANT]
>
>Wenn Sie es vorziehen, einen bestehenden AMQP-basierten Service wie [!DNL RabbitMQ] zu verwenden, anstatt sich bei seiner Erstellung auf die Cloud-Infrastruktur von Adobe Commerce zu verlassen, verwenden Sie die [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) Umgebungsvariable , um ihn mit Ihrer Site zu verbinden.

{{service-instruction}}

**So aktivieren Sie RabbitMQ**:

1. Fügen Sie den erforderlichen Namen, den Typ und den Wert der Festplatte (in MB) zusammen mit der installierten RabbitMQ-Version zur `.magento/services.yaml` hinzu.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Konfigurieren Sie die Beziehungen in der `.magento.app.yaml`.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

## Verbinden mit RabbitMQ zum Debugging

Zu Debugging-Zwecken ist es hilfreich, eine direkte Verbindung zu einer Service-Instanz auf eine der folgenden Arten herzustellen:

- Verbinden über Ihre lokale Entwicklungsumgebung
- Verbindung mit der Anwendung herstellen
- Verbinden von Ihrer PHP-Anwendung

### Verbinden über Ihre lokale Entwicklungsumgebung

1. Melden Sie sich bei der `magento-cloud` CLI an und führen Sie folgendes Projekt aus:

   ```bash
   magento-cloud login
   ```

1. Überprüfen Sie die Umgebung mit installiertem und konfiguriertem RabbitMQ.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH verwenden, um eine Verbindung zur Cloud-Umgebung herzustellen:

   ```bash
   magento-cloud ssh
   ```

1. Rufen Sie die RabbitMQ-Verbindungsdetails und Anmeldedaten aus der [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)-Variablen ab:

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   oder

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Suchen Sie in der Antwort die RabbitMQ-Informationen, z. B.:

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Aktivieren der lokalen Portweiterleitung an RabbitMQ (wenn sich Ihr Projekt in einer anderen Region wie z. B. US-3, EU-5 oder AP-3 befindet, ersetzen Sie ``us-3`` durch ``eu-5``/``ap-3``/``us``)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Ein Beispiel für den Zugriff auf die Web-Benutzeroberfläche der RabbitMQ-Verwaltung unter `http://localhost:15672`:

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. Während die Sitzung geöffnet ist, können Sie einen RabbitMQ-Client Ihrer Wahl von Ihrer lokalen Workstation aus starten, der so konfiguriert ist, dass er über die Portnummer, den Benutzernamen und Kennwortinformationen der Variablen MAGENTO_CLOUD_RELATIONSHIPS eine Verbindung zur `localhost:<portnumber>` herstellt.

### Verbindung mit der Anwendung herstellen

Um eine Verbindung zu RabbitMQ herzustellen, das in einer Anwendung ausgeführt wird, installieren Sie einen Client wie [amqp-utils](https://github.com/dougbarth/amqp-utils) als Projektabhängigkeit in Ihrer `.magento.app.yaml`.

Beispiel:

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

Wenn Sie sich bei Ihrem PHP-Container anmelden, geben Sie einen beliebigen `amqp-` Befehl ein, der zur Verwaltung Ihrer Warteschlangen verfügbar ist.

### Verbinden von Ihrer PHP-Anwendung

Um RabbitMQ mit Ihrer PHP-Anwendung zu verbinden, fügen Sie Ihrer Quellstruktur eine PHP-Bibliothek hinzu.

## Fehlerbehebung beim [!DNL RabbitMQ]-Service

Siehe [Verbindung zu RabbitMQ in Adobe Commerce Cloud nicht möglich](https://experienceleague.adobe.com/de/docs/experience-cloud-kcs/kbarticles/ka-27688).

## Aktualisieren des [!DNL RabbitMQ]

>
>
>Überspringen Sie beim Upgrade von [!DNL RabbitMQ] in einer Integrationsumgebung keine Versionen. Es werden nur [sequenzielle Upgrades](https://www.rabbitmq.com/docs/upgrade#rabbitmq-version-upgradability) unterstützt (z. B. 3.8 → 3.9 → 3.10 → 3.11 → 3.12 → 3.13 → 4.0 → 4.1) und jede Versionsüberhöhung muss einer tatsächlichen, erfolgreichen Bereitstellung Ihrer Cloud-Umgebung entsprechen.
>
>Anweisungen für allgemeine Service-Upgrades finden Sie unter [Ändern der Service-Version](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version).
