---
title: Einrichten des Redis-Service
description: Erfahren Sie, wie Sie Redis als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können.
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# Einrichten des Redis-Service

[Redis](https://redis.io) ist eine optionale Backend-Cache-Lösung, die den `Zend Framework Zend_Cache_Backend_File` ersetzt, den Adobe Commerce standardmäßig verwendet.

>[!IMPORTANT]
>
>Redis-Cache wird nicht unterstützt für Adobe Commerce 2.4.9 oder Patch-Versionen nach 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 und 2.4.8-p4. Verwenden Sie [Valkey](valkey.md) für die Cache-Konfiguration, bei der Redis nicht unterstützt wird. Siehe [Systemanforderungen](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) für unterstützte Cache-Services nach Version.

{{service-instruction}}

## Redis aktivieren

Um Redis zu aktivieren, aktualisieren Sie die folgenden Dateien:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Konfigurieren des Service

Fügen Sie `.magento/services.yaml` die Definition des Redis-Dienstes hinzu. Ersetzen Sie `<version>` durch eine Redis-Version, die von Ihrer Adobe Commerce-Version und der aktuellen Cloud-Vorlage unterstützt wird.

```yaml
cache:
  type: redis:<version>
```

Beispiel: Für eine Commerce-Version und eine Cloud-Vorlage, die Redis 7.2 unterstützen:

```yaml
cache:
  type: redis:7.2
```

Die Beispielversion ist nicht universell. Die tatsächlichen standardmäßigen und unterstützten Dienstversionen hängen von Ihrer Adobe Commerce-Version, der Patch-Ebene und der aktuellen Cloud-Vorlage ab. Überprüfen Sie die unterstützte Kombination in [Systemanforderungen](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) und der aktuellen Projektvorlage.

### Konfigurieren der Service-Beziehung

Konfigurieren Sie `.magento.app.yaml` die Beziehung zwischen der Anwendung und dem Redis-Service:

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

Der Beziehungsschlüssel `redis` ist der Name, der von der Anwendung für den Zugriff auf den Service verwendet wird. Der Wert `cache:redis` besteht aus der Service-ID (`cache`) und dem Service-Typ (`redis`), die in `.magento/services.yaml` definiert sind.

### Übertragen und Bereitstellen der Änderungen

Fügen Sie die Konfigurationsänderungen hinzu, übertragen Sie sie und übertragen Sie sie:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

Stellen Sie nach Abschluss der Bereitstellung sicher, dass die Redis-Service-Beziehung verfügbar ist.

{{service-change-tip}}

## Überprüfen der Service-Beziehung

Führen Sie nach der Bereitstellung der Konfiguration den folgenden Befehl aus einem Anwendungs-Container aus, um das decodierte `MAGENTO_CLOUD_RELATIONSHIPS`-Objekt anzuzeigen:

SSH verwenden, um eine Verbindung zur Remote-Cloud-Umgebung herzustellen, und dann ausführen:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Der Befehl zeigt alle konfigurierten Dienstbeziehungen an. Suchen Sie die `redis`, um die Redis-Verbindungsdetails zu identifizieren.

Das folgende gekürzte Beispiel zeigt die `redis`. Es ist kein universelles Schema.

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

Die Ausgabe variiert je nach Umgebung und Service-Konfiguration. Hartcodieren Sie in diesem Beispiel keine Hostnamen, Ports, IP-Adressen, Cluster-Namen, Service-Versionen, Benutzernamen oder Kennwörter. Verwenden Sie die von `MAGENTO_CLOUD_RELATIONSHIPS` in der Zielumgebung zurückgegebenen Werte.

Wenn `jq` verfügbar ist, verwenden Sie den folgenden Befehl, um nur die Redis-Beziehung anzuzeigen:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

Weitere Informationen zu Service-Beziehungen finden Sie unter [Konfigurieren von Services](services-yaml.md).

## Anpassen der Redis-Konfiguration

Empfehlungen zu Cache-, Sitzungs-, L2- und Replikatverbindungen finden Sie unter [Best Practices für die Konfiguration von Valkey- und ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)-Services im _Handbuch zu Best Practices für Implementierungsplaybooks_.

## Verwenden der Redis-CLI

Wenn Ihre Redis-Beziehung `redis` heißt, verwenden Sie den Host und Port, die von `MAGENTO_CLOUD_RELATIONSHIPS` zurückgegeben werden, um eine Verbindung zu Redis herzustellen.

Stellen Sie eine Verbindung zur -Umgebung her, in der Redis installiert und konfiguriert ist, und führen Sie den folgenden Befehl aus:

```terminal
redis-cli -h <host> -p <port>
```

**Beispiel**

```terminal
redis-cli -h redis.internal -p 6379
```

## Abrufen der installierten Redis-Version

>[!BEGINTABS]

>[!TAB Integrationsumgebung]

Verwenden Sie in einer Integrationsumgebung den Host und Port, die von der `redis` Beziehung zurückgegeben werden, um Folgendes auszuführen:

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**Beispielantwort**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

Die Versions- und Build-Details variieren je nach Umgebung. Behandeln Sie eine angezeigte Beispielversion nicht als erforderliche Version oder Universaldienstversion.

>[!TAB Pro Staging und Produktion]

Führen Sie in Pro-Staging- und Produktionsumgebungen Folgendes aus:

```terminal
redis-server -v
```

**Beispielantwort**

```text
Redis server v=<installed-version> ...
```

Die Versions- und Build-Details variieren je nach Umgebung. Behandeln Sie eine angezeigte Beispielversion nicht als erforderliche Version oder Universaldienstversion.

>[!ENDTABS]

## Fehlerbehebung für Redis

Hilfe bei der Fehlerbehebung bei Redis-Problemen finden Sie in den folgenden Artikeln zum Adobe Commerce-Support:

- [Verwaltete Warnhinweise auf Adobe Commerce: Warnung bei Redis-Speicher](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Verwaltete Warnhinweise auf Adobe Commerce: Kritischer Warnhinweis zu RediS-Speicher](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### Referenz zur Cache-Bereinigung von Fehlern auf einem Valley-konfigurierten Cache

Ein Fehler bei der Cache-Bereinigung vor der Bereitstellung kann die Fehlercode-`[107]` (`clean-redis-cache`) und eine `Connection to Redis` anzeigen, selbst wenn der `cache`-Service als Valkey konfiguriert ist. `ece-tools` verwendet diesen veralteten Redis-orientierten Fehlercode und diese Nachricht für den Cache-Bereinigungsschritt, unabhängig davon, welcher Service die `cache`-Beziehung unterstützt, sodass der Wortlaut nicht anzeigt, dass Redis installiert ist.

Wenn der zugrunde liegende Fehler ein DNS-Fehler ist, z. B. `Name or service not known` für den Beziehungshost, wurde der Bereitstellungsschritt ausgeführt, bevor die Service-Beziehung verfügbar war, oder der Beziehungsname in `.magento.app.yaml` stimmt nicht mit der Service-ID in `.magento/services.yaml` überein. Siehe [Überprüfen der Service-Beziehung](#verify-the-service-relationship).
