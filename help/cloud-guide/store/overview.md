---
title: Überblick über Store-Optionen und Konfigurationsverwaltung
description: Passen Sie Ihren Adobe Commerce Store in der Cloud-Infrastruktur an.
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '191'
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
