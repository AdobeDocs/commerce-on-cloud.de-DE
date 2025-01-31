---
title: GitHub-Integration
description: Erfahren Sie, wie Sie Ihr Adobe Commerce in ein Cloud-Infrastrukturprojekt mit GitHub integrieren.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# GitHub-Integration

Durch die GitHub-Integration können Sie Ihre Adobe Commerce in Cloud-Infrastrukturumgebungen direkt über Ihr GitHub-Repository verwalten. Die Integration verwaltet Inhalte, die sich bereits in GitHub befinden, und synchronisiert sie mit Ihrem Code-Repository für die Adobe Commerce in der Cloud-Infrastruktur. Im Wesentlichen wird das Code-Repository zu einem Spiegel des GitHub-Repositorys.

{{private-repository}}

Diese Integration bietet folgende Möglichkeiten:

- Erstellen einer Umgebung beim Erstellen einer Verzweigung
- Umgebung beim Zusammenführen einer Pull-Anfrage erneut bereitstellen
- Löschen der Umgebung beim Löschen der Verzweigung

Sie müssen ein GitHub-Token und einen Webhook abrufen, um den Prozess fortzusetzen.

## Voraussetzungen

- Administratorzugriff auf das Adobe Commerce on Cloud-Infrastrukturprojekt
- GitHub-Repository
- Persönliches GitHub-Zugriffstoken

## Generieren eines GitHub-Tokens

Erstellen Sie in den GitHub-Entwicklereinstellungen ein klassisches persönliches Zugriffstoken. Sie müssen Mitglied einer Gruppe mit Schreibzugriff auf das GitHub-Repository sein, damit Sie in das Repository _pushen_ können. Schließen Sie beim Erstellen Ihres Tokens die folgenden Bereiche ein:

- `admin:repo_hook` - Erstellen von Webhooks
- `repo` - Integration mit Ihrem Repository
- `read:org` - Integration mit dem Repository Ihres Unternehmens

