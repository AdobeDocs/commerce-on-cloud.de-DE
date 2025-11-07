---
title: Versionshinweise für Cloud Tools Suite
description: Erfahren Sie mehr über die neuesten Verbesserungen an der Cloud Tools-Suite für Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: 4f96ed89edbbc148c5558050368d8366bd89053a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# Versionshinweise für Commerce Cloud Tools Suite

Diese Versionshinweise enthalten die neuesten Verbesserungen an der Cloud Tools-Suite für Commerce-Pakete, die für die Bereitstellung und Verwaltung von Adobe Commerce-Installationen und -Upgrades auf der Cloud-Plattform entwickelt wurden.

| Versionshinweise | Version | Beschreibung | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ECE-Tools-Paket](ece-tools-package.md) | 2002,2,8 | Eine Reihe von Skripten und Tools zur Verwaltung und Bereitstellung von Cloud-Projekten | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.8) |
| [Cloud-Patches für Commerce](cloud-patches.md) | 1.1.11 | Eine Reihe von Patches, die die Integration aller Adobe Commerce-Versionen in Cloud-Umgebungen verbessern. Dieses Paket enthält Adobe Commerce-Patches und verfügbare Hotfixes, die bei der Bereitstellung von `ece-tools` angewendet werden | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.11) |
| [Cloud Docker für Commerce](cloud-docker.md) | 1,4,5 | Funktionen und Konfigurationsdateien für Docker-Images zum Bereitstellen von Adobe Commerce in einer lokalen Cloud-Umgebung | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.5) |
| [Cloud-Komponenten von Commerce](cloud-components.md) | 1.1.3 | Erweiterte Adobe Commerce-Kernfunktionen für Sites, die in der Cloud-Infrastruktur bereitgestellt werden | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.3) |

Wenn Sie auf ECE-Tools 2002.1.0 oder höher aktualisieren, aktualisieren Sie automatisch auf die neuesten Versionen der anderen Pakete, bei denen es sich um Abhängigkeiten für das `ece-tools` Paket handelt. Siehe [Cloud-](../development/overview.md#cloud-metapackage)) für eine Liste von Abhängigkeiten.

Die neue Version von `ece-tools` (2002.2.0) ist nur in PHP Version 8.1 und höher (8.2, 8.3) verfügbar. Ältere PHP-Versionen waren veraltet (7.4, 7.3, 7.2). Sie können frühere Versionen von `ece-tools` mit alten PHP-Versionen verwenden.

