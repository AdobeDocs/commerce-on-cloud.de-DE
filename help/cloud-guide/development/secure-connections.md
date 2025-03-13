---
title: Sichere Verbindungen
description: Erfahren Sie, wie Sie SSH-Schlüssel auf Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt anwenden und sich bei Remote-Umgebungen anmelden.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: c25e5b74ae8105995107860246ecb9ba45910bb1
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---

# Sichere Verbindungen zu Remote-Umgebungen

Secure Shell (SSH) ist ein gängiges Protokoll, das verwendet wird, um sich sicher bei Remote-Servern und Systemen anzumelden. Sie können SSH verwenden, um auf Ihre Remote-Umgebungen zuzugreifen, um die Adobe Commerce-Anwendung zu verwalten und auf Remote-Umgebungsprotokolle zuzugreifen. Adobe unterstützt nur sichere FTP (sFTP)-Verbindungen, die den öffentlichen SSH-Schlüssel verwenden. FTP-Verbindungen werden nicht unterstützt.

## Erzeugen eines SSH-Schlüsselpaars

Erstellen Sie auf jedem Computer und Arbeitsbereich ein SSH-Schlüsselpaar, das Zugriff auf den Quell-Code und die Umgebungen Ihres Projekts erfordert. Mit dem SSH-Schlüssel können Sie eine Verbindung zu GitHub herstellen, um Quell-Code zu verwalten und eine Verbindung zu Cloud-Servern herzustellen, ohne ständig Ihren Benutzernamen und Ihr Passwort angeben zu müssen. Siehe [Verbindung zu GitHub mit SSH herstellen](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) für weitere Anweisungen zum Erstellen eines SSH-Schlüsselpaars.

- Der _öffentliche Schlüssel_ kann für den Zugriff auf eine Site, SSH und sFTP bereitgestellt werden.
- Der _private Schlüssel_ bleibt auf der Workstation privat.

>[!CAUTION]
>
>**Geben Sie niemals Ihren privaten Schlüssel frei.** Fügen Sie es nicht einem Ticket hinzu, kopieren Sie es nicht in einen Chat oder fügen Sie es E-Mails hinzu.

## Hinzufügen eines öffentlichen SSH-Schlüssels zu Ihrem Konto

Nachdem Sie Ihren öffentlichen SSH-Schlüssel zu Ihrem Adobe Commerce-Konto in der Cloud-Infrastruktur hinzugefügt oder aktualisiert haben[ stellen Sie „Alle aktiven Umgebungen erneut bereitstellen](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy) auf Ihrem Konto bereit, um den Schlüssel zu installieren.

