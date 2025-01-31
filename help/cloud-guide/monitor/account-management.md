---
title: New Relic-Kontoverwaltung
description: Erfahren Sie, wie Sie auf Ihr New Relic-Konto zugreifen und den Zugriff, die Integrationen und die Tool-Nutzung für Ihr Adobe Commerce on Cloud-Infrastrukturprojekt verwalten können.
feature: Cloud, Observability
role: Admin
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 0%

---

# New Relic-Kontoverwaltung

Wenn Adobe Ihr Cloud-Infrastrukturprojekt bereitstellt, erhält der Lizenzinhaber eine E-Mail von New Relic mit Anmeldeinformationen und Anweisungen für den Zugriff auf das New Relic-Konto. Wenn Sie die E-Mail nicht erhalten haben, setzen Sie das New Relic-Kennwort mithilfe der E-Mail-Adresse des Lizenzinhabers zurück.

## Verwalten des Benutzerzugriffs

>[!NOTE]
>
>Gewähren Sie nur Benutzern vollständigen Zugriff, die Zugriff auf den gesamten Funktionssatz benötigen.

**Zugriff auf die Benutzerverwaltung in New Relic**:

1. Melden Sie sich bei Ihrem [New Relic-Konto an](https://login.newrelic.com/login).

1. Wählen Sie in der linken unteren Navigationsleiste Ihren Benutzernamen aus.

1. Klicken Sie auf **[!UICONTROL Administration]** und wählen Sie eine der folgenden Optionen aus der Liste aus:

   - **[!UICONTROL User management]** Sie einen Benutzer hinzufügen und aktive Benutzer und ausstehende Einladungen verwalten.

   - **[!UICONTROL Access management]** zum Verwalten von Benutzergruppen, Rollen und Konten.

Siehe [Benutzerverwaltung](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) in der Dokumentation zu _New Relic_.

## Konfigurieren von New Relic für die Starter-Umgebung

>[!NOTE]
>
>**Pro-Umgebungen** sind für die Verwendung von New Relic-Services vorkonfiguriert und können Anweisungen zum Aktivieren und Verbinden überspringen. Wenn New Relic APM nicht in der Staging- und Produktionsumgebung installiert ist oder New Relic Infrastructure in der Produktionsumgebung nicht verfügbar ist, [ Sie ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um die Installation anzufordern.

Für Starterumgebungen müssen Sie die `.magento.app.yaml` überprüfen, um sicherzustellen, dass der Abschnitt `runtime` die New Relic-Erweiterung enthält. Wenn die Erweiterung nicht konfiguriert wurde, fügen Sie Folgendes hinzu:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Lizenzschlüssel anwenden

Um eine Cloud-Umgebung mit New Relic zu verbinden, fügen Sie den New Relic-Lizenzschlüssel zur Umgebung hinzu.

- Bei **Pro-Projekten** fügt Adobe während des Bereitstellungsprozesses den Lizenzschlüssel zu Ihren Produktions- und Staging-Umgebungen hinzu. Sie können sich bei Ihrem [New Relic-Konto anmelden](https://login.newrelic.com/login) um die Verbindung zwischen Ihrer Adobe Commerce on Cloud Infrastructure-Site und New Relic zu überprüfen.

- Für **Starterprojekte** verfügen Sie über einen New Relic-Lizenzschlüssel, der bis zu _drei_ Umgebungen unterstützt. Sie müssen den Schlüssel manuell zu Ihren Umgebungskonfigurationen hinzufügen. Starterumgebungen sind für die Verwendung des New Relic-Service nicht vorab bereitgestellt.

Aktivieren Sie für Einstiegsumgebungen die New Relic-Integration, indem Sie den New Relic-Lizenzschlüssel zur Umgebungskonfiguration hinzufügen. Fügen Sie den Schlüssel zu den Staging- und Produktionsumgebungen und einer anderen Umgebung Ihrer Wahl hinzu. Für die Konfiguration ist nur der New Relic-Lizenzschlüssel erforderlich. Informationen zu zusätzlichen Konfigurationsoptionen finden Sie im Thema [New Relic-Berichte](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) im _Adobe Commerce-Benutzerhandbuch_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Anmeldedaten für die Adobe Commerce-Kontoseite oder für die mit Ihrem Projekt verknüpfte New Relic-Lizenz
>- [Zugriff auf Admin-Ebene](../project/user-access.md) auf die zu konfigurierenden Starter-Umgebungen
>- Anmeldedaten für den Zugriff auf [Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) für die Umgebung

**So konfigurieren Sie New Relic für Starter-Umgebungen**:

1. Suchen Sie Ihren New Relic-Lizenzschlüssel über die [!DNL Cloud Console] oder die Cloud-CLI.

   **[!DNL Cloud Console]Methode**:

   - Öffnen Sie Ihr Cloud[Projekt (Kontoseite](https://accounts.magento.cloud/user).

   - Suchen Sie auf _Registerkarte_ Ihr Projekt.

   - Klicken Sie **Details anzeigen**, um Informationen zur Projektinfrastruktur anzuzeigen.

   - Erweitern Sie den Abschnitt **New Relic Service**, um den Lizenzschlüssel anzuzeigen.

   - Kopieren Sie den Lizenzschlüssel.

   **Cloud-CLI-Methode**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Fügen Sie den New Relic-Lizenzschlüssel mithilfe der `magento-cloud` CLI einer Umgebung hinzu.

   - Ändern Sie die Umgebung, für die der Lizenzschlüssel erforderlich ist.
   - Aktualisieren Sie den Variablenwert mit dem folgenden `magento-cloud` CLI-Befehl:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Optional können Sie es über die [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store) hinzufügen.

1. Melden Sie sich bei Ihrem [New Relic-](https://login.newrelic.com/login) an, um sicherzustellen, dass Sie Daten aus der Adobe Commerce-Umgebung anzeigen können. Siehe [Untersuchung der Leistung](investigate-performance.md).

### Lizenzschlüssel entfernen

Sie können Ihren New Relic-Lizenzschlüssel nur in drei aktiven Umgebungen verwenden. Wenn der Schlüssel in drei Umgebungen verwendet wird, müssen Sie den Schlüssel aus einer der Umgebungen entfernen, bevor Sie ihn einer anderen Umgebung hinzufügen können.

**So entfernen Sie einen Lizenzschlüssel aus einer Umgebung**:

1. Auflisten von Umgebungsvariablen.

   ```bash
   magento-cloud variable:list
   ```

   Beispielantwort:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Wenn Sie den Lizenzschlüssel als _Projekt“-_ hinzugefügt haben, müssen Sie diese Variable auf Projektebene entfernen. Eine Projektvariable fügt die Lizenz zu _erstellten_ hinzu, die das Lizenzlimit verbrauchen oder überschreiten kann. So listen Sie Projektvariablen auf: `magento-cloud variable:list --level project`

1. Löschen Sie die Lizenzvariable .

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
