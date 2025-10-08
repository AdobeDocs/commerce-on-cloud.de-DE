---
title: ECE-Tools - Versionshinweise
description: Hier finden Sie eine Liste der neuesten Verbesserungen am ECE-Tools-Paket.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: e39b348b02b4edfbe091420c54a20b320387a4b6
workflow-type: tm+mt
source-wordcount: '3286'
ht-degree: 0%

---

# ECE-Tools - Versionshinweise

Das Paket [ece-tools](https://github.com/magento/ece-tools) besteht aus einer Reihe von Skripten und Tools zur Verwaltung und Bereitstellung von Cloud-Projekten. In diesen Versionshinweisen werden die neuesten Verbesserungen an diesem Paket beschrieben, das Teil der [Cloud-Tools-Suite für Commerce](cloud-tools-suite.md) ist.

>[!NOTE]
>
>Informationen [&#x200B; Aktualisierung auf die neueste Version des &#x200B;](../dev-tools/update-package.md)-Pakets finden Sie unter `ece-tools`Aktualisieren der ECE-Tools“.

Das `ece-tools`-Paket verwendet die folgende Versionssequenz: `200<major>.<minor>.<patch>`

Die Versionshinweise umfassen Folgendes:

- ![neues Symbol](../../assets/new.svg) Neue Funktionen
- ![Fehlerbehebungssymbol](../../assets/fix.svg) Fehlerbehebungen und Verbesserungen

<!--Add release notes below-->

## v2002.2.8 {#latest}

Veröffentlichungsdatum: 8. Oktober 2025

- ![neues Symbol](../../assets/new.svg) **ActiveMQ**-Unterstützung für ActiveMQ hinzugefügt.<!-- MCLOUD-13770 -->
- ![neues Symbol](../../assets/new.svg) **ActiveMQ**-Hinzugefügte Funktionstests.<!-- MCLOUD-13813 -->


## v2002.2.7

Veröffentlichungsdatum: 7. August 2025

- ![fix icon](../../assets/fix.svg) **PHP 8.4 fixes**-added type compatibility.<!-- MCLOUD-13965 -->
- ![fix icon](../../assets/fix.svg) **EOL validator**-Updated End of Life (EOL) services dates.<!-- MCLOUD-13929 -->
- ![neues Symbol](../../assets/new.svg) **Valkey**-Hinzugefügte PHP 8.2- und PHP 8.3-Funktionstests.<!-- MCLOUD-13610 -->
- ![fix icon](../../assets/fix.svg) **Valkey validator**-Die ECE-Tools-Warnmeldung wurde behoben.<!-- MCLOUD-13896 -->
- ![fix icon](../../assets/fix.svg) **ECE-Tools**-Hinzugefügte Verbesserungen bei Unit-Tests.<!-- MCLOUD-13838 -->
- ![neues Symbol](../../assets/new.svg) **Validator für Dienste**-Hinzugefügte neue Versionen unterstützen OpenSearch, MariaDB und PHP.<!-- MCLOUD-13923 -->
- ![neues Symbol](../../assets/new.svg) **OpenSearch3**-Unterstützung für OpenSearch3.<!-- MCLOUD-13763 --> hinzugefügt
- ![Fix icon](../../assets/fix.svg) **OpenSearch-Unterstützung für 2.4.4-p7/p12-** Validator-Skript aktualisiert.<!-- MCLOUD-13945 -->
- ![neues Symbol](../../assets/new.svg) **OpenSearch3-Tests**-Funktionstests hinzugefügt.<!-- MCLOUD-13769 -->

## v2002.2.6

Veröffentlichungsdatum: 3. Juni 2025

- ![Fix-Symbol](../../assets/fix.svg) **Verbesserte Kompatibilität mit 2.4.8**-Aktualisierte Drittanbieterbibliotheken für bessere Kompatibilität mit 2.4.8<!-- MCLOUD-13707 -->

## v2002.2.5

Veröffentlichungsdatum: 27. Mai 2025

- ![neues Symbol](../../assets/new.svg) **Kompatibilität mit Extended Valkey**-Kompatibilität mit Extended Valkey in Adobe Commerce.<!-- MCLOUD-13595 -->
- ![Fix icon](../../assets/fix.svg) **Aktualisierter RabbitMQ-Validator**-Aktualisierter Validator für RabbitMQ.<!-- MCLOUD-13589 -->
- ![Fix icon](../../assets/fix.svg) **Aktualisierter MariaDB-Validator**-Aktualisierter ECE-Tools-Validator für MariaDB 10.11.<!-- MCLOUD-13593 -->
- ![fix icon](../../assets/fix.svg) **Extended Opensearch2 compatibility**-made Opensearch2 kompatibel mit den neuesten 2.4.4 Versionen.<!-- MCLOUD-13710 -->

## v2002.2.4

Veröffentlichungsdatum: 24. April 2025

- ![Fix icon](../../assets/fix.svg) **OpenSearch2 für 2.4.4/2.4.5**—Es wurde ein Problem im Zusammenhang mit der Unterstützung für `opensearch2` in Adobe Commerce-Versionen 2.4.4/2.4.5.<!-- MCLOUD-13607 --> behoben

## v2002.2.3

Veröffentlichungsdatum: 9. April 2025

- ![Fix icon](../../assets/fix.svg) **Fix Valkey** Problem mit der benutzerdefinierten Valkey-Konfiguration behoben.<!-- MCLOUD-13569 -->
- ![fix icon](../../assets/fix.svg) **Fix validator**-Fixed validator für RabbitMQ 4.0.<!-- MCLOUD-13560 -->

## v2002.2.2

Veröffentlichungsdatum: 7. April 2025

## v2002.2.2

Veröffentlichungsdatum: 7. April 2025

- ![neues Symbol](../../assets/new.svg) **Valkey** - Es wurde Unterstützung für einen neuen Service (Valkey) hinzugefügt, der ein Ersatz für Redis ist.<!-- MCLOUD-13455 -->
- ![fix icon](../../assets/fix.svg) **Opensearch2 for 2.4.4/2.4.5**—Unterstützung für `opensearch2` in Adobe Commerce-Versionen 2.4.4/2.4.5.<!-- MCLOUD-13493 --> hinzugefügt

## v2002.2.1

Veröffentlichungsdatum: 6. Februar 2024

- ![neues Symbol](../../assets/new.svg) **PHP 8.4**—Unterstützung für PHP 8.4.<!-- MCLOUD-13145 --> hinzugefügt
- ![Fix icon](../../assets/fix.svg) **Validator for OpenSearch**-Fixes Der Validator, der eine irreführende Meldung über die falsche Version des Dienstes erzeugt hat.<!-- MCLOUD-13184 -->

## v2002.2.0

Veröffentlichungsdatum: 7. Oktober 2024

- ![neues Symbol](../../assets/new.svg) **MariaDB 11.4**-Unterstützung von MariaDB 11.4 hinzugefügt.
- ![fix icon](../../assets/fix.svg) **Refactored code**-Entfernte Unterstützung der alten PHP-Versionen 7.4, 7.3, 7.2 und der zugehörigen Bibliotheken.<!-- MCLOUD-9278 -->
- ![Fix icon](../../assets/fix.svg) **Upgrade der Monolog-Version**-Unterstützung für Monolog 3.6.<!-- MCLOUD-12855 --> hinzugefügt
- ![fix icon](../../assets/fix.svg) **Validator für RabbitMQ, MariaDB und PHP**-Behebt den Validator, der eine irreführende Meldung über die falsche Version des Dienstes erzeugt hat.

## v2002.1.19

Veröffentlichungsdatum: 21. Mai 2024

- ![neues Symbol](../../assets/new.svg) **Lua** - Option useLua für CACHE_CONFIGURATION hinzugefügt.
- ![fix icon](../../assets/fix.svg) **Validator** - Aktualisierte Validatoren für neue Versionen von Redis und RabbitMQ.

## v2002.1.18

Veröffentlichungsdatum: 8. April 2024

- ![new icon](../../assets/new.svg) **PHP** — Unterstützung für PHP 8.3 hinzugefügt.
- ![Fix icon](../../assets/fix.svg) **Validator** - EOL-Validator wurde aktualisiert.

## v2002.1.17

Veröffentlichungsdatum: 16. Januar 2024

- ![Fix icon](../../assets/fix.svg) **Validator for Elasticsearch &amp; OpenSearch** - Der Validator wurde korrigiert, der eine irreführende Meldung zur Installation eines Suchdiensts bei aktivierter LiveSearch-Funktion ausgab.<!-- MCLOUD-10167 -->
- ![Fix icon](../../assets/fix.svg) **Bereitstellungswarnung** - Es wurde ein Problem behoben, das zu Bereitstellungswarnungen zu nicht leeren Ordnern führte.<!-- MCLOUD-8958 -->

## v2002.1.16

Veröffentlichungsdatum: 16. Oktober 2023

- ![neues Symbol](../../assets/new.svg) **Globale Umgebungsvariable ENABLE_WEBHOOKS** - Die globale Variable [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) zur Verwendung mit Commerce-Webhooks wurde hinzugefügt, um eine Verbindung zu einem externen Endpunkt herzustellen, z. B. zu einer App Builder-Laufzeitaktion oder einem Inventarverwaltungssystem eines Drittanbieters.

## v2002.1.15

Veröffentlichungsdatum: 31. Juli 2023

- ![Fehlersymbol](../../assets/fix.svg) **Fehlercodes** - Aktualisiertes Fehlercodeschema und Fehlercodedokumentgenerator.
- ![Fix icon](../../assets/fix.svg) **Validator für benutzerdefiniertes Redis-Modell**-Der Validator für benutzerdefinierte Redis-Backend-Modelle wurde aktualisiert. [Siehe Beispiel für die Cache-Konfiguration](../environment/variables-deploy.md#cache_configuration).
- ![Fix icon](../../assets/fix.svg) **Validator for RabbitMQ**-Hinzugefügte Unterstützung für RabbitMQ 3.11
- ![Fix-Symbol](../../assets/fix.svg) **Falscher Link korrigiert**-Falscher Link zur Onboarding-Dokumentation in der Willkommens-E-Mail-Vorlage korrigiert.

## v2002.1.14

Veröffentlichungsdatum: 10. März 2023

- ![new icon](../../assets/new.svg) **PHP**—Unterstützung für PHP 8.2 hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **Validatoren für Services** - Aktualisierte Validatoren für Commerce 2.4.6 erfordern Services: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x und RabbitMQ 3.9.
- ![Fix icon](../../assets/fix.svg) **ece-tools db-dump** - Ein Problem wurde behoben, das dazu führte, dass der `db-dump`-Vorgang vorzeitig beendet wurde.

## v2002.1.13

Veröffentlichungsdatum: 27. Oktober 2022

- ![neues Symbol](../../assets/new.svg) **Es wurde Unterstützung für Adobe I/O Events für Adobe Commerce hinzugefügt**. Erweiterungsentwickler können jetzt das [Adobe I/O Events](https://developer.adobe.com/events/docs/)-Framework verwenden, um Commerce-Ereignisinformationen von Cloud-Instanzen an ihre Programme zu senden, die für [Adobe App Builder geschrieben &#x200B;](https://developer.adobe.com/app-builder/docs/overview/). Adobe I/O Events für Adobe Commerce befindet sich in der Partnervorschau.<!-- CEXT-932 -->
- ![neues Symbol](../../assets/new.svg) **Validator für OPcache-Konfiguration** - Es wurde ein Validator hinzugefügt, um die OPcache-Konfiguration auf ausgeschlossene Pfade zu überprüfen.<!-- MCLOUD-9485 -->
- ![Fix icon](../../assets/fix.svg) **Es wurde ein Problem mit der GraphQL-Cache-** behoben. Jetzt behält ECE-Tools den GraphQL-`id_salt` in `cache` Konfiguration in der `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Veröffentlichungsdatum: 13. September 2022

- ![neues Symbol](../../assets/new.svg) **Aktivieren`synchronous_replication`** - ECE-Tools legt `synchronous_replication=>true` in der `app/etc/env.php` fest, wenn `MYSQL_USE_SLAVE_CONNECTION` aktiviert ist. Diese Konfiguration betrifft nur Commerce 2.4.6+. Eine Beschreibung der `MYSQL_USE_SLAVE_CONNECTION` finden Sie unter [Variablen bereitstellen](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![neues Symbol](../../assets/new.svg) **OpenSearch** - Zusätzliche Funktionalität zum Konfigurieren und Festlegen der `opensearch`-Engine für die nächste Adobe Commerce-Version 2.4.6. Siehe [Einrichten des OpenSearch-Service](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Veröffentlichungsdatum: 4. August 2022

- ![Fix icon](../../assets/fix.svg) **ElasticSuite Validator und OpenSearch**: ElasticSuite-Integritätsprüfungsproblem bei der Installation von OpenSearch wurde behoben.<!-- MCLOUD-8767 -->
- ![Fix icon](../../assets/fix.svg) **Rückgabetypen für Bereitstellungsbefehle**—Feste Rückgabetypen für Bereitstellungsbefehle.<!-- AC-3208 -->
- ![Fix icon](../../assets/fix.svg) **[!DNL RabbitMQ]Problem mit der Neuinstallation von Commerce 2.4.5** - Fehlerkorrektur [!DNL RabbitMQ] Absturzproblem bei der neuen Installation von Commerce 2.4.5. <!-- MCLOUD-9059 -->

## v2002.1.10

Veröffentlichungsdatum: 31. März 2022

- ![Fix-Symbol](../../assets/fix.svg) **Elasticsearch 7.10** - Validatoren wurden aktualisiert, um die Elasticsearch-Version 7.10 zu unterstützen.<!-- MCLOUD-8548 -->

## v2002.1.9

Veröffentlichungsdatum: 10. März 2022

- ![neues Symbol](../../assets/new.svg) **OpenSearch** - Unterstützung für OpenSearch für Adobe Commerce 2.4.4, 2.4.3-p2 und 2.3.7-p3.<!-- MCLOUD-8296 --> hinzugefügt
- ![new icon](../../assets/new.svg) **PHP**—Unterstützung für PHP 8.1 hinzugefügt.
- ![fix icon](../../assets/fix.svg) **symfony/process** - Kompatibilität mit symfony/process ^5.3.<!-- MCLOUD-8283 --> hinzugefügt

- ![neues Symbol](../../assets/new.svg) **Verbraucher - Mehrere Prozesse** Es wurde eine `multiple_processes` Option hinzugefügt, mit der Sie die Anzahl der Prozesse angeben können, die für jeden Verbraucher erzeugt werden sollen. Eine Beschreibung der `CRON_CONSUMERS_RUNNER` finden Sie unter [Variablen bereitstellen](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![neues Symbol](../../assets/new.svg) **OpenSearch-Schema und vollständiger Host-Pfad** - Es wurde die Möglichkeit hinzugefügt, ein Elasticsearch-Schema und einen vollständigen Host-Pfad zu konfigurieren.
- ![Fix-Symbol](../../assets/fix.svg) **AWS S3** - Die Methode zur Aktivierung von AWS S3 wurde geändert.
- ![Fix icon](../../assets/fix.svg) **Fix Driver_Options Reader** - Es wurde das Lesen der Driver_Options-Konfiguration für die DB-Verbindung aus der `env.php`-Datei durch `ece-tools` für Validatoren hinzugefügt.<!-- MCLOUD-8420 -->

## v2002.1.8

Veröffentlichungsdatum: 25. Oktober 2021

- ![neues Symbol](../../assets/new.svg) **Alternativer Speicherort für Speicherauszüge** Die `--dump-directory`-Option wurde hinzugefügt, mit der Sie ein Zielverzeichnis für einen DB-Speicherauszug auswählen können. Jetzt ist `/app/var/dump-main` der Standard-Zielordner für einen DB-Dump. Siehe [Backup-Verwaltung: Datenbank sichern](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![Fix icon](../../assets/fix.svg) **Update Monolog** - Die Mindestversion, die für das `monolog`-Paket erforderlich ist, wurde auf `^2.3` aktualisiert.<!-- ACMP-1263 -->
- ![Fix icon](../../assets/fix.svg) **Symfony aktualisieren** - Die Symfony-Abhängigkeiten wurden aktualisiert, damit sie mit Adobe Commerce 2.4.4.<!-- ACMP-1533 --> kompatibel sind
- ![Fix icon](../../assets/fix.svg) **Feature/resolve autoload** - Es wurde ein Problem bei der Bereitstellung in einer Integrationsumgebung und der Anzeige des `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`-Fehlers behoben.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Veröffentlichungsdatum: 29. Juli 2021

**Konfigurationsaktualisierungen**—

- ![neues Symbol](../../assets/new.svg) Unterstützung für Composer 2.0.<!--MCLOUD-8003--> hinzugefügt

- ![Fix icon](../../assets/fix.svg) **Aktualisierte Composer-Anforderungen für`symphony/console`** - Die ECE-Tools-`composer.json`-Versionsanforderungen für das `symphony/console`-Paket wurden aktualisiert, um ein Problem zu beheben, das dazu führte, dass die `di:compile`-Befehle mit dem folgenden Fehler fehlschlugen: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![Fix-Symbol](../../assets/fix.svg) Die End-of-Life-Softwareprüfungen (`eol.yaml`) wurden aktualisiert und enthalten jetzt Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Veröffentlichungsdatum: 20. April 2021

- ![neues Symbol](../../assets/new.svg) **Redis-Authentifizierungsberechtigungen** - Es wurde die Möglichkeit hinzugefügt, während der Bereitstellungsphase Redis-Autorisierungsberechtigungen aus der `relationships`-Eigenschaft zu lesen.<!--MCLOUD-7694-->

- ![neues Symbol](../../assets/new.svg) **Elasticsearch-Autorisierungs-**: Es wurde die Möglichkeit hinzugefügt, während der Bereitstellungsphase Elasticsearch-Autorisierungs-Anmeldeinformationen aus der `relationships`-Eigenschaft zu lesen.<!--MCLOUD-7695-->

- ![neues Symbol](../../assets/new.svg) **Dedizierter Sitzungsspeicher-Service** - `redis-session` als zweite Option für die Sitzungsspeicherung hinzugefügt. Sie können den `redis-session`-Dienst verwenden, um Sitzungsinformationen zu speichern, und den `redis`-Dienst für den Cache verwenden, um eine bessere Leistung zu erzielen.<!--MCLOUD-7698-->

- ![neues Symbol](../../assets/new.svg) **Veraltete SPLIT_DB-Meldungen** - Es wurden Validierungswarnungen und kritische Meldungen für die veraltete `SPLIT_DB`-Option für Adobe Commerce 2.4.2 und deren Entfernung in Adobe Commerce 2.5.0 hinzugefügt.<!--MCLOUD-7806-->

- ![Fix-Symbol](../../assets/fix.svg) **Elasticsearch-Version aus Beziehungen** - Service-Validator wurde korrigiert, um die richtige Version von Elasticsearch aus den `relationships` in Cloud Docker- und Integrationsumgebungen abzurufen.<!--MCLOUD-7572-->

- ![Fix icon](../../assets/fix.svg) **Flexible Redis-Port-Validierung** - Redis kann jetzt den Port in einer benutzerdefinierten Cache-Verbindung über die `server`-URL validieren. Sie können beispielsweise Ihre Portnummer wie folgt zu Ihrer Server-URL hinzufügen: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Dadurch lassen sich Validierungsfehler vermeiden, bei denen die `port` entweder fehlt oder falsch ist.<!--MCLOUD-7722-->

- ![Fix icon](../../assets/fix.svg) **Upgrade auf Adobe Commerce 2.4.2** - Es wurde ein Problem behoben, bei dem Benutzer `bin/magento setup:upgrade` manuell ausführen mussten, um ihre Sites nach dem Upgrade auf Adobe Commerce 2.4.2.<!--MCLOUD-7776--> betriebsbereit zu machen

## v2002.1.5

Veröffentlichungsdatum: 1. Februar 2021

- ![neues Symbol](../../assets/new.svg) **Remote-Speicher** - Die Umgebungsvariable `REMOTE_STORAGE` wurde hinzugefügt, um Cloud-Projekte für die Remote-Speicherung von Mediendateien mithilfe eines Speicher-Services wie AWS S3 zu aktivieren. Diese Konfigurationsoption ist Teil des Pakets „ECE-Tools“, wird jedoch in Adobe Commerce in der Cloud-Infrastruktur nicht unterstützt.<!--MCLOUD-7153-->

- ![neues Symbol](../../assets/new.svg) **neuer `cloud:config:validate`-Befehl** - Der Befehl `php vendor/bin/ece-tools cloud:config:validate` wurde hinzugefügt, um die `.magento.env.yaml`-Konfiguration zu validieren, bevor Änderungen an die Remote-Cloud-Umgebung gesendet werden.<!--MCLOUD-7120-->

- ![neues Symbol](../../assets/new.svg) **Leeren des opcache**—Es wurde Unterstützung für die `opcache.enable_cli` PHP-Option hinzugefügt, um den OPcache zu leeren, bevor der Bereitstellungs-Hook ausgeführt wird. Durch diese Konfiguration wird die Cache-Konfiguration zurückgesetzt, um sicherzustellen, dass die aktuellen Konfigurationseinstellungen auf jede Bereitstellung angewendet werden.<!--MCLOUD-7015-->

- ![neues Symbol](../../assets/new.svg) **Validierung von Aurora DB** - Die Validierung des Datenbank-Service wurde aktualisiert, sodass er mit der Aurora-Datenbank kompatibel ist.<!--MCLOUD-7269-->

- ![neues Symbol](../../assets/new.svg) **Neue SCD_NO_PARENT-Umgebungsvariable** - Die `SCD_NO_PARENT` Umgebungsvariable (für Adobe Commerce >=2.4.2) wurde hinzugefügt, um die Erstellung von statischem Inhalt für übergeordnete Designs zu verwalten.<!--MCLOUD-7284-->

- ![fix icon](../../assets/fix.svg) **Speicherbeschränkungen und Befehle** - Es wurde ein Problem behoben, bei dem `php vendor/bin/ece-tools` Befehle nicht funktionierten, wenn die Größe der `cloud.log`-Datei das PHP-Speicherlimit überschritt. Anstatt die gesamte `cloud.log`-Datei in den Speicher zu lesen, lesen wir jetzt nur noch eine kleinere Teilmenge von Daten aus der Protokolldatei.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![Fix icon](../../assets/fix.svg) **Benutzerdefinierte Datenbankverbindungen** - Es wurde ein `.magento.env.yaml`-Konfigurationsproblem behoben, bei dem benutzerdefinierte Datenbankverbindungen, die für `DATABASE_CONFIGURATION` definiert wurden, nicht verwendet wurden. Die Verbindungseinstellungen wurden `app/etc/env.php` nicht hinzugefügt.<!--MCLOUD-7426-->

- ![Fehlerbehebungssymbol](../../assets/fix.svg) **Leere Fehlerprotokolle** - Es wurde ein Problem behoben, das dazu führte, dass Bereitstellungen fehlschlugen, wenn die `cloud.error.log` leer war.<!--MCLOUD-7296-->

- ![Fix icon](../../assets/fix.svg) **MariaDB 10.3 validation**—Behobene Validierung von MariaDB 10.3 für Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![Fix icon](../../assets/fix.svg) **Cache:flush Protokollierung** - Verbesserte Protokolleinträge, um den Beginn und das Ende des `cache:flush` anzugeben.<!--MCLOUD-7503-->

## v2002.1.4

Veröffentlichungsdatum: 19. November 2020

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu einem Bereitstellungsfehler führte, wenn die in der `SEARCH_CONFIGURATION` Umgebungsvariablen angegebene Suchmaschine einen anderen Wert als `elasticsearch` hatte.<!--MCLOUD-7283-->

## v2002.1.3

Veröffentlichungsdatum: 9. November 2020

**Infrastrukturaktualisierungen**—

- ![neues Symbol](../../assets/new.svg) Es wurde ECE-Tools-Unterstützung für das schreibgeschützte `pub/static` hinzugefügt, wenn statische Inhalte in der Build-Phase bereitgestellt werden sollen.<!--MC-37699-->

- ![neues Symbol](../../assets/new.svg) Es wurde Unterstützung für Elasticsearch 7.9 und Redis 6 hinzugefügt, um die Kompatibilität mit kommenden Adobe Commerce-Versionen zu gewährleisten.<!--MCLOUD-7191-->

- ![fix icon](../../assets/fix.svg) Die ECE-Tools-`composer.json` wurde aktualisiert, um eine erforderliche Abhängigkeit für das Quality Patches Tool hinzuzufügen. Dies behebt eine zirkuläre Abhängigkeit, die zwischen den Paketen ECE-Tools und magento-cloud-patches bestand.<!--MCLOUD-6910-->

**Validierung und Protokollverbesserungen**—

- ![neues Symbol](../../assets/new.svg) Es wurde eine Suchmaschinenvalidierung hinzugefügt, um sicherzustellen, dass `elasticsearch` für Adobe Commerce auf Cloud-Infrastruktur 2.4 und höher festgelegt ist. Wenn die Validierung fehlschlägt, wird die Bereitstellung mit einer kritischen Fehlermeldung gestoppt, die Fehlerbehebungen für das Problem vorschlägt. Siehe [Kritische Fehler, Bereitstellungsphase](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![neues Symbol](../../assets/new.svg) Es wurde eine Elasticsearch-Validierung hinzugefügt, um die Kompatibilität zwischen der Elasticsearch-Service-Version und der Adobe Commerce-Version zu überprüfen.<!--MCLOUD-7193-->

- ![neues Symbol](../../assets/new.svg) Die Elasticsearch-Kompatibilitätsfehlermeldung wurde aktualisiert, um die Versionen von Elasticsearch anzuzeigen, die mit dem Adobe Commerce Elasticsearch-Modul kompatibel sind. Die Fehlermeldung enthält jetzt die spezifischen Elasticsearch-Versionen, die in Ihrer Cloud-Infrastruktur installiert werden müssen, damit sie mit dem Elasticsearch-Modul kompatibel sind, das von Ihrer Version von Adobe Commerce verwendet wird. Siehe [Warnfehler, Bereitstellungs-Staging](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![neues Symbol](../../assets/new.svg) Es wurden Warnfehler `2026` und `2027` für die Einstellung der ungültigen `MAGE_MODE`-Umgebungsvariablen hinzugefügt. Der einzige gültige Wert ist `production`. Vor dieser Fehlerbehebung konnte `MAGE_MODE` auf `developer` ohne Bereitstellungsfehler festgelegt werden, um später beim Versuch, in schreibgeschützte Dateien zu schreiben, Fehler zu verursachen. Siehe [Warnfehler](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![Fehlerbehebung](../../assets/fix.svg) Die Validierung für Redis-, RabbitMQ- und MySQL-Services wurde korrigiert, um sicherzustellen, dass diese Versionen mit der Adobe Commerce-Version kompatibel sind. Gültige Versionen dieser Services werden jetzt in die `cloud.log` geschrieben.<!--MCLOUD-7098-->

- ![Fix-Symbol](../../assets/fix.svg) Der `cloud.log` wurde aktualisiert und enthält jetzt das Limit für gleichzeitige Anfragen zum Senden von Anfragen während der Cache-Aufwärmung. Dieser Wert wird in der Variablen [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) nach der Bereitstellung konfiguriert.<!--MCLOUD-5563-->

**CLI-Befehlsaktualisierungen**—

- ![neues Symbol](../../assets/new.svg) Es wurden CLI-Befehle (`cloud:config:create` und `cloud:config:update`) hinzugefügt, um die `.magento.env.yaml`-Datei mit einer Konfiguration zu erstellen und zu aktualisieren, die eine oder mehrere Build-, Bereitstellungs- und Post-Deploy-Variablen enthalten kann. Siehe [Konfigurationsdatei von CLI erstellen](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Aktualisierungen von Umgebungsvariablen**—

- ![neues Symbol](../../assets/new.svg) Die Build-Variable [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) wurde hinzugefügt. Wenn Sie die Variable auf `true` setzen, kann die Anwendung den `composer dump-autoload`-Befehl während einer Commerce-Installation von Cloud Docker nicht ausführen. Die Variable ist nur für Cloud Docker für Commerce-Container mit beschreibbaren Dateisystemen (erstellt für Tests und Entwicklung mit `./vendor/bin/ece-docker build:compose --with-test`) relevant. Bei solchen Installationen verhindert das Überspringen des Befehls `composer dump-autoload` Fehler, wenn andere Befehle ausgeführt werden, die versuchen, auf Dateien aus einem gelöschten `generated` zuzugreifen.<!--MCLOUD-6939-->

## v2002.1.2

Veröffentlichungsdatum: 5. August 2020

**Validierung und Protokollverbesserungen**—

- ![neues Symbol](../../assets/new.svg) Es wurde die `schema.error.yaml`-Datei hinzugefügt, die alle Fehler- und Warnbenachrichtigungen enthält, die während des Erstellungs-, Bereitstellungs- und Nachbereitstellungsprozesses auftreten können, sowie Vorschläge zur Behebung der Fehler. Die Informationen in dieser Datei finden Sie auch im _Cloud-Handbuch für Commerce_. Siehe [Fehlermeldungsreferenz für ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![neues Symbol](../../assets/new.svg) Die Einträge im Cloud-Fehlerprotokoll (`/var/log/cloud.error.log`) wurden in das JSON-Format geändert, um das programmgesteuerte Parsen des Protokolls zu vereinfachen.<!--MCLOUD-5879-->

- ![neues Symbol](../../assets/new.svg) Es wurden zusätzliche Fehlerprüfungen für die Build-, Bereitstellungs- und Nachbereitstellungsverarbeitung sowie verbesserte vorhandene Prüfungen hinzugefügt:

   - Fehler-Code 2026 - Einige während der Build-Phase generierte Daten konnten nicht in den bereitgestellten Verzeichnissen wiederhergestellt werden

   - Fehler-Code 3004 - Sicherungsdateien können nicht erstellt werden

   - Fehler-Code 102 - Es wurden zusätzliche Prüfungen für Probleme hinzugefügt, die auftreten, wenn die `env.php`-Datei nicht beschreibbar ist <!--MCLOUD-6221-->

- ![neues Symbol](../../assets/new.svg) Die Umgebungsvariable **QUALITY_PATCHES** wurde hinzugefügt, um einen oder mehrere Qualitäts-Patches anzugeben, die während des Bereitstellungsprozesses angewendet werden sollen. Siehe [Variablen erstellen](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Veröffentlichungsdatum: 25. Juni 2020

- ![neues Symbol](../../assets/new.svg) **Infrastrukturaktualisierungen**—

   - ![neues Symbol](../../assets/new.svg) **Verbesserungen bei der Protokollierung** - Verbesserte Protokollierungsfunktion durch Zuweisung von Ausstiegscodes zu kritischen Bereitstellungsfehlern und Offenlegung der Ausstiegscodes in Fehlerbenachrichtigungen und Protokollereignissen. Siehe [Fehlermeldungsreferenz für ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![neues Symbol](../../assets/new.svg) Der Prozess für Datenbank-Dumps (`vendor/bin/ece-tools db-dump`) und aktualisierte Protokollmeldungen wurden verbessert, um klarzustellen, dass der Vorgang zum Datenbank-Dump die Anwendung in den Wartungsmodus wechselt, Prozesse in der Verbraucherwarteschlange stoppt und Cron-Aufträge deaktiviert, bevor der Dump beginnt.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, um sicherzustellen, dass die Projekt-URL bei der Bereitstellung in Staging- und Produktionsumgebungen korrekt aktualisiert wird. Jetzt verwendet `ece-tools` die URL für die Route, wobei das Attribut `primary:true` in der Projektrouten-Konfiguration festgelegt ist. Siehe [Variablen bereitstellen](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![Fix-Symbol](../../assets/fix.svg) Der `generate.xml` Build-Szenario-Workflow zum Anwenden von Patches wurde aktualisiert. Patches müssen früher angewendet werden, um Adobe Commerce zu aktualisieren und Probleme zu beheben, die dazu führen können, dass die `di:compile`- und `module:refresh` fehlschlagen.<!--MCLOUD-5941-->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem im Installationsprozess behoben, das fälschlicherweise den `Crypt key missing` zurückgibt. Der `crypt/key` wird während der Installation automatisch generiert.<!--MCLOUD-6120-->

- ![neues Symbol](../../assets/new.svg) **Service-Updates**—

   - ![neues Symbol](../../assets/new.svg) Unterstützung für PHP 7.4 und MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144--> hinzugefügt

- ![neues Symbol](../../assets/new.svg) **Aktualisierungen von Umgebungsvariablen**—

   - ![neues Symbol](../../assets/new.svg) Die Variable **SCD_USE_BALER** wurde hinzugefügt, um das Ballenmodul für die JavaScript-Bündelung während des Build-Prozesses von Adobe Commerce in der Cloud-Infrastruktur zu aktivieren. Weitere Informationen finden Sie in der Beschreibung der [&#x200B; im Abschnitt &#x200B;](../environment/variables-build.md#scd_use_baler)Build-Variablen<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![neues Symbol](../../assets/new.svg) Die Umgebungsvariable **REDIS_BACKEND** wurde hinzugefügt, um das Redis-Backend-Modell für den Redis-Cache für Adobe Commerce 2.3.5 oder höher zu konfigurieren. Weitere Informationen finden Sie in der Beschreibung der Variablen [Variablen bereitstellen](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![Neues Symbol](../../assets/new.svg) **CLI-Befehlsaktualisierungen**—

   - ![neues Symbol](../../assets/new.svg) Die folgenden CLI-Befehle wurden mit einer Option für eine detailliertere Protokollierung aktualisiert:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     Die Protokollierungsebene für jeden Aufruf wird durch die Konfiguration der [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) in der `.magento.env.yaml` bestimmt.<!--MCLOUD-3503-->

- ![neues Symbol](../../assets/new.svg) **Validierungsverbesserungen**—

   - ![neues Symbol](../../assets/new.svg) **Elasticsearch 7.x-Kompatibilitätsprüfungen**—Aktualisierte Elasticsearch-Validierung für Software-Kompatibilitätsprüfungen für Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![neues Symbol](../../assets/new.svg) **Aktualisierte Service-Version und EOL-Validierungsprüfungen**—Aktualisierte Validierung, um installierte Service-Versionen mit Adobe Commerce 2.4 zu überprüfen. Anforderungen.<!--MCLOUD-6144-->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Validierungsproblem behoben, sodass die folgende Warnmeldung nach der Bereitstellung nur angezeigt wird, wenn die `post-deploy` Hook-Konfiguration in der `.magento.app.yaml`-Datei fehlt:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![neues Symbol](../../assets/new.svg) **Hinzugefügte Validierung für Zend Framework-Abhängigkeiten** - Es wurde eine Komponentenabhängigkeitsvalidierung für das Zend Framework hinzugefügt, das zum Laminas-Projekt migriert wurde. Wenn die erforderlichen Abhängigkeiten fehlen, wird während des Build-Prozesses die folgende Fehlermeldung angezeigt.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Siehe [Überprüfen von Zend Framework-Abhängigkeiten](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![neues Symbol](../../assets/new.svg) **Es wurde eine Validierung für `env.php` Datei und** hinzugefügt - während des Installations- und Aktualisierungsprozesses wird nach der `env.php` und den Daten gesucht.<!--MCLOUD-5991-->

      - Wenn die `env.php`-Datei in der Installation fehlt und der `crypt/key` nicht in der `.magento.app.yaml`-Datei angegeben ist, schlägt die Bereitstellung mit der folgenden Benachrichtigung fehl:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Wenn die Installation die `env.php`-Datei nicht enthält oder die Konfiguration nur einen Cache-Typ enthält, wird während des Upgrade-Prozesses der Befehl `cron:enable` ausgeführt, um die Datei mit allen `cache_types` wiederherzustellen. Die folgende Benachrichtigung wird dem Protokoll hinzugefügt:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Veröffentlichungsdatum: 6. Februar 2020

- ![neues Symbol](../../assets/new.svg) **Infrastrukturaktualisierungen**—

   - ![neues Symbol](../../assets/new.svg) **Separates Paket für Cloud Docker für Commerce hinzugefügt** - Docker-Paket vom `ece-tools`-Paket entkoppelt, um die Code-Qualität zu erhalten und unabhängige Versionen bereitzustellen. Aktualisierungen und Fehlerbehebungen im Zusammenhang mit `ece-tools` werden über das [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub-Repository verwaltet.<!--MAGECLOUD-2927-->

   - ![neues Symbol](../../assets/new.svg) **Aktualisierte Patch-Funktionen** - Die Patch-Funktion wurde aus dem Paket ECE-Tools in ein separates Paket [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) verschoben. Während der Bereitstellung verwendet `ece-tools` das neue Paket, um Patches anzuwenden. Siehe [Versionshinweise zu Cloud-Patches](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![neues Symbol](../../assets/new.svg) **Aktualisierte Composer-Abhängigkeiten** - Die `composer.json` für Adobe Commerce in der Cloud-Infrastruktur wurde durch eine Abhängigkeit für das `magento/magento-cloud-docker`-Paket aktualisiert. Jetzt enthält `ece-tools` Abhängigkeiten für alle Pakete im [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Diese Pakete werden bei der Installation oder Aktualisierung von `ece-tools` automatisch installiert und aktualisiert.

- ![neues Symbol](../../assets/new.svg) **Unterstützung für szenariobasierte Bereitstellungen**—<!--MAGECLOUD-4101-->

   - ![neues Symbol](../../assets/new.svg) Jetzt können Sie die Erstellungs-, Bereitstellungs- und Nachbereitstellungsprozesse mithilfe von XML-Konfigurationsdateien anpassen, um die Standardkonfiguration zu überschreiben oder anzupassen.

   - ![neues Symbol](../../assets/new.svg) **hat die `hooks` in`.magento.app.yaml`** geändert - Wir haben das `hooks`-Konfigurationsformat aktualisiert, um szenariobasierte Bereitstellungen zu unterstützen. Das alte Format früherer ECE-Tools 2002.0.x-Versionen wird weiterhin unterstützt. Sie müssen jedoch auf das neue Format aktualisieren, um die szenariobasierte Bereitstellungsfunktion verwenden zu können. Siehe [Szenario-basierte Bereitstellungen](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Bevor Sie auf die ECE-Tools-Version 2002.1.0 aktualisieren, überprüfen Sie [rückwärts   Inkompatible Änderungen](backward-incompatible-changes.md) um mehr über Änderungen zu erfahren, die Folgendes erfordern können   Aktualisieren Sie Adobe Commerce in der Cloud-Infrastruktur-Projektkonfiguration oder -prozesse.

- ![neues Symbol](../../assets/new.svg) **Service-Updates**—

   - ![neues Symbol](../../assets/new.svg) Unterstützung für PHP 7.3.<!--MAGECLOUD-4022--> hinzugefügt

   - ![neues Symbol](../../assets/new.svg) Unterstützung für RabbitMQ 3.8.<!--MAGECLOUD-4674--> hinzugefügt

   - ![neues Symbol](../../assets/new.svg) Es wurde eine Validierung hinzugefügt, um installierte Service-Versionen für jeden Service mit dem EOL-Datum abzugleichen. Kunden erhalten jetzt eine Benachrichtigung, wenn eine Service-Version innerhalb von drei Monaten nach dem Ende der Nutzungsdauer veröffentlicht wird, und eine Warnung, wenn das Ende der Nutzungsdauer in der Vergangenheit liegt.<!--MAGECLOUD-4076-->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Elasticsearch-Konfigurationsproblem behoben, um sicherzustellen, dass die richtigen Elasticsearch-Einstellungen in allen Umgebungen konfiguriert sind.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Unter [Dienstversionen](../services/services-yaml.md#service-versions) finden Sie eine Liste der in Adobe Commerce verwendeten Dienste zur Cloud-Infrastruktur und deren Versionskompatibilität mit der Cloud-Vorlage.

- ![neues Symbol](../../assets/new.svg) **Aktualisierungen von Umgebungsvariablen**—

   - ![neues Symbol](../../assets/new.svg) Die Funktionalität der `WARM_UP_PAGES` Umgebungsvariablen wurde erweitert, um das Vorausfüllen des Cache für bestimmte Produktseiten zu unterstützen. Siehe die erweiterte Definition im Thema [Variablen nach der Bereitstellung](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-4444-->

   - ![neues Symbol](../../assets/new.svg) Die `ERROR_REPORT_DIR_NESTING_LEVEL` Umgebungsvariable wurde hinzugefügt, um die Verwaltung von Fehlerberichtdaten im `<magento_root>/var/report/` zu vereinfachen. Siehe die Variablenbeschreibung im Thema [Build-Variablen](../environment/variables-build.md#error_report_dir_nesting_level) .

   - ![Fix-Symbol](../../assets/fix.svg) Die Umgebungsvariablen `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`, `DO_DEPLOY_STATIC_CONTENT` und `STATIC_CONTENT_SYMLINK` wurden entfernt. Siehe [Abwärtsinkompatible Änderungen](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem im Elastic Suite-Konfigurationsprozess behoben, sodass die Standardkonfiguration wie erwartet überschrieben wird, wenn Sie die Variable `ELASTICSUITE_CONFIGURATION`-Bereitstellung ohne die Option `_merge` konfigurieren.<!--MAGECLOUD-4388-->

- ![Neues Symbol](../../assets/new.svg) **CLI-Befehlsaktualisierungen**—

   - ![neues Symbol](../../assets/new.svg) **neuer Cron-Befehl** - Sie können jetzt die Cron-Verarbeitung in Ihrer Adobe Commerce in der Cloud-Infrastrukturumgebung manuell mithilfe der `cron:disable`- und `cron:enable`-Befehle verwalten. Verwenden Sie den Befehl deaktivieren , um alle aktiven Cron-Prozesse zu stoppen und alle Cron-Aufträge zu deaktivieren. Verwenden Sie den Befehl enable , um Cron-Aufträge erneut zu aktivieren, wenn Sie bereit sind. Siehe [Deaktivieren von Cron-Aufträgen](../application/crons-property.md#disable-cron-jobs).

   - ![neues Symbol](../../assets/new.svg) **Verbessertes Fehler-Reporting** Es wurde eine bessere Protokollierung für CLI-Befehlsfehler hinzugefügt, die während der ECE-Tools-Verarbeitung auftreten.<!--MAGECLOUD-4849-->

   - ![neues Symbol](../../assets/new.svg) **Veraltete Build-Befehle entfernen**— Die folgenden Build-Befehle wurden entfernt: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` und `ece-tools docker` in `ece-docker` umbenannt. Siehe [Abwärtsinkompatible Änderungen](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![neues Symbol](../../assets/new.svg) Veraltete `build_options.ini`-Datei wurde entfernt und es wurde eine Validierung hinzugefügt, die fehlschlägt, wenn der Build vorhanden ist. Verwenden Sie die Datei [.magento.env.yaml](../environment/configure-env-yaml.md) zum Konfigurieren von Build-Optionen.

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass der Build-Prozess fehlschlug, wenn die `config.php`-Datei leer war.<!--MAGECLOUD-4127-->

## 2002,0,23

Veröffentlichungsdatum: 27. Februar 2020

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Kompatibilitätsproblem mit `ece-tools` 2002.0.x behoben, das verhinderte, dass die Erstellung statischer Inhalte auf Anfrage im Produktionsmodus erfolgreich abgeschlossen werden konnte.

## Ältere Versionen

Siehe [Archiv mit Versionshinweisen](cloud-release-archive.md) für Version 2002.0.22 und früher.
