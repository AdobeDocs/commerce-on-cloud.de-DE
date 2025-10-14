---
title: Variablen bereitstellen
description: Siehe die Liste der Umgebungsvariablen, die Aktionen in der Bereitstellungsphase der Adobe Commerce in der Cloud-Infrastruktur steuern.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 3f2a4f7dc9c23afb3af80304023d9e742c974ccd
workflow-type: tm+mt
source-wordcount: '2483'
ht-degree: 0%

---

# Variablen bereitstellen

Die folgenden _deploy_-Variablen steuern Aktionen in der Bereitstellungsphase und können Werte von den (globalen [) &#x200B;](variables-global.md) und überschreiben. Fügen Sie diese Variablen in den `deploy` Schritt der `.magento.env.yaml` ein:

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Weitere Informationen zum Anpassen des Build- und Bereitstellungsprozesses finden Sie unter:

- [Bereitstellungskonfiguration](configure-env-yaml.md)
- [Bereitstellungsprozess](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Konfigurieren Sie die Redis-Seite und das Standard-Caching. Beim Festlegen des `cm_cache_backend_redis` müssen Sie die Optionen `server`, `port` und `database` angeben.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

Im folgenden Beispiel werden neue Werte mit einer vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

Im folgenden Beispiel wird die [Redis-Vorabladefunktion](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html?lang=de#redis-preload-feature) verwendet, wie im _Konfigurationshandbuch_ definiert:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Um ein benutzerdefiniertes [REDIS_BACKEND](#redis_backend)-Modell (nicht nur aus der Zulassungsliste) zu verwenden, setzen Sie die `_custom_redis_backend`-Option auf `true`, um die richtige Validierung zu aktivieren, wie im folgenden Beispiel gezeigt:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **default**—`true`
- **Version**—Adobe Commerce 2.1.4 und höher

Aktiviert oder deaktiviert [&#x200B; Bereinigung von (statischen &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=de)), die während der Build- oder Bereitstellungsphase generiert wurde. Verwenden Sie den Standardwert _true_ in der Entwicklungsumgebung als Best Practice.

- **`true`** - Entfernt alle vorhandenen statischen Inhalte, bevor der aktualisierte statische Inhalt bereitgestellt wird.
- **`false`** - Die Bereitstellung überschreibt nur vorhandene statische Inhaltsdateien, wenn der generierte Inhalt eine neuere Version enthält.

Wenn Sie statische Inhalte über einen separaten Prozess ändern, setzen Sie den Wert auf _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Wenn statische Ansichtsdateien vor der Bereitstellung nicht bereinigt werden, können Probleme auftreten, wenn Sie Aktualisierungen für vorhandene Dateien bereitstellen, ohne die vorherigen Versionen zu entfernen. Aufgrund von [statischen Datei-Fallback](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache)-Regeln können Fallback-Vorgänge die falsche Datei anzeigen, wenn das Verzeichnis mehrere Versionen derselben Datei enthält.

## `CRON_CONSUMERS_RUNNER`

- **Standard**—`cron_run = false`, `max_messages = 1000`
- **Version**—Adobe Commerce 2.2.0 und höher

Verwenden Sie diese Umgebungsvariable, um zu bestätigen, dass Nachrichtenwarteschlangen nach einer Bereitstellung ausgeführt werden.

- `cron_run` - Ein boolescher Wert, der den `consumers_runner` Cron-Auftrag aktiviert oder deaktiviert (Standard = `false`).
- `max_messages` - Eine Zahl, die die maximale Anzahl an Nachrichten angibt, die jeder Consumer vor dem Beenden verarbeiten muss (Standard = `1000`). Sie können den Wert auf `0` setzen, um zu verhindern, dass der Verbraucher beendet wird.
- `consumers` - Ein Array von Zeichenfolgen, das angibt, welche Consumers ausgeführt werden sollen. Ein leeres Array führt &quot;_&quot;_.

- `multiple_processes`-Eine Zahl, die die Anzahl der Prozesse angibt, die für jeden Verbraucher erzeugt werden sollen. Wird in Commerce **2.4.4** oder höher unterstützt.

>[!NOTE]
>
>Um eine Liste der `consumers` für die Nachrichtenwarteschlange zurückzugeben, führen Sie den Befehl `./bin/magento queue:consumers:list` in der Remote-Umgebung aus.

Beispiel-Array, das bestimmte `consumers` ausführt und die für jeden Verbraucher zu erstellenden `multiple_processes` enthält:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Beispiel eines leeren Arrays, das alle `consumers` ausführt:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Standardmäßig überschreibt der Bereitstellungsprozess alle Einstellungen in der `env.php`. Siehe [Verwalten von Nachrichtenwarteschlangen](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html?lang=de) im _Commerce-Konfigurationshandbuch_ für lokale Adobe Commerce.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **default**—`false`
- **Version**—Adobe Commerce 2.2.0 und höher

Konfigurieren Sie `consumers` Verarbeitung von Nachrichten aus der Nachrichtenwarteschlange, indem Sie eine der folgenden Optionen auswählen:

- `false` - `Consumers` verfügbare Nachrichten in der Warteschlange verarbeiten, die TCP-Verbindung schließen und beenden. `Consumers` warten nicht auf den Eintritt zusätzlicher Nachrichten in die Warteschlange, selbst wenn die Anzahl der verarbeiteten Nachrichten kleiner ist als der in der Variablen `max_messages`-Bereitstellung angegebene `CRON_CONSUMERS_RUNNER`.

- `true` - `Consumers` weiter Nachrichten aus der Nachrichtenwarteschlange verarbeiten, bis die in der Variablen &quot;`max_messages` Deploy“ angegebene maximale Anzahl an Nachrichten (`CRON_CONSUMERS_RUNNER`) erreicht ist, bevor die TCP-Verbindung geschlossen und der Consumerprozess beendet wird. Wenn die Warteschlange geleert wird, bevor `max_messages` erreicht wird, wartet der -Benutzer, bis weitere Nachrichten eingehen.

>[!WARNING]
>
>Wenn Sie Worker verwenden, um `consumers` auszuführen, anstatt einen Cron-Auftrag zu verwenden, setzen Sie diese Variable auf „true“.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

>[!WARNING]
>
>Legen Sie den `CRYPT_KEY` über die [!DNL Cloud Console] statt über die `.magento.env.yaml` fest, um zu vermeiden, dass der Schlüssel im Quell-Code-Repository für Ihre Umgebung verfügbar gemacht wird. Siehe [Festlegen von Umgebungs- und &#x200B;](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html?lang=de#configure-environment).

Wenn Sie die Datenbank ohne Installationsvorgang von einer Umgebung in eine andere verschieben, benötigen Sie die entsprechenden kryptografischen Informationen. Adobe Commerce verwendet den im [!DNL Cloud Console] festgelegten Verschlüsselungsschlüsselwert als `crypt/key` Wert in der `env.php`.

## `DATABASE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Wenn Sie eine Datenbank in der [Relations-Eigenschaft](../application/properties.md#relationships) der `.magento.app.yaml`-Datei definiert haben, können Sie Ihre Datenbankverbindungen für die Bereitstellung anpassen.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

Im folgenden Beispiel werden neue Werte mit einer vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

Außerdem können Sie ein Tabellenpräfix konfigurieren.

>[!WARNING]
>
>Wenn Sie die Option „Zusammenführen“ nicht mit dem Tabellenpräfix verwenden, müssen Sie die Standardverbindungseinstellungen angeben, da sonst die Validierung der Bereitstellung fehlschlägt.

Im folgenden Beispiel wird das `ece_`-Tabellenpräfix mit standardmäßigen Verbindungseinstellungen anstelle der Option `_merge` verwendet:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Beispielausgabe:

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.2.0 und höher

Behält benutzerdefinierte [!DNL Elastic Suite] zwischen Bereitstellungen bei und verwendet sie im Abschnitt „system/default/smile_elasticsuite_core_base_settings“ der [!DNL Elastic Suite]. Wenn das [!DNL Elastic Suite] Composer-Paket installiert ist, wird es automatisch konfiguriert.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>In einem Pro-Staging-/Produktions-Cluster mit drei Knoten (oder drei Service-Knoten in [Scaled Architecture](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier) sollte die `indices_settings` wie folgt festgelegt werden:
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

Im folgenden Beispiel wird ein neuer Wert mit der vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**Bekannte**:

- Das Ändern der Suchmaschine in einen anderen Typ als `elasticsuite` führt zu einem Bereitstellungsfehler mit einem entsprechenden Validierungsfehler
- Das Entfernen des Elasticsearch-Service führt zu einem Bereitstellungsfehler, der mit einem entsprechenden Validierungsfehler einhergeht

>[!NOTE]
>
>Weitere Informationen zur Verwendung oder Fehlerbehebung beim [!DNL Elastic Suite]-Plug-in mit Adobe Commerce finden Sie in der [[!DNL Elastic Suite] Dokumentation](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **default**—`false`
- **Version**—Adobe Commerce 2.1.4 und höher

Aktiviert und deaktiviert Google Analytics bei der Bereitstellung in Staging- und Integrationsumgebungen. Standardmäßig gilt Google Analytics nur für die Produktionsumgebung. Legen Sie diesen Wert auf `true` fest, um Google Analytics in den Staging- und Integrationsumgebungen zu aktivieren.

- **`true`**: Ermöglicht Google Analytics in Staging- und Integrationsumgebungen.
- **`false`**: Deaktiviert Google Analytics in Staging- und Integrationsumgebungen.

Fügen Sie die Umgebungsvariable `ENABLE_GOOGLE_ANALYTICS` zum `deploy` in der `.magento.env.yaml` hinzu:

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>Der Bereitstellungsprozess aktiviert Google Analytics immer in Produktionsumgebungen.

## `FORCE_UPDATE_URLS`

- **default**—`true`
- **Version**—Adobe Commerce 2.1.4 und höher

Bei der Bereitstellung in Pro- oder Starter-Staging- und Produktionsumgebungen ersetzt diese Variable die Adobe Commerce-Basis-URLs in der Datenbank durch die Projekt-URLs, die durch die [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) Variable angegeben werden. Verwenden Sie diese Einstellung, um das Standardverhalten der Bereitstellungsvariablen [UPDATE_URLS](#update_urls) zu überschreiben, das bei der Bereitstellung in Staging- oder Produktionsumgebungen ignoriert wird.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **default**—`file`
- **Version**—Adobe Commerce 2.2.5 und höher

Der Sperranbieter verhindert den Start doppelter Cron-Aufträge und Cron-Gruppen. Verwenden Sie den `file`-Anbieter in der Produktionsumgebung. Starterumgebungen und die Pro-Integrationsumgebung verwenden nicht die Variable [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md). `ece-tools` wendet daher automatisch den `db` an.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Siehe [Konfigurieren der Sperre](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html?lang=de) im _Installationshandbuch_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **default**—`false`
- **Version**—Adobe Commerce 2.1.4 und höher

>[!TIP]
>
>Die Variable `MYSQL_USE_SLAVE_CONNECTION` wird nur in Adobe Commerce in Cloud-Infrastruktur-Staging- und Produktions-Pro-Cluster-Umgebungen unterstützt und nicht in Starter-Projekten.

Adobe Commerce kann mehrere Datenbanken asynchron lesen. Wenn auf `true` gesetzt, wird automatisch eine _schreibgeschützte)_ zur Datenbank verwendet, um schreibgeschützten Traffic auf einem Nicht-Master-Knoten zu empfangen. Diese Verbindung verbessert die Leistung durch Lastenausgleich, da nur ein Knoten Lese-/Schreib-Traffic verarbeitet. Mit der Einstellung auf `false` entfernen Sie ein vorhandenes schreibgeschütztes Verbindungs-Array aus der `env.php`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Wenn die Variable `MYSQL_USE_SLAVE_CONNECTION` auf `true` festgelegt ist, wird der Parameter `synchronous_replication` in der `true` in der Pro-Staging- und Produktionsumgebung standardmäßig auf `env.php` festgelegt. Wenn der `MYSQL_USE_SLAVE_CONNECTION` auf `false` gesetzt ist, ist der `synchronous_replication` nicht konfiguriert.

## `QUEUE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Verwenden Sie diese Umgebungsvariable, um benutzerdefinierte AMQP-Diensteinstellungen zwischen Bereitstellungen beizubehalten. Wenn Sie beispielsweise lieber einen vorhandenen Message Queue-Service verwenden, anstatt ihn für Sie über die Cloud-Infrastruktur zu erstellen, verwenden Sie die Umgebungsvariable `QUEUE_CONFIGURATION` , um ihn mit Ihrer Site zu verbinden:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

Im folgenden Beispiel werden neue Werte mit einer vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **default**—`Cm_Cache_Backend_Redis`
- **Version**—Adobe Commerce 2.3.0 und höher

Gibt die Backend-Modellkonfiguration für den Redis-Cache an.

Adobe Commerce Version 2.3.0 und höher enthält die folgenden Backend-Modelle:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Das Beispiel zum Festlegen von `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Wenn Sie `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` als Redis-Backend-Modell angeben, um [L2-Cache](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=de) zu aktivieren, generiert `ece-tools` die Cache-Konfiguration automatisch. Ein Beispiel [Konfigurationsdatei](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=de#configuration-example) finden Sie im _Adobe Commerce-Konfigurationshandbuch_. Um die generierte Cache-Konfiguration zu überschreiben, verwenden Sie die Bereitstellungsvariable [CACHE_CONFIGURATION](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **default**—`false`
- **Version**—Adobe Commerce 2.1.16 und höher

>[!WARNING]
>
>Aktivieren _diese_ nicht in einem [skalierten &#x200B;](../architecture/scaled-architecture.md). Dies verursacht Redis-Verbindungsfehler. Redis-Slaves sind immer noch aktiv, werden aber nicht für Redis-Lesevorgänge verwendet. Als Alternative empfiehlt Adobe die Verwendung von Adobe Commerce 2.3.5 oder höher, die Implementierung einer neuen Redis-Backend-Konfiguration und die Implementierung des L2-Caching für Redis.

>[!TIP]
>
>Die Variable `REDIS_USE_SLAVE_CONNECTION` wird nur in Adobe Commerce in Cloud-Infrastruktur-Staging- und Produktions-Pro-Cluster-Umgebungen unterstützt und nicht in Starter-Projekten.

Adobe Commerce kann mehrere Redis-Instanzen asynchron lesen. Wenn auf `true` gesetzt, wird automatisch eine _schreibgeschützte_ Verbindung zu einer Redis-Instanz verwendet, um schreibgeschützten Traffic auf einem Nicht-Master-Knoten zu empfangen. Diese Verbindung verbessert die Leistung durch Lastenausgleich, da nur ein Knoten Lese-/Schreib-Traffic verarbeitet. Mit der Einstellung auf `false` entfernen Sie ein vorhandenes schreibgeschütztes Verbindungs-Array aus der `env.php`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

In der `.magento.app.yaml`-Datei und in der `services.yaml`-Datei muss ein Redis-Dienst konfiguriert sein.

[ECE-Tools Version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) und höher verwendet fehlertolerantere Einstellungen. Wenn Adobe Commerce keine Daten aus der Redis-Instanz _Slave_ lesen kann, liest es Daten aus der Redis-Instanz _Master_.

Die schreibgeschützte Verbindung ist nicht für die Verwendung in der Integrationsumgebung verfügbar oder wenn Sie die Variable [`CACHE_CONFIGURATION` verwenden](#cache_configuration).

## `VALKEY_BACKEND`

- **default**—`Cm_Cache_Backend_Redis`
- **Version**—Adobe Commerce 2.8.0 und höher

`VALKEY_BACKEND` gibt die Backend-Modellkonfiguration für den Valley-Cache an.

Adobe Commerce Version 2.8.0 und höher enthält die folgenden Backend-Modelle:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Im folgenden Beispiel wird beschrieben, wie Sie `VALKEY_BACKEND` festlegen:

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Wenn Sie `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` als Valkey-Backend-Modell angeben, um [L2-Cache](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=de) zu aktivieren, generiert `ece-tools` die Cache-Konfiguration automatisch. Ein Beispiel [Konfigurationsdatei](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=de#configuration-example) finden Sie im _Adobe Commerce-Konfigurationshandbuch_. Um die generierte Cache-Konfiguration zu überschreiben, verwenden Sie die Bereitstellungsvariable [CACHE_CONFIGURATION](#cache_configuration).

## `VALKEY_USE_SLAVE_CONNECTION`

- **default**—`false`
- **Version**—Adobe Commerce 2.4.8 und höher

>[!WARNING]
>
>Aktivieren _diese_ nicht in einem [skalierten &#x200B;](../architecture/scaled-architecture.md). Dies verursacht Valley-Verbindungsfehler. Redis-Slaves sind immer noch aktiv, werden aber nicht für Redis-Lesevorgänge verwendet. Alternativ dazu empfiehlt Adobe die Verwendung von Adobe Commerce 2.4.8 oder höher, die Implementierung einer neuen Valkey-Backend-Konfiguration und die Implementierung von L2-Caching für Valkey.

>[!TIP]
>
>Die Variable `VALKEY_USE_SLAVE_CONNECTION` wird nur in Adobe Commerce in Cloud-Infrastruktur-Staging- und Produktions-Pro-Cluster-Umgebungen unterstützt und nicht in Starter-Projekten.

Adobe Commerce kann mehrere Redis-Instanzen asynchron lesen. `VALKEY_USE_SLAVE_CONNECTION` Auf `true` gesetzt, um automatisch eine _schreibgeschützte_ Verbindung zu einer Redis-Instanz zu verwenden und schreibgeschützten Traffic auf einem Nicht-Master-Knoten zu empfangen. Diese Verbindung verbessert die Leistung durch Lastenausgleich, da nur ein Knoten Lese-/Schreib-Traffic verarbeitet. Legen Sie `VALKEY_USE_SLAVE_CONNECTION` auf `false` fest, um ein vorhandenes schreibgeschütztes Verbindungs-Array aus der `env.php`-Datei zu entfernen.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

In der `.magento.app.yaml`-Datei und in der `services.yaml`-Datei muss ein Redis-Dienst konfiguriert sein.

[ECE-Tools Version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) und höher verwendet fehlertolerantere Einstellungen. Wenn Adobe Commerce keine Daten aus der Valkey-Instanz _Slave_ lesen kann, liest es Daten aus der Redis-Instanz _Master_.

Die schreibgeschützte Verbindung ist nicht für die Verwendung in der Integrationsumgebung verfügbar oder wenn Sie die Variable [`CACHE_CONFIGURATION` verwenden](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Standard** - Nicht festgelegt
- **Version**—Adobe Commerce 2.1.4 und höher

Ordnet einer Datenbankverbindung einen Ressourcennamen zu. Diese Konfiguration entspricht dem `resource` Abschnitt der `env.php`.

{{merge-options}}

Im folgenden Beispiel werden neue Werte mit einer vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **default**—`4`
- **Version**—Adobe Commerce 2.1.4 und höher

Gibt an[&#x200B; welche GZIP](https://www.gnu.org/software/gzip)-Komprimierungsstufe (`0` zu `9`) beim Komprimieren statischer Inhalte verwendet werden soll; `0` deaktiviert die Komprimierung.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **default**—`600`
- **Version**—Adobe Commerce 2.1.4 und höher

Wenn die Zeit, die zum Komprimieren der statischen Assets benötigt wird, das Komprimierungs-Timeout überschreitet, wird der Bereitstellungsprozess unterbrochen. Legen Sie die maximale Ausführungszeit in Sekunden für den Befehl zur Komprimierung statischer Inhalte fest.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Sie können mehrere Gebietsschemata pro Design konfigurieren. Diese Anpassung beschleunigt den Bereitstellungsprozess, indem die Anzahl der unnötigen Design-Dateien reduziert wird. Sie können beispielsweise das Design _magento/backend_ auf Englisch und ein benutzerdefiniertes Design in anderen Sprachen bereitstellen.

Im folgenden Beispiel wird das `Magento/backend`-Design mit drei Gebietsschemata bereitgestellt:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Sie können auch wählen, _ein Design bereitzustellen_ nicht):

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.2.0 und höher

Ermöglicht die Verlängerung der maximal erwarteten Ausführungszeit für die Bereitstellung statischer Inhalte.

Standardmäßig legt Adobe Commerce die maximal erwartete Ausführung auf 900 Sekunden fest. In einigen Szenarien benötigen Sie jedoch möglicherweise mehr Zeit, um die statische Inhaltsbereitstellung für ein Cloud-Projekt abzuschließen.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **default**—`false`
- **Version**—Adobe Commerce 2.4.2 und höher

Stellen Sie in der Bereitstellungsphase `SCD_NO_PARENT: true` so ein, dass während der Bereitstellungsphase keine statischen Inhalte für die übergeordneten Designs generiert werden. Diese Einstellung minimiert die Bereitstellungszeit und verhindert Website-Ausfallzeiten, die auftreten können, wenn der statische Inhaltserstellungsvorgang während der Bereitstellung fehlschlägt. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **default**—`quick`
- **Version**—Adobe Commerce 2.2.0 und höher

Ermöglicht die Anpassung der [-Bereitstellungsstrategie &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=de) statische Inhalte. Siehe [Bereitstellen von statischen Ansichtsdateien](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=de).

Verwenden Sie diese Optionen _nur_ wenn Sie mehr als ein Gebietsschema haben:

- `standard`: Stellt alle statischen Ansichtsdateien für alle Pakete bereit.
- `quick` - (_Standard_) minimiert die Bereitstellungszeit.
- `compact` - Spart Speicherplatz auf dem Server. In Adobe Commerce Version 2.2.4 und früher überschreibt diese Einstellung den Wert für `scd_threads` mit dem Wert `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **default**—automatic
- **Version**—Adobe Commerce 2.1.4 und höher

Legt die Anzahl der Threads für die Bereitstellung statischer Inhalte fest. Der Standardwert wird anhand der erkannten CPU-Thread-Anzahl festgelegt und überschreitet den Wert 4 nicht. Eine Erhöhung der Thread-Anzahl beschleunigt die Bereitstellung statischer Inhalte; eine Verringerung der Thread-Anzahl verlangsamt die Bereitstellung. Sie können den Thread-Wert festlegen, z. B.:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Um die Bereitstellungszeit weiter zu verkürzen, verwenden [Konfigurationsverwaltung](../store/store-settings.md) mit dem Befehl `scd-dump` , um die statische Bereitstellung in die Build-Phase zu verschieben.

## `SEARCH_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Verwenden Sie diese Umgebungsvariable, um benutzerdefinierte Suchdiensteinstellungen zwischen Bereitstellungen beizubehalten. Beispiel:

Elasticsearch-Konfiguration:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

OpenSearch-Konfiguration (für Commerce 2.4.6 und höher):

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

Im folgenden Beispiel wird ein neuer Wert mit der vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Konfigurieren Sie den Redis-Sitzungsspeicher. Erfordert die Optionen `save`, `redis`, `host`, `port` und `database` für die Sitzungsspeichervariable. Beispiel:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

Im folgenden Beispiel wird ein neuer Wert mit der vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Standard**— _Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Wenn auf `true` gesetzt, wird die Bereitstellung statischer Inhalte während der Bereitstellungsphase übersprungen.

Stellen Sie in der Bereitstellungsphase `SKIP_SCD: true` so ein, dass der statische Inhaltserstellungsvorgang während der Bereitstellungsphase nicht ausgeführt wird. Diese Einstellung minimiert die Bereitstellungszeit und verhindert Website-Ausfallzeiten, die auftreten können, wenn der statische Inhaltserstellungsvorgang während der Bereitstellung fehlschlägt. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **default**—`true`
- **Version**—Adobe Commerce 2.1.4 und höher

Ersetzen Sie bei der Bereitstellung die Adobe Commerce-Basis-URLs in der Datenbank durch die von der [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) angegebenen Projekt-URLs. Diese Konfiguration ist für die lokale Entwicklung nützlich, bei der Basis-URLs für Ihre lokale Umgebung eingerichtet werden. Bei der Bereitstellung in einer Cloud-Umgebung werden die URLs aktualisiert, damit Sie über die Projekt-URLs auf Ihre Storefront und Ihren Admin zugreifen können.

Wenn Sie URLs bei der Bereitstellung in Pro- oder Starter-Staging- und Produktionsumgebungen aktualisieren müssen, verwenden Sie die Variable [`FORCE_UPDATE_URLS`](#force_update_urls) .

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Aktivieren oder deaktivieren Sie die [Symfony](https://symfony.com/doc/current/console/verbosity.html) Debug-Ausführlichkeitsstufe für `bin/magento` CLI-Befehle, die während der Bereitstellungsphase ausgeführt werden.

>[!NOTE]
>
>Um mithilfe der VERBOSE_COMMANDS-Einstellung die Details in der Befehlsausgabe sowohl für erfolgreiche als auch für fehlgeschlagene `bin/magento` CLI-Befehle zu steuern, müssen Sie [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug` festlegen.

Wählen Sie den Detaillierungsgrad der Protokolle aus:

- `-v`= Normalausgabe
- `-vv`= Ausführlichere Ausgabe
- `-vvv` = ausführliche Ausgabe Ideal für die Fehlersuche

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
