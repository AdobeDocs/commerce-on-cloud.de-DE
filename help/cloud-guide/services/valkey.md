---
title: Valley-Service einrichten
description: Erfahren Sie, wie Sie Valkey als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können.
feature: Cloud, Cache, Services
source-git-commit: f73c742cbdbf56ac073802074d5a9cd921591f0f
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Valley-Service einrichten

[Valkey](https://valkey.io) ist eine optionale Backend-Cache-Lösung, die den `Zend Framework Zend_Cache_Backend_File` ersetzt, den Adobe Commerce standardmäßig verwendet.

Siehe [Konfigurieren von Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html?lang=de){target="_blank"}  im _Konfigurationshandbuch_.

{{service-instruction}}

**So aktivieren Sie Valkey**:

1. Fügen Sie den erforderlichen Namen und den erforderlichen Typ zur `.magento/services.yaml` hinzu.

   ```yaml
   cache:
       type: valkey:<version>
   ```

   Um Ihre eigene Valkey-Konfiguration bereitzustellen, fügen Sie einen `core_config` Schlüssel in Ihrer `.magento/services.yaml` hinzu:

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. Konfigurieren Sie die Beziehungen in der `.magento.app.yaml`.

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

## Verwenden der Valley-CLI

Wenn Ihre Valley-Beziehung `valkey` heißt, können Sie mit dem `valkey-cli`-Tool darauf zugreifen.

1. Verwenden Sie SSH, um eine Verbindung zur Integrationsumgebung herzustellen, wenn Valley installiert und konfiguriert ist.

1. Öffnen Sie einen SSH-Tunnel zu einem Host.

   ```bash
   valkey-cli -h valkeycache.internal
   ```

## Installierte Valley-Version abrufen

Verwenden Sie den folgenden Befehl, um die Valley-Version in einer Integrationsumgebung zu installieren:

```bash
valkey-cli -h valkeycache.internal info | grep version
```

Antwort:

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valley auf Pro-Staging und Produktion

Um die Valkey-Version in einer Staging- oder Produktionsumgebung zu installieren, verwenden Sie den `valkey-server`:

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

Verwenden Sie den folgenden Befehl, um die Valley-Konfiguration in einer Pro-Staging- oder Produktionsumgebung zu installieren:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Antwort:

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
