---
title: Cloud-CLI
description: Erfahren Sie mehr über die Magento-Cloud-CLI und wie sie Ihnen bei der Verwaltung lokaler Entwicklungsumgebungen für Ihr Adobe Commerce on Cloud-Infrastrukturprojekt hilft.
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# Cloud-CLI

Mit dem `magento-cloud` CLI-Tool können Entwickler und Systemadministratoren Cloud-Projekte und -Umgebungen verwalten, Routinen ausführen und Automatisierungsaufgaben lokal ausführen. Die `magento-cloud` CLI erweitert die Funktionen und Möglichkeiten der [[!DNL Cloud Console]](../../get-started/cloud-console.md). Nachdem Sie die `magento-cloud` CLI auf Ihrer lokalen Workstation installiert haben, können Sie sie zur Verwaltung Ihrer Adobe Commerce in Cloud Infrastructure Starter- und Pro-Integrationsumgebungen verwenden.

>[!NOTE]
>
>Dies ist ein lokales Tool und kann nicht mit dieser Methode in der Cloud-Umgebung installiert werden (die schreibgeschützt ist). Module können nur über den Bereitstellungs-Workflow (**) in der Cloud-Umgebung installiert**
>- [Pro-Bereitstellungs-Workflow](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [Workflow „Bereitstellung starten](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**So installieren Sie die `magento-cloud` CLI**:

1. Ändern Sie auf Ihrer _lokalen Workstation_ in das Verzeichnis, in dem Sie das Cloud-Projekt klonen möchten und in dem der [Dateisystembesitzer](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=de) über _Schreibzugriff_ verfügt.

1. Installieren Sie `magento-cloud` CLI.

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Fügen Sie `magento-cloud` CLI zum Bash-Profil hinzu.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. Laden Sie das aktualisierte Bash-Profil neu.

   ```bash
   . ~/.bash_profile
   ```

1. Um die CLI zu starten, rufen Sie `magento-cloud` auf und geben Sie bei Aufforderung Ihre Anmeldedaten für das Cloud-Konto ein.

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. Stellen Sie sicher, dass sich der `magento-cloud`-Befehl im Pfad befindet. Im folgenden Beispiel werden die verfügbaren Befehle aufgelistet.

   ```bash
   magento-cloud list
   ```

## Allgemeine Befehle

Adobe hat diese Befehle für die Verwaltung von Cloud-Integrationsumgebungen entwickelt und empfiehlt, die `magento-cloud` CLI in einem Projektverzeichnis auszuführen, damit Sie den `-p <project-ID>`-Parameter weglassen können.

Die folgende Liste der häufig verwendeten `magento-cloud` CLI-Befehle enthält nur die erforderlichen Optionen. Sie können die `--help`-Option mit jedem Befehl verwenden, um weitere Informationen anzuzeigen.

| Befehl | Beschreibung |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | Melden Sie sich beim Projekt an. |
| `magento-cloud list` | Auflisten der verfügbaren Befehle für das CLI-Tool. |
| `magento-cloud environment:list` | Listen Sie die Umgebungen im aktuellen Projekt auf. |
| `magento-cloud environment:checkout` | Überprüfen einer vorhandenen Umgebung. |
| `magento-cloud environment:merge -e` | Änderungen in dieser Umgebung mit dem übergeordneten Element zusammenführen. |
| `magento-cloud variables` | Listenvariablen in dieser Umgebung. |
| `magento-cloud ssh` | Verwenden Sie SSH, um eine Verbindung zur Remote-Umgebung herzustellen. |
| `magento-cloud url` | Öffnen Sie die Adobe Commerce-Storefront in einem Browser. |
| `magento-cloud web` | Öffnen Sie die [!DNL Cloud Console]. |

## Umgebungsbefehle

Der Umgebungsname _name_ unterscheidet sich von der Umgebung _ID_ nur dann, wenn Sie im Umgebungsnamen Leerzeichen oder Großbuchstaben verwenden. Eine Umgebungs-ID besteht aus Kleinbuchstaben, Zahlen und zulässigen Symbolen. Großbuchstaben im Umgebungsnamen werden in der ID in Kleinbuchstaben umgewandelt, Leerzeichen im Umgebungsnamen in Bindestriche.

Ein Umgebungsname _darf keine Zeichen enthalten_ die für Ihre Linux-Shell oder für reguläre Ausdrücke reserviert sind. Unzulässige Zeichen sind geschweifte Klammern (`{ }`), Klammern, Sternchen (`*`), spitze Klammern (`< >`), kaufmännisches Und-Zeichen (`&`), Prozent (`%`) und andere Zeichen.

Der Befehl `magento-cloud environment:list` zeigt Umgebungshierarchien an, während `git branch` dies nicht tut. Wenn Sie verschachtelte Umgebungen haben, verwenden Sie Folgendes:

```bash
magento-cloud environment:list
```

### Umgebung erneut bereitstellen

Trigger einer erneuten Bereitstellung ohne Verwendung einer Push-Benachrichtigung. Überprüfen und bestätigen Sie die Umgebung für die erneute Bereitstellung. Verwenden Sie die erneute Bereitstellung nicht, wenn sich ein Build in einem ausstehenden Status befindet.

```bash
magento-cloud environment:redeploy
```

Beispielantwort:

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git-Befehle

Möglicherweise stellen Sie fest, dass einige dieser Befehle Git-Befehlen ähneln. Die `magento-cloud`-Befehle stellen eine direkte Verbindung zum Git-basierten Cloud-Projekt mit zusätzlichen Funktionen her. Wenn Sie eine Verzweigung erstellen, ohne die `magento-cloud` CLI zu verwenden, wird sie nicht „aktiviert“ und wird nicht automatisch erstellt, wenn Sie Änderungen in die Remote-Umgebung pushen. Der `magento-cloud` CLI-Befehl umfasst die Aktivierung.

Um eine Verzweigung zu erstellen, verwenden Sie den Befehl `magento-cloud` , damit die Verzweigung aktiviert wird.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

Für den Status der Verzweigung:

- Verwenden Sie den Befehl `magento-cloud env` , um eine Liste der Verzweigungen der Umgebung und ihres Status anzuzeigen: aktiv oder inaktiv.
- Verwenden Sie den Befehl `magento-cloud environment:activate` , um eine Umgebungsverzweigung zu aktivieren.

Pushen Sie einen leeren Git-Commit, um einen Trigger für eine Bereitstellung auszuführen. Beispiel:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

Einige Aktionen, z. B. das Hinzufügen eines Benutzers, führen nicht zur Bereitstellung.

### Erstellen einer Umgebungsverzweigung

Die folgenden Schritte zeigen, wie Sie die CLI- und Git-Befehle synonym zur Verwaltung Ihrer lokalen Umgebung verwenden:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Wechseln Sie zum [Dateisystembesitzer](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=de).

1. Melden Sie sich bei Ihrem Projekt an.

   ```bash
   magento-cloud login
   ```

1. Auflisten der Projekte

   ```bash
   magento-cloud project:list
   ```

1. Auflisten der Umgebungen im Projekt. Jede Umgebung enthält eine aktive Git-Verzweigung, die Ihren Code, Ihre Datenbank, Umgebungsvariablen, Konfigurationen und Services enthält.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >Es ist wichtig, den Befehl `magento-cloud environment:list` zu verwenden, da er Umgebungshierarchien anzeigt, der Befehl `git branch` dagegen nicht.

1. Abrufen von Ursprungszweigen, um den neuesten Code zu erhalten.

   ```bash
   git fetch origin
   ```

1. Auschecken oder Wechseln zu einer bestimmten Verzweigung und Umgebung.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git-Befehle checken nur die Git-Verzweigung aus. Der Befehl `magento-cloud checkout` checkt die Verzweigung aus und wechselt zur aktiven Umgebung.

   >[!TIP]
   >
   >Sie können eine Umgebungsverzweigung mithilfe der `magento-cloud environment:branch <environment-name> <parent-environment-ID>`-Befehlssyntax erstellen. Es kann einige Zeit dauern, bis eine Umgebungsverzweigung erstellt und aktiviert wird.

1. Verwenden Sie die Umgebungs-ID, um aktualisierten Code an Ihren lokalen Computer zu übertragen. Dies ist nicht erforderlich, wenn die Verzweigung „Umgebung“ neu ist.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_Optional_) Erstellen [Momentaufnahme](../storage/snapshots.md) der Umgebung als Backup.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## Aktualisieren der CLI

Die `magento-cloud` CLI sucht nach verfügbaren Aktualisierungen, wenn Sie sich anmelden. Sie können jedoch mit dem Befehl `self:update` nach Aktualisierungen suchen. Wenn ein Update verfügbar ist, befolgen Sie die Anweisungen zum Aktualisieren der CLI.

Wenn Ihre `magento-cloud`-CLI auf dem neuesten Stand ist, wird die folgende Antwort angezeigt:

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
