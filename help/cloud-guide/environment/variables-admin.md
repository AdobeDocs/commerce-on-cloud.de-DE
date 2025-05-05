---
title: ADMIN-Variablen
description: Hier finden Sie eine Liste der Umgebungsvariablen, die bei der Installation von Adobe Commerce in der Cloud-Infrastruktur verwendet werden.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Admin-Variablen

Benutzer mit administrativem Zugriff auf das Adobe Commerce on Cloud-Infrastrukturprojekt können die folgenden Projektumgebungsvariablen verwenden, um die Konfigurationseinstellungen für das Administratorbenutzerkonto für den Zugriff auf die Admin-Benutzeroberfläche zu überschreiben.

## Admin-Anmeldedaten

Sie können die Admin-Benutzeranmeldeinformationen während der Commerce-Installation mit den ADMIN-Variablen in der folgenden Tabelle überschreiben.

Wenn Sie die Werte nach der Installation ändern möchten, stellen Sie eine Verbindung mit Ihrer Umgebung her, indem Sie SSH verwenden, und verwenden Sie den Adobe Commerce CLI-[`admin:user`](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=de), um die Admin-Benutzeranmeldeinformationen zu erstellen oder zu bearbeiten.

| Variable | Standard | Beschreibung |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Lizenzinhaber - E-Mail-Adresse | Ein Benutzername für den Benutzer mit Administratorrechten, der andere Benutzer erstellen kann, einschließlich Benutzer mit Administratorrechten. |
| `ADMIN_EMAIL` |                             | E-Mail-Adresse des Administrationsbenutzers. Diese Adresse wird verwendet, um Benachrichtigungen zum Zurücksetzen des Kennworts zu senden. |
| `ADMIN_PASSWORD` |                             | Kennwort für den Benutzer mit Administratorrechten. Wenn das Projekt erstellt wird, wird ein zufälliges Kennwort generiert und eine E-Mail an den Lizenzinhaber gesendet. Bei der Projekterstellung sollte der Lizenzinhaber das Kennwort bereits geändert haben. Wenden Sie sich an den Lizenzinhaber, um das aktualisierte Passwort zu erhalten. |
| `ADMIN_LOCALE` | `en_US` | Das vom Administrator verwendete Standardgebietsschema. |

## Admin-URL

Verwenden Sie die folgende Umgebungsvariable, um den Zugriff auf Ihre Admin-Benutzeroberfläche zu sichern. Falls angegeben, überschreibt dieser Wert die Standard-URL während der Installation.

`ADMIN_URL` - Die relative URL für den Zugriff auf die Admin-Benutzeroberfläche. Die Standard-URL lautet `/admin`. Aus Sicherheitsgründen empfiehlt Adobe, die Standardeinstellung in eine eindeutige, benutzerdefinierte Admin-URL zu ändern, die nicht leicht zu erraten ist.

### Admin-URL ändern

Adobe empfiehlt, die Variable auf Umgebungsebene für die Admin-URL nach der Installation zu ändern. Konfigurieren Sie diese Einstellung aus Sicherheitsgründen, bevor Sie eine Verzweigung aus der geklonten `master` erstellen. Alle aus der `master` Verzweigung erstellten Verzweigungen übernehmen die Variablen und deren Werte auf Umgebungsebene.

Verwenden Sie den Befehl `magento-cloud variable:update` , um den Variablenwert zu aktualisieren. (Der `variable:set`-Befehl wird nicht mehr unterstützt und ist nicht verfügbar.) Im folgenden Beispiel wird die ADMIN_URL zu `newAdmin_A8v10` aktualisiert:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>Der `ADMIN_URL`-Wert akzeptiert Buchstaben (a-z oder A-Z), Zahlen (0-9) und den Unterstrich (_) für einen benutzerdefinierten Admin-Pfad. Leerzeichen oder andere Zeichen werden **nicht** akzeptiert.

**So ändern Sie die URL mithilfe der[!DNL Cloud Console]**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.

1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.

1. Wählen Sie in der Projektübersicht die Umgebung aus und klicken Sie auf das Konfigurationssymbol.

   ![Projektkonfiguration](../../assets/icon-configure.png){width="36"}

1. Wählen Sie die **Variablen** aus.

1. Klicken Sie **Variable erstellen**.

1. Geben Sie Folgendes ein:

   - **Variablenname** = `ADMIN_URL`
   - **value** = Neue URL. Legen Sie beispielsweise die Admin-URL auf `magento_A8v10` fest.

   Standardmäßig sind `Available during runtime` und `Make inheritable` ausgewählt.

1. Klicken Sie auf **Variable erstellen** und warten Sie, bis die Bereitstellung abgeschlossen ist. Diese Schaltfläche ist nur sichtbar, wenn die erforderlichen Felder Werte enthalten.
