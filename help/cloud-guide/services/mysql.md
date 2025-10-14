---
title: Einrichten des MySQL-Service
description: Erfahren Sie, wie Sie den MySQL-Service für die persistente Datenspeicherung mit Adobe Commerce in der Cloud-Infrastruktur verwalten.
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Einrichten des MySQL-Service

Der `mysql`-Service bietet eine persistente Datenspeicherung basierend auf den [MariaDB](https://mariadb.com/)-Versionen 10.2 bis 10.4, unterstützt die [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html)-Speicher-Engine und implementierte Funktionen aus MySQL 5.6 und 5.7.

Die Neuindizierung auf MariaDB 10.4 dauert im Vergleich zu anderen MariaDB- oder MySQL-Versionen länger. Siehe [Indexer](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=de#indexers) im Handbuch _Best Practices für die_&quot;.

>[!WARNING]
>
>Seien Sie beim Upgrade von MariaDB von Version 10.1 auf 10.2 vorsichtig. MariaDB 10.1 ist die letzte Version, die _XtraDB_ als Speicher-Engine unterstützt. MariaDB 10.2 verwendet _InnoDB_ für die Speicher-Engine. Nach dem Upgrade von 10.1 auf 10.2 können Sie die Änderung nicht mehr zurücksetzen. Adobe Commerce unterstützt beide Speicher-Engines. Sie müssen jedoch Erweiterungen und andere von Ihrem Projekt verwendete Systeme überprüfen, um sicherzustellen, dass sie mit MariaDB 10.2 kompatibel sind. Siehe [Inkompatible Änderungen zwischen 10.1 und 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**So aktivieren Sie MySQL**:

1. Fügen Sie den erforderlichen Namen, den erforderlichen Typ und den erforderlichen Datenträgerwert (in MB) zur `.magento/services.yaml` hinzu.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >MySQL-Fehler, wie z. B. `PDO Exception: MySQL server has gone away`, können aufgrund von ungenügendem Speicherplatz auftreten. Stellen Sie sicher, dass dem Service in der [`.magento/services.yaml`](services-yaml.md#disk) genügend Speicherplatz zugewiesen wurde.

1. Konfigurieren Sie die Beziehungen in der `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Überprüfen Sie die Service-Beziehungen](services-yaml.md#service-relationships).

{{service-change-tip}}

## MySQL-Datenbank konfigurieren

Sie haben beim Konfigurieren der MySQL-Datenbank die folgenden Optionen:

- **`schemas`** - Ein Schema definiert eine Datenbank. Das Standardschema ist die `main`.
- **`endpoints`** - Jeder Endpunkt stellt eine Berechtigung mit bestimmten Berechtigungen dar. Der standardmäßige Endpunkt ist `mysql`, der `admin` Zugriff auf die `main`-Datenbank hat.
- **`properties`** - Eigenschaften werden zum Definieren zusätzlicher Datenbankkonfigurationen verwendet.

Im Folgenden finden Sie eine einfache Beispielkonfiguration in der `.magento/services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

Mit dem `properties` im obigen Beispiel werden die standardmäßigen `optimizer` wie [&#x200B; im Handbuch für Best Practices für die Leistung &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=de#indexers).

**MariaDB-Konfigurationsoptionen**:

| Optionen | Beschreibung | Standardwert |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Der Standardzeichensatz. | UTF8MB4 |
| `default_collation` | Die Standardsortierung. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Maximale Größe für Pakete in MB. Bereich `1` bis `100`. | 16 |
| `optimizer_switch` | Legen Sie Werte für den Abfrage-Optimizer fest. Siehe [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Wählen Sie die vom Optimizer verwendeten Statistiken aus. Bereich `1` bis `5`. Siehe [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 für 10.4 und höher |

### Einrichten mehrerer Datenbankbenutzer

Optional können Sie mehrere Benutzer mit unterschiedlichen Berechtigungen für den Zugriff auf die `main`-Datenbank einrichten.

Standardmäßig gibt es einen Endpunkt mit dem Namen `mysql`, der Administratorzugriff auf die Datenbank hat. Um mehrere Datenbankbenutzer einzurichten, müssen Sie mehrere Endpunkte in der `services.yaml` definieren und die Beziehungen in der `.magento.app.yaml` deklarieren. Für Pro-Staging- und Produktionsumgebungen [Senden Sie ein Adobe Commerce-Support-Ticket](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um den zusätzlichen Benutzer anzufordern.

Verwenden Sie ein verschachteltes -Array, um die Endpunkte für einen bestimmten Benutzerzugriff zu definieren. Jeder Endpunkt kann Zugriff auf ein oder mehrere Schemas (Datenbanken) und verschiedene Berechtigungsebenen für jedes Schema zuweisen.

Die gültigen Berechtigungsebenen sind:

- `ro`: Nur SELECT-Abfragen sind zulässig.
- `rw`: SELECT-Abfragen und INSERT-, UPDATE- und DELETE-Abfragen sind zulässig.
- `admin`: Alle Abfragen sind zulässig, einschließlich DDL-Abfragen (CREATE TABLE, DROP TABLE und mehr).

Beispiel:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

Im vorherigen Beispiel bietet der `admin`-Endpunkt Zugriff auf die `main`-Datenbank auf Admin-Ebene, der `reporter`-Endpunkt nur Lesezugriff und der `importer`-Endpunkt Lese-/Schreibzugriff, was bedeutet:

- Der `admin` hat die volle Kontrolle über die Datenbank.
- Der `reporter` Benutzer hat nur SELECT-Berechtigungen.
- Der `importer` hat die Berechtigungen SELECT, INSERT, UPDATE und DELETE.

Fügen Sie die im obigen Beispiel definierten Endpunkte zur `relationships` der `.magento.app.yaml` hinzu. Beispiel:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Wenn Sie einen MySQL-Benutzer konfigurieren, können Sie den [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) Zugriffskontrollmechanismus nicht für gespeicherte Prozeduren und Ansichten verwenden.

## Verbindung zur Datenbank herstellen

Für den direkten Zugriff auf die MariaDB-Datenbank müssen Sie ein SSH verwenden, um sich bei der Remote-Cloud-Umgebung anzumelden und eine Verbindung zur Datenbank herzustellen.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Rufen Sie die MySQL-Anmeldedaten aus den `database`- und `type`-Eigenschaften in der Variablen [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) ab.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   oder

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Suchen Sie in der Antwort die MySQL-Informationen. Beispiel:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Stellen Sie eine Verbindung zur Datenbank her.

   - Verwenden Sie zunächst den folgenden Befehl:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Für Pro verwenden Sie den folgenden Befehl mit Host-Namen, Port-Nummer, Benutzername und Kennwort, die aus der `$MAGENTO_CLOUD_RELATIONSHIPS`-Variablen abgerufen wurden.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Sie können den `magento-cloud db:sql`-Befehl verwenden, um eine Verbindung zur Remote-Datenbank herzustellen und SQL-Befehle auszuführen.

## Verbindung mit sekundärer Datenbank herstellen

>[!IMPORTANT]
>
>Diese Funktion ist nur für Pro-Produktions- und Staging-Cluster verfügbar.

Manchmal müssen Sie eine Verbindung zur sekundären Datenbank herstellen, um die Datenbankleistung zu verbessern oder Probleme mit der Datenbanksperre zu beheben. Wenn diese Konfiguration erforderlich ist, verwenden Sie `"port" : 3304` , um die Verbindung herzustellen. Siehe das Thema [Best Practice zur Konfiguration der MySQL-Slave](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html?lang=de)Verbindung im _Implementierungs-Best-Practices_ Handbuch.

## Fehlerbehebung

In den folgenden Adobe Commerce-Support-Artikeln finden Sie Hilfe bei der Fehlerbehebung bei MySQL-Problemen:

- [Überprüfen langsamer Abfragen und Prozesse MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html?lang=de)
- [Erstellen eines Datenbank-Dump in der Cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html?lang=de)
- [Fehlerbehebung beim Datenmigrations-Tool](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html?lang=de)
- [Adobe Commerce-Upgrade: Kompakt auf dynamische Tabellen 2.2.x, 2.3.x auf 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html?lang=de)
