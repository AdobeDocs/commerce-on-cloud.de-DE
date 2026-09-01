---
title: Variablen bereitstellen
description: Siehe die Liste der Umgebungsvariablen, die Aktionen in der Bereitstellungsphase der Adobe Commerce in der Cloud-Infrastruktur steuern.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
TQID: https://experienceleague.adobe.com/TNuUxXzCiXnKefww0DmKbjfJygEz2HFG-0PjCsCy2nA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: bdc2bedd2696e7dde0ffb55f846a8bced2dbd25d
workflow-type: tm+mt
source-wordcount: 3106
ht-degree: 0%

---

# Variablen bereitstellen

Die folgenden _deploy_-Variablen steuern Aktionen in der Bereitstellungsphase und können Werte von den (globalen [) ](variables-global.md) und überschreiben. Fügen Sie diese Variablen in den `deploy` Schritt der `.magento.env.yaml` ein:

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

Verwenden Sie `CACHE_CONFIGURATION`, um während der Bereitstellung generierte Cache-Frontend- und -Backend-Optionen zusammenzuführen oder zu überschreiben.

Bearbeiten Sie für Adobe Commerce in der Cloud-Infrastruktur `app/etc/env.php` nicht direkt. Das `ece-tools`-Paket generiert die Bereitstellungskonfiguration aus `.magento.env.yaml`, Service-Beziehungen und unterstützten Bereitstellungsvariablen.

Verwenden Sie `VALKEY_BACKEND` oder `REDIS_BACKEND`, um den unterstützten Cache oder die L2-Implementierung für die exakte Adobe Commerce-Version auszuwählen. Verwenden Sie `CACHE_CONFIGURATION`, um Optionen wie Verbindungsversuche, Lesetimeouts, Cache-Präfixe oder Vorabladeschlüssel anzupassen.

Die unterstützte Kombination aus Backend und Cache-Service hängt von der Commerce-Version und der Patch-Ebene ab. Redis wird für Adobe Commerce 2.4.9 oder für Patch-Versionen nach 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 und 2.4.8-p4 nicht unterstützt. Verwenden Sie Valkey für Versionen, [ die (](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)) dies erfordern.

>[!NOTE]
>
>Weitere Konfigurationsanleitungen für Redis- und Valkey-Services finden Sie unter [Best Practices für die Konfiguration von Valkey- und Redis-Services](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)

Standardmäßig überschreibt der Bereitstellungsprozess die entsprechende Cache-Konfiguration. Um die angegebenen Werte mit der generierten Konfiguration zusammenzuführen, setzen Sie `_merge` auf `true`:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            connect_retries: 3
          remote_backend_options:
            read_timeout: 10
