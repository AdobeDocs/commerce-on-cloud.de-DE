---
title: Valley-Service einrichten
description: Erfahren Sie, wie Sie Valkey als Backend-Cache-Lösung für Adobe Commerce in der Cloud-Infrastruktur einrichten und optimieren können, einschließlich des Ersetzens von Redis und der Anpassung der Cache-Backend-Einstellungen.
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
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Valley-Service einrichten

[Valkey](https://valkey.io) ist eine optionale Backend-Cache-Lösung für Adobe Commerce auf Cloud-Infrastrukturen. Valkey ist erforderlich, wenn Sie die standardmäßige Cache-Konfiguration in Adobe Commerce 2.4.9 und höher oder in Patch-Versionen nach 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 und 2.4.8-p4 überschreiben.

{{service-instruction}}

## Konfigurieren von Valley

Um Redis durch Valley zu ersetzen, aktualisieren Sie die folgenden Dateien:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Konfigurieren des Service

Ersetzen Sie in `.magento/services.yaml` die Redis-Dienstdefinition durch eine Valley-Dienstdefinition. Ersetzen Sie `<version>` durch eine Valkey-Version, die von Ihrer Adobe Commerce-Version und der aktuellen Cloud-Vorlage unterstützt wird.

```yaml
cache:
  type: valkey:<version>
```

**Beispiel**

```yaml
cache:
  type: valkey:8.0
```

Die Beispielversion ist nicht universell. Die tatsächlichen standardmäßigen und unterstützten Dienstversionen hängen von Ihrer Adobe Commerce-Version und Ihrer aktuellen Cloud-Vorlage ab. Verwendet die in der aktuellen Projektvorlage angegebene Version. Weitere Informationen finden [&#x200B; unter &#x200B;](services-yaml.md#service-versions) von Diensten .

>[!WARNING]
>
>Wenn Sie die Service-ID ändern, wird der vorhandene Service entfernt und ein neuer Service erstellt. Vorhandene Daten im entfernten Dienst werden dauerhaft gelöscht. Sichern Sie die Umgebung, bevor Sie einen Service umbenennen.

Gehen Sie nicht davon aus, dass Cache- und Sitzungsdaten bestehen bleiben, wenn Sie den `type` von `redis:<version>` in `valkey:<version>` ändern, selbst wenn Sie dieselbe Service-ID beibehalten. Behandeln Sie die Migration als Erstellen eines neuen Cache: Vorhandene Cache- und Sitzungsdaten werden nicht garantiert beibehalten, und Benutzer werden nach Abschluss der Migration abgemeldet.

### Konfigurieren der Service-Beziehung

Konfigurieren Sie `.magento.app.yaml` die Beziehung zwischen der Anwendung und dem Valley-Service:

```yaml
relationships:
  valkey: "cache:valkey"
```

Der Beziehungsschlüssel `valkey` ist der Name, der von der Anwendung für den Zugriff auf den Service verwendet wird. Der Wert `cache:valkey` verweist auf die Service-ID und den Service-Typ, die in `.magento/services.yaml` definiert sind.

>[!TIP]
>
>Adobe Commerce kommuniziert mit Valkey über die `credis`-Client-Bibliothek, die standardmäßig über einfache PHP-Sockets funktioniert. Um die Leistung zu verbessern, aktivieren Sie die `redis` PHP-Erweiterung in `.magento.app.yaml`. `credis` verwendet die kompilierte Erweiterung automatisch, wenn sie verfügbar ist.
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### Übertragen und Bereitstellen der Änderungen

Fügen Sie die Konfigurationsänderungen hinzu, übertragen Sie sie und übertragen Sie sie:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

Überprüfen Sie nach Abschluss der Bereitstellung, ob die Valley-Service-Beziehung verfügbar ist.

{{service-change-tip}}

{{valkey-newrelic}}

## Anpassen der Valley-Konfiguration

Empfehlungen zu Cache-, Sitzungs-, L2- und Replikatverbindungen finden Sie unter [Best Practices für die Konfiguration von Valkey- und &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)-Services im _Handbuch zu Best Practices für Implementierungsplaybooks_.

## Überprüfen der Service-Beziehung

Um das decodierte `MAGENTO_CLOUD_RELATIONSHIPS`-Objekt anzuzeigen, führen Sie den folgenden Befehl aus einem Anwendungs-Container aus, nachdem Sie die Konfiguration bereitgestellt haben:

SSH verwenden, um eine Verbindung zur Remote-Cloud-Umgebung herzustellen, und dann ausführen:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Der Befehl zeigt alle konfigurierten Dienstbeziehungen an. Suchen Sie die Valkey-Beziehung, um die Valley-Verbindungsdetails zu identifizieren.

**Beispielausgabe**

Das folgende gekürzte Beispiel zeigt die `valkey`. Es ist kein universelles Schema.

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

Die Ausgabe variiert je nach Umgebung und Service-Konfiguration. Hartcodieren Sie in diesem Beispiel keine Hostnamen, Ports, IP-Adressen, Cluster-Namen, Service-Versionen, Benutzernamen oder Kennwörter. Verwenden Sie die von `MAGENTO_CLOUD_RELATIONSHIPS` in der Zielumgebung zurückgegebenen Werte.

Wenn `jq` verfügbar ist, zeigen Sie nur die Valkey-Beziehung an:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

Weitere Informationen zu Service-Beziehungen finden Sie unter [Konfigurieren von Services](services-yaml.md).

## Verwenden der Valley-CLI

Wenn Ihre Valkey-Beziehung `valkey` heißt, verwenden Sie den Host und den Port, die von `MAGENTO_CLOUD_RELATIONSHIPS` zurückgegeben werden, um eine Verbindung zu Valkey herzustellen:

```terminal
valkey-cli -h <host> -p <port>
```

**Beispiel**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## Abrufen der installierten Valley-Version

>[!BEGINTABS]

>[!TAB Integrationsumgebung]

Verwenden Sie in einer Integrationsumgebung den Host und Port, die von der `valkey` Beziehung zurückgegeben werden, um Folgendes auszuführen:

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**Beispielantwort**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

Die Versions- und Build-Details variieren je nach Umgebung. Behandeln Sie eine angezeigte Beispielversion nicht als erforderliche Version oder Universaldienstversion.

>[!TAB Pro Staging und Produktion]

Führen Sie in Pro-Staging- und Produktionsumgebungen Folgendes aus:

```terminal
valkey-server -v
```

**Beispielantwort**

```text
Valkey server v=<installed-version> ...
```

Die Versions- und Build-Details variieren je nach Umgebung. Behandeln Sie eine angezeigte Beispielversion nicht als erforderliche Version oder Universaldienstversion.

>[!ENDTABS]

## Fehlerbehebung im Tal

### Referenz zur Cache-Bereinigung von Fehlern auf einem Valley-konfigurierten Cache

Ein Fehler bei der Cache-Bereinigung vor der Bereitstellung kann die Fehlercode-`[107]` (`clean-redis-cache`) und eine `Connection to Redis` anzeigen, selbst wenn der `cache`-Service als Valkey konfiguriert ist. `ece-tools` verwendet diesen Fehlercode und diese Meldung für den Cache-Bereinigungsschritt, unabhängig davon, ob der unterstützende Cache-Service Redis oder Valkey ist.

Wenn der zugrunde liegende Fehler ein DNS-Fehler ist, z. B. `Name or service not known` für den Beziehungshost, wurde der Bereitstellungsschritt ausgeführt, bevor die Service-Beziehung verfügbar war, oder der Beziehungsname in `.magento.app.yaml` stimmt nicht mit der Service-ID in `.magento/services.yaml` überein. Siehe [Überprüfen der Service-Beziehung](#verify-the-service-relationship).
