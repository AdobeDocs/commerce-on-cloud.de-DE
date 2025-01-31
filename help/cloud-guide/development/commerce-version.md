---
title: Commerce-Version aktualisieren
description: Erfahren Sie, wie Sie die Adobe Commerce-Version im Cloud-Infrastrukturprojekt aktualisieren.
feature: Cloud, Upgrade
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# Commerce-Version aktualisieren

Sie können die Adobe Commerce-Code-Basis auf eine neuere Version aktualisieren. Bevor Sie Ihr Projekt aktualisieren, lesen Sie [Systemanforderungen](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) im _Installationshandbuch_, um die neuesten Anforderungen an die Softwareversion zu ermitteln.

Abhängig von Ihrer Projektkonfiguration können Ihre Upgrade-Aufgaben Folgendes umfassen:

- Aktualisierungsdienste wie MariaDB (MySQL), OpenSearch, RabbitMQ und Redis für die Kompatibilität mit neuen Adobe Commerce-Versionen.
- Konvertieren Sie eine ältere Konfigurationsverwaltungsdatei.
- Aktualisieren Sie die `.magento.app.yaml` Datei mit neuen Einstellungen für Erweiterungspunkte und Umgebungsvariablen.
- Aktualisieren Sie Erweiterungen von Drittanbietern auf die neueste unterstützte Version.
- Aktualisieren Sie die `.gitignore`.

{{upgrade-tip}}

{{pro-update-service}}

## Upgrade von älteren Versionen

Wenn Sie mit dem Upgrade von einer Commerce-Version beginnen, die älter als 2.1 ist, können einige Einschränkungen in der Adobe Commerce-Code-Basis Ihre Möglichkeit beeinträchtigen, _Update_ auf eine bestimmte ECE-Tools-Version oder _Upgrade_ auf die nächste unterstützte Commerce-Version durchzuführen. Verwenden Sie die folgende Tabelle, um den besten Pfad zu ermitteln:

