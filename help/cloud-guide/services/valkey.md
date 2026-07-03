---
title: Valley-Service einrichten
description: Erfahren Sie, wie Sie Valkey als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d962353ed14bd976cf19560edf1802836cb76c4b
workflow-type: tm+mt
source-wordcount: 229
ht-degree: 0%

---

# Valley-Service einrichten

[Valkey](https://valkey.io) ist eine optionale Backend-Cache-Lösung, die den `Zend Framework Zend_Cache_Backend_File` ersetzt, den Adobe Commerce standardmäßig verwendet.

Siehe [Konfigurieren von Valkey](https://experienceleague.adobe.com/de/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration){target="_blank"} im _Handbuch zu Best Practices für Implementierungswiedergaben_.

{{service-instruction}}

**Um Redis durch Valkey zu ersetzen, aktualisieren Sie die Konfiguration in den folgenden drei Dateien**:

1. Ersetzen Sie die Redis-Konfiguration durch den erforderlichen Valley-Namen und geben Sie die `.magento/services.yaml` ein.

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

1. Konfigurieren Sie `.magento.env.yaml` so, dass die Redis-Konfiguration wie folgt ersetzt wird:.

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

{{valkey-newrelic}}

## Verwenden der Valley-CLI

Wenn Ihre Valley-Beziehung `valkey` heißt, können Sie mit dem `valkey-cli`-Tool darauf zugreifen.

1. Verwenden Sie SSH, um eine Verbindung zur Integrationsumgebung herzustellen, wenn Valley installiert und konfiguriert ist.

1. Öffnen Sie einen SSH-Tunnel zu einem Host.

   ```bash
   valkey-cli -h valkey.internal
   ```

## Installierte Valley-Version abrufen

Verwenden Sie den folgenden Befehl, um die Valley-Version in einer Integrationsumgebung zu installieren:

```bash
valkey-cli -h valkey.internal info | grep version
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
