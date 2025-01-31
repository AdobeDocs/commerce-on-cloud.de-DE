---
title: Bitbucket-Integration
description: Erfahren Sie, wie Sie Ihr Adobe Commerce in ein Cloud-Infrastrukturprojekt mit Bitbucket integrieren.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Bitbucket-Integration

Sie können Ihr Bitbucket-Repository so konfigurieren, dass automatisch eine Umgebung erstellt und bereitgestellt wird, wenn Sie Code-Änderungen per Push übertragen. Diese Integration synchronisiert Ihr Bitbucket-Repository mit Ihrem Adobe Commerce in einem Cloud-Infrastrukturkonto.

{{private-repository}}

## Voraussetzungen

- Administratorzugriff auf das Adobe Commerce on Cloud-Infrastrukturprojekt
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md)-Tool in Ihrer lokalen Umgebung
- Ein Bitbucket-Konto
- Administratorzugriff auf das Bitbucket-Repository
- Ein SSH-Zugriffsschlüssel für das Bitbucket-Repository

## Repository vorbereiten

Klonen Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt aus einer bestehenden Umgebung und migrieren Sie die Projektverzweigungen in ein neues, leeres Bitbucket-Repository, wobei dieselben Verzweigungsnamen beibehalten werden. Es ist **wichtig** eine identische Git-Struktur beizubehalten, damit Sie keine vorhandenen Umgebungen oder Verzweigungen in Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt verlieren.

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