| Aktuelle Version | Aktualisierungspfad |
| ----------------- | ------------ |
| 2.1.3 und früher | Aktualisieren Sie Adobe Commerce auf Version 2.1.4 oder höher, bevor Sie fortfahren. Führen Sie dann ein [einmaliges Upgrade durch, um ECE-Tools zu installieren](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [ECE-Tools-Paket ](../dev-tools/update-package.md).<br>Siehe Versionshinweise für [2002.0.9](../release-notes/cloud-release-archive.md#v200209) und spätere Versionen von 2002.0.x. |
| 2.1.15 - 2.1.16 | [ECE-Tools-Paket ](../dev-tools/update-package.md).<br>Siehe Versionshinweise für [.2002.0.](../release-notes/cloud-release-archive.md#v200209) und höher. |
| 2.2.x und höher | [ECE-Tools-Paket ](../dev-tools/update-package.md).<br>Siehe Versionshinweise für [.2002.0.8 ](../release-notes/cloud-release-archive.md#v200208) höher. |

{style="table-layout:auto"}

{{ece-tools-package}}

## Konfigurationsverwaltung

In älteren Adobe Commerce-Versionen (z. B. 2.1.4 oder höher bis 2.2.x oder höher) wurde eine `config.local.php`-Datei für die Konfigurationsverwaltung verwendet. Adobe Commerce Version 2.2.0 und höher verwendet die `config.php`-Datei, die genau wie die `config.local.php`-Datei funktioniert, aber unterschiedliche Konfigurationseinstellungen hat, die eine Liste Ihrer aktivierten Module und zusätzliche Konfigurationsoptionen enthalten.

Beim Upgrade von einer älteren Version müssen Sie die `config.local.php` migrieren, um die neuere `config.php`-Datei zu verwenden. Führen Sie die folgenden Schritte aus, um Ihre Konfigurationsdatei zu sichern und eine zu erstellen.

**Erstellen einer temporären `config.php`-Datei**:

1. Erstellen Sie eine Kopie `config.local.php` Datei und benennen Sie sie `config.php`.

1. Fügen Sie diese Datei zum `app/etc` Ihres Projekts hinzu.

1. Fügen Sie die Datei zu Ihrer Verzweigung hinzu und übertragen Sie sie.

1. Übertragen Sie die Datei in die Integrationsverzweigung.

1. Fahren Sie mit dem Upgrade-Prozess fort.

>[!WARNING]
>
>Nach einem Upgrade können Sie die `config.php`-Datei entfernen und eine neue, vollständige Datei erstellen. Sie können diese Datei nur löschen, um sie dieses Mal zu ersetzen. Nach dem Generieren einer neuen, vollständigen `config.php` können Sie die Datei nicht löschen, um eine neue zu generieren. Siehe [Konfigurationsverwaltung und Pipeline-](../store/store-settings.md).

### Überprüfen von Zend Framework Composer-Abhängigkeiten

Beim Upgrade auf **2.3.x oder höher von 2.2.x**, stellen Sie sicher, dass die Abhängigkeiten des Zend-Frameworks zur `autoload`-Eigenschaft der `composer.json`-Datei hinzugefügt wurden, um Laminas zu unterstützen. Dieses Plug-in unterstützt neue Anforderungen für das Zend Framework, das zum Laminas-Projekt migriert wurde. Siehe [Migration von Zend Framework zum Laminas-Projekt](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) auf dem _Magento DevBlog_.

**So überprüfen Sie die `auto-load:psr-4`-Konfiguration**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Sehen Sie sich Ihre Integrationsverzweigung an.

1. Öffnen Sie die `composer.json` in einem Texteditor.

1. Überprüfen Sie den `autoload:psr-4` Abschnitt für die Zend Plug-in Manager Implementierung auf die Abhängigkeit von Controllern.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Wenn die Zend-Abhängigkeit fehlt, aktualisieren Sie die `composer.json`:

   - Fügen Sie dem Abschnitt `autoload:psr-4` die folgende Zeile hinzu.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - Aktualisieren Sie die Projektabhängigkeiten.

     ```bash
     composer update
     ```

   - Code-Änderungen hinzufügen, übertragen und per Push übertragen.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - Zusammenführen von Änderungen in der Staging-Umgebung und anschließend in der Produktion.

## Konfigurationsdateien

Vor dem Upgrade der Anwendung müssen Sie Ihre Projektkonfigurationsdateien aktualisieren, um Änderungen an den Standardkonfigurationseinstellungen für Adobe Commerce in der Cloud-Infrastruktur oder der Anwendung zu berücksichtigen. Die neuesten Standardeinstellungen finden Sie im [Magento-Cloud-GitHub-Repository](https://github.com/magento/magento-cloud).

### .magento.app.yaml

Überprüfen Sie immer die in der Datei [.magento.app.yaml](../application/configure-app-yaml.md) enthaltenen Werte für Ihre installierte Version, da sie steuert, wie Ihre Anwendung die Cloud-Infrastruktur erstellt und bereitstellt. Das folgende Beispiel gilt für Version 2.4.7 und verwendet Composer 2.7.2. Die `build: flavor:`-Eigenschaft wird nicht für Composer 2.x verwendet; siehe [Installieren und Verwenden von Composer 2](../application/properties.md#installing-and-using-composer-2).

**So aktualisieren Sie die `.magento.app.yaml`-Datei**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Öffnen und bearbeiten Sie die `magento.app.yaml`.

1. Aktualisieren Sie die PHP-Optionen.

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. Ändern Sie die `hooks`-`build` und `deploy`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Fügen Sie die folgenden Umgebungsvariablen am Ende der Datei hinzu.

   Für Adobe Commerce 2.2.x bis 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Für Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. Speichern Sie die Datei. Übernehmen oder übertragen Sie noch keine Änderungen an die Remote-Umgebung.

1. Fahren Sie mit dem Upgrade-Prozess fort.

### composer.json

Überprüfen Sie vor einem Upgrade immer, ob die Abhängigkeiten in der `composer.json` mit der Adobe Commerce-Version kompatibel sind.

**So aktualisieren Sie die `composer.json` für Adobe Commerce Version 2.4.4 und höher**:

1. Fügen Sie dem `config` Abschnitt die folgenden `allow-plugins` hinzu:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Fügen Sie dem Abschnitt `require` das folgende Plug-in hinzu:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Fügen Sie dem Abschnitt `extra:component_paths` die folgende Komponente hinzu:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Speichern Sie die Datei. Übernehmen oder übertragen Sie noch keine Änderungen an Ihre Verzweigung.

1. Fahren Sie mit dem Upgrade-Prozess fort.

## Projekt-Backup

Es wird empfohlen, vor einem Upgrade eine Sicherungskopie des Projekts zu erstellen. Führen Sie die folgenden Schritte aus, um Ihre Integrations-, Staging- und Produktionsumgebungen zu sichern.

**So sichern Sie die Datenbank und den Code Ihrer Integrationsumgebung**:

1. Erstellen Sie eine lokale Sicherung der Remote-Datenbank.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >Der Befehl `magento-cloud db:dump` führt den Befehl [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) mit dem Flag `--single-transaction` aus, mit dem Sie Ihre Datenbank sichern können, ohne die Tabellen zu sperren.

1. Sichern Sie Code und Medien.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Optional können Sie `[--media]` auslassen, wenn sich bereits eine große Anzahl von statischen Dateien in der Versionsverwaltung befinden.

**So sichern Sie die Datenbank Ihrer Staging- oder Produktionsumgebung vor der Bereitstellung**:

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

1. Erstellen Sie einen [Datenbank-Dump](../storage/database-dump.md). Um einen Zielordner für den DB-Dump auszuwählen, verwenden Sie die Option `--dump-directory` .

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   Der Dump-Vorgang erstellt eine `dump-<timestamp>.sql.gz` Archivdatei in Ihrem Remote-Projektverzeichnis. Siehe [Datenbank sichern](../storage/database-dump.md).

## Anwendungs-Upgrade

Lesen Sie die [Service-Versionen](../services/services-yaml.md#service-versions), um die neuesten Anforderungen an die Softwareversion zu erfüllen, bevor Sie Ihr Programm aktualisieren.

**So aktualisieren Sie die Anwendungsversion**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Legen Sie die Upgrade-Version mithilfe der [Syntax der Versionsbeschränkung](overview.md#cloud-metapackage) fest.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Sie müssen die Versionsbeschränkungssyntax verwenden, um das `ece-tools` Paket erfolgreich zu aktualisieren. Die Versionsbeschränkung finden Sie in der `composer.json`-Datei für die Version der [Anwendungsvorlage](https://github.com/magento/magento-cloud/blob/master/composer.json) die Sie für das Upgrade verwenden.

1. Aktualisieren Sie das Projekt.

   ```bash
   composer update
   ```

1. Überprüfen Sie die aktuell angewendeten Patches:

   - Wenn im `m2-hotfixes`-Verzeichnis Patches installiert sind, [ Sie ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case), und prüfen Sie gemeinsam mit dem Adobe Commerce-Support, welche Patches weiterhin auf die neue Version angewendet werden können. Entfernen Sie die nicht zutreffenden Patches aus dem `m2-hotfixes`.

   - Wenn [Quality Patches] in der `.magento.env.yaml`-Datei angewendet wurden, überprüfen Sie, ob diese weiterhin auf die neue Version angewendet werden können. Entfernen Sie die nicht zutreffenden Patches aus dem `QUALITY_PATCHES` Abschnitt der `.magento.env.yaml`.

   **Methode 1**: [Überprüfen Sie die entsprechenden Versionen in den Versionshinweisen zu Qualitäts-Patches](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Methode 2**: [Anzeigen verfügbarer Patches und Status](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Methode 3**: [Suchen nach Patches](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` ist erforderlich, um alle geänderten Dateien zur Versionsverwaltung hinzuzufügen, da Composer Basispakete marshallt. Sowohl `composer install` als auch `composer update` marshallen Dateien aus dem Basispaket (`magento/magento2-base` und `magento/magento2-ee-base`) in den Paketstamm.

   Die Dateien, die Composer marshallt, gehören zur neuen Version von Adobe Commerce, um die veraltete Version derselben Dateien zu überschreiben. Derzeit ist das Marshalling in Adobe Commerce deaktiviert, sodass Sie die marshallten Dateien zur Quellcodeverwaltung hinzufügen müssen.

1. Warten Sie, bis die Bereitstellung abgeschlossen ist.

1. Überprüfen Sie das Upgrade in Ihrer Integrations-, Staging- oder Produktionsumgebung, indem Sie sich mit SSH anmelden und die Version überprüfen.

   ```bash
   php bin/magento --version
   ```

### Erstellen einer Datei config.php

Wie unter [Konfigurationsverwaltung](#configuration-management) erwähnt, müssen Sie nach dem Upgrade eine aktualisierte `config.php` erstellen. Schließen Sie alle zusätzlichen Konfigurationsänderungen über den Administrator in Ihrer Integrationsumgebung ab.

**Erstellen einer systemspezifischen Konfigurationsdatei**:

1. Verwenden Sie am Terminal einen SSH-Befehl, um die `/app/etc/config.php`-Datei für die Umgebung zu generieren.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   So führen Sie beispielsweise für Pro die `scd-dump` auf der `integration` Verzweigung aus:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. Übertragen Sie die `config.php`-Datei mithilfe von `rsync` oder `scp` auf Ihre lokalen Workstations. Sie können diese Datei nur lokal zur Verzweigung hinzufügen.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   Dadurch wird eine aktualisierte `/app/etc/config.php`-Datei mit einer Modulliste und Konfigurationseinstellungen generiert.

>[!WARNING]
>
>Für ein Upgrade löschen Sie die `config.php`. Nachdem diese Datei zu Ihrem Code hinzugefügt wurde, sollten Sie **nicht** löschen. Wenn Sie Einstellungen entfernen oder bearbeiten müssen, bearbeiten Sie die Datei manuell.

### Erweiterungen aktualisieren

Überprüfen Sie Ihre Erweiterungs- und Modulseiten von Drittanbietern auf Marketplace oder anderen Unternehmens-Sites und überprüfen Sie, ob Adobe Commerce und Adobe Commerce in der Cloud-Infrastruktur unterstützt werden. Wenn Sie Erweiterungen und Module von Drittanbietern aktualisieren müssen, empfiehlt Adobe, in einer neuen Integrationsverzweigung zu arbeiten, wobei Ihre Erweiterungen deaktiviert sind.

**So überprüfen und aktualisieren Sie Ihre Erweiterungen**:

1. Erstellen Sie eine Verzweigung auf Ihrer lokalen Workstation.

1. Deaktivieren Sie Ihre Erweiterungen nach Bedarf.

1. Laden Sie, sofern verfügbar, Erweiterungs-Upgrades herunter.

1. Installieren Sie das Upgrade, wie in der Dokumentation des Drittanbieters beschrieben.

1. Aktivieren und Testen der Erweiterung.

1. Fügen Sie die Code-Änderungen hinzu, übertragen Sie sie und übertragen Sie sie auf die Fernbedienung.

1. Push-Benachrichtigung und Testen in der Integrationsumgebung.

1. Pushen Sie in die Staging-Umgebung, um sie in einer Vorproduktionsumgebung zu testen.

Adobe empfiehlt dringend, die Produktionsumgebung (_)_ aktualisieren, einschließlich der aktualisierten Erweiterungen in Ihrem Site-Launch-Prozess.

>[!NOTE]
>
>Wenn Sie Ihre Anwendungsversion aktualisieren, wird der Upgrade-Prozess automatisch auf die neueste Version des [Fastly CDN-Moduls](../cdn/fastly.md#fastly-cdn-module-for-magento-2) aktualisiert.

## Fehlerbehebung bei Upgrades

Wenn das Upgrade fehlgeschlagen ist, erhalten Sie eine Fehlermeldung im Browser, die Sie darauf hinweist, dass Sie nicht auf Ihre Storefront oder das Admin-Panel zugreifen können:

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**So beheben Sie den Fehler**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Öffnen Sie die `./app/var/report/<error number>`.

1. [Überprüfen Sie die ](../test/log-locations.md) und ermitteln Sie die Ursache des Problems.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
