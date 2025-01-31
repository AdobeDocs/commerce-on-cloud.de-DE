---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Include file for modify custom VCL for Fastly

## Ändern des benutzerdefinierten VCL-Snippets

1. [Anmelden](/help/get-started/onboarding.md#access-your-admin-panel) beim Administrator.

1. Klicken Sie auf **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

   ![Verwalten benutzerdefinierter VCL-Snippets](/help/assets/cdn/fastly-manage-snippets.png)

1. Klicken Sie in _Spalte_ Aktion“ auf das Einstellungssymbol neben dem Snippet, das bearbeitet werden soll.

1. Nachdem die Seite neu geladen wurde, klicken Sie im Abschnitt **Fastly-Konfiguration** auf _VCL zu Fastly_.

1. Aktualisieren Sie nach Abschluss des Uploads den Cache entsprechend der Benachrichtigung oben auf der Seite.

>[!WARNING]
>
>Die _Benutzeroberflächenoption „Benutzerdefinierte VCL_ Ausschnitte“ zeigt nur die Ausschnitte an, die über die Adobe Commerce-Admin hinzugefügt wurden. Wenn Sie Snippets mit der Fastly-API hinzufügen, verwenden Sie die API, um [sie zu verwalten](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