```

Um die vorhandene Konfiguration durch die in `CACHE_CONFIGURATION` angegebenen Werte zu ersetzen, setzen Sie `_merge` auf `false`.

>[!IMPORTANT]
>
> Kopieren Sie lokale `bin/magento setup:config:set`, wie z. B. `cm_cache_backend_redis`, nicht direkt in `CACHE_CONFIGURATION`. In Cloud-Projekten ruft `ece-tools` Details zur Service-Verbindung aus den konfigurierten Beziehungen ab. Verwenden Sie die dokumentierte Struktur für die ausgewählte Commerce-Release- und Cache-Implementierung.

Im folgenden Beispiel werden Datenbankzuweisungen in einer vorhandenen Cache-Konfiguration zusammengeführt. Verwenden Sie diese Art der Überschreibung nur, wenn das ausgewählte Backend und die Commerce-Version sie unterstützen. Wenden Sie die Frontend-Einstellungen nur dann auf `symfony_l2` an, wenn die aktuelle Symfony L2-Dokumentation die Option ausdrücklich unterstützt.

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

Im folgenden Beispiel wird die [Redis-Vorabladefunktion](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache#redis-preload-feature) wie im _Konfigurationshandbuch“_. Verwenden Sie die entsprechenden Valkey-Anleitungen für Versionen, die Valkey verwenden.

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

Um ein benutzerdefiniertes [REDIS_BACKEND](#redis_backend)-Modell zu verwenden, das nicht in der Zulassungsliste enthalten ist, legen Sie `_custom_redis_backend` auf `true` fest, sodass ec-tools die entsprechende Validierung anwendet:

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

Aktiviert oder deaktiviert [ Bereinigung von (statischen ](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment)), die während der Build- oder Bereitstellungsphase generiert wurde. Verwenden Sie den Standardwert _true_ in der Entwicklungsumgebung als Best Practice.

- **`true`** - Entfernt alle vorhandenen statischen Inhalte, bevor der aktualisierte statische Inhalt bereitgestellt wird.
- **`false`** - Die Bereitstellung überschreibt nur vorhandene statische Inhaltsdateien, wenn der generierte Inhalt eine neuere Version enthält.

Wenn Sie statische Inhalte über einen separaten Prozess ändern, setzen Sie den Wert auf _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Wenn statische Ansichtsdateien vor der Bereitstellung nicht bereinigt werden, können Probleme auftreten, wenn Sie Aktualisierungen für vorhandene Dateien bereitstellen, ohne die vorherigen Versionen zu entfernen. Aufgrund von [statischen Datei-Fallback](https://developer.adobe.com/commerce/frontend-core/guide/css/preprocess#clean-static-view-files)-Regeln können Fallback-Vorgänge die falsche Datei anzeigen, wenn das Verzeichnis mehrere Versionen derselben Datei enthält.

## `CRON_CONSUMERS_RUNNER`

- **Standard**—`cron_run = false`, `max_messages = 1000`

Verwenden Sie diese Umgebungsvariable, um zu bestätigen, dass Nachrichtenwarteschlangen nach einer Bereitstellung ausgeführt werden.

- `cron_run` - Ein boolescher Wert, der den `consumers_runner` Cron-Auftrag aktiviert oder deaktiviert. Der Standardwert lautet `false`.
- `max_messages` - Die maximale Anzahl von Nachrichten, die jeder Verbraucher vor dem Beenden verarbeitet. Der Standardwert lautet `1000`. Um zu verhindern, dass der Verbraucher beendet wird, setzen Sie ihn auf `0`.
- `consumers` - Ein Array von Zeichenfolgen, das die Namen der auszuführenden Verbraucher angibt. Ein leeres Array führt &quot;_&quot;_.
- `multiple_processes`-Die Anzahl der Prozesse, die für jeden Verbraucher erzeugt werden sollen. Diese Option wird ab Adobe Commerce 2.4.4 unterstützt.

>[!NOTE]
>
>Um die verfügbaren Nachrichtenwarteschlangen-Benutzer aufzulisten, führen Sie den Befehl `./bin/magento queue:consumers:list` in der Remote-Umgebung aus.

Im folgenden Beispiel werden ausgewählte Verbraucher ausgeführt und für jeden Prozess werden mehrere Prozesse gestartet:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
       example_consumer_1
       example_consumer_2
      multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Im folgenden Beispiel werden alle Verbraucher ausgeführt:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Standardmäßig überschreibt der Bereitstellungsprozess die entsprechenden Einstellungen in der `env.php`. Siehe [Verwalten von Nachrichtenwarteschlangen](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues) im _Commerce-Konfigurationshandbuch_ für lokale Adobe Commerce.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **default**—`false`

Konfigurieren Sie `consumers` Verarbeitung von Nachrichten aus der Nachrichtenwarteschlange, indem Sie eine der folgenden Optionen auswählen:

- `false` - `Consumers` verfügbare Nachrichten zu verarbeiten, die TCP-Verbindung zu schließen und unabhängig von der in der `CRON_CONSUMERS_RUNNER`-Bereitstellungsvariablen angegebenen `max_messages` zu beenden.

- `true` - `Consumers` weiter Nachrichten aus der Nachrichtenwarteschlange verarbeiten, bis die in der Variablen &quot;`CRON_CONSUMERS_RUNNER` Deploy“ angegebene maximale Anzahl an Nachrichten (`max_messages`) erreicht ist, bevor die TCP-Verbindung geschlossen und der Consumerprozess beendet wird. Wenn die Warteschlange geleert wird, bevor `max_messages` erreicht wird, wartet der -Benutzer, bis weitere Nachrichten eingehen.

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

>[!WARNING]
>
>Um zu vermeiden, dass der Schlüssel im Quell-Code-Repository verfügbar gemacht wird, legen Sie den `CRYPT_KEY`-Wert über die [!DNL Cloud Console] anstelle der `.magento.env.yaml` fest. Siehe [Festlegen von Umgebungs- und ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/overview#configure-environment).

Wenn Sie die Datenbank ohne Installationsvorgang von einer Umgebung in eine andere verschieben, benötigen Sie die entsprechenden kryptografischen Informationen. Adobe Commerce verwendet den im [!DNL Cloud Console] festgelegten Verschlüsselungsschlüsselwert als `crypt/key` Wert in der `env.php`.

## `DATABASE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_

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
>In einem Pro-Staging-/Produktions-Cluster mit drei Knoten (oder drei Service-Knoten in [Scaled Architecture](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)) sollte die `indices_settings` wie folgt festgelegt werden:
>
>```yaml
>           indices_settings:
>               number_of_shards: 1
>               number_of_replicas: 2
>```

