---
title: Verzweigungen mit dem  [!DNL Cloud Console] verwalten
description: Erfahren Sie, wie Sie die Umgebungszweige für Adobe Commerce in der Cloud-Infrastruktur mit dem  [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Verwalten von Verzweigungen mit dem [!DNL Cloud Console]

Sie können Ihre Umgebungen entweder über die [!DNL Cloud Console] oder die `magento-cloud` CLI verwalten. Ihre Projektdateien werden in einem Git-Repository gespeichert. Sie können Git-Befehle verwenden, um Ihren Code zu verwalten, aber die `magento-cloud`-CLI ist für die Interaktion mit Platform-Funktionen konzipiert, während die Git-Befehle dies nicht tun. Siehe [Git-Befehle](../dev-tools/cloud-cli-overview.md#git-commands) im Cloud-CLI-Thema.

In diesem Abschnitt wird die Verwendung des [!DNL Cloud Console] für folgende Zwecke erläutert:

- Hinzufügen oder Löschen einer Umgebung
- Synchronisieren (`git pull`) mit der übergeordneten Umgebung
- Zusammenführen (`git push`) mit der übergeordneten Umgebung

>[!TIP]
>
>Es können keine Verzweigungen aus Pro-Staging- und Produktionsumgebungen erstellt werden. Sie können eine Verzweigung aus der `master` Verzweigung erstellen.

## Erstellen einer Umgebung

Die Verzweigungsstrategie verwendet einen gängigen Git-Workflow, bei dem Sie Code entwickeln und Erweiterungen in einer Entwicklungsverzweigung hinzufügen. Siehe [Starter](../architecture/starter-architecture.md) und [Pro](../architecture/starter-develop-deploy-workflow.md) Architekturübersichten.

- Erstellen Sie zunächst eine `staging` Verzweigung aus der `master` Verzweigung und dann eine Verzweigung aus `staging` für die Entwicklung.
- Erstellen Sie für Pro eine Entwicklungsverzweigung aus der `Integration`.

Ihr Konto unterstützt eine begrenzte Anzahl von ![aktiven](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inaktiven) Entwicklungszweigen. Verwalten Sie aktive und inaktive Verzweigungen, indem Sie eine Verzweigung nur mit der [!DNL Cloud Console] oder der Cloud-CLI hinzufügen oder löschen. Bevor Sie eine Verzweigung löschen können, deaktivieren Sie die Verzweigung, die in der Liste _Umgebungen_ als &quot;_&quot;_ bleibt. Sie können die Verzweigung später erneut aktivieren oder [die Verzweigung löschen](../dev-tools/cloud-cli-overview.md#) in den Umgebungseinstellungen oder über die Cloud-CLI.

Wenn Sie zusätzliche aktive Umgebungen für die Entwicklung benötigen, reichen Sie ein [Support-Ticket](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) ein.

**So fügen Sie eine Verzweigung**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie eine Umgebung.

   >[!TIP]
   >
   >Die neue Verzweigung wird aus dieser Umgebung geklont. Wählen Sie eine übergeordnete Umgebung aus, die der Umgebung ähnelt, die Sie gerade erstellen.

1. Klicken Sie auf **[!UICONTROL Branch]**.

   ![Verzweigung erstellen](../../assets/button-branch.png){width="150"}

1. Geben _im Formular Verzweigung von …_ einen Namen für die Verzweigung ein.

   Der Umgebungsname _name_ unterscheidet sich von der Umgebung _ID_ nur dann, wenn Sie im Umgebungsnamen Leerzeichen oder Großbuchstaben verwenden. Eine Umgebungs-ID besteht aus Kleinbuchstaben, Zahlen und zulässigen Symbolen. Großbuchstaben im Umgebungsnamen werden in der ID in Kleinbuchstaben umgewandelt, Leerzeichen im Umgebungsnamen in Bindestriche.

   Ein Umgebungsname **darf keine Zeichen enthalten** die für Ihre Linux-Shell oder für reguläre Ausdrücke reserviert sind. Unzulässige Zeichen sind geschweifte Klammern (`{ }`), Klammern, Sternchen (`*`), spitze Klammern (`>`), kaufmännisches Und-Zeichen (`&`), Prozent (<code>%</code>) und andere Zeichen.

1. **[!UICONTROL Environment type]** auswählen.

1. Klicken Sie auf **[!UICONTROL Create Branch]**.

1. Warten Sie, während die Umgebung bereitgestellt wird.

   Während der Bereitstellung lautet der Umgebungsstatus &quot;**&quot;**. Nach erfolgreicher Bereitstellung ändert sich der Status in ein grünes Häkchen für **Erfolg**.

## Erstellen einer inaktiven Verzweigung

Sie können keine inaktive Verzweigung über die Adobe Commerce Cloud-Konsole oder die CLI erstellen. Wenn Sie eine inaktive Verzweigung erstellen möchten, erstellen Sie sie im Git-Repository und drücken Sie die Option `environment.Parent` im Befehl .

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Löschen einer Umgebung

Bevor Sie eine Umgebung löschen können, müssen Sie sie deaktivieren. Sobald eine Umgebung inaktiv ist, können Sie sie löschen.

**So deaktivieren Sie eine**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie die Umgebung in der Navigationsleiste _Umgebung_ aus.

1. Klicken Sie auf das Symbol Konfigurieren rechts in der oberen Navigationsleiste, um die Umgebungseinstellungen zu öffnen.

1. Scrollen Sie auf der Registerkarte _[!UICONTROL General]_&#x200B;nach unten zum Abschnitt&#x200B;_[!UICONTROL Deactivate environment]_, klicken Sie auf **[!UICONTROL Deactivate environment and delete data]** und befolgen Sie die Anweisungen.

## Synchronisieren einer Umgebung

Das Synchronisieren einer Umgebung (oder Verzweigung) entspricht dem `git pull origin <parent>`. Sie können aktualisierten Code aus einer übergeordneten Umgebung synchronisieren. Sie können diese Funktion über die [!DNL Cloud Console] für alle Starter- und Pro-Umgebungen verwenden.

Für den Pro-Plan können Sie von der Staging- und Produktionsumgebung aus mit Ihrer `master` synchronisieren. Diese Synchronisierung ruft nur Code ab und pusht ihn, keine Daten. Um Daten zu synchronisieren, sichern Sie die Datenbankdaten und übertragen Sie sie in die Datenbank einer anderen Umgebung. Siehe [Migrieren und Bereitstellen von statischen Dateien und Daten](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**So synchronisieren Sie eine Umgebung**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Klicken Sie in der Liste Umgebung auf den Namen der zu synchronisierenden Verzweigung.

1. Klicken (Synchronisieren).

   ![Synchronisieren einer Umgebung](../../assets/button-sync.png){width="150"}

1. Auswahl der zu synchronisierenden Elemente.

   - Ersetzen Sie die Daten - (Daten und Dateien) synchronisiert Änderungen in der Datenbank und den Inhaltsdateien aus der übergeordneten Verzweigung.
   - Zusammenführen - (Code) Synchronisiert aktualisierten Code aus der übergeordneten Verzweigung.

   Dadurch wird auch ein CLI-Befehl zum Kopieren und Verwenden erstellt.

1. Klicken Sie auf **Synchronisieren**.

## Mit übergeordneter Umgebung zusammenführen

Das Zusammenführen einer Umgebung (oder Verzweigung) entspricht dem `git push origin`. Sie führen zusammen, um aktualisierten Code aus einer Umgebung in die übergeordnete Umgebung zu pushen. Sie können diesen Code zu `master` zusammenführen. Sie können die Bereitstellung für die Staging- und Produktionsumgebung mit dem Befehl `merge` durchführen.

**Zusammenführen mit der übergeordneten Umgebung**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Klicken Sie in der Liste Umgebung auf den Namen der zusammenzuführenden Verzweigung.

1. Klicken (Zusammenführen).

   ![Zusammenführen einer Umgebung](../../assets/button-merge.png){width="150"}

1. Klicken Sie **Zusammenführen** und bestätigen Sie die Aktion.

## Protokolle anzeigen

Über die [!DNL Cloud Console] können Sie verschiedene Protokolle für Umgebungen überprüfen, einschließlich des Verlaufs von Erstellung, Bereitstellung und Bereitstellung.

Für **Starter** können Sie Build- und Bereitstellungsprotokolle und den Bereitstellungsverlauf überprüfen. Zu diesen Umgebungen gehören die `master` (Produktions-)Verzweigung und alle daraus erstellten Verzweigungen.

Für **Pro** können Sie die folgenden Protokolle in jeder Umgebung überprüfen:

- Integration - Erstellung und Bereitstellung sowie Bereitstellungsverlauf
- Staging : Erstellen von Protokollen und Bereitstellungsverlauf. Verwenden Sie SSH, um sich beim Server anzumelden und Bereitstellungsprotokolle anzuzeigen.
- Produktion - Erstellen von Protokollen und Bereitstellungsverlauf. Verwenden Sie SSH, um sich beim Server anzumelden und Bereitstellungsprotokolle anzuzeigen.

**So zeigen Sie Protokolle in der[!DNL Cloud Console]** an:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie eine Umgebung.

   Die Umgebungsansicht enthält eine [Aktivitätenliste](activity-stream.md) in der _kürzliche_ Ereignisse, ein Eintrag pro versuchter Aktion, einschließlich Synchronisierungen, Zusammenführungen, Verzweigungen, Sicherungen und mehr, angezeigt werden. Klicken Sie **Alle**, um den vollständigen Bereitstellungsverlauf anzuzeigen.

1. Um das Build-Protokoll anzuzeigen, wählen Sie den Link Erfolg oder Fehler pro Bereitstellungseintrag im Konto aus.

>[!TIP]
>
>Klicken Sie auf **Symbol** Filtern nach“ für eine Dropdown-Liste und wählen Sie den Nachrichtentyp aus, der angezeigt werden soll.

## Code aus einem privaten Git-Repository abrufen

Ihr Adobe Commerce in Cloud-Infrastrukturprojekt kann Code aus einem privaten Git-Repository enthalten. Beispielsweise können Sie Code für ein benutzerdefiniertes Modul oder Design in einem privaten Repository haben. Dazu müssen Sie den öffentlichen SSH-Schlüssel Ihres Projekts zu Ihrem privaten Git-Repository hinzufügen und Ihre Projekt-`composer.json` aktualisieren.

Um Ihrem privaten GitHub-Repository einen Bereitstellungsschlüssel hinzuzufügen, müssen Sie der Administrator dieses Repositorys sein. Mit GitHub können Sie einen Bereitstellungsschlüssel nur für ein Repository verwenden.

Wenn Sie es vorziehen, dass Ihr Projekt auf mehrere Repositorys zugreift, können Sie einen SSH-Schlüssel an ein automatisiertes Benutzerkonto anhängen. Da dieses Konto nicht von einem Menschen verwendet wird, wird es als &quot;[&quot; ](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Fügen Sie das Computerkonto als Mitarbeiter hinzu oder fügen Sie den Computerbenutzer einem Team mit Zugriff auf die Repositorys hinzu.

>[!INFO]
>
>Adobe empfiehlt, diesen Code zu Ihren Projekt-Git-Repositorys hinzuzufügen und zusammenzuführen. Wenn Sie die Verbindung nicht konfigurieren, können Build-Probleme auftreten.

**So finden Sie Ihren öffentlichen SSH-Schlüssel**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Klicken Sie auf das Konfigurationssymbol auf der rechten Seite der oberen Navigationsleiste.

1. Klicken _in &quot;_&quot; auf **[!UICONTROL Deploy Key]**.

1. Kopieren Sie den Bereitstellungsschlüssel in die Zwischenablage zur Verwendung in einer der folgenden Git-basierten Methoden:

>[!BEGINTABS]

>[!TAB GitHub]

### GitHub-Bereitstellungsschlüssel eingeben

Auf GitHub sind Bereitstellungsschlüssel standardmäßig schreibgeschützt.

**So geben Sie Ihren öffentlichen Projektschlüssel als GitHub-Bereitstellungsschlüssel**:

1. Melden Sie sich bei Ihrem GitHub-Repository als Administrator an.
1. Klicken Sie auf die Registerkarte Repository-**[!UICONTROL Settings]** .

   >[!NOTE]
   >
   >Wenn diese Option nicht angezeigt wird, sind Sie nicht als Repository-Administrator angemeldet und können diese Aufgabe nicht abschließen. Bitten Sie Ihren GitHub-Repository-Administrator, dies zu tun.

1. Klicken Sie auf _Registerkarte_ Einstellungen“ im linken Navigationsbereich auf **[!UICONTROL Deploy Keys]**.
1. Klicken Sie auf **[!UICONTROL Add deploy key]**.
1. Befolgen Sie die Eingabeaufforderungen.

Verwenden Sie in `composer.json` das `<user>@<host>:<.git</code>`-Format oder `ssh://<user>@<host>:<port>/<path>.git`, wenn Sie einen nicht standardmäßigen Port verwenden.

>[!TAB Bitbucket]

### Bitbucket-Bereitstellungsschlüssel eingeben

**So geben Sie Ihren öffentlichen Projektschlüssel als Bitbucket-Bereitstellungsschlüssel ein**:

1. Melden Sie sich bei Ihrem Bitbucket-Repository als Administrator an.
1. Klicken Sie im linken Navigationsbereich auf **[!UICONTROL Settings]**.
1. Klicken Sie auf Allgemein > **[!UICONTROL Deployment Keys]**.
1. Klicken Sie auf **[!UICONTROL Add Key]**.
1. Befolgen Sie die Eingabeaufforderungen.

>[!TAB GitLab]

### GitLab-Bereitstellungsschlüssel eingeben

**So fügen Sie den öffentlichen SSH-Schlüssel für Ihr Projekt als GitLab-Bereitstellungsschlüssel hinzu**:

1. Melden Sie sich bei Ihrem GitLab-Repository als Besitzer an.
1. Stellen Sie sicher _dass die Option_ Pipelines“ für Ihr Projekt aktiviert ist:

   1. Erweitern Sie in den Projekteinstellungen den Abschnitt **[!UICONTROL Visibility, project, features, permissions]** .
   1. Klicken Sie ggf. auf **[!UICONTROL Pipelines]** , um die Option zu aktivieren.

1. Fügen Sie Ihren öffentlichen SSH-Schlüssel zu den CI/CD-Einstellungen hinzu.

   1. Klicken Sie in der linken Navigation auf Einstellungen > **[!UICONTROL CI / CD]**.
   1. Klicken Sie auf Schlüssel bereitstellen **Erweitern**, um den Schlüssel zu konfigurieren.
   1. Fügen Sie im Formular _Schlüssel bereitstellen_ einen Namen für den Bereitstellungsschlüssel zum Feld **[!UICONTROL Title]** hinzu und fügen Sie Ihren öffentlichen SSH-Schlüssel in das Feld **[!UICONTROL Key]** ein.
   1. Klicken Sie auf **[!UICONTROL Add Key]** , um die Konfiguration zu speichern.

>[!ENDTABS]

## Sichere Umgebungen und Verzweigungen

Sie können über einen Webbrowser von jedem Speicherort aus auf Ihr Projekt und Ihre Umgebungen zugreifen, indem Sie die [!DNL Cloud Console] verwenden. Möglicherweise ist die Sicherheit für die Produktionsumgebung, die Stores und die Sites festgelegt. In diesem Abschnitt erfahren Sie, wie Sie Ihre Integrations- und Staging-Umgebungen ausschließlich für Ihre Entwickler, DBAs und mehr sichern.

>[!WARNING]
>
>**VERWENDEN SIE** folgenden Methoden zum Schützen von Pro-Staging- und Produktionsumgebungen. Dadurch wird die Fastly-Zwischenspeicherung unterbrochen. Verwenden Sie die Funktion [Blockieren](../cdn/fastly-vcl-blocking.md), die im Fastly CDN für Adobe Commerce verfügbar ist.

**Sichere Umgebungen**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie eine Umgebung aus und klicken Sie auf das Konfigurationssymbol in der Navigationsleiste.

1. Klicken Sie auf der Registerkarte _Allgemein_ für **[!UICONTROL HTTP access control enabled]** auf **EIN**, um den sicheren Zugriff zu aktivieren. Sie können zwischen Anmeldeinformationen oder IP-Adressen wählen, um nach Zugriff zu filtern.

1. Um nach Anmeldeinformationen zu filtern, klicken Sie auf **[!UICONTROL Add Login]**, geben Sie einen Benutzernamen und ein Kennwort ein und klicken Sie auf **[!UICONTROL Add Login]** , um sie hinzuzufügen.

1. Um nach IP-Adresse zu filtern, geben Sie die IP-Adressen in eine Liste mit `deny` oder `allow` ein. Beispiel:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Klicken Sie auf **[!UICONTROL Save]**. Dadurch wird die Umgebung erneut bereitgestellt, um die Sicherheits- und Konfigurationseinstellungen zu aktualisieren. Adobe empfiehlt, die Umgebung nach Abschluss der Sicherheitseinstellungen zu testen.
