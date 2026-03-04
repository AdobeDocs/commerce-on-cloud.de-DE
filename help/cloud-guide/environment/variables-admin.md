---
title: ADMIN-Variablen
description: Hier finden Sie eine Liste der Umgebungsvariablen, die bei der Installation von Adobe Commerce in der Cloud-Infrastruktur verwendet werden.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: 4e751f02b92f954cd41d5523237da295a068661a
workflow-type: tm+mt
source-wordcount: '758'
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

Verwenden Sie die folgende Umgebungsvariable, um den Zugriff auf Ihre Admin-Benutzeroberfläche zu sichern. Falls angegeben, überschreibt dieser Wert die Standard-URL während der Installation. In [!DNL Adobe Commerce] auf der Cloud-Infrastruktur müssen Sie die Admin-URL mithilfe der `ADMIN_URL` in der ([!DNL Cloud Console] oder [!DNL Cloud CLI]) festlegen oder ändern. Das Ändern der Einstellung über die [!DNL Admin] gilt nur für lokale Installationen.

`ADMIN_URL` - Die relative URL für den Zugriff auf die Admin-Benutzeroberfläche. Die Standard-URL lautet `/admin`.

### Admin-URL ändern

Standardmäßig ist die URL [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html?lang=de) auf *&lt;domain_name>/admin* festgelegt. Aus Sicherheitsgründen empfiehlt Adobe, sie in eine eindeutige, benutzerdefinierte Admin-URL zu ändern, die nicht einfach zu erraten ist.

**In [!DNL Adobe Commerce] auf der Cloud** Infrastruktur müssen Sie die Admin-URL mithilfe der `ADMIN_URL` Umgebungsvariablen in der ([!DNL Cloud Console] oder [!DNL Cloud CLI]) ändern. Das Ändern der Einstellung über die [!DNL Admin] gilt nur für lokale Installationen. Bei On-Premise-Installationen folgen Sie [Benutzerdefinierte Admin-URL verwenden](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=de#use-a-custom-admin-url).

Adobe empfiehlt, die Variable auf Umgebungsebene für die Admin-URL nach der Installation zu ändern. Konfigurieren Sie diese Einstellung aus Sicherheitsgründen, bevor Sie eine Verzweigung aus der geklonten `master` erstellen. Alle aus der `master` Verzweigung erstellten Verzweigungen übernehmen die Variablen und deren Werte auf Umgebungsebene, es sei denn, die Vererbung wird auf „false“ festgelegt.

Verwenden Sie entweder die [!DNL Cloud Console] oder die [!DNL Cloud CLI], um `ADMIN_URL` festzulegen oder zu aktualisieren.

#### Option A: Ändern der Admin-URL mithilfe der [!DNL Cloud Console]

##### Integrationsumgebung

Fügen Sie in [Cloud-](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=de)&quot; eine neue Variable hinzu mit:

- **name:** `ADMIN_URL`
- **Wert:** Ihre neue Admin-URL (z. B. `magento_A8v10`)

- Detaillierte Anweisungen finden Sie unter [Hinzufügen von &#x200B;](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=de#configure-environment) oder [Umgebungsvariablen](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html?lang=de) in unserer Entwicklerdokumentation.

##### Admin-URL im [!DNL Cloud Console] festlegen

1. Melden Sie sich bei der [Cloud-Konsole](https://console.adobecommerce.com/) an.
2. Wählen Sie ein Projekt aus der Liste **[!UICONTROL All projects]** aus.
3. Wählen Sie in der Projektübersicht die Umgebung aus und klicken Sie auf das Konfigurationssymbol.
4. Wählen Sie die Registerkarte **[!UICONTROL Variables]** aus.
5. Klicken Sie auf **[!UICONTROL Create Variable]** (oder bearbeiten Sie die vorhandene `ADMIN_URL`, falls vorhanden).
6. Geben Sie Folgendes ein:
   - **Variablenname:** `ADMIN_URL`
   - **Wert:** Ihr neuer Administratorpfad (z. B. `magento_A8v10`).

   Standardmäßig sind **[!UICONTROL Available during runtime]** und **[!UICONTROL Make inheritable]** ausgewählt. Um zu verhindern, dass untergeordnete Umgebungen diesen Wert erben, löschen Sie die **[!UICONTROL Make inheritable]** für diese Variable.
7. Klicken Sie auf **[!UICONTROL Create variable]** (oder **[!UICONTROL Save]**) und warten Sie, bis die Bereitstellung abgeschlossen ist. Die Schaltfläche ist nur sichtbar, wenn die erforderlichen Felder Werte enthalten.

##### Wenn Staging und Produktion in der [!DNL Cloud Console] nicht verfügbar sind

[Senden eines Support-Tickets](https://experienceleague.adobe.com/de/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) mit der Aufforderung, die `ADMIN_URL` Variable für Ihre Staging- oder Produktionsumgebung hinzuzufügen. Wenn über die [!DNL Cloud Console] auf Staging- und Produktionsumgebung zugegriffen werden kann, fügen Sie die Variable hinzu, wie in [Integrationsumgebung](#integration-environment) beschrieben.

#### Option B: Ändern der Admin-URL mithilfe der [!DNL Cloud CLI]

Verwenden Sie den Befehl `magento-cloud variable:update` , um die Variable zu aktualisieren. (Der `variable:set`-Befehl wird nicht mehr unterstützt und ist nicht verfügbar.)

Im folgenden Beispiel wird der `ADMIN_URL` der `master` Umgebung auf `newAdmin_A8v10` aktualisiert und untergeordnete Umgebungen werden daran gehindert, den Wert zu erben:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **Neu-Bereitstellung** Das Ändern der `ADMIN_URL` in den [!DNL Cloud CLI] Triggern führt zu einer erneuten Bereitstellung der Umgebung.
- **Vererbung** Variablen werden standardmäßig vererbt. Um zu verhindern, dass der Wert an untergeordnete Umgebungen vererbt wird, verwenden Sie die `--inheritable false` Option wie abgebildet. Weitere Informationen finden Sie unter [Sichtbarkeit auf Variablenebene](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html?lang=de#visibility).

>[!NOTE]
>
>Der `ADMIN_URL` akzeptiert Buchstaben (a-z, A-Z), Zahlen (0-9) und den Unterstrich (_). Leerzeichen oder andere Zeichen werden nicht akzeptiert.
