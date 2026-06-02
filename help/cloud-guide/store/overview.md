---
title: Überblick über Store-Optionen und Konfigurationsverwaltung
description: Passen Sie Ihren Adobe Commerce Store in der Cloud-Infrastruktur an.
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 193
ht-degree: 0%

---

# Überblick über Store-Optionen und Konfigurationsverwaltung

Es gibt viele Möglichkeiten, Ihren Store anzupassen, z. B. das Hinzufügen eines benutzerdefinierten Designs, das Installieren einer Erweiterung oder das Erzwingen einer bestimmten Konfiguration in Cloud-Infrastrukturumgebungen. Sie können Einstellungen für bestimmte Services direkt in Staging- und Produktionsumgebungen konfigurieren. Sie können mehrere Websites und Stores einrichten. Die Store-Konfiguration hilft Ihnen beim Konfigurieren dieser Optionen auf Ihrer lokalen Workstation und beim Bereitstellen spezifischer Einstellungen in allen Umgebungen.

Um auf Ihre Storefront zuzugreifen, verwenden Sie den Befehl `magento-cloud url` und beantworten Sie die Eingabeaufforderungen. Oder Sie finden die URL in der [!DNL Cloud Console] unter **Zugriffssite**.

## Store-Optionen konfigurieren

Zu den Store-Optionen gehören die folgenden:

* [Business-to-Business-Modul (B2B)](b2b-module.md)
* [Benutzerdefinierte Designs](custom-theme.md)
* [Erweiterungen](extensions.md)
* [Mehrere Sites](multiple-sites.md)
* [Zahlungsdienste](paypal.md)

## Konfigurieren von Services und Integrationen

Es gibt spezifische [Konfigurationsdateien](../environment/overview.md) die bestimmte Bereitstellungsverhaltensweisen in Remote-Umgebungen verwalten. Sie können diese Themen separat überprüfen:

* [Anwendungsbereitstellung](../application/configure-app-yaml.md)
* [Erstellen und Bereitstellen von Aktionen für Umgebungen](../environment/configure-env-yaml.md)
* [Routen eingehender Anfragen](../routes/routes-yaml.md)
* [Unterstützte Services](../services/services-yaml.md)

## Konfigurationsverwaltung

Verwenden Sie nach der Konfiguration der Store-Optionen, -Services und -Integrationen die Konfigurationsverwaltung, um diese Konfigurationen konsistent und mit minimaler Ausfallzeit für alle Umgebungen bereitzustellen. Siehe [Konfigurationsverwaltung](store-settings.md).
