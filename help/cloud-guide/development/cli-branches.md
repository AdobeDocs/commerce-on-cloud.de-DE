---
title: Verwalten von Verzweigungen mit der CLI
description: Erfahren Sie, wie Sie die Umgebungsverzweigungen für Adobe Commerce in der Cloud-Infrastruktur mithilfe der Cloud-CLI verwalten.
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Verwalten von Verzweigungen mit der CLI

Informationen zur Installation der `magento-cloud` CLI finden Sie in der [Cloud CLI-Referenz](../dev-tools/cloud-cli-overview.md). Nachdem Sie die `magento-cloud` CLI installiert und SSH-Schlüssel für den Remote-Zugriff auf Ihre Cloud-Infrastruktur eingerichtet haben, können Sie `magento-cloud` CLI-Befehle verwenden, um die Umgebungen für Ihre Projekte zu verwalten. Informationen zur Umgebungsarchitektur finden Sie unter [Starter-Architektur](../architecture/starter-architecture.md) oder [Pro-Architektur](../architecture/pro-architecture.md).

Informationen zum Verwalten der Verzweigungen und Umgebungen mit der [!DNL Cloud Console] finden Sie unter [Verwalten von Verzweigungen mit der [!DNL Cloud Console]](../project/console-branches.md).

## CLI-Befehle verwenden

Die `magento-cloud` CLI-Befehle ähneln den Git-Befehlen. Sie können sie verwenden, um eine Verbindung zu Ihrem Projekt herzustellen und Ihre Umgebungen zu verwalten. Obwohl Sie die Befehle in jedem Verzeichnis ausführen können, wird empfohlen, sie in einem Projektverzeichnis auszuführen. Bei der Ausführung in einem Projektverzeichnis können Sie den `-p <project-ID>` Parameter weglassen. Siehe die [Cloud-CLI-Referenz](../dev-tools/cloud-cli-overview.md).

## Klonen des Projekts

Die folgenden Anweisungen verwenden eine Kombination aus `magento-cloud` CLI- und Git-Befehlen, um Ihr Projekt auf Ihrer lokalen Workstation zu klonen. Um eine vollständige Liste der `magento-cloud` CLI-Befehle anzuzeigen, verwenden Sie den `magento-cloud list`.

>[!IMPORTANT]
>
>Einige Git-Befehle können eine Aktion in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt nicht abschließen. Sie können beispielsweise eine Verzweigung mit einem Git-Befehl erstellen, aber keine neue Umgebung erstellen und aktivieren. Sie müssen mit dem Befehl `magento-cloud environment:branch <branch-name>` eine Umgebung erstellen, damit die Umgebung _aktiv_ wird. Alternativ können Sie die [!DNL Cloud Console] verwenden, um aktive Umgebungen zu erstellen. Siehe [Cloud-CLI-Referenz](../dev-tools/cloud-cli-overview.md#git-commands).

**So klonen Sie ein Projekt `master` eine Umgebung**:

1. Melden Sie sich bei Ihrer lokalen Workstation mit einem Konto [Dateisystemeigentümer](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=de) an.

1. Wechseln Sie zum Verzeichnis Webserver oder virtueller Host _docroot_.

1. Melden Sie sich über die `magento-cloud` CLI an.

   ```bash
   magento-cloud login
   ```

1. Auflisten der Projekte

   ```bash
   magento-cloud project:list
   ```

1. Klonen Sie ein Projekt.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Geben Sie bei Aufforderung einen Verzeichnisnamen an.

1. Wechseln Sie in das Verzeichnis `magento2` .

1. Auflisten der für das Projekt verfügbaren Umgebungen.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >Der Befehl `magento-cloud environment:list` zeigt Umgebungshierarchien an, der Befehl `git branch` dagegen nicht.

1. Rufen Sie die Remote-Verzweigungen ab.

   ```bash
   git fetch origin
   ```

1. Aktualisierten Code abrufen.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Unter [Integrationen](../integrations/overview.md) finden Sie Informationen zur Verwendung von Git-basierten Hosting-Services mit Adobe Commerce in der Cloud-Infrastruktur.

## Erstellen einer Verzweigung für die Entwicklung

Nachdem Sie Ihr Projekt geklont und die Konfiguration des Adobe Commerce-Administratorkontos aktualisiert haben, können Sie eine Verzweigung für die Entwicklung erstellen. Wie bereits erwähnt, müssen Sie eine Umgebung mit dem Befehl `magento-cloud environment:branch <branch-name>` oder dem [!DNL Cloud Console] erstellen, damit die Umgebung _aktiv_ wird.

- Erwägen [ zunächst](../architecture/starter-develop-deploy-workflow.md#clone-and-branch) eine Verzweigung für `staging` zu erstellen und dann eine Entwicklungsverzweigung basierend auf der `staging` Verzweigung zu erstellen.
- Erstellen Sie für [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow) Entwicklungsverzweigungen basierend auf der `Integration`.

**So erstellen Sie eine**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Erstellen Sie eine Umgebung basierend auf der für Ihren Projekt-Workflow empfohlenen Verzweigung.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Aktualisieren von Abhängigkeiten.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_optional_] Erstellen [Sicherung](../storage/snapshots.md) der Umgebung.

### Zusammenführen einer Verzweigung

Führen Sie nach Abschluss der Entwicklung diese Verzweigung mit dem übergeordneten Element zusammen:

1. Code-Änderungen übernehmen und übertragen:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Zusammenführen mit der übergeordneten Umgebung:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Löschen einer Umgebung

Löschen Sie eine Umgebung nur, wenn Sie sicher sind, dass Sie sie nicht mehr benötigen. Eine Umgebung kann nach dem Löschen nicht wiederhergestellt werden.

>[!WARNING]
>
>Die `master` Verzweigung eines Projekts kann nicht gelöscht werden.

Sie müssen Projekt-Admin, Umgebungs-Admin oder Kontoinhaber sein, um diese Aufgabe auszuführen. Siehe [Benutzerzugriff auf Cloud-Projekte verwalten](../project/user-access.md).

Wenn Sie eine Umgebung löschen, wird für die Umgebung _inaktiv_ festgelegt. Der Code ist weiterhin in der Git-Verzweigung verfügbar, enthält jedoch nicht mehr die Services oder die Datenbank. Um die Umgebung vollständig zu löschen, müssen Sie auch die entsprechende Remote-Git-Verzweigung löschen.

**Löschen einer Umgebung**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Aktualisierungen vom Remote-Server abrufen.

   ```bash
   git fetch
   ```

1. Löschen Sie die Umgebungsverzweigung.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   Optional können Sie mehrere Umgebungen gleichzeitig löschen, indem Sie dem Löschbefehl mehrere Umgebungs-IDs hinzufügen.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Reagieren Sie auf die Aufforderungen zum Löschen der lokalen Umgebung und der entsprechenden Remote-Umgebung.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   Wenn Sie die Umgebung löschen, wird sie in einen _Status_.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   Durch Löschen der Remote-Git-Verzweigung wird die Umgebung aus dem Projekt entfernt.

1. Warten Sie, bis die Umgebung gelöscht wurde.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Um eine inaktive Umgebung zu aktivieren, verwenden Sie den Befehl `magento-cloud environment:activate` .

## Interaktion mit Remote-Umgebungen

Nachdem Sie [SSH-Schlüssel eingerichtet](../development/secure-connections.md) haben, können Sie [eine Verbindung zwischen Ihrem lokalen Arbeitsbereich und einer Remote-Umgebung herstellen](../development/secure-connections.md#connect-to-a-remote-environment) mit Ihren Projekt-Services interagieren und Einstellungen ändern.
