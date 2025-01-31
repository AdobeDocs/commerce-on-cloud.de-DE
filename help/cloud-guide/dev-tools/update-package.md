---
title: Aktualisieren des ECE-Tools-Pakets
description: Erfahren Sie, wie Sie das ECE-Tools-Paket aktualisieren, um die neuesten Fehlerbehebungen und Funktionen für Adobe Commerce in der Cloud-Infrastruktur nutzen zu können.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Aktualisieren des ECE-Tools-Pakets

Eine Aktualisierung des `ece-tools` aktualisiert auch die anderen [Cloud-Tools-Suite für Commerce](../release-notes/cloud-tools-suite.md)-Pakete, die Abhängigkeiten für `ece-tools` sind. Daher müssen Sie eine Version von Adobe Commerce in der Cloud-Infrastruktur verwenden, die das `ece-tools` unterstützt.

{{ece-tools-package}}

**Voraussetzungen**:

- Bevor Sie `ece-tools` aktualisieren, lesen Sie die [Versionshinweise zu Cloud-Tools für Commerce](../release-notes/cloud-tools-suite.md).
- Wenn Sie von `ece-tools` 2002.0.22 oder früher auf 2002.1.0 aktualisieren, überprüfen Sie [Abwärtsinkompatible Änderungen](../release-notes/backward-incompatible-changes.md) und nehmen Sie die erforderlichen Änderungen an Ihrem Adobe Commerce in Cloud-Infrastrukturprojekt vor.
- Überprüfen Sie [Upgrades und Patches](../development/commerce-version.md#upgrade-from-older-versions) um die ECE-Tools-Versionen zu ermitteln, die mit Ihrem Adobe Commerce on Cloud Infrastructure-Projekt kompatibel sind.

{{upgrade-tip}}

**So aktualisieren Sie das `ece-tools`-Paket**:

1. Führen Sie auf Ihrer lokalen Workstation ein Update mit Composer durch.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Wenn Sie nicht über `ece-tools` Version 2002.0.8 hinaus aktualisieren können, lesen Sie [Upgrade-Projekt für das ECE-Tools-Paket](install-package.md).

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Führen Sie nach der Testvalidierung diese Verzweigung mit der Integrationsverzweigung zusammen.
