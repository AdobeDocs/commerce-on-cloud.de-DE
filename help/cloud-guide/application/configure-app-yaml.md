---
title: Konfigurieren der Anwendungsbereitstellung
description: Erfahren Sie, wie Sie die Eigenschaften in der Anwendungskonfigurationsdatei konfigurieren, die steuern, wie  [!DNL Commerce]  Anwendung die Cloud-Umgebung erstellt und bereitstellt.
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '184'
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