Sie können SSH-Schlüssel zu Ihrem Konto hinzufügen, indem Sie eine der folgenden Methoden verwenden: Cloud-CLI oder [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Hinzufügen Ihres SSH-Schlüssels mithilfe der Cloud-CLI

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Melden Sie sich bei Ihrem Projekt an:

   ```bash
   magento-cloud login
   ```

1. Fügen Sie den öffentlichen Schlüssel hinzu.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Sie können SSH-Schlüssel mithilfe der Cloud-CLI-Befehle `ssh-key:list` und `ssh-key:delete` auflisten und löschen.

>[!TAB Konsole]

### Fügen Sie Ihren SSH-Schlüssel mit dem [!DNL Cloud Console] hinzu

**So fügen Sie einen SSH-Schlüssel zu einem neuen Projekt hinzu**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Klicken Sie auf **[!UICONTROL No SSH key]**. Dieses Symbol befindet sich rechts neben dem Befehlsfeld und ist sichtbar, wenn das Projekt keinen SSH-Schlüssel enthält.

1. Kopieren Sie den Inhalt Ihres öffentlichen SSH-Schlüssels und fügen Sie ihn in das Feld **Öffentlicher Schlüssel** ein.

1. Befolgen Sie die restlichen Eingabeaufforderungen.

**So fügen Sie einen SSH-Schlüssel zu Ihrem Cloud-Profil hinzu**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Klicken Sie im Kontomenü oben rechts auf **Mein Profil**.

1. Klicken Sie in der _SSH_ Ansicht auf **Öffentlichen Schlüssel hinzufügen**.

1. Geben Sie im _SSH-Schlüssel hinzufügen_-Formular Ihrem Schlüssel einen **Titel** und fügen Sie den öffentlichen SSH-Schlüssel in das Feld **Schlüssel** ein.

1. Klicken Sie **Speichern**.

>[!ENDTABS]

## Verbindung zu einer Remote-Umgebung herstellen

Sie können eine Verbindung zu einer Remote-Umgebung über die `magento-cloud` CLI oder einen SSH-Befehl herstellen. Die `magento-cloud` CLI-Befehle können nur in Starter- und Pro-Integrationsumgebungen verwendet werden.

### Verwenden der Cloud-CLI

**So melden Sie sich bei einer Remote-Integrationsumgebung an**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Listen Sie die Umgebungen in diesem Projekt auf.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### SSH-Befehl verwenden

Die [!DNL Cloud Console] enthält eine Liste von Web- und SSH-Zugriffsbefehlen für jede Umgebung.

**So kopieren Sie den SSH-Befehl**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie eine Umgebung.

1. Klicken Sie auf **[!UICONTROL SSH]**.

1. Klicken Sie auf der _SSH_-Registerkarte auf die Schaltfläche Kopieren , um den vollständigen SSH-Befehl in die Zwischenablage zu kopieren.

1. Öffnen Sie ein Terminal und fügen Sie den SSH-Befehl ein, um eine Verbindung herzustellen.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Bei Pro-Staging- und Produktionsumgebungen kann der SSH-Befehl wie folgt aussehen:
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce in der Cloud-Infrastruktur unterstützt den Zugriff auf Ihre Umgebungen über sFTP (sicheres FTP) mit SSH-Authentifizierung. Verwenden Sie einen Client, der die SSH-Schlüsselauthentifizierung für sFTP unterstützt, und verwenden Sie Ihren öffentlichen SSH-Schlüssel. Ihr öffentlicher SSH-Schlüssel muss der Zielumgebung hinzugefügt werden. Für Starter-Umgebungen und Pro-Integrationsumgebungen können Sie [über die  [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

Schreibgeschützte sFTP-Verbindungen werden _nicht_ unterstützt. Der sFTP-Zugriff wird standardmäßig mit der Berechtigung _Schreiben_ bereitgestellt.

Verwenden Sie beim Konfigurieren von sFTP die Informationen aus Ihrem SSH-Zugriffsumgebungsbefehl: `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Benutzername**: Alle Inhalte vor dem `@` in Ihrem SSH-Zugriffsziel.
- **Kennwort**: Sie benötigen kein Kennwort für sFTP. Der SFTP-Zugriff verwendet die SSH-Schlüsselauthentifizierung.
- **Host**: Alle Inhalte nach dem `@` in Ihrem SSH-Zugriff.
- **Port**: 22. Dies ist der standardmäßige SSH-Port.
- **SSH** Privater Schlüssel: Geben Sie bei Bedarf den Speicherort Ihres privaten Schlüssels für den sFTP-Client an. Standardmäßig werden private Schlüssel im `~/.ssh` gespeichert.

Je nach Client sind möglicherweise zusätzliche Optionen erforderlich, um die SSH-Authentifizierung für sFTP abzuschließen. Lesen Sie die Dokumentation für Ihren ausgewählten Client.

Für **Starter-Umgebungen und Pro-Integrationsumgebungen** sollten Sie auch erwägen, [eine `mount`](../application/properties.md#mounts) für den Zugriff auf ein bestimmtes Verzeichnis hinzuzufügen. Sie würden das Einhängeelement zu Ihrer `.magento.app.yaml` Datei hinzufügen. Eine Liste der beschreibbaren Verzeichnisse finden Sie unter [Projektstruktur](../project/file-structure.md). Dieser Bereitstellungspunkt funktioniert nur in diesen Umgebungen.

Wenn Sie für **Pro Staging- und Produktionsumgebungen** keinen SSH-Zugriff auf die Umgebung haben, müssen Sie [ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um den sFTP-Zugriff anzufordern, und einen Bereitstellungspunkt für den Zugriff auf den spezifischen Ordner, z. B. `pub/media`.

>[!NOTE]
>Wenn die sFTP-Verbindung für Pro Staging und Produktion für einen _generischen_ Benutzer vorgesehen ist, der **nicht** zum Cloud-Projekt [ muss](../project/user-access.md) müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) mit dem **öffentlichen**-Schlüssel angehängt. **Geben Sie niemals Ihren privaten SSH-Schlüssel an.**

## SSH-Tunneleffekt

Sie können SSH-Tunneling verwenden, um eine Verbindung zu einem Service aus Ihrer lokalen Entwicklungsumgebung herzustellen, als ob der Service lokal wäre. Konfigurieren Sie vor dem Tunneln Ihr [SSH](#add-an-ssh-public-key-to-your-account).

Verwenden Sie eine Terminal-Anwendung, um sich anzumelden und Befehle auszugeben.

```bash
magento-cloud login
```

Überprüfen Sie mithilfe von , ob Tunnel geöffnet sind.

```bash
magento-cloud tunnel:list
```

Um einen Tunnel zu erstellen, müssen Sie den [Anwendungsnamen“ ](../application/properties.md#name). Sie können den Anwendungsnamen mithilfe der CLI überprüfen:

```bash
magento-cloud apps
```

### Einrichten des SSH-Tunnels

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Um beispielsweise einen Tunnel zum `sprint5` Zweig in einem Projekt mit einer App namens `mymagento` zu öffnen, geben Sie Folgendes ein

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Beispielantwort:

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**So zeigen Sie Informationen über Ihren Tunnel an**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Mit Services verbinden

Nachdem Sie einen SSH-Tunnel eingerichtet haben, können Sie eine Verbindung zu -Services herstellen, als ob Sie lokal ausgeführt würden. Um beispielsweise eine Verbindung zur Datenbank herzustellen, verwenden Sie den folgenden Befehl:

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
