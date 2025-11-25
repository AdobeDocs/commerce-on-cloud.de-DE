---
title: Abwärtsinkompatible Änderungen
description: Erfahren Sie mehr über die Abwärtskompatibilität beim Upgrade vorhandener Cloud-Projekte.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Abwärtsinkompatible Änderungen

Abwärtsinkompatible Änderungen erfordern möglicherweise, dass Sie die Cloud-Konfiguration und Prozesse für bestehende Cloud-Projekte anpassen, wenn Sie auf die neueste Version des `ece-tools`-Pakets oder andere Cloud-Tools-Pakete für Commerce aktualisieren.

## Änderungen an `ece-tools` Paket

Einige Funktionen, die zuvor im `ece-tools` enthalten waren, werden jetzt in separaten Paketen bereitgestellt. Diese Pakete sind Composer-Abhängigkeiten für `ece-tools`, die automatisch installiert und aktualisiert werden, wenn Sie ece-tools installieren oder aktualisieren.

Die neue Architektur sollte keine Auswirkungen auf Ihre Installations- oder Aktualisierungsprozesse haben. Möglicherweise müssen Sie jedoch bei der Arbeit mit Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt einige Befehlssyntax und Prozesse ändern. Weitere Informationen finden Sie in den folgenden abwärtsinkompatiblen Änderungen und in den [Versionshinweisen zur Cloud-Tools-Suite](cloud-tools-suite.md).

### Änderungen der Dienstversionsanforderungen

Wir haben die Mindestanforderung für die PHP-Version von 7.0.x auf 7.1.x für Cloud-Projekte geändert, die `ece-tools` v2002.1.0 und höher verwenden. Wenn Ihre Umgebungskonfiguration PHP 7.0 angibt, aktualisieren Sie die [php-Konfiguration](../application/php-settings.md) in der `.magento.app.yaml`.

>[!WARNING]
>
>Aufgrund der Änderung der PHP-Versionsanforderung unterstützt `ece-tools` 2002.1.0 nur Adobe Commerce in Cloud-Infrastrukturprojekten, die Adobe Commerce 2.1.15 oder höher ausführen. Wenn Ihr Projekt eine frühere Version verwendet, müssen Sie [aktualisieren](../development/commerce-version.md) bevor Sie auf `ece-tools` 2002.1.0 aktualisieren.

### Änderungen an der Umgebungskonfiguration

Die folgende Tabelle enthält Informationen zu Umgebungsvariablen und anderen Umgebungskonfigurationsdateien, die in `ece-tools` v2002.1.0 entfernt wurden oder veraltet sind.

