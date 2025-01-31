---
title: Benutzerdefiniertes Design
description: Erfahren Sie, wie Sie ein benutzerdefiniertes Design mit Adobe Commerce in der Cloud-Infrastruktur installieren.
feature: Cloud, Themes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Benutzerdefiniertes Design

Sie können ein oder mehrere Designs installieren, die für einen oder alle Ihre Stores und Sites in Ihrem Projekt verwendet werden sollen. Designs umfassen mehrere statische Dateien, einschließlich Bilder, Schriftarten, CSS, JavaScript, PHP und mehr, um Ihre Stores vollständig zu gestalten. Sie können das Design hinzufügen, indem Sie entweder den Code in das Dateisystem extrahieren oder den Composer verwenden.

## Manuelles Installieren eines Designs

Um ein Design manuell zu installieren, müssen Sie den Code des Designs in einem komprimierten Archiv oder in einer Verzeichnisstruktur ähnlich der folgenden haben:

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**So installieren Sie ein Design manuell**:

1. Kopieren Sie den Code des Designs unter `<Project root dir>/app/design/frontend` für ein Storefront-Design oder `<Project root dir>/app/design/adminhtml` für ein Admin-Design. Stellen Sie sicher, dass der Ordner der obersten Ebene `<VendorName>` ist. Andernfalls wird das Design nicht ordnungsgemäß installiert.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. Bestätigen Sie, dass das Design an die richtige Stelle kopiert wurde.

   * Storefront-Design: `ls <project-root>/app/design/frontend`
   * Admin-Design: `ls <project-root>/app/design/adminhtml`

   Es folgt ein Beispiel:

   ExampleTheme Adobe Commerce

1. Dateien hinzufügen und übertragen.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. Übertragen Sie die Dateien in Ihre Verzweigung.

   ```bash
   git push origin <branch name>
   ```

1. Warten Sie, bis die Bereitstellung abgeschlossen ist.
1. Melden Sie sich beim Administrator an.
1. Klicken Sie **Inhalt** > Design > **Designs**.

   Das Design wird im rechten Bereich angezeigt.

## Installieren eines Designs mithilfe von Composer

Das Installieren eines Designs mit Composer entspricht dem Installieren einer anderen Erweiterung mit Composer. Siehe [Installieren, Verwalten und Aktualisieren von Modulen](extensions.md) für weitere Informationen.

So installieren Sie ein Design mit dem Composer:

1. Kaufen Sie das Design von Commerce Marketplace.
1. Rufen Sie den Namen des Designers ab.
1. Wechseln Sie in Ihr Adobe Commerce-Stammverzeichnis und geben Sie den Befehl ein:

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   Beispiel:

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. Warten Sie, bis die Abhängigkeiten aktualisiert werden.
1. Geben Sie die folgenden Befehle ein:

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. Melden Sie sich beim Administrator an.
1. Klicken Sie **Inhalt** > Design > **Designs**.

   Das Design wird im rechten Bereich angezeigt.

## Mehrere Designs

Wenn Sie mehrere Designs verwenden, z. B. verschiedene Designs pro Gebietsschema, überprüfen Sie die `SCD_MATRIX` Umgebungsvariable , um die Designbereitstellung anzupassen. Siehe die [Erstellen](../environment/variables-build.md#scd_matrix) oder [Bereitstellen](../environment/variables-deploy.md#scd_matrix) in der [Umgebungskonfiguration](../environment/configure-env-yaml.md).
