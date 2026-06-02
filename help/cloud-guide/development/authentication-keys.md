---
title: Authentifizierungsschlüssel
description: Erfahren Sie, wie Sie in Adobe Commerce in der Cloud-Infrastruktur Authentifizierungsschlüssel auf ein Entwicklungsprojekt anwenden.
feature: Cloud, Security
topic: Security
exl-id: b5a24fcd-9b43-4ec9-8a0c-52956a74e45e
TQID: https://experienceleague.adobe.com/nYBr0uvw1SZPSQqAU6uHTiitjZ0kcudsLdWagiWRLP8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 318
ht-degree: 0%

---

# Authentifizierungsschlüssel

Sie müssen über einen Authentifizierungsschlüssel verfügen, um auf das Adobe Commerce-Repository zuzugreifen und um Installations- und Aktualisierungsbefehle für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt zu aktivieren. Es gibt zwei Methoden zum Angeben von Composer-Autorisierungsberechtigungen.

- **Authentifizierungsdatei** - Eine Datei, die Ihre Adobe Commerce-[Autorisierungsberechtigungen](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html?lang=de) in Ihrem Stammverzeichnis der Adobe Commerce in der Cloud-Infrastruktur enthält.
- **Umgebungsvariable** - Eine Umgebungsvariable, um Authentifizierungsschlüssel in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt einzurichten, um versehentliches Offenlegen zu verhindern.

>[!BEGINSHADEBOX]

**Sicherheitshinweis**

Adobe empfiehlt die Verwendung der [Umgebungsvariable](#composer-auth-environment-variable)-Methode mit Ihrem Cloud-Projekt, um zu verhindern, dass Ihre Autorisierungsdaten versehentlich offen gelegt werden.

Die Authentifizierungsdateimethode ist ideal für die Verwendung von Cloud Docker für Commerce als lokales Entwicklungs-Tool, achten Sie jedoch darauf, die `auth.json` nicht in ein öffentliches Git-basiertes Repository hochzuladen. Sie können die `auth.json` Datei zur [`.gitignore` Datei hinzufügen](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Authentifizierungsdatei

**So erstellen Sie eine `auth.json` Datei**:

1. Wenn sich keine `auth.json`-Datei im Stammverzeichnis des Projekts befindet, erstellen Sie eine.

   - Erstellen Sie mit einem Texteditor eine `auth.json`-Datei in Ihrem Projektstammverzeichnis.
   - Kopieren Sie den Inhalt der [`auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) in die neue `auth.json`.

1. Ersetzen Sie `<public-key>` und `<private-key>` durch Ihre Adobe Commerce-Authentifizierungsdaten.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Speichern Sie Ihre Änderungen und beenden Sie den Texteditor.

## Composer-Auth-Umgebungsvariable

Die folgende Methode ist die beste Methode, um zu verhindern, dass vertrauliche Anmeldeinformationen in einem öffentlichen Git-basierten Repository versehentlich offen gelegt werden.

**So fügen Sie Authentifizierungsschlüssel mithilfe einer Umgebungsvariablen hinzu**:

1. Klicken Sie in der _[!DNL Cloud Console]_&#x200B;auf das Konfigurationssymbol auf der rechten Seite der Projektnavigation.

   ![Projekt konfigurieren](../../assets/icon-configure.png){width="36"}

1. Klicken Sie in _Liste_ Projekteinstellungen“ auf **[!UICONTROL Variables]**.

1. Klicken Sie auf **[!UICONTROL Create variable]**.

1. Geben Sie im Feld **[!UICONTROL Variable name]** `env:COMPOSER_AUTH` ein.

1. Fügen Sie im Feld _Wert_ Folgendes hinzu und ersetzen Sie `<public-key>` und `<private-key>` durch Ihre Adobe Commerce-Authentifizierungsdaten:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Wählen Sie **[!UICONTROL Available during buildtime]** aus und heben Sie die Auswahl für **[!UICONTROL Available during runtime]** auf.

1. Klicken Sie auf **[!UICONTROL Create variable]**.

1. Entfernen Sie die `auth.json` aus jeder Umgebung.