| Element | Ersatz |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` | Keine. Jetzt erstellt der Build immer einen Symlink zum statischen Inhaltsverzeichnis `pub/static`. |
| `build_options.ini` | Verwenden Sie die [`.magento.env.yaml`](../application/configure-app-yaml.md)-Datei, um Umgebungsvariablen zu konfigurieren, um Aktionen zum Erstellen und Bereitstellen in allen Ihren Umgebungen zu verwalten.<p>Wenn Sie eine Cloud-Umgebung erstellen, die die `build_options.ini` enthält, schlägt der Build fehl. |

### CLI-Befehlsänderungen

Die folgende Tabelle fasst die CLI-Befehlsänderungen in ECE-Tools v2002.1.0 zusammen, die möglicherweise eine Aktualisierung von Befehlen oder Skripten erfordern.

| Befehl | Ersatz |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

In früheren ECE-Tools-Versionen konnten Sie die `m2-ece-build` und `m2-ece-deploy`-Befehle verwenden, um Bereitstellungs-Hooks in der `.magento.app.yaml`-Datei zu konfigurieren. Überprüfen Sie beim Aktualisieren auf Version 2002.1.0 die `hooks` in der `.magento.app.yaml` auf veraltete Befehle, und ersetzen Sie sie bei Bedarf.

## Änderungen an Cloud-Patches

- **Heruntergeladene Patches entfernen**-Das `magento/magento-cloud-patches` Paket bündelt alle auf der Seite „Software[Downloads“ verfügbaren Patches &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html?lang=de) und wendet sie automatisch bei der Bereitstellung in der Cloud an. Um Patch-Konflikte nach einem Upgrade auf ECE-Tools 2002.1.0 oder höher zu vermeiden, entfernen Sie alle von Adobe bereitgestellten Patches, die Sie heruntergeladen und Ihrem Projekt manuell hinzugefügt haben.

- **Befehl zum Anwenden von Patches wurde aktualisiert**-Der Befehl zum Anwenden von Patches wurde aus dem `vendor/bin/ece-tools` in das `vendor/bin/ece-patches`-Verzeichnis verschoben. Wenn Sie diesen Befehl verwenden, um Patches manuell anzuwenden, verwenden Sie den neuen Pfad.

  > Patches manuell anwenden

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker-Änderungen

- **Die Mindestanforderung für die PHP-Version lautet jetzt PHP 7.1**-Wenn Ihr Cloud Docker für Commerce-Host eine frühere Version ausführt, führen Sie ein Upgrade auf PHP v7.1 oder höher durch.

- **Befehlsänderungen von Cloud Docker für Commerce**-

   - **Aktualisieren von Cloud Docker für Commerce-Befehle für Docker-Build-Vorgänge**-Wir haben die Cloud Docker für Commerce-Befehle aus dem `vendor/bin/ece-tools` in das `vendor/bin/ece-docker`-Verzeichnis verschoben. Aktualisieren Sie Ihre Skripte und Befehle, um den neuen Pfad zu verwenden.

     Nach dem Upgrade auf `ece-tools` 2002.1.0 verwenden Sie den folgenden Befehl, um verfügbare `ece-docker` anzuzeigen.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Aktualisieren der Cloud-Docker-**-Wir haben den Pfad zur Befehlsdatei von `./bin/docker` in `./bin/magento-docker` umbenannt. Aktualisieren Sie Ihre Skripte und Befehle, um den neuen Pfad zu verwenden.

   - **Cron-Container ist nicht mehr in der standardmäßigen Docker-Konfiguration enthalten**-Jetzt müssen Sie die `--with-cron`-Option zum `ece-docker build:compose`-Befehl hinzufügen, um den Cron-Container in die Docker-Umgebungskonfiguration aufzunehmen. Siehe [Verwalten von Cron-Aufträgen](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs) im Handbuch _Cloud Docker for Commerce_.

     Skripte, die zuvor Container mit Cron-Aufträgen generiert haben, sind jetzt ohne den Cron-Container.

   - **Verwenden temporärer Container**-In früheren Versionen wurden die von `bin/magento-docker` Befehlsvorgängen erstellten Container nicht entfernt, sodass Sie sie für andere Vorgänge verwenden konnten. Jetzt entfernen die `magento-docker`-Befehle alle Container, die sie nach Abschluss des Befehls erstellen.

     Wenn Sie einen durch einen Docker-Compose-Vorgang erstellten Container beibehalten möchten, verwenden Sie den `docker-compose run`-Befehl anstelle des `bin/magento-docker`-Befehls.

   - **Ausführen von Hooks nach der Bereitstellung**-Der `cloud-deploy` Befehl führt keine Hooks nach der Bereitstellung mehr aus. Verwenden Sie den neuen `cloud-post-deploy`-Befehl, um nach der Bereitstellung Hooks auszuführen. Aktualisieren Sie Ihre Skripte, um den Befehl zum Ausführen von Hooks nach der Bereitstellung hinzuzufügen.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Wenn Sie `docker-compose`-Befehle direkt verwenden, führen Sie alternativ den `docker-compose run deploy cloud-post-deploy`-Befehl nach dem Bereitstellungsbefehl aus.

- **Datenbank aktualisieren**-Der Datenbank-Container ist jetzt im `magento-db` persistenten Docker-Volume gespeichert. Wenn Sie die Docker-Umgebung aktualisieren, wird die Datenbank nicht mehr automatisch gelöscht. Verwenden Sie bei Bedarf einen der folgenden Befehle, um ihn manuell zu entfernen.

   - Entfernen Sie den `magento-db`:

     ```bash
     docker volume rm magento-db
     ```

   - Entfernen Sie alle zugehörigen Volumes, wenn Sie die Docker-Container herunterfahren:

     ```bash
     docker-compose down -v
     ```

- **Dateisynchronisierungseinstellungen für Archiv- und Sicherungsdateien überschreiben**-Archiv- und Sicherungsdateien mit den folgenden Erweiterungen werden bei Verwendung von docker-sync oder mutagen nicht mehr synchronisiert: SQL, GZ, ZIP und BZ2. Sie können die standardmäßige Dateisynchronisierung für diese Dateitypen überschreiben, indem Sie die Datei so umbenennen, dass sie mit einer anderen Erweiterung endet. Beispiel: `synchronize-me.zip-backup`
