---
title: Projekt auf ECE-Tools aktualisieren
description: Erfahren Sie, wie Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt aktualisieren, um das ECE-Tools-Paket zu verwenden und die neuesten Fehlerbehebungen und Funktionen zu nutzen.
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Upgrade des Projekts auf das ECE-Tools-Paket

Adobe hat die `magento/magento-cloud-configuration`- und `magento/ece-patches`-Pakete zugunsten des `ece-tools`-Pakets verworfen, was viele Cloud-Prozesse vereinfacht. Wenn Sie ein älteres Adobe Commerce in einem Cloud-Infrastrukturprojekt verwenden, das _nicht_ das `ece-tools` enthält, müssen Sie für Ihr Projekt einen einmaligen manuellen Prozess _Upgrade_ durchführen.

>[!WARNING]
>
>Wenn Ihr Projekt das `ece-tools` enthält, können Sie das folgende Upgrade überspringen. Rufen Sie zur Überprüfung die [!DNL Commerce] Version mit dem Befehl `php vendor/bin/ece-tools -V` im lokalen Projektstammverzeichnis ab.

Für diesen Projekt-Upgrade-Prozess müssen Sie die `magento/magento-cloud-metapackage` Versionsbeschränkung in der `composer.json`-Datei im Stammverzeichnis aktualisieren. Diese Einschränkung ermöglicht Aktualisierungen für Adobe Commerce in Cloud-Infrastruktur-Metapaketen, einschließlich der Entfernung veralteter Pakete, ohne die aktuelle Adobe Commerce-Version zu aktualisieren.

{{upgrade-tip}}

## Veraltete Pakete entfernen

Bevor Sie ein Upgrade für die Verwendung des `ece-tools` durchführen, überprüfen Sie die `composer.lock`-Datei auf die folgenden veralteten Pakete:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Metapaket aktualisieren

Jede Adobe Commerce-Version erfordert eine andere Einschränkung, die auf den folgenden Elementen basiert:

```
>=current_version <next_version
```

- Geben Sie `current_version` die zu installierende Adobe Commerce-Version an.
- Geben Sie `next_version` die nächste Patch-Version nach dem in `current_version` angegebenen Wert an.

Wenn Sie Adobe Commerce `2.3.5-p2` installieren möchten, setzen Sie `current_version` auf `2.3.5` und die `next_version` auf `2.3.6`. Mit der `">=2.3.5 <2.3.6"` wird das neueste verfügbare Paket für 2.3.5 installiert.

Die neueste Metapaket-Einschränkung finden Sie immer in der [`magento-cloud` Vorlage](https://github.com/magento/magento-cloud/blob/master/composer.json).

Im folgenden Beispiel wird eine Einschränkung für das Adobe Commerce on Cloud Infrastructure-Metapaket auf eine beliebige Version festgelegt, die größer oder gleich der aktuellen Version 2.4.7 und kleiner als die nächste Version 2.4.8 ist:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Aktualisieren des Projekts

Um Ihr Projekt für die Verwendung des `ece-tools`-Pakets zu aktualisieren, müssen Sie das Metapaket und die `.magento.app.yaml` Hooks-Eigenschaften aktualisieren und eine Composer-Aktualisierung durchführen.

**So aktualisieren Sie das Projekt auf ECE-Tools**:

1. Aktualisieren Sie die `magento/magento-cloud-metapackage` Versionsbeschränkung in der `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Aktualisieren Sie das Metapaket.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Ändern Sie die Hook-Befehle in der `magento.app.yaml`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Suchen Sie nach den [veralteten Paketen](#remove-deprecated-packages) und entfernen Sie diese. Die veralteten Pakete können ein erfolgreiches Upgrade verhindern.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Möglicherweise muss das `ece-tools` aktualisiert werden.

   ```bash
   composer update magento/ece-tools
   ```

1. Fügen Sie die Code-Änderungen hinzu und übertragen Sie sie. In diesem Beispiel wurden die folgenden Dateien aktualisiert:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Übertragen Sie Ihre Code-Änderungen auf den Remote-Server und führen Sie diese Verzweigung mit der `integration` Verzweigung zusammen.

   ```bash
   git push origin <branch-name>
   ```
