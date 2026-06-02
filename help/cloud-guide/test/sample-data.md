---
title: Beispieldaten
description: Erfahren Sie, wie Sie Beispieldaten mit Adobe Commerce in der Cloud-Infrastruktur installieren.
exl-id: 59e23cfa-d364-4e70-8a86-644c339778cc
TQID: https://experienceleague.adobe.com/hEowDeOnXfkdlB-GCNRylKYrJLtXITR11-zsveyIA0Y
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 205
ht-degree: 0%

---

# Beispieldaten

Wenn Sie beim Entwickeln Ihres Stores Beispieldaten benötigen, können Sie Beispieldaten installieren. Diese Daten simulieren einen aktiven Adobe Commerce-Store mit Kunden, Produkten und anderen Daten. Diese Beispieldaten funktionieren am besten mit einer neuen Vorlageninstallation von Adobe Commerce auf der Cloud-Infrastruktur.

Installieren Sie als Best Practice Beispieldaten in Entwicklungs- und Integrationsumgebungen. Wenn Sie Beispieldaten in der Staging- oder Produktionsumgebung verwenden, müssen Sie [&#x200B; Informationen &#x200B;](#reset-or-uninstall-sample-data) Produkte vor der Live-Schaltung entfernen.

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
