---
title: Einrichten des Redis-Service
description: Erfahren Sie, wie Sie Redis als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können.
feature: Cloud, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Einrichten des Redis-Service

[Redis](https://redis.io) ist eine optionale Backend-Cache-Lösung, die das Zend-Framework Zend_Cache_Backend_File ersetzt, das Adobe Commerce standardmäßig verwendet.

Siehe [Konfigurieren von Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html) im _Konfigurationshandbuch_.

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

- [Redis Problem verzögert Admin-Anmeldung oder Checkout](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [Erweiterte Redis-Cache-Implementierung Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [Verwaltete Warnhinweise auf Adobe Commerce: Warnung bei Redis-Speicher](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Verwaltete Warnhinweise auf Adobe Commerce: Kritischer Warnhinweis zu Redis-Speicher](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
