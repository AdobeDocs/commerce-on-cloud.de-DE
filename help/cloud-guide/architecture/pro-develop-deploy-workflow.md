---
title: Pro-Projekt-Workflow
description: Erfahren Sie, wie Sie die Pro-Entwicklungs- und Bereitstellungs-Workflows verwenden.
feature: Cloud, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Pro-Projekt-Workflow

Das Pro-Projekt umfasst ein einzelnes Git-Repository mit einer globalen `master` und drei Hauptumgebungen:

1. **Produktion** Umgebung für das Starten und Verwalten der Live-Site
1. **Staging**-Umgebung zum Testen mit allen Services
1. **Integration** Umgebung für Entwicklung und Tests

![Pro Umgebungsliste](../../assets/pro-environments.png)

Diese Umgebungen sind `read-only` und akzeptieren bereitgestellte Code-Änderungen von Verzweigungen, die aus Ihrem lokalen Arbeitsbereich gepusht werden. Siehe [Pro Architektur](pro-architecture.md) für einen vollständigen Überblick über die Pro-Umgebungen. Siehe [[!DNL Cloud Console]](../project/overview.md#cloud-console) für einen Überblick über die Liste der Pro-Umgebungen in der Projektansicht.

Die folgende Grafik zeigt den Pro-Workflow zum Entwickeln und Bereitstellen , der einen einfachen, Git-verzweigenden Ansatz verwendet. Sie [ Code ](#development-workflow) einer aktiven Verzweigung auf Grundlage der `integration`-Umgebung entwickeln, indem Sie Code-Änderungen _pushen_ und _abrufen_ auf und von Ihrer entfernten, aktiven Verzweigung aus. Sie stellen verifizierten Code bereit _indem Sie die_-Verzweigung mit der Basisverzweigung zusammenführen, wodurch ein automatisierter [Build- und Bereitstellungs](#deployment-workflow)-Prozess für diese Umgebung aktiviert wird.

![Allgemeine Ansicht des Entwicklungs-Workflows der Pro-Architektur](../../assets/pro-dev-workflow.png)

## Entwicklungs-Workflow

Die Integrationsumgebung bietet eine zentrale `integration`-Verzweigung mit dem Code Ihrer Adobe Commerce on Cloud-Infrastruktur. Sie können eine zusätzliche Verzweigung für die aktive Umgebung erstellen. Dies ermöglicht bis zu zwei aktive Verzweigungen, die in Platform as a Service (PaaS)-Containern bereitgestellt werden. Die Anzahl der inaktiven Umgebungen ist unbegrenzt.

{{enhanced-integration-envs}}

Die Projektumgebungen unterstützen einen flexiblen, kontinuierlichen Integrationsprozess. Klonen Sie zunächst die `integration` Verzweigung in den lokalen Projektordner. Erstellen Sie eine oder mehrere Verzweigungen, entwickeln Sie neue Funktionen, konfigurieren Sie Änderungen, fügen Sie Erweiterungen hinzu und stellen Sie Aktualisierungen bereit:

- **Änderungen** von `integration` abrufen

- **Verzweigung** aus `integration`

- **Entwickeln** von Code auf einer lokalen Workstation, einschließlich [!DNL Composer]

- **Push**-Code ändert sich in Remote und validiert

- **Zusammenführen** zum `integration` und Testen

Mit einer entwickelten Code-Verzweigung und den entsprechenden Konfigurationsdateien können Ihre Code-Änderungen zur umfassenderen Testung in die `integration`-Verzweigung zusammengeführt werden. Die `integration` Umgebung eignet sich auch am besten für:

- **Integrieren von Drittanbieterdiensten** - Nicht alle Dienste sind in der PaaS-Umgebung verfügbar.

- **Erstellen von Konfigurationsverwaltungsdateien** - Einige Konfigurationseinstellungen sind _schreibgeschützt_ in einer bereitgestellten Umgebung.

- **Store konfigurieren** - Sie sollten alle Store-Einstellungen mithilfe der Integrationsumgebung vollständig konfigurieren. Sie finden die **Store-Admin** URL) in der Ansicht _Integration_ in der _[!DNL Cloud Console]_.

## Bereitstellungs-Workflow

Jedes Mal, wenn Sie Code von Ihrer lokalen Workstation in die Remote-Umgebung pushen oder Code in eine Umgebungsverzweigung zusammenführen, generieren die Build- und Bereitstellungsskripte neuen Code und stellen die konfigurierten Services in der Remote-Umgebung bereit.

Skript-Aktionen erstellen:

- Site in der Zielumgebung wird während eines Builds weiterhin ausgeführt

- Überprüfen und Ausführen von Adobe Commerce auf Cloud-Infrastruktur-Patches und -Hotfixes

- Kompilieren von Code mit einem Build und Bereitstellen von Protokollen

- Überprüfen Sie, ob die Konfigurationsverwaltung funktioniert, und die Bereitstellung statischer Inhalte findet in dieser Phase statt.

- Erstellen oder verwenden Sie einen Slug mit unverändertem Code, um den Prozess zu beschleunigen

- Bereitstellung aller Backend-Services und -Programme

Skriptaktionen bereitstellen:

- Platzieren Sie die Site in der Zielumgebung in einen _Wartungsmodus_.

- Statischen Inhalt bereitstellen, wenn er beim Build nicht abgeschlossen wurde

- Installieren oder Aktualisieren von Adobe Commerce in der Cloud-Infrastruktur

- Konfigurieren des Routings für Traffic

Nach dem Build- und Bereitstellungsprozess wird Ihr Store mit Ihren neuesten Code-Änderungen und Konfigurationen wieder online geschaltet. Siehe [Bereitstellungsprozess](../deploy/process.md).

### Zusammenführen mit Integration

Kombinieren Sie alle überprüften Code-Änderungen, indem Sie Ihre aktive Entwicklungsverzweigung mit der `integration` zusammenführen. Sie können alle Ihre Änderungen auf der `integration`-Verzweigung testen, bevor Sie Änderungen in die Staging-Umgebung hochstufen.

### Mit Staging zusammenführen

Staging ist eine Vorproduktionsumgebung, die alle Services und Einstellungen so nah wie möglich an der Produktionsumgebung bereitstellt. Übertragen Sie Code-Änderungen immer von der `integration` Umgebung in die `staging` Umgebung, damit Sie alle Services gründlich testen können. Wenn Sie die Staging-Umgebung zum ersten Mal verwenden, müssen Sie Services wie [Fastly CDN](../cdn/fastly.md) und [New Relic](../monitor/new-relic-service.md) konfigurieren. Konfigurieren Sie Zahlungs-Gateways, Versand, Benachrichtigungen und andere wichtige Services mit Sandbox- oder Testberechtigungen.

Es ist am besten, jeden Service gründlich zu testen, Ihre Leistungstesttools zu überprüfen und UAT-Tests als Administrator und als Kunde durchzuführen, bis Sie das Gefühl haben, dass Ihr Store für die Produktionsumgebung bereit ist. Siehe [Bereitstellen Ihres Stores](../deploy/staging-production.md).

{{second-staging}}

### Mit Produktion fusionieren

Führen Sie nach gründlichen Tests in der Staging-Umgebung eine Verbindung zur Produktionsumgebung her und testen Sie sie sorgfältig mit Live-Anmeldeinformationen. Zum Zeitpunkt des Starts der Produktionssite müssen Kunden in der Lage sein, Käufe abzuschließen, und Administratoren müssen in der Lage sein, den Live Store zu verwalten. Unter den folgenden Themen finden Sie eine detaillierte, übersichtliche Anleitung zur Bereitstellung Ihres Stores und zur Live-Schaltung:

- [Bereitstellen des Stores](../deploy/staging-production.md)
- [Site-Launch](../launch/overview.md)

### Zusammenführen mit dem globalen Stamm

Senden Sie immer eine Kopie des Produktions-Codes an die globale `master`, falls eine dringende Notwendigkeit besteht, die Produktionsumgebung zu debuggen, ohne die Services zu unterbrechen.

Erstellen **keine** aus der globalen `master`. Verwenden Sie die `integration` Verzweigung, um neue aktive Verzweigungen für die Entwicklung und Fehlerbehebungen zu erstellen.