1. Fügen Sie Ihr Bitbucket-Repository als Remote-Datenquelle hinzu.

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   Der Standardname für die Remoteverbindung kann `origin` oder `magento` sein. Wenn `origin` vorhanden ist, können Sie einen anderen Namen auswählen oder den vorhandenen Verweis umbenennen oder löschen. Siehe [Git-Remote-Dokumentation](https://git-scm.com/docs/git-remote).

1. Stellen Sie sicher, dass Sie den Bitbucket-Remote-Zugriff korrekt hinzugefügt haben.

   ```bash
   git remote -v
   ```

   Erwartete Antwort:

   ```
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. Übertragen Sie die Projektdateien in das neue Bitbucket-Repository. Denken Sie daran, alle Verzweigungsnamen gleich zu halten.

   ```bash
   git push -u origin master
   ```

   Wenn Sie mit einem neuen Bitbucket-Repository beginnen, müssen Sie möglicherweise die Option `-f` verwenden, da das Remote-Repository nicht mit Ihrer lokalen Kopie übereinstimmt.

1. Stellen Sie sicher, dass Ihr Bitbucket-Repository alle Ihre Projektdateien enthält.

## Erstellen eines OAuth-Verbrauchers

Die Bitbucket-Integration erfordert einen [OAuth-Verbraucher](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). Sie benötigen die OAuth-`key` und `secret` von diesem Verbraucher, um den nächsten Abschnitt abzuschließen.

**So erstellen Sie einen OAuth-Verbraucher in Bitbucket**:

1. Melden Sie sich bei Ihrem [Bitbucket](https://id.atlassian.com/login)-Konto an.

1. Klicken Sie auf **Einstellungen** > **Zugriffsverwaltung** > **OAuth**.

1. Klicken Sie **Verbraucher hinzufügen** und konfigurieren Sie ihn wie folgt:

   ![Bitbucket-OAuth-Verbraucherkonfiguration](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >Eine gültige **Callback-URL** ist nicht erforderlich, aber Sie müssen einen Wert in dieses Feld eingeben, um die Integration erfolgreich abzuschließen.

1. Klicken Sie **Speichern**.

1. Klicken Sie auf Verbraucher **Name**, um Ihre OAuth-`key` und -`secret` anzuzeigen.

1. Kopieren Sie Ihre OAuth-`key` und -`secret` zum Konfigurieren der Integration.

## Integration konfigurieren

1. Navigieren Sie vom Terminal zu Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt.

1. Erstellen Sie eine temporäre Datei mit dem Namen `bitbucket.json` und fügen Sie Folgendes hinzu. Ersetzen Sie dabei die Variablen in den spitzen Klammern durch Ihre Werte:

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >Stellen Sie sicher, dass Sie den Namen Ihres Bitbucket-Repositorys und nicht die URL verwenden. Die Integration schlägt fehl, wenn eine URL verwendet wird.

1. Fügen Sie die Integration mithilfe des `magento-cloud` CLI-Tools zu Ihrem Projekt hinzu.

   >[!WARNING]
   >
   >Der folgende Befehl überschreibt _all_-Code in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt mit Code aus Ihrem Bitbucket-Repository. Dies umfasst alle Verzweigungen, einschließlich der `production`. Diese Aktion erfolgt sofort und kann nicht rückgängig gemacht werden. Als Best Practice ist es wichtig, alle Ihre Verzweigungen aus Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt zu klonen und in Ihr Bitbucket-Repository zu pushen, **die Bitbucket** Integration hinzuzufügen.

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   Dadurch wird eine lange HTTP-Antwort mit Kopfzeilen zurückgegeben. Bei einer erfolgreichen Integration wird der Status-Code 200 oder 201 zurückgegeben. Ein Status von 400 oder höher zeigt an, dass ein Fehler aufgetreten ist.

1. Löschen Sie die temporäre `bitbucket.json`.

1. Überprüfen Sie die Projektintegration.

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   Notieren Sie sich die **Hook-URL**, um einen Webhook in BitBucket zu konfigurieren.

### Webhook in BitBucket hinzufügen

Um Ereignisse - z. B. eine Push-Benachrichtigung - mit Ihrem Cloud Git-Server zu kommunizieren, ist ein Webhook für Ihr BitBucket-Repository erforderlich. Die Methode zum Einrichten einer auf dieser Seite beschriebenen Bitbucket-Integration erstellt bei korrekter Befolgung automatisch einen Webhook. Es ist wichtig, den Webhook zu überprüfen, um zu vermeiden, dass mehrere Integrationen erstellt werden.

1. Melden Sie sich bei Ihrem [Bitbucket](https://id.atlassian.com/login)-Konto an.

1. Klicken Sie **Repositorys** und wählen Sie Ihr Projekt aus.

1. Klicken Sie **Repository-Einstellungen** > **Workflow** > **Webhooks**.

1. Überprüfen Sie den Webhook, bevor Sie fortfahren.

   Wenn der Hook aktiv ist, überspringen Sie die verbleibenden Schritte und [ Sie die Integration ](#test-the-integration). Der Hook sollte einen Namen ähnlich **&quot;Adobe Commerce on Cloud Infrastructure &lt;project_id>&quot;** und ein Hook-URL-Format ähnlich `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook` haben.

1. Klicken Sie **Webhook hinzufügen**.

1. Bearbeiten _in der Ansicht_ Neuen Webhook hinzufügen“ die folgenden Felder:

   - **Title**: Adobe Commerce-Integration
   - **URL**: Verwenden Sie die Hook-URL aus Ihrer `magento-cloud`-Integrationsliste
   - **Trigger**: Der Standardwert ist ein einfacher _Repository-Push_

1. Klicken Sie **Speichern**.

### Testen der Integration

Nach dem Konfigurieren der Bitbucket-Integration können Sie mithilfe der `magento-cloud` CLI überprüfen, ob die Integration funktionsfähig ist:

```bash
magento-cloud integration:validate
```

Sie können es auch testen, indem Sie eine einfache Änderung an Ihr Bitbucket-Repository senden.

1. Erstellen Sie eine Testdatei.

   ```bash
   touch test.md
   ```

1. Übertragen Sie die Änderung in das Bitbucket-Repository und übertragen Sie sie.

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. Melden Sie sich bei der [[!DNL Cloud Console]](../project/overview.md) an und stellen Sie sicher, dass Ihre Commit-Nachricht angezeigt wird und Ihr Projekt bereitgestellt wird.

   ![Testen der Bitbucket-Integration](../../assets/bitbucket-integration.png)

## Erstellen einer Cloud-Verzweigung

Die Bitbucket-Integration kann keine neuen Umgebungen in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt aktivieren. Wenn Sie eine Umgebung mit Bitbucket erstellen, müssen Sie die Umgebung manuell aktivieren. Um diesen zusätzlichen Schritt zu vermeiden, empfiehlt es sich, Umgebungen mit dem `magento-cloud` CLI-Tool oder dem [!DNL Cloud Console] zu erstellen.

**So aktivieren Sie eine mit Bitbucket erstellte Verzweigung**:

1. Verwenden Sie die `magento-cloud` CLI, um die Verzweigung per Push zu übertragen.

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. Stellen Sie sicher, dass die Umgebung aktiv ist.

   ```bash
   magento-cloud environment:list
   ```

   ```
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

Nachdem Sie eine Umgebung erstellt haben, können Sie die entsprechende Verzweigung mit regulären Git-Befehlen an Ihr Remote-Bitbucket-Repository pushen. Nachfolgende Änderungen an Ihrer Verzweigung in Bitbucket erstellen und stellen die Umgebung automatisch bereit.

## Integration entfernen

Sie können die Bitbucket-Integration sicher aus Ihrem Projekt entfernen, ohne den Code zu beeinflussen.

**So entfernen Sie die Bitbucket-Integration**:

1. Melden Sie sich vom Terminal aus bei Ihrem Adobe Commerce on Cloud-Infrastrukturprojekt an.

1. Auflisten der Integrationen. Sie benötigen die Bitbucket-Integrations-ID , um den nächsten Schritt abzuschließen.

   ```bash
   magento-cloud integration:list
   ```

1. Löschen Sie die Integration.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Sie können die Bitbucket-Integration auch entfernen, indem Sie sich bei Ihrem Bitbucket-Konto anmelden und die OAuth-Gewährung auf der Seite „Einstellungen _des Kontos_.

## Bitbucket-Serverintegration

Um die Bitbucket-Server-Integration zu verwenden, benötigen Sie Folgendes:

- [Bitbucket-Zugriffstoken](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html) - Generieren eines Tokens, das Project `read` Zugriff und Repository-`admin` gewährt
- [Bitbucket-Server-URL](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html) - Fügen Sie die Basis-URL Ihrer Bitbucket-Instanz hinzu

Sie können die Cloud-CLI zwar verwenden, um die Schritte zur Integration des Bitbucket-Servers zu durchlaufen, der vollständige Befehl sieht jedoch ähnlich wie der folgende aus:

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

Verwenden Sie den Hilfe-Befehl für weitere Nutzungsanforderungen und Optionen: `magento-cloud integration:add --help`
