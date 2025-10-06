---
title: Commerce-Version aktualisieren
description: Erfahren Sie, wie Sie die Adobe Commerce-Version in der Cloud-Infrastrukturumgebung aktualisieren.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: fe1da39c1d00d74d3f116423e06d11cefd3c2659
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 0%

---

# Commerce-Version aktualisieren

Sie können die Adobe Commerce-Code-Basis auf eine neuere Version aktualisieren. Lesen Sie vor dem Upgrade der Umgebung [&#x200B; Abschnitt &quot;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)&quot; im _Installationshandbuch_, um die neuesten Anforderungen an die Softwareversion zu ermitteln.

Je nach Umgebungstyp (Entwicklung, Staging oder Produktion) können Ihre Upgrade-Aufgaben Folgendes umfassen:

- Aktualisieren Sie die `.magento/services.yaml` mit neuen Versionen für MariaDB (MySQL), OpenSearch, RabbitMQ und Redis, um die Kompatibilität mit neuen Adobe Commerce-Versionen zu gewährleisten. Bei Pro-Projekten müssen Sie ein Adobe Commerce-Support-Ticket einreichen, um Services in Staging- und Produktionsumgebungen zu installieren oder zu aktualisieren.
- Aktualisieren Sie die `.magento.app.yaml` Datei mit neuen Einstellungen für Erweiterungspunkte und Umgebungsvariablen.
- Aktualisieren Sie Erweiterungen von Drittanbietern auf die neueste unterstützte Version.

{{upgrade-tip}}

{{pro-update-service}}

## Konfigurationsdateien

Vor dem Upgrade der Anwendung müssen Sie Ihre Projektkonfigurationsdateien aktualisieren, um Änderungen an den Standardkonfigurationseinstellungen für Adobe Commerce in der Cloud-Infrastruktur oder der Anwendung zu berücksichtigen. Die neuesten Standardeinstellungen finden Sie im [Magento-Cloud-GitHub-Repository](https://github.com/magento/magento-cloud).

### composer.json

Überprüfen Sie vor einem Upgrade immer, ob die Abhängigkeiten in der `composer.json` mit der Adobe Commerce-Version kompatibel sind.

So aktualisieren Sie die `composer.json` für Adobe Commerce Version 2.4.4 und höher**:

1. Fügen Sie dem `allow-plugins` Abschnitt die folgenden `config` hinzu:

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

## Umgebung-Backup

Es wird empfohlen, vor einem Upgrade eine Sicherungskopie der Instanz zu erstellen. Führen Sie die folgenden Schritte aus, um Ihre Integrations-, Staging- und Produktionsumgebungen zu sichern.

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

1. Legen Sie die [Versionsbeschränkung](overview.md#cloud-metapackage) für die Ziel-Upgrade-Version fest. Dieser Schritt ist nur erforderlich, wenn sich die Zielversion außerhalb der bestehenden Einschränkung befindet.

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Sie müssen die Versionsbeschränkungssyntax verwenden, um das `ece-tools` Paket erfolgreich zu aktualisieren. Die Versionsbeschränkung finden Sie in der `composer.json`-Datei für die Version der [Anwendungsvorlage](https://github.com/magento/magento-cloud/blob/master/composer.json) die Sie für das Upgrade verwenden.

1. Aktualisieren Sie Ihre `composer.json` mit der Commerce-Kernaktualisierungsversion.

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. Wenn Sie B2B verwenden, aktualisieren Sie Ihre `composer.json` mit der [unterstützten Version](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions) für Commerce.

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. Projektabhängigkeiten aktualisieren.

   ```bash
   composer update
   ```

1. Überprüfen Sie die aktuell angewendeten Patches:

   - Wenn im `m2-hotfixes`-Verzeichnis Patches installiert sind, [&#x200B; Sie ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case), und prüfen Sie gemeinsam mit dem Adobe Commerce-Support, welche Patches weiterhin auf die neue Version angewendet werden können. Entfernen Sie die nicht zutreffenden Patches aus dem `m2-hotfixes`.

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

### Erweiterungen aktualisieren

Überprüfen Sie Ihre Erweiterungs- und Modulseiten von Drittanbietern auf Marketplace oder anderen Unternehmens-Sites und überprüfen Sie, ob Adobe Commerce und Adobe Commerce in der Cloud-Infrastruktur unterstützt werden. Wenn Sie Erweiterungen und Module von Drittanbietern aktualisieren müssen, empfiehlt Adobe, eine neue Integrationsverzweigung zu verwenden, bei der die Erweiterungen deaktiviert sind.

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

1. [Überprüfen Sie die &#x200B;](../test/log-locations.md) und ermitteln Sie die Ursache des Problems.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
