---
title: GitLab-Integration
description: Erfahren Sie, wie Sie Ihr Adobe Commerce on Cloud-Infrastrukturprojekt mit GitLab integrieren.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# GitLab-Integration

Sie können ein GitLab-Repository so konfigurieren, dass automatisch eine Umgebung erstellt und bereitgestellt wird, wenn Sie Code-Änderungen per Push übertragen. Diese Integration synchronisiert Ihr GitLab-Repository mit Ihrem Adobe Commerce on Cloud Infrastructure-Konto.

{{private-repository}}

Diese Integration bietet folgende Möglichkeiten:

- Erstellen einer Umgebung beim Erstellen einer Verzweigung
- Umgebung beim Zusammenführen einer Pull-Anfrage erneut bereitstellen
- Löschen der Umgebung beim Löschen der Verzweigung

Sie müssen ein GitLab-Token und einen Webhook abrufen, um den Prozess fortzusetzen.

## Voraussetzungen

- Administratorzugriff auf das Adobe Commerce on Cloud-Infrastrukturprojekt
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md)-Tool in Ihrer lokalen Umgebung
- Ein GitLab-Konto
- Ein persönliches GitLab-Zugriffstoken mit Schreibzugriff auf das GitLab-Repository. Ausgewählte Bereiche müssen mindestens sein: `api` und `read_repository`.

## Repository vorbereiten

Klonen Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt aus einer bestehenden Umgebung und migrieren Sie die Projektverzweigungen in ein neues, leeres GitLab-Repository, wobei dieselben Verzweigungsnamen beibehalten werden. Es ist **wichtig** eine identische Git-Struktur beizubehalten, damit Sie keine vorhandenen Umgebungen oder Verzweigungen in Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt verlieren.

1. Melden Sie sich vom Terminal aus bei Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt an.

   ```bash
   magento-cloud login
   ```

1. Listen Sie Ihre Projekte auf und kopieren Sie die Projekt-ID.

   ```bash
   magento-cloud project:list
   ```

1. Klonen Sie das Projekt in Ihrer lokalen Umgebung.

   ```bash
   magento-cloud project:get <project-id>
   ```

