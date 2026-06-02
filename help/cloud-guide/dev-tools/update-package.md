---
title: Aktualisieren des ECE-Tools-Pakets
description: Erfahren Sie, wie Sie das ECE-Tools-Paket aktualisieren, um die neuesten Fehlerbehebungen und Funktionen für Adobe Commerce in der Cloud-Infrastruktur nutzen zu können.
feature: Cloud, Upgrade
exl-id: acb4fd0d-6ffb-4094-8dd4-83bb7735d64f
TQID: https://experienceleague.adobe.com/WqcVa0BJN0ot6yg2unhzeEB8nkjKXM7o26tOwmrg5mU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 172
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