Siehe [GitHub: Erstellen](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## Repository vorbereiten

Klonen Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt aus einer bestehenden Umgebung und migrieren Sie die Projektverzweigungen in ein neues, leeres GitHub-Repository, wobei dieselben Verzweigungsnamen beibehalten werden. Es ist **wichtig** eine identische Git-Struktur beizubehalten, damit Sie keine vorhandenen Umgebungen oder Verzweigungen in Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt verlieren.

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
   magento-cloud project:get <project-ID>
   ```

1. Fügen Sie Ihr GitHub-Repository als Remote-Datenquelle hinzu.

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   Der Standardname für die Remoteverbindung kann `origin` oder `magento` sein. Wenn `origin` vorhanden ist, können Sie einen anderen Namen auswählen oder den vorhandenen Verweis umbenennen oder löschen. Siehe [Git-Remote-Dokumentation](https://git-scm.com/docs/git-remote).

1. Stellen Sie sicher, dass Sie die GitHub-Fernbedienung korrekt hinzugefügt haben.

   ```bash
   git remote -v
   ```

   Erwartete Antwort:

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. Übertragen Sie die Projektdateien in das neue GitHub-Repository. Denken Sie daran, alle Verzweigungsnamen gleich zu halten.

   ```bash
   git push -u origin master
   ```

   Wenn Sie mit einem neuen GitHub-Repository beginnen, müssen Sie möglicherweise die Option `-f` verwenden, da das Remote-Repository nicht mit Ihrer lokalen Kopie übereinstimmt.

1. Stellen Sie sicher, dass das GitHub-Repository alle Ihre Projektdateien enthält.

## Aktivieren der GitHub-Integration

Bevor Sie beginnen, müssen sich Ihr Projekt-Code und Ihre Umgebungen im GitHub-Repository befinden. Nach der Aktivierung der Integration wird das GitHub-Repository zur Code-Quelle. Wenn Sie Code-Änderungen an das ursprüngliche `magento`-Repository pushen, wird es durch die Integration überschrieben, wenn Sie Code-Änderungen an Ihr GitHub-Repository pushen.

Im Folgenden wird die GitHub-Integration aktiviert und eine Payload-URL bereitgestellt, die beim Erstellen eines Webhooks verwendet werden kann.

>[!WARNING]
>
>Der folgende Befehl überschreibt _all_-Code in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt mit Code aus Ihrem GitHub-Repository, der alle Verzweigungen einschließlich der `production`-Verzweigung enthält. Diese Aktion erfolgt sofort und kann nicht rückgängig gemacht werden. Als Best Practice ist es wichtig, alle Ihre Verzweigungen aus Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt zu klonen und in Ihr GitHub-Repository zu **vor** Hinzufügen der GitHub-Integration.

Sie können die CLI-Eingabeaufforderungen mithilfe von `magento-cloud integration:add` schrittweise durchlaufen oder den Integrationsbefehl mit den folgenden Optionen erstellen:

| Option | Erforderlich? | Beschreibung |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | Ja | Die Basis-URL der Server-Installation, die `https://github.com/` oder benutzerdefiniert sein kann. Lassen Sie diese Option weg, wenn Ihr Repository mit öffentlichem GitHub gehostet wird oder wenn Ihr Repository nicht auf privaten Servern gehostet wird. Lassen Sie diese Option weg, wenn Ihre Repository-URL `https://github.com/{account}/{repository-name}` ähnelt. Dies kann zu Fehlern wie `Unable to connect to GitHub: repository not found` führen. |
| `--token` | Ja | Das persönliche Zugriffstoken, das Sie für GitHub generiert haben |
| `--repository` | Ja | Der Repository-Name: `owner-or-organisation/repository` |
| `--build-pull-requests` | optional | Weist Adobe Commerce an, die Cloud-Infrastruktur bereitzustellen, nachdem Sie eine Pull-Anfrage zusammengeführt haben (standardmäßig `true`) |
| `--fetch-branches` | optional | Bewirkt, dass Adobe Commerce in der Cloud-Infrastruktur Verzweigungen verfolgt und bereitstellt, nachdem Sie eine Verzweigung aktualisiert haben (standardmäßig `true`) |
| `--prune-branches` | optional | Löschen von Verzweigungen, die auf der Remote-Instanz nicht vorhanden sind (standardmäßig `true`) |

Es gibt viele weitere Optionen, die Sie mithilfe der Hilfeoption sehen können:

```bash
magento-cloud integration:add --help
```

**So aktivieren Sie die GitHub-**:

1. Aktivieren Sie die Integration.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **Beispiel 1**: Aktivieren der GitHub-Integration für ein persönliches, privates Repository:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **Beispiel 2**: Aktivieren der GitHub-Integration für ein Organisations-Repository:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. Geben Sie bei Aufforderung die erforderlichen Informationen ein.

1. Kopieren Sie die **Payload-URL**, die in der Rückgabeausgabe angezeigt wird.

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## Hinzufügen des Webhooks in GitHub

Um Ereignisse - z. B. eine Push-Benachrichtigung - mit Ihrem Cloud-Git-Server zu kommunizieren, müssen Sie einen Webhook für Ihr GitHub-Repository erstellen:

1. Klicken Sie in Ihrem GitHub-Repository auf die Registerkarte **Einstellungen**.

1. Klicken Sie in der linken Navigationsleiste auf **Webhooks**.

1. Klicken Sie _Bereich_ Webhooks“ auf **Webhook hinzufügen**.

1. Bearbeiten Sie _Formular Webhooks_ Webhook hinzufügen die folgenden Felder:

   - **Payload-URL**: Geben Sie die URL ein, die zurückgegeben wird, als Sie die GitHub-Integration aktiviert haben.
   - **Content-Typ**: Wählen Sie **application/json** aus der Liste aus.
   - **Geheime Daten**: Geben Sie einen geheimen Verifizierungsschlüssel ein.
   - **Welche Ereignisse möchten Sie als Trigger für diesen Webhook verwenden?**: Wählen Sie **Senden Sie mir alles**.
   - Aktivieren Sie **Kontrollkästchen** Aktiv“.

1. Klicken Sie **Webhook hinzufügen**.

## Testen der Integration

Nach dem Konfigurieren der GitHub-Integration können Sie mithilfe der `magento-cloud` CLI überprüfen, ob die Integration funktionsfähig ist:

```bash
magento-cloud integration:validate
```

Sie können sie auch testen, indem Sie eine einfache Änderung an Ihr GitHub-Repository senden.

1. Erstellen Sie eine Testdatei.

   ```bash
   touch test.md
   ```

1. Übertragen Sie die Änderung in das GitHub-Repository und übertragen Sie sie.

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. Melden Sie sich bei der [[!DNL Cloud Console]](../project/overview.md) an und stellen Sie sicher, dass Ihre Commit-Nachricht angezeigt wird und Ihr Projekt bereitgestellt wird.

## Integration entfernen

Sie können die GitHub-Integration sicher aus Ihrem Projekt entfernen, ohne den Code zu beeinflussen.

**Entfernen der GitHub-Integration**:

1. Melden Sie sich vom Terminal aus bei Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt an.

1. Auflisten der Integrationen. Sie benötigen die GitHub-Integrations-ID , um den nächsten Schritt abzuschließen.

   ```bash
   magento-cloud integration:list
   ```

1. Löschen Sie die Integration.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Sie können die GitHub-Integration auch entfernen, indem Sie sich bei Ihrem GitHub-Konto anmelden und den Webhook auf der Registerkarte _Webhooks_ des Repositorys (_)_.
