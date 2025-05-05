---
title: Erweiterungen verwalten
description: Erfahren Sie, wie Sie Erweiterungen in Adobe Commerce auf der Cloud-Infrastruktur installieren und verwalten.
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Erweiterungen verwalten

Sie können die Funktionen Ihrer Adobe Commerce-Anwendung erweitern, indem Sie eine Erweiterung von der [Commerce Marketplace](https://marketplace.magento.com) hinzufügen. Sie können beispielsweise ein Design hinzufügen, um das Erscheinungsbild Ihrer Storefront zu ändern, oder Sie können ein Sprachpaket hinzufügen, um Ihre Storefront und Ihren Admin zu lokalisieren.

>[!NOTE]
>
>Um Installationsprobleme zu vermeiden, müssen alle Käufe auf dem Marketplace mit demselben Konto (MAGEID) abgeschlossen werden, dem das Cloud-Projekt gehört.

## Composer-Name einer Erweiterung

Obwohl in diesem Abschnitt erläutert wird, wie Sie den Namen und die Version einer Erweiterung von Commerce Marketplace abrufen können, finden Sie den Namen und die Version _any_-Moduls in der Composer-Datei des Moduls. Öffnen Sie die `composer.json` in einem Texteditor und notieren Sie sich die `"name"` und `"version"` Werte.

**Den Composer-Namen eines Moduls von der Commerce Marketplace abrufen**:

1. Melden Sie sich bei [Commerce Marketplace](https://marketplace.magento.com) mit dem Benutzernamen und Kennwort an, mit dem Sie die Komponente erworben haben.

1. Klicken Sie in der oberen rechten Ecke auf Ihren Benutzernamen und wählen Sie **Mein Profil**.

   ![Zugriff auf Ihr Marketplace-Konto](../../assets/marketplace/my-profile.png)

1. Klicken Sie auf der _Mein Konto_-Seite auf **Meine Käufe**.

   ![Marketplace-Kaufverlauf](../../assets/marketplace/my-purchases.png)

1. Wählen Sie auf der _Meine_&quot; ein von Ihnen erworbenes Modul aus und klicken Sie auf **Technische Details**.

1. Klicken Sie **Kopieren**, um die [!UICONTROL Component name] in die Zwischenablage zu kopieren.

1. Öffnen Sie einen Texteditor und fügen Sie den Komponentennamen ein und fügen Sie einen Doppelpunkt (`:`) an.

1. Klicken Sie **Technische Details** auf **Kopieren**, um die [!UICONTROL Component version] in die Zwischenablage zu kopieren.

1. Hängen Sie im Texteditor die Versionsnummer an den Komponentennamen nach dem Doppelpunkt an. Beispiel:

   ```text
   extension-name/magento2:1.0.1
   ```

## Installieren einer Erweiterung

Adobe empfiehlt, in einer Entwicklungsverzweigung zu arbeiten, wenn Sie Ihrer Implementierung eine Erweiterung hinzufügen. Bei der Installation einer Erweiterung wird der Name der Erweiterung (`<VendorName>_<ComponentName>`) automatisch in die [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html?lang=de)-Datei eingefügt. Es ist nicht erforderlich, die Datei direkt zu bearbeiten.

**So installieren Sie eine**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Erstellen oder Auschecken einer Entwicklungsverzweigung. Siehe [Verzweigung](../development/cli-branches.md).

1. Fügen Sie unter Verwendung des Namens und der Version des Composers die Erweiterung zum Abschnitt `require` der `composer.json` hinzu.

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. Aktualisieren Sie die Projektabhängigkeiten.

   ```bash
   composer update
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >Wenn Sie eine Erweiterung installieren, müssen Sie die `composer.lock`-Datei einbeziehen, wenn Sie Code-Änderungen in die Remote-Umgebung pushen. Der Befehl `composer install` liest die `composer.lock`, um die definierten Abhängigkeiten in der Remote-Umgebung zu aktivieren.

1. Nachdem der Build und die Bereitstellung abgeschlossen sind, verwenden Sie ein SSH, um sich bei der Remote-Umgebung anzumelden und die installierte Erweiterung zu überprüfen.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Ein Erweiterungsname verwendet das Format `<VendorName>_<ComponentName>`.

   Beispielantwort:

   ```
   Module is enabled
   ```

   Wenn Bereitstellungsfehler auftreten, finden Sie weitere Informationen unter [Fehler bei der Erweiterungsbereitstellung](../deploy/recover-failed-deployment.md).

## Erweiterungen verwalten

Wenn Sie eine Erweiterung mithilfe von Composer hinzufügen, wird die Erweiterung automatisch vom Bereitstellungsprozess aktiviert. Wenn Sie die Erweiterung bereits installiert haben, können Sie sie über die CLI aktivieren oder deaktivieren. Verwenden Sie beim Verwalten von Erweiterungen das Format `<VendorName>_<ComponentName>`

Aktivieren oder deaktivieren Sie niemals eine Erweiterung, während Sie bei den Remote-Umgebungen angemeldet sind.

**Aktivieren oder Deaktivieren einer Erweiterung**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Aktivieren oder Deaktivieren eines Moduls. Der Befehl `module` aktualisiert die `config.php` mit dem angeforderten Status des Moduls.

   >Aktivieren eines Moduls.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Deaktivieren Sie ein Modul.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Wenn Sie ein Modul aktiviert haben, verwenden Sie `ece-tools` , um die Konfiguration zu aktualisieren.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Überprüfen Sie den Status eines Moduls.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Aktualisieren einer Erweiterung

Bevor Sie fortfahren, benötigen Sie den Namen des Komponisten und die Version für die Erweiterung. Überprüfen Sie außerdem, ob die Erweiterung mit Ihrem Projekt und der Adobe Commerce-Version kompatibel ist. Überprüfen Sie [ die erforderliche PHP-Version, ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=de) Sie beginnen.

**Aktualisieren einer Erweiterung**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Erstellen oder Auschecken einer Entwicklungsverzweigung. Siehe [Verzweigung](../development/cli-branches.md).

1. Öffnen Sie die `composer.json` in einem Texteditor.

1. Suchen Sie Ihre Erweiterung und aktualisieren Sie die Version.

1. Speichern Sie Ihre Änderungen und beenden Sie den Texteditor.

1. Aktualisieren Sie die Projektabhängigkeiten.

   ```bash
   composer update
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Wenn Fehler auftreten, lesen Sie [Nach Komponentenfehler wiederherstellen](../deploy/recover-failed-deployment.md). Weitere Informationen zur Verwendung von Erweiterungen mit Adobe Commerce finden Sie unter [Erweiterungen](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html?lang=de) im _Admin-Handbuch_.