1. Fügen Sie Ihr GitLab-Repository als Remote hinzu (vorausgesetzt, GitLab wird in der zugehörigen SaaS-Version verwendet).

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   Der Standardname für die Remoteverbindung kann `origin` oder `magento` sein. Wenn `origin` vorhanden ist, können Sie einen anderen Namen auswählen oder den vorhandenen Verweis umbenennen oder löschen. Siehe [Git-Remote-Dokumentation](https://git-scm.com/docs/git-remote).

1. Stellen Sie sicher, dass Sie die GitLab-Fernbedienung korrekt hinzugefügt haben.

   ```bash
   git remote -v
   ```

   Erwartete Antwort:

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. Übertragen Sie die Projektdateien in das neue GitLab-Repository. Denken Sie daran, alle Verzweigungsnamen gleich zu halten.

   ```bash
   git push -u origin master
   ```

   Wenn Sie mit einem neuen GitLab-Repository beginnen, müssen Sie möglicherweise die Option `-f` verwenden, da das Remote-Repository nicht mit Ihrer lokalen Kopie übereinstimmt.

1. Stellen Sie sicher, dass das GitLab-Repository alle Ihre Projektdateien enthält.

## Aktivieren der GitLab-Integration

Verwenden Sie den Befehl `magento-cloud integration` , um die GitLab-Integration zu aktivieren, und rufen Sie die Payload-URL für den GitLab-Webhook ab, um Aktualisierungen von GitLab an Ihr Adobe Commerce on Cloud Infrastructure-Projekt zu senden.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| Option | Beschreibung |
| ------ | ----------- |
| `<project-ID>` | Ihre Projekt-ID für Adobe Commerce in der Cloud-Infrastruktur |
| `<your-GitLab-token>` | Das persönliche Zugriffstoken, das Sie für GitLab generiert haben |
| `--base-url` | GitLab-URL (`https://gitlab.com/` wenn GitLab in der SaaS-Version verwendet wird) |
| `--server-project` | Projektname in GitLab (Teil nach der Basis-URL) |
| `--build-merge-requests` | Ein _optionaler_ Parameter, der Adobe Commerce in der Cloud-Infrastruktur anweist, für jede Zusammenführungsanfrage eine neue Umgebung zu erstellen (standardmäßig `true`) |
| `--merge-requests-clone-parent-data` | Ein _optionaler_ Parameter, der Adobe Commerce in der Cloud-Infrastruktur anweist, die Daten der übergeordneten Umgebung für Zusammenführungsanfragen zu klonen (standardmäßig `true`) |
| `--fetch-branches` | Ein _optionaler_ Parameter, der bewirkt, dass Adobe Commerce in der Cloud-Infrastruktur alle Verzweigungen aus dem Remote abruft (standardmäßig als inaktive Umgebungen `true`) |
| `--prune-branches` | Ein _optionaler_ Parameter, der Adobe Commerce in der Cloud-Infrastruktur anweist, Verzweigungen zu löschen, die nicht auf der Remote-Instanz vorhanden sind (`true`) |

>[!WARNING]
>
>Der Befehl `magento-cloud integration` überschreibt _all_-Code in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt mit dem Code aus Ihrem GitLab-Repository. Dies umfasst alle Verzweigungen, einschließlich der `production`. Diese Aktion erfolgt sofort und kann nicht rückgängig gemacht werden. Als Best Practice ist es wichtig, alle Ihre Verzweigungen aus Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt zu klonen und in Ihr GitLab-Repository zu pushen, bevor Sie die GitLab-Integration hinzufügen.

**So aktivieren Sie die GitLab-Integration**:

1. Fügen Sie vom Terminal aus die GitLab-Integration zu Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt hinzu:

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. Geben Sie nach Aufforderung `y` ein, um die Integration hinzuzufügen.

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. Kopieren Sie die **Hook-URL**, die in der Rückgabeausgabe angezeigt wird.

   ```
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### Webhook in GitLab hinzufügen

Um Ereignisse - z. B. Push- oder Merge-Anfragen - mit Ihrem Cloud Git-Server zu kommunizieren, müssen Sie [Webhook erstellen](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) für Ihr GitLab-Repository erstellen

1. Klicken Sie in Ihrem GitLab-Repository auf die Registerkarte **Einstellungen**.

1. Klicken Sie in der linken Navigationsleiste auf **Webhooks**.

1. Bearbeiten Sie im _Webhooks_-Formular die folgenden Felder:

   - **URL**: Geben Sie den `Hook URL` ein, der zurückgegeben wurde, als Sie die GitLab-Integration aktiviert haben.
   - **Geheimer Token**: Geben Sie bei Bedarf einen Verifizierungsschlüssel ein.
   - **Trigger**: Überprüfen Sie `Merge request events` und/oder `Push events` je nach Bedarf.
   - **SSL-Überprüfung aktivieren**: Sie müssen diese Option auswählen.

1. Klicken Sie **Webhook hinzufügen**.

### Testen der Integration

Nach der Konfiguration der GitLab-Integration können Sie mithilfe der `magento-cloud` CLI überprüfen, ob die Integration funktionsfähig ist:

```bash
magento-cloud integration:validate
```

Sie können sie auch testen, indem Sie eine einfache Änderung an Ihr GitLab-Repository senden.

1. Erstellen Sie eine Testdatei.

   ```bash
   touch test.md
   ```

1. Übertragen Sie die Änderung in das GitLab-Repository und übertragen Sie sie.

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. Melden Sie sich bei der [[!DNL Cloud Console]](../project/overview.md) an und stellen Sie sicher, dass Ihre Commit-Nachricht angezeigt wird und Ihr Projekt bereitgestellt wird.

## Erstellen einer Cloud-Verzweigung

Verwenden Sie den `magento-cloud` CLI-`environment:push`, um eine neue Umgebung zu erstellen und zu aktivieren. Siehe [Erstellen einer Cloud-Verzweigung](bitbucket.md#create-a-cloud-branch).

## Integration entfernen

Verwenden Sie den `magento-cloud` CLI-`integration:delete`, um die Integration zu entfernen. Siehe [Entfernen der Integration](bitbucket.md#remove-the-integration).
