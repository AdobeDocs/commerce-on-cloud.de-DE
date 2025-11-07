---
title: Konfigurieren von Services
description: Erfahren Sie, wie Sie Services konfigurieren, die von Adobe Commerce in der Cloud-Infrastruktur verwendet werden.
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 322f7af2c79dd4eeeabafa2ba7e5a32cbd8b1925
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---

# Konfigurieren von Services

Die `services.yaml` definiert die Services, die von Adobe Commerce in Cloud-Infrastrukturen wie MySQL, Redis und Elasticsearch oder OpenSearch unterstützt und verwendet werden. Sie müssen keine externen Dienstleister abonnieren.

>[!NOTE]
>
>Die `.magento/services.yaml` wird lokal im `.magento` des Projekts verwaltet. Die Konfiguration wird nur während des Build-Prozesses aufgerufen, um die erforderlichen Service-Versionen in der Integrationsumgebung zu definieren, und wird entfernt, sobald die Bereitstellung abgeschlossen ist. Sie finden sie daher nicht auf dem Server.

Das Bereitstellungsskript verwendet die Konfigurationsdateien im `.magento`, um die Umgebung mit den konfigurierten Services bereitzustellen. Ein Dienst wird für die Anwendung verfügbar, wenn er in der [`relationships`](../application/properties.md#relationships) der `.magento.app.yaml` enthalten ist. Die `services.yaml`-Datei enthält die Werte _type_ und _disk_. Der Diensttyp definiert den Dienst _name_ und _version_.

Wenn Sie eine Service-Konfiguration ändern, stellt eine -Bereitstellung die Umgebung mit den aktualisierten Services bereit, was sich auf die folgenden Umgebungen auswirkt:

- Alle Starter-Umgebungen einschließlich `master`
- Pro-Integrationsumgebungen

{{pro-update-service}}

## Standard- und unterstützte Services

Die Cloud-Infrastruktur unterstützt und stellt die folgenden Services bereit:

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>
>Nach dem Upgrade auf eine neue Version von RabbitMQ sollten Sie eine vollständige Bereitstellung in Trigger nehmen, um sicherzustellen, dass Ihre benutzerdefinierten Nachrichtenwarteschlangen in RabbitMQ neu erstellt werden.

Sie können Standardversionen und Datenträgerwerte in der aktuellen ([-`services.yaml`) ](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). Das folgende Beispiel zeigt die `mysql`-, `redis`-, `opensearch`- oder `elasticsearch`-, `rabbitmq`- und `activemq-artemis`-Services, die in der `services.yaml`-Konfigurationsdatei definiert sind:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## Service-Werte

Sie müssen die Service-ID und den `type: <name>:<version>` für den Service-Typ angeben. Wenn der Service persistenten Speicher verwendet, müssen Sie einen Datenträgerwert angeben.

Verwenden Sie das folgende Format:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

Der `service-id` identifiziert den Service im Projekt. Sie können nur alphanumerische Kleinbuchstaben verwenden: `a` zu `z` und `0` zu `9`, z. B. `redis`.

Dieser _service-id_-Wert wird in der [`relationships`](../application/properties.md#relationships)-Eigenschaft der `.magento.app.yaml`-Konfigurationsdatei verwendet:

```yaml
relationships:
    redis: "<name>:redis"
```

Sie können mehrere Instanzen jedes Service-Typs benennen. Sie können beispielsweise mehrere Redis-Instanzen verwenden - eine für die Sitzung und eine für den Cache.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

Beim Umbenennen eines Dienstes in der `services.yaml`-Datei **Folgendes** entfernt:

- Öffnen Sie den vorhandenen Dienst, bevor Sie einen Dienst mit dem von Ihnen angegebenen neuen Namen erstellen.
- Alle vorhandenen Daten für den Service werden entfernt. Adobe empfiehlt dringend, [Ihre Starter-Umgebung zu sichern](../storage/snapshots.md) bevor Sie den Namen eines vorhandenen Services ändern.

### `type`

Der `type` gibt den Namen und die Version des Services an. Beispiel:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

Der `disk` gibt die Größe des persistenten Festplattenspeichers (in MB) an, der dem Service zugewiesen werden soll. Dienste, die persistenten Speicher verwenden, wie MySQL, müssen einen Festplattenwert angeben. Dienste, die Speicher anstelle von persistentem Speicher verwenden, wie z. B. Redis, benötigen keinen Festplattenwert.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

Der aktuelle standardmäßige Speicherbedarf pro Projekt beträgt 5 GB oder 512 0 MB. Sie können diesen Betrag zwischen Ihrer Anwendung und den einzelnen zugehörigen Diensten aufteilen.

## Service-Beziehungen

In Adobe Commerce in Cloud-Infrastrukturprojekten bestimmen Service[Beziehungen](../application/properties.md#relationships), die in der `.magento.app.yaml`-Datei konfiguriert sind, welche Services für Ihre Anwendung verfügbar sind.

Sie können die Konfigurationsdaten für alle Service-Beziehungen aus der [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md) Umgebungsvariablen abrufen. Die Konfigurationsdaten umfassen den Dienstnamen, den Typ und die Version sowie alle erforderlichen Verbindungsdetails wie Port-Nummer und Anmeldeinformationen.

**So überprüfen Sie Beziehungen in der lokalen Umgebung**:

1. Zeigen Sie in Ihrer lokalen Umgebung die Beziehungen für die aktive Umgebung an.

   ```bash
   magento-cloud relationships
   ```

1. Bestätigen Sie die `service` und `type` aus der Antwort. Die Antwort enthält Verbindungsinformationen wie IP-Adresse und Port-Nummer.

   >Abgekürzte Beispielantwort

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**So überprüfen Sie Beziehungen in Remote-Umgebungen**:

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

1. Auflisten der Beziehungskonfigurationsdaten für alle in der Umgebung konfigurierten Services.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   Oder verwenden Sie den folgenden `ece-tools`-Befehl, um Beziehungen anzuzeigen:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Bestätigen Sie die `service` und `type` aus der Antwort. Die Antwort enthält Verbindungsinformationen wie die IP-Adresse und Port-Nummer sowie alle erforderlichen Benutzernamen- und Kennwortberechtigungen.

## Service-Versionen

Die Unterstützung der Service-Version und der Kompatibilität für Adobe Commerce in der Cloud-Infrastruktur wird durch Versionen bestimmt, die in der Cloud-Infrastruktur bereitgestellt und getestet werden, und unterscheidet sich manchmal von Versionen, die von Adobe Commerce On-Premise-Bereitstellungen unterstützt werden. Siehe [Systemanforderungen](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) im _Installationshandbuch_ für eine Liste der Abhängigkeiten von Drittanbieterprogrammen, die Adobe mit bestimmten Adobe Commerce- und Magento Open Source-Versionen getestet hat.

### Software-EOL-Prüfungen

Während des Bereitstellungsprozesses überprüft das `ece-tools`-Paket die installierten Service-Versionen für jeden Service mit den Ablaufdaten (End of Life, EOL).

- Wenn eine Service-Version innerhalb von drei Monaten nach dem Ende der Nutzungsdauer veröffentlicht wird, wird im Bereitstellungsprotokoll eine Benachrichtigung angezeigt.
- Wenn das Ende der Nutzungsdauer in der Vergangenheit liegt, wird eine Warnmeldung angezeigt.

Um die Speichersicherheit aufrechtzuerhalten, aktualisieren Sie installierte Softwareversionen, bevor sie das Ende der Nutzungsdauer erreichen. Die EOL-Daten können in der [-Datei `eol.yaml`ece-tools“ eingesehen ](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Zu OpenSearch migrieren

{{elasticsearch-support}}

Informationen zu Adobe Commerce Version 2.4.4 und höher finden Sie unter [Einrichten des OpenSearch-Service](opensearch.md).

## Service-Version ändern

Sie können die installierte Version des Services aktualisieren, um die Kompatibilität mit der Adobe Commerce-Version zu gewährleisten, die in Ihrer Cloud-Umgebung bereitgestellt wird.

Ein Downgrade der Service-Version für einen installierten Service ist nicht direkt möglich. Sie können jedoch einen Service mit der erforderlichen Version erstellen. Siehe [Downgrade-Service-](#downgrade-version).

### Upgrade der installierten Dienstversion

Sie können die installierte Version des Services aktualisieren, indem Sie die Service-Konfiguration in der `services.yaml`-Datei aktualisieren.

1. Ändern Sie den [`type`](#type) für den Dienst in der `.magento/services.yaml`:

   > Ursprüngliche Service-Definition

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Aktualisierte Service-Definition

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Version herunterstufen

Ein Downgrade eines installierten Services ist nicht direkt möglich. Sie haben zwei Möglichkeiten:

1. Benennen Sie einen vorhandenen Service mit der neuen Version um. Dadurch werden der vorhandene Service und die Daten entfernt und ein neuer hinzugefügt.

1. Erstellen Sie einen Service und speichern Sie die Daten aus dem vorhandenen Service.

Wenn Sie die Service-Version ändern, müssen Sie die Service-Konfiguration in der `services.yaml`-Datei aktualisieren und die Beziehungen in der `.magento.app.yaml`-Datei aktualisieren.

**So stufen Sie eine Dienstversion durch Umbenennen eines vorhandenen Dienstes herunter**:

1. Benennen Sie den vorhandenen Service in der `.magento/services.yaml` um und ändern Sie die Version.

   >[!WARNING]
   >
   >Beim Umbenennen eines vorhandenen Dienstes wird dieser ersetzt und alle Daten werden gelöscht. Wenn Sie die Daten beibehalten müssen, erstellen Sie einen Service, anstatt den vorhandenen Service umzubenennen.

   Um beispielsweise die MariaDB-Version für den Service _mysql_ von Version 10.4 auf 10.3 herunterzustufen, ändern Sie die vorhandene Konfiguration _service-id_ und _type_.

   > `services.yaml`

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Neue `services.yaml`

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Aktualisieren Sie die Beziehungen in der `.magento.app.yaml`.

   > `.magento.app.yaml` Originalkonfiguration

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Aktualisierte `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

**So stufen Sie einen Service durch Erstellen eines Service herunter**:

1. Fügen Sie der `services.yaml`-Datei für Ihr Projekt eine Service-Definition mit der heruntergestuften Versionsspezifikation hinzu. Siehe _mysql2_ im folgenden Beispiel:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Ändern Sie die Beziehungskonfiguration in der `.magento.app.yaml`-Datei, um den neuen Service zu verwenden.

   > `.magento.app.yaml` Originalkonfiguration

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Neue `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.