{{merge-options}}

Im folgenden Beispiel wird ein neuer Wert mit der vorhandenen Konfiguration zusammengeführt:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 1
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

Aktiviert und deaktiviert Google Analytics bei der Bereitstellung in Staging- und Integrationsumgebungen. Standardmäßig gilt Google Analytics nur für die Produktionsumgebung. Um Google Analytics in den Staging- und Integrationsumgebungen zu aktivieren, setzen Sie diesen Wert auf `true`.

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

Bei der Bereitstellung in Pro- oder Starter-Staging- und Produktionsumgebungen ersetzt diese Variable die Adobe Commerce-Basis-URLs in der Datenbank durch die Projekt-URLs, die durch die [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) Variable angegeben werden. Verwenden Sie diese Einstellung, um das Standardverhalten der [UPDATE_URLS](#update_urls) Bereitstellungsvariable zu überschreiben.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Standard**- In Produktions- und Staging-Umgebungen ist der Standardwert `file` und kann nicht geändert werden. Für Pro-Integrations- und Starter-Umgebungen ist standardmäßig `db` festgelegt.

Der Sperranbieter verhindert, dass doppelte Cron-Aufträge und Cron-Gruppen ausgeführt werden. Adobe Commerce on Cloud unterstützt die Anbieter von `file`- und `db`.

In Pro-Staging- und Produktionsumgebungen konfiguriert `MAGENTO_CLOUD_LOCKS_DIR` den `file`. Diese Einstellung kann nicht überschrieben werden. In den Umgebungen Pro Integration und Starter legt `ece-tools` standardmäßig den `db` fest. Um die lokale Leistung zu optimieren und die Produktionsarchitektur zu spiegeln, legen Sie den Anbieter auf `file` in diesen Umgebungen fest.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: 'file'
```

## `MYSQL_USE_SLAVE_CONNECTION`

- **default**—`false`

>[!TIP]
>
>Die Variable `MYSQL_USE_SLAVE_CONNECTION` wird nur auf Adobe Commerce auf Cloud-Infrastruktur-Staging- und Produktions-Pro-Clustern unterstützt. Sie wird nicht für Starter-Projekte unterstützt.

Adobe Commerce kann mehrere Datenbanken asynchron lesen. Wenn auf `true` gesetzt, wird automatisch eine _schreibgeschützte_ Verbindung zur Datenbank verwendet, um schreibgeschützten Traffic auf einem Nicht-Master-Knoten zu empfangen. Diese Verbindung verbessert die Leistung durch Lastenausgleich, da nur ein Knoten Lese-/Schreib-Traffic verarbeitet. Um ein vorhandenes schreibgeschütztes Verbindungs-Array aus der `env.php`-Datei zu entfernen, setzen Sie auf `false`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Wenn die Variable `MYSQL_USE_SLAVE_CONNECTION` auf `true` festgelegt ist, legt das System den `synchronous_replication`-Parameter in der `env.php`-Datei in Pro-Staging- und Produktionsumgebungen standardmäßig auf `true` fest. Wenn der `MYSQL_USE_SLAVE_CONNECTION` auf `false` gesetzt ist, ist der `synchronous_replication` nicht konfiguriert.

## `QUEUE_CONFIGURATION`

- **Standard**—_Nicht festgelegt_

Verwenden Sie diese Umgebungsvariable, um benutzerdefinierte Warteschlangendiensteinstellungen zwischen Bereitstellungen beizubehalten. Diese Variable unterstützt sowohl AMQP (für RabbitMQ) als auch STOMP (für ActiveMQ Artemis). Wenn Sie beispielsweise lieber einen vorhandenen Message Queue-Service verwenden, anstatt ihn für Sie über die Cloud-Infrastruktur zu erstellen, verwenden Sie die Umgebungsvariable `QUEUE_CONFIGURATION` , um ihn mit Ihrer Site zu verbinden:

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

Für ActiveMQ Artemis mit STOMP-Protokoll:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      stomp:
        host: activemq.host
        port: 61616
        user: username
        password: password
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

Gibt die Backend-Modellkonfiguration für den Redis-Cache an.

Redis-Cache wird für Adobe Commerce 2.4.9 oder für Patch-Versionen nach 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 und 2.4.8-p4 nicht unterstützt. Verwenden Sie für diese Versionen Valkey und die entsprechende `VALKEY_BACKEND`. Überprüfen Sie immer den unterstützten Cache-Service in [Systemanforderungen](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements).

Für Redis-unterstützte Versionen umfassen die verfügbaren Backend-Modelle:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Das folgende Beispiel aktiviert das remote-synchronisierte Cache-Backend und den L2-Cache:

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
> Wenn `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` ausgewählt ist, generiert `ece-tools` automatisch die L2-Cache-Konfiguration. Um die generierte Konfiguration anzupassen, verwenden Sie [`CACHE_CONFIGURATION`](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **default**—`false`

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION` wird nur auf Adobe Commerce auf Cloud-Staging- und Produktions-Pro-Clustern unterstützt. Sie wird nicht für Starter-Projekte unterstützt.

Adobe Commerce kann mehrere Redis-Instanzen asynchron lesen. Legen Sie diese Variable auf `true` fest, um eine schreibgeschützte Verbindung zu einem Redis-Replikat für Lese-Traffic zu verwenden, während die primäre Instanz Lese-/Schreib-Traffic verarbeitet. Um ein vorhandenes schreibgeschütztes Verbindungs-Array aus `env.php` zu entfernen, setzen Sie es auf `false`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

In den `.magento.app.yaml`- und `services.yaml`-Dateien muss ein [Redis](../services/redis.md)Dienst konfiguriert sein.

[ECE-Tools Version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) und höher verwendet fehlertolerantere Einstellungen. Wenn Adobe Commerce keine Daten aus dem Redis-Replikat lesen kann, wird auf die primäre Redis-Instanz zurückgegriffen.

Die schreibgeschützte Verbindung ist in der Integrationsumgebung nicht verfügbar. Wenn Sie [`CACHE_CONFIGURATION`](#cache_configuration) verwenden, führen Sie Änderungen in der generierten Konfiguration zusammen und stellen Sie sicher, dass die resultierende Konfiguration die Replikatverbindung beibehält.

## `VALKEY_BACKEND`

- **default**—`Cm_Cache_Backend_Redis`
- **Version** - Adobe Commerce-Versionen, die Valkey unterstützen

`VALKEY_BACKEND` gibt das Backend-Modell für die Valley-Cache-Konfiguration an. Der Standardwert verwendet einen alten Redis-kompatiblen Klassennamen. Dies bedeutet nicht, dass der Service Redis sein muss.

Bei Adobe Commerce-Versionen vor 2.4.9, die Valkey unterstützen, umfassen die Backend-Modelle Folgendes:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Adobe Commerce 2.4.9 und höher unterstützt auch `symfony_l2`, die Symfony Cache-basierte L2-Implementierung. `symfony_l2` wird nur mit Valkey unterstützt.

### Konfigurieren des synchronisierten Remote-Cache

Verwenden Sie für Adobe Commerce 2.4.8 die folgende Konfiguration, wenn die Implementierung des remote synchronisierten Caches angemessen ist:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

Durch die Angabe des remote synchronisierten Backends wird der L2-Cache aktiviert, und `ece-tools` generiert die Cache-Konfiguration automatisch. Siehe die [Beispielkonfigurationsdatei](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#customize-the-symfony-l2-cache-configuration). Um die generierte Konfiguration anzupassen, verwenden Sie [`CACHE_CONFIGURATION`](#cache_configuration).

### Moderne Symfony L2-Cache-Implementierung konfigurieren

Verwenden Sie für Adobe Commerce 2.4.9 und höher die Symfony L2-Implementierung:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: 'symfony_l2'
```

Wenn Sie `symfony_l2` als Valkey-Backend-Modell angeben, wird der L2-Cache aktiviert, und `ece-tools` generiert die L2-Cache-Konfiguration automatisch aus Ihren Valkey-Service-Verbindungsdetails, einschließlich der `default`- und `stale_cache_enabled`-Frontends. Definieren Sie `CACHE_CONFIGURATION` nur, wenn Sie unterstützte Backend-Optionen anpassen müssen, z. B. das lokale Cache-Verzeichnis. Siehe [Symfony L2-Cache](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#configure-symfony-l2-cache){target="_blank"}Implementierung im _Adobe Commerce-Konfigurationshandbuch_.

>[!NOTE]
>
>Adobe Commerce 2.4.9 umfasst Verbesserungen am Symfony L2-Cache, einschließlich Cache-Tag-Speicherung, Invalidierung und Komprimierung, mit Patch ACP2E-5132, wodurch Datenträger-E/A reduziert, veraltete Cache-Einträge vermieden und Speicher- und Netzwerk-Overhead reduziert werden.

## `VALKEY_USE_SLAVE_CONNECTION`

- **default**—`false`
- **Version**—Adobe Commerce 2.4.8 und höher

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION` wird nur auf Adobe Commerce auf Cloud-Staging- und Produktions-Pro-Clustern unterstützt. Sie wird nicht für Starter-Projekte unterstützt.

Adobe Commerce kann mehrere Valley-Instanzen asynchron lesen. Legen Sie `VALKEY_USE_SLAVE_CONNECTION` auf `true` fest, um eine _schreibgeschützte_ Verbindung zu einer Valley-Replikation für schreibgeschützten Traffic zu verwenden, während die primäre Instanz Lese-/Schreibverkehr verarbeitet. Diese Verbindung verbessert die Leistung durch Lastenausgleich, da nur ein Knoten Lese-/Schreib-Traffic verarbeitet. Um ein vorhandenes schreibgeschütztes Verbindungs-Array aus `env.php` zu entfernen, setzen Sie es auf `false`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Sie müssen einen [Valkey-Service](../services/valkey.md) in `.magento.app.yaml` und `.magento/services.yaml` konfiguriert haben. Ob eine Replikatverbindung verfügbar ist, hängt von der Projekttopologie und der installierten `ece-tools` ab.

Bevor Sie sich auf diese Einstellung verlassen, überprüfen Sie den decodierten `MAGENTO_CLOUD_RELATIONSHIPS` und stellen Sie sicher, dass eine Replikationsbeziehung vorhanden ist. Beispiel:

```bash
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Für die Replikatunterstützung sind `symfony_l2` die entsprechenden `ece-tools`- und Cloud-Patches-Aktualisierungen erforderlich. Aktualisieren Sie auf die neueste `ece-tools` Version, bevor Sie diese Einstellung aktivieren. Wenn nach der erneuten Bereitstellung keine Replikationsbeziehung vorhanden ist, wenden Sie sich an den Adobe Commerce-Support.

Bei Verwendung von [`CACHE_CONFIGURATION`](#cache_configuration) werden unterstützte Überschreibungen mit der generierten Konfiguration zusammengeführt, anstatt die generierte Verbindungsstruktur zu ersetzen.

## `RESOURCE_CONFIGURATION`

- **Standard** - Nicht festgelegt

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

Gibt an[ welche ](https://www.gnu.org/software/gzip) (`0` bis `9`) beim Komprimieren statischer Inhalte verwendet werden soll. Setzen Sie ihn auf `0`, um die Komprimierung zu deaktivieren.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **default**—`600`

Wenn die Zeit, die zum Komprimieren der statischen Assets benötigt wird, das Komprimierungs-Timeout überschreitet, wird der Bereitstellungsprozess unterbrochen. Legen Sie die maximale Ausführungszeit in Sekunden für den Befehl zur Komprimierung statischer Inhalte fest.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Standard**—_Nicht festgelegt_

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

Ermöglicht die Verlängerung der maximal erwarteten Ausführungszeit für die Bereitstellung statischer Inhalte.

Standardmäßig legt Adobe Commerce die maximal erwartete Ausführung auf 900 Sekunden fest. In einigen Szenarien ist jedoch mehr Zeit erforderlich, um die statische Inhaltsbereitstellung für ein Cloud-Projekt abzuschließen.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **default**—`false`

Stellen Sie in der Bereitstellungsphase `SCD_NO_PARENT: true` so ein, dass während der Bereitstellungsphase keine statischen Inhalte für die übergeordneten Designs generiert werden. Diese Einstellung minimiert die Bereitstellungszeit und verhindert Website-Ausfallzeiten, die auftreten können, wenn der statische Inhaltserstellungsvorgang während der Bereitstellung fehlschlägt. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **default**—`quick`

Ermöglicht die Anpassung der [-Bereitstellungsstrategie ](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy) statische Inhalte. Siehe [Bereitstellen von statischen Ansichtsdateien](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment).

Verwenden Sie diese Optionen _nur_ wenn Sie mehr als ein Gebietsschema haben:

- `standard`: Stellt alle statischen Ansichtsdateien für alle Pakete bereit.
- `quick` - (_Standard_) minimiert die Bereitstellungszeit.
- `compact` - Spart Speicherplatz auf dem Server.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **default**—automatic

Legt die Anzahl der Threads für die Bereitstellung statischer Inhalte fest. Der Standardwert wird anhand der erkannten CPU-Thread-Anzahl festgelegt und überschreitet den Wert 4 nicht. Eine Erhöhung der Thread-Anzahl beschleunigt die Bereitstellung statischer Inhalte. Die Verringerung der Thread-Anzahl verlangsamt die Geschwindigkeit. Sie können den Thread-Wert festlegen, z. B.:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Um die Bereitstellungszeit weiter zu verkürzen, verwenden [Konfigurationsverwaltung](../store/store-settings.md) mit dem Befehl `scd-dump` , um die statische Bereitstellung in die Build-Phase zu verschieben.

## `SEARCH_CONFIGURATION`

- **Standard**—_Nicht festgelegt_

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

Verwenden Sie `SESSION_CONFIGURATION`, um den Sitzungsspeicher zu konfigurieren. Im folgenden Beispiel wird die Redis-kompatible Sitzungskonfigurationsstruktur verwendet. Verwenden Sie sie nur mit der Kombination aus Sitzungsspeicherbenennung und Service , die von der Commerce-Version unterstützt wird. Befolgen Sie für Valkey-unterstützte Sitzungen das [Beispiel für Valkey-Sitzungsspeicher](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#apply-all-best-practice-recommendations).

Gehen Sie nicht davon aus, dass Cache-Variablen wie `VALKEY_BACKEND` oder `REDIS_BACKEND` Sitzungen konfigurieren. Cache- und Sitzungskonfiguration sind unabhängig. Verwenden Sie in Cloud-Projekten nach Möglichkeit die Service-Beziehung und die generierte Konfiguration. programmieren Sie keine umgebungsspezifischen Werte, ohne den Beispiel-Host und -Port zu ersetzen.

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: 'redis.internal'
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

Ersetzen Sie `redis.internal` und `6379` durch den Sitzungs-Service-Host und -Port für die Zielumgebung, wenn die Bereitstellungskonfiguration explizite Verbindungsdetails erfordert.

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

Wenn auf `true` gesetzt, wird die Bereitstellung statischer Inhalte während der Bereitstellungsphase übersprungen.

Stellen Sie in der Bereitstellungsphase `SKIP_SCD: true` so ein, dass der statische Inhaltserstellungsvorgang während der Bereitstellungsphase nicht ausgeführt wird. Diese Einstellung minimiert die Bereitstellungszeit und verhindert Website-Ausfallzeiten, die auftreten können, wenn der statische Inhaltserstellungsvorgang während der Bereitstellung fehlschlägt. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **default**—`true`

Ersetzen Sie bei der Bereitstellung die Adobe Commerce-Basis-URLs in der Datenbank durch die von der [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) angegebenen Projekt-URLs. Diese Konfiguration ist für die lokale Entwicklung nützlich, bei der Basis-URLs für Ihre lokale Umgebung eingerichtet werden. Bei der Bereitstellung in einer Cloud-Umgebung werden die URLs aktualisiert, damit Sie über die Projekt-URLs auf Ihre Storefront und Ihren Admin zugreifen können.

Wenn Sie URLs bei der Bereitstellung in Pro- oder Starter-Staging- und Produktionsumgebungen aktualisieren müssen, verwenden Sie die Variable [`FORCE_UPDATE_URLS`](#force_update_urls) .

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `USE_LUA`

- **default**—`false`
- **Version**—Adobe Commerce 2.4.7 und höher

Steuert die `use_lua` Cache-Backend-Option in `env.php` für das standardmäßige Cache-Frontend (und bei Verwendung des `symfony_l2` Backends die Remote-Backend-Optionen des `stale_cache_enabled`-Frontends). Diese Option wird nicht auf das `page_cache` Frontend angewendet.

Verwenden Sie den Standardwert `false`, es sei denn, die Adobe-Unterstützung weist explizit eine andere Anweisung aus.

```yaml
stage:
  deploy:
    USE_LUA: false
```

>[!WARNING]
>
>Unter Adobe Commerce 2.4.7 und 2.4.8 kann das Festlegen von `USE_LUA: true` zu Cache-Beschädigungen und Problemen mit GraphQL-Cache-Fehlern führen.
>
>Verwenden Sie ab Adobe Commerce 2.4.9 die Valkey Cache-Konfigurationsanleitung für Ihre Commerce-Version und verlassen Sie sich bei neuen Bereitstellungen nicht auf `USE_LUA`.

## `LUA_KEY`

Die `LUA_KEY` ist veraltet. Wenn `LUA_KEY` in `.magento.env.yaml` enthalten ist, entfernen Sie es während der Migration. Verwenden Sie stattdessen die Variablen `USE_LUA` und `USE_LUA_ON_GC` .

## `USE_LUA_ON_GC`

- **default**—`true`
- **Version**—Adobe Commerce 2.4.8 und höher

Steuert die `use_lua_on_gc` Cache-Backend-Option in `env.php` für das standardmäßige Cache-Frontend (und, bei Verwendung des `symfony_l2` Backends, die Remote-Backend-Optionen des `stale_cache_enabled`-Frontends) für die Speicherbereinigung. Diese Option wird nicht auf das `page_cache` Frontend angewendet.

Verwenden Sie den Standardwert `true` , um die atomare Cache-Tag-Bereinigung während des `backend_clean_cache` Cron-Auftrags beizubehalten.

```yaml
stage:
  deploy:
    USE_LUA_ON_GC: true
```

>[!WARNING]
>
>Unter Adobe Commerce 2.4.8 kann das Setzen von `USE_LUA_ON_GC: false` dazu führen, dass die Tag-basierte Cache-Invalidierung im Hintergrund fehlschlägt und eine vollständige Cache-Leerung erforderlich ist, um wiederhergestellt zu werden.
>
>Befolgen Sie unter 2.4.9 und höher die [Anleitung zum Cache-](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache)) für Ihre installierte Version.

## `VERBOSE_COMMANDS`

- **Standard**—_Nicht festgelegt_

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
