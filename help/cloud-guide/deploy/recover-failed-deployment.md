---
title: Wiederherstellung nach Komponentenausfall
description: Erfahren Sie, wie Sie eine Wiederherstellung durchführen können, wenn eine Komponente in Adobe Commerce in der Cloud-Infrastruktur nicht ordnungsgemäß bereitgestellt werden kann.
feature: Cloud, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Wiederherstellung nach Komponentenausfall

In diesem Abschnitt wird beschrieben, wie Sie wiederherstellen können, wenn eine Komponente nicht ordnungsgemäß bereitgestellt werden kann. Typische Beispiele sind Komponenten mit Abhängigkeiten, die von Ihrer Remote-Umgebung nicht erfüllt werden, wie inkompatible PHP-Versionen.

Sie können eine fehlgeschlagene Bereitstellung auf eine der folgenden Arten wiederherstellen:

- [Wiederherstellen einer Sicherung](../storage/snapshots.md#restore-a-snapshot)
- Projekt und Code von vorherigen Änderungen bereinigen und erneut bereitstellen

## Bereinigen, Entfernen und erneutes Bereitstellen

Um eine Bereinigung von der vorherigen Bereitstellung durchzuführen, identifizieren Sie die Komponente, die hinzugefügt oder aktualisiert wurde, und entfernen Sie sie dann. Melden Sie sich zunächst bei der Remote-Umgebung an und löschen Sie manuell den Inhalt des `var`. Entfernen Sie dann die Komponente aus der `composer.json` und stellen Sie die Umgebung erneut bereit.

**Bereinigen der `var` Verzeichnisse**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Löschen Sie die `var`.

   ```shell
   rm -rf var/*
   ```

1. Abmelden.

**Entfernen der Komponente**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Löschen Sie den Cache.

   ```bash
   composer clear-cache
   ```

1. Entfernen Sie die Komponente aus der `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Wenn die folgende Meldung angezeigt wird, müssen Sie nichts weiter tun:

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Warten Sie, während die Abhängigkeiten aktualisiert werden.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Weitere Informationen zum Wiederherstellen einer Umgebung ohne Backup finden Sie in [Wiederherstellen einer Umgebung](../development/restore-environment.md).

{{stuck-deployment-tip}}
