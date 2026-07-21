---
title: Einrichten des Redis-Service
description: Erfahren Sie, wie Sie Redis als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können.
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 9e1fd3699623816ea3368816820daf799a43284f
workflow-type: tm+mt
source-wordcount: 388
ht-degree: 0%

---

# Einrichten des Redis-Service

[Redis](https://redis.io) ist eine optionale Backend-Cache-Lösung, die den `Zend Framework Zend_Cache_Backend_File` ersetzt, den Adobe Commerce standardmäßig verwendet.

>[!IMPORTANT]
>
>Redis-Cache wird für Adobe Commerce 2.4.9 oder Patch-Versionen nach 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 und 2.4.8-p4 nicht unterstützt. Verwenden Sie Valley für die Cache-Konfiguration, bei der Redis nicht unterstützt wird. Siehe [Systemanforderungen](https://experienceleague.adobe.com/de/docs/commerce-operations/installation-guide/system-requirements) für unterstützte Cache-Services nach Version.

{{service-instruction}}

**So aktivieren Sie Redis**:

1. Fügen Sie den erforderlichen Namen und den erforderlichen Typ zur `.magento/services.yaml` hinzu.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Um Ihre eigene Redis-Konfiguration bereitzustellen, fügen Sie einen `core_config` Schlüssel in Ihrer `.magento/services.yaml` hinzu:

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Konfigurieren Sie die Beziehungen in der `.magento.app.yaml`.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

## Anpassen der Redis-Konfiguration

Einzelheiten zum Anpassen der Redis-Konfiguration finden Sie unter [Konfigurieren von Redis](https://experienceleague.adobe.com/de/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) im _Handbuch für Best Practices für Implementierungs-Playbooks_.

## Verwenden der Redis-CLI

Wenn Ihre Redis-Beziehung `redis` heißt, können Sie mit dem `redis-cli`-Tool darauf zugreifen.

1. Verwenden Sie SSH, um eine Verbindung zur Integrationsumgebung herzustellen, in der Redis installiert und konfiguriert ist.

1. Öffnen Sie einen SSH-Tunnel zu einem Host.

   ```bash
   redis-cli -h redis.internal
   ```

## Installierte Redis-Version abrufen

Verwenden Sie den folgenden Befehl, um die Redis-Version in einer Integrationsumgebung zu installieren:

```bash
redis-cli -h redis.internal info | grep version
```

Beispielantwort:

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis zu Pro-Staging und Produktion

Um die Redis-Version in einer Staging- oder Produktionsumgebung zu installieren, verwenden Sie den `redis-server`:

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

Verwenden Sie den folgenden Befehl, um die Redis-Konfiguration in einer Pro Staging- oder Produktionsumgebung zu installieren:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Beispielantwort:

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Fehlerbehebung für Redis

Hilfe bei der Fehlerbehebung bei Redis-Problemen finden Sie in den folgenden Artikeln zum Adobe Commerce-Support:

- [Problem mit verzögerter Admin-Anmeldung oder Checkout neu bearbeiten](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [Erweiterte Redis-Cache-Implementierung Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html?lang=de)
- [Verwaltete Warnhinweise auf Adobe Commerce: Warnung bei Redis-Speicher](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html?lang=de)
- [Verwaltete Warnhinweise auf Adobe Commerce: Kritischer Warnhinweis zu RediS-Speicher](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html?lang=de)
