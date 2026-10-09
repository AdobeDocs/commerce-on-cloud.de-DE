---
title: Wiederherstellung nach Komponentenausfall
description: Erfahren Sie, wie Sie eine Wiederherstellung durchführen können, wenn eine Komponente in Adobe Commerce in der Cloud-Infrastruktur nicht ordnungsgemäß bereitgestellt werden kann.
feature: Cloud, Deploy
exl-id: f5e79366-5548-40dd-ac2a-56dc84c5d4e2
TQID: 'https://experienceleague.adobe.com/1krotAWilGcnp3OUMk-fl5iHFObGpWFUC8EpYSPjQfU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
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
