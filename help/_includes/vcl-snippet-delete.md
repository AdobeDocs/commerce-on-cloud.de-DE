---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Include file for deleting custom VCL for Fastly

## Löschen des benutzerdefinierten VCL-Snippets

1. [Anmelden](/help/get-started/onboarding.md#access-your-admin-panel) beim Administrator.

1. Klicken Sie auf **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

   ![Verwalten benutzerdefinierter VCL-Snippets](/help/assets/cdn/fastly-manage-snippets.png)

1. Klicken Sie in _Spalte_ Aktion“ auf das Papierkorbsymbol neben dem zu löschenden Snippet.

1. Klicken Sie im nächsten modalen Fenster auf **DELETE** aktivieren Sie eine neue Version.

>[!WARNING]
>
>Die _Benutzeroberflächenoption „Benutzerdefinierte VCL_ Ausschnitte“ zeigt nur die Ausschnitte an, die über die Adobe Commerce-Admin hinzugefügt wurden. Wenn Sie Snippets mit der Fastly-API hinzufügen, verwenden Sie die API, um [sie zu verwalten](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
