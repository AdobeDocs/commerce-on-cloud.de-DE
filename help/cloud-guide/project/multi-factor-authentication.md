---
title: Multi-Faktor-Authentifizierung für SSH-Zugriff aktivieren
description: Erfahren Sie, wie Sie Authentifizierungsanforderungen für den SSH-Zugriff auf Adobe Commerce in Cloud-Infrastrukturumgebungen verwalten.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Multi-Faktor-Authentifizierung für SSH-Zugriff aktivieren

Um die Sicherheit zu erhöhen, bietet Adobe Commerce in der Cloud-Infrastruktur eine MFA-Durchsetzung (Multi-Factor Authentication, mehrstufige Authentifizierung), um die Authentifizierungsanforderungen für den SSH-Zugriff auf Cloud-Umgebungen zu verwalten.

Wenn MFA für ein Projekt aktiviert ist, benötigen alle Benutzerkonten mit SSH-Zugriff entweder einen Zwei-Faktor-Authentifizierungs-Code (TFA) oder ein API-Token und ein SSH-Zertifikat, um auf die Umgebung zuzugreifen.

>[!NOTE]
>
>MFA ist in Cloud-Projekten standardmäßig nicht aktiviert. Der Kontoinhaber für das Adobe Commerce on Cloud-Infrastrukturprojekt muss [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um es zu aktivieren. Wenn MFA aktiviert ist, muss für alle Benutzenden in ihrem Adobe Commerce in der Cloud-Infrastruktur-Konto die Zwei-Faktor-Authentifizierung (TFA) aktiviert sein, damit SSH-Zugriff auf die Projektumgebungen möglich ist.

## Zertifikate für SSH-Zugriff

MFA ermöglicht es Benutzenden, ein OAUTH-Zugriffstoken gegen ein kurzlebiges SSH-Zertifikat auszutauschen, das von der Adobe Cloud Certifier-API generiert wurde. Wenn der Benutzer über die Admin- oder Mitwirkende-Rolle, einen gültigen SSH-Schlüssel und einen gültigen TFA-Code oder ein gültiges API-Token verfügt, verwendet Adobe Commerce in der Cloud-Infrastruktur diese Anmeldeinformationen, um das temporäre SSH-Zertifikat zu generieren. Die Gültigkeitsdauer des Zertifikats ist auf eine Stunde festgelegt, wird aber während der aktuellen Sitzung automatisch aktualisiert.

Nach der Anmeldung bei einem Projekt mit MFA müssen Benutzer die `magento-cloud` CLI verwenden, um das SSH-Zertifikat zu generieren:

```bash
magento-cloud ssh-cert:load
```

Der Befehl `ssh-cert:load` generiert das SSH-Zertifikat und installiert es im SSH-Agenten des lokalen Benutzers.

### Zertifikat bei der Anmeldung automatisch generieren

Sie können Ihre lokale Umgebung so konfigurieren, dass das SSH-Zertifikat automatisch generiert wird, wenn Sie sich bei der `magento-cloud` CLI authentifizieren.

**So fügen Sie die automatische SSH-Zertifikatgenerierung zu Ihrer `magento-cloud` CLI-Konfiguration hinzu**:

1. Erstellen Sie auf Ihrer lokalen Workstation eine Datei mit dem Namen `config.yaml` im Ordner `.magento-cloud` in Ihrem Basisverzeichnis, falls diese noch nicht vorhanden ist.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Fügen Sie der `config.yaml`-Datei die folgende Konfiguration hinzu.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Verwenden Sie die `magento-cloud` CLI, um sich erneut zu authentifizieren:

   >Abmelden:

   ```bash
   magento-cloud logout
   ```

   >Anmelden:

   ```bash
   magento-cloud login
   ```

   >Befolgen Sie die Antwort:

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Verbinden mit einer Umgebung mithilfe von SSH mit TFA

Wenn MFA für ein Projekt aktiviert ist, muss TFA für Ihr Konto aktiviert sein, bevor Sie mithilfe eines SSH eine Verbindung zu einer Remote-Umgebung herstellen können. Siehe [Aktivieren von TFA](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Voraussetzungen:**

Für Projekte, die mit MFA-Durchsetzung aktiviert sind, erfordert der SSH-Zugriff die folgenden Berechtigungen und Kontoeinstellungen:

- [Admin- oder Mitwirkender-Zugriff auf die Umgebung](user-access.md)
- [SSH-Zugriffsschlüssel für Konto konfiguriert](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA für Konto aktiviert](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**So stellen Sie eine Verbindung mithilfe von SSH mit Anmeldeinformationen für TFA-Benutzerkonten her**:

1. Melden Sie sich bei [Ihr Konto](https://console.adobecommerce.com) an.

1. Verwenden Sie auf Ihrer lokalen Workstation die `magento-cloud` CLI, um das SSH-Zertifikat zu generieren.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Beispielantwort:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Verwenden Sie ein SSH, um eine Verbindung zur Remote-Umgebung herzustellen.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Verwalten von Quell-Code mithilfe von SSH mit TFA

Beim Verwalten des Quell-Codes für Adobe Commerce in Cloud-Infrastrukturprojekten verwenden Sie SSH, um sich beim Git-Repository für das Projekt zu authentifizieren. Wenn für Ihr Projekt die MFA-Durchsetzung aktiviert ist, müssen Sie ein SSH-Zertifikat generieren, bevor Sie Befehlszeilenvorgänge mit dem Git-Repository durchführen können.

**So stellen Sie eine Verbindung mithilfe von SSH mit Anmeldeinformationen für TFA-Benutzerkonten her**:

1. Melden Sie sich bei [Ihr Konto](https://console.adobecommerce.com) an und authentifizieren Sie sich mithilfe von TFA.

   >[!NOTE]
   >
   >Wenn Sie TFA in Ihrem Konto nicht aktiviert haben, müssen Sie es aktivieren. Siehe [Aktivieren von TFA in Cloud-Konten](user-access.md#enable-tfa-for-cloud-accounts).

1. Verwenden Sie auf Ihrer lokalen Workstation die `magento-cloud` CLI, um das SSH-Zertifikat zu generieren.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Beispielantwort:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Klonen Sie das Git-Repository für Ihre Projektumgebung:

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Beispielantwort:

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Herstellen einer Verbindung zu einer Umgebung mithilfe von SSH mit einem API-Token

Wenn MFA für ein Projekt aktiviert ist, erfordern automatisierte Prozesse, die SSH-Zugriff auf eine Cloud-Umgebung erfordern, ein API-Token. Sie können das Token aus einem Adobe Commerce in einem Cloud-Infrastrukturkonto mit Admin- oder Mitwirkendenzugriff für das Projekt generieren.

Für die Authentifizierung mit einem API-Token ist weiterhin die Erstellung eines SSH-Zertifikats erforderlich. Automatisierte Prozesse müssen auch die Erzeugung eines SSH-Zertifikats automatisieren.

>[!BEGINSHADEBOX]

**Voraussetzungen:**

- [Admin- oder Mitwirkender-Zugriff auf die Adobe Commerce in der Cloud-Infrastrukturumgebung](user-access.md)
- [Gültiges API-Token für Konto verfügbar](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**So stellen Sie eine Verbindung mithilfe von SSH mit einer API-Token-Berechtigung her**:

1. Melden Sie sich beim Cloud-Projekt mit API-Schlüsselauthentifizierung an.

   ```bash
   magento-cloud auth:api-token
   ```

1. Geben Sie an der Eingabeaufforderung den Wert für ein gültiges API-Token ein.

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Beispiel: automatisiertes SSH-Skript

Es gibt zwei Möglichkeiten, das API-Token zu speichern.

>[!NOTE]
>
>Wenn ein API-Token gespeichert wird, authentifiziert sich die `magento-cloud`-CLI automatisch, sodass der `magento-cloud login`-Befehl nicht ausgeführt werden muss.

**Option 1**: Erstellen einer Umgebungsvariablen zum Speichern des API-Tokens

Token in das bash_profile schreiben

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Option 2**: Hinzufügen des Tokens zur `config.yaml`

1. Erstellen Sie auf Ihrer lokalen Workstation eine Datei mit dem Namen `config.yaml` im Ordner `.magento-cloud` in Ihrem Basisverzeichnis, falls diese noch nicht vorhanden ist.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Fügen Sie der `config.yaml`-Datei die folgende Konfiguration hinzu.

   ```yaml
   api:
      token: <your api token>
   ```

>Bash-Beispielskript

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Fehlerbehebung

Verwenden Sie die folgenden Informationen, um SSH-Verbindungsanfragen aufgrund von Authentifizierungsfehlern wie `access requires MFA` oder `permission denied` zu beheben.

### Ihre Anfrage enthält kein gültiges Zertifikat

Wenn Ihre Anfrage kein gültiges Zertifikat bereitstellt, wird eine Meldung ähnlich der folgenden angezeigt:

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Führen Sie die folgenden Verfahren zur Fehlerbehebung aus, um das Verbindungsproblem zu beheben:

- Überprüfen der TFA-Konfiguration des Kontos
- Erneutes Authentifizieren und erneutes Laden des Zertifikats

**So überprüfen Sie die TFA-Konfiguration und -Authentifizierung**:

1. Melden Sie sich bei [Ihr Konto](https://console.adobecommerce.com) an.

1. Klicken Sie im Kontomenü oben rechts auf **[!UICONTROL My Profile]**.

1. Klicken Sie auf der _Mein Profil_ auf die Registerkarte **[!UICONTROL Security]** .

   Wenn TFA aktiviert ist, enthält der Abschnitt Sicherheit Optionen zum Verwalten der TFA-Konfiguration.

1. Wenn der TFA nicht eingerichtet ist, klicken Sie auf **[!UICONTROL Set up application]** und befolgen Sie die Anweisungen zum Aktivieren. Siehe [Aktivieren von TFA](user-access.md#enable-tfa-for-cloud-accounts).

1. Wenn TFA konfiguriert ist, versuchen Sie erneut, sich zu authentifizieren.

**So authentifizieren Sie sich und laden das SSH-Zertifikat neu**:

1. Verwenden Sie die `magento-cloud` CLI, um sich erneut zu authentifizieren:

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Laden Sie das SSH-Zertifikat neu:

   ```bash
   magento-cloud ssh-cert:load
   ```

### Berechtigung verweigert

Wenn der SSH-Schlüssel fehlt oder ungültig ist, gibt die SSH-Verbindungsanfrage einen `Permission denied (publickey)` Fehler zurück.

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Um das Problem zu beheben, fügen Sie den SSH-Schlüssel zu Ihrer aktuellen Sitzung hinzu oder aktualisieren Sie die SSH-Konfigurationsdatei, um Ihre SSH-Schlüssel automatisch zu laden. Siehe [Öffentlichen SSH-Schlüssel hinzufügen](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### Zugriff auf Projekte ohne MFA nicht möglich

Wenn Sie sich bei einem Projekt mit aktivierter Multifaktor-Authentifizierung (MFA) authentifizieren, erhalten Sie möglicherweise die folgende Fehlermeldung, wenn Sie eine Verbindung zu anderen Projekten herstellen, für die keine MFA erforderlich ist:

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Beispielantwort:

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Während der SSH-Zertifikatgenerierung fügt die `magento-cloud`-CLI einen zusätzlichen SSH-Schlüssel zu Ihrer lokalen Umgebung hinzu. Dieser Schlüssel wird standardmäßig verwendet, wenn Ihre lokale SSH-Konfiguration den SSH-Schlüssel für den Projektzugriff nicht enthält.

**So fügen Sie Ihren SSH-Schlüssel zur lokalen Konfiguration hinzu**:

1. Erstellen Sie die `config`, falls sie noch nicht existiert.

   ```bash
   touch ~/.ssh/config
   ```

1. Fügen Sie eine `IdentityFile` hinzu.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>Sie können mehrere SSH-Schlüssel angeben, indem Sie Ihrer Konfiguration mehrere `IdentityFile` hinzufügen.
