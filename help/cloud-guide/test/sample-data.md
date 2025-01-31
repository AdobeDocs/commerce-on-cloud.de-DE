---
title: Beispieldaten
description: Erfahren Sie, wie Sie Beispieldaten mit Adobe Commerce in der Cloud-Infrastruktur installieren.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Beispieldaten

Wenn Sie beim Entwickeln Ihres Stores Beispieldaten benötigen, können Sie Beispieldaten installieren. Diese Daten simulieren einen aktiven Adobe Commerce-Store mit Kunden, Produkten und anderen Daten. Diese Beispieldaten funktionieren am besten mit einer neuen Vorlageninstallation von Adobe Commerce auf der Cloud-Infrastruktur.

Installieren Sie als Best Practice Beispieldaten in Entwicklungs- und Integrationsumgebungen. Wenn Sie Beispieldaten in der Staging- oder Produktionsumgebung verwenden, müssen Sie [ Informationen ](#reset-or-uninstall-sample-data) Produkte vor der Live-Schaltung entfernen.

## Installieren von Beispieldaten

So installieren Sie Beispieldaten:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Geben Sie im Stammverzeichnis den folgenden Befehl ein, um Beispieldaten hinzuzufügen:

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. Warten Sie, bis die Komponenten aktualisiert wurden.

1. Bestätigen und Übertragen der Änderungen:

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Warten Sie, bis das Projekt bereitgestellt wird.

1. Überprüfen Sie, ob die Installation erfolgreich war, indem Sie in der Integrationsumgebung zu Ihrer Storefront-Seite gehen. Sie können den URL-Link zur Storefront über die [!DNL Cloud Console] finden.

1. Erstellen Sie einen Schnappschuss Ihrer Umgebung:

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

Sie können mit dem Testen Ihrer Entwicklung mit Live-Daten beginnen!

## Beispieldaten zurücksetzen oder deinstallieren

Sie können Beispieldaten mit demselben Verfahren zurücksetzen oder entfernen, das für die Installation der Beispieldaten verwendet wurde:

- Beispieldaten löschen: `./bin/magento sampledata:remove`
- Beispieldaten zurücksetzen: `./bin/magento sampledata:reset`
