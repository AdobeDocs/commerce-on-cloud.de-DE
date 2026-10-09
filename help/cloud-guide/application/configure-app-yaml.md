---
title: Konfigurieren der Anwendungsbereitstellung
description: Erfahren Sie, wie Sie die Eigenschaften in der Anwendungskonfigurationsdatei konfigurieren, die steuern, wie die [!DNL Commerce] Anwendung die Cloud-Umgebung erstellt und bereitstellt.
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: 'https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
subfeature_v2:
  - id: adedf3b3-e153-47a3-ae73-b5d65067b544
    internal-label: Build system
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%
---
# Konfigurieren der Anwendungsbereitstellung

Die `.magento.app.yaml`-Datei steuert die Art und Weise, wie Ihre Anwendung erstellt und bereitstellt. Obwohl Adobe Commerce in der Cloud-Infrastruktur mehrere Programme pro Projekt unterstützt, verfügt ein Projekt normalerweise über eine einzige Anwendung mit der `.magento.app.yaml`-Datei im Stammverzeichnis des Repositorys.

Die `.magento.app.yaml` hat viele Standardwerte. Siehe [eine Beispiel-`.magento.app.yaml`-Datei](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Überprüfen Sie immer die `.magento.app.yaml` für Ihre installierte Version. Diese Datei kann sich in Cloud-Infrastrukturversionen von Adobe Commerce unterscheiden.

Verwenden Sie die `.magento.app.yaml`-Datei, um die folgenden Konfigurationswerte zu definieren:

- [Properties](properties.md) - Definieren Sie Eigenschaftswerte für die Anwendungsinstanz.
- [Variables-Eigenschaft](variables-property.md) - Umgebungsvariablen überprüfen, die für die Version der [!DNL Commerce]-Anwendung erforderlich sind.
- [PHP settings](php-settings.md)—Konfigurieren von PHP-Laufzeitoptionen.
- [Cache für statische Dateien festlegen](set-cache.md) - Festlegen der Cache-TTL für Ihre Medien- und statischen Dateien.

>[!NOTE]
>
>Die `.magento.app.yaml` wird lokal oder im Git-Repository verwaltet. Die Konfiguration wird nur für die Zwecke des Bereitstellungs- und Build-Prozesses gelesen und wird nach Abschluss der Bereitstellung entfernt, sodass sie nicht auf dem Server zu finden ist.

