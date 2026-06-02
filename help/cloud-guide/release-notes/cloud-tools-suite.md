---
title: Versionshinweise für Cloud Tools Suite
description: Erfahren Sie mehr über die neuesten Verbesserungen an der Cloud Tools-Suite für Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Versionshinweise für Commerce Cloud Tools Suite

Diese Versionshinweise enthalten die neuesten Verbesserungen an der Cloud Tools-Suite für Commerce-Pakete, die für die Bereitstellung und Verwaltung von Adobe Commerce-Installationen und -Upgrades auf der Cloud-Plattform entwickelt wurden.

| Versionshinweise | Version | Beschreibung | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ECE-Tools-Paket](ece-tools-package.md) | 2002.2.11 | Eine Reihe von Skripten und Tools zur Verwaltung und Bereitstellung von Cloud-Projekten | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| [Cloud-Patches für Commerce](cloud-patches.md) | 1.1.14 | Eine Reihe von Patches, die die Integration aller Adobe Commerce-Versionen in Cloud-Umgebungen verbessern. Dieses Paket enthält Adobe Commerce-Patches und verfügbare Hotfixes, die bei der Bereitstellung von `ece-tools` angewendet werden | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.14) |
| [Cloud Docker für Commerce](cloud-docker.md) | 1.4.8 | Funktionen und Konfigurationsdateien für Docker-Images zum Bereitstellen von Adobe Commerce in einer lokalen Cloud-Umgebung | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| [Cloud-Komponenten von Commerce](cloud-components.md) | 1.1.4 | Erweiterte Adobe Commerce-Kernfunktionen für Sites, die in der Cloud-Infrastruktur bereitgestellt werden | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

Wenn Sie auf ECE-Tools 2002.1.0 oder höher aktualisieren, aktualisieren Sie automatisch auf die neuesten Versionen der anderen Pakete, bei denen es sich um Abhängigkeiten für das `ece-tools` Paket handelt. Siehe [Cloud-](../development/overview.md#cloud-metapackage)) für eine Liste von Abhängigkeiten.
