---
title: Anmelden bei [!DNL Cloud Console]
description: Erfahren Sie mehr über  [!DNL Cloud Console]  für Adobe Commerce in der Cloud-Infrastruktur.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Anmelden bei [!DNL Cloud Console]

Die [!DNL Cloud Console] bietet interaktive Methoden zum Erstellen, Verwalten und Bereitstellen von Commerce-Code. Das [!DNL Cloud Console] ist ein moderneres, benutzerfreundlicheres Erlebnis und bildet die Grundlage für zukünftige Verbesserungen der Benutzeroberfläche.

[Melden Sie sich bei an [!DNL Cloud Console]](https://console.adobecommerce.com) um Ihre Projektliste anzuzeigen.

![Projektliste](../assets/ui-allprojects-list.png)

## Funktionen

Zu den neuen oder verbesserten Funktionen gehören:

- Klare Übersicht über Projekt- und Umgebungsmerkmale
- Aktivitäts-Stream mit sortierbarem Verlauf
- Manuelles Backup-Management und Verlauf für Starter-Projekte
- Erweiterte Protokollansichten
- Sortierbare Listen
- Einfache Formulare und Anleitungen zum Hinzufügen von Integrationen
- Konformität mit den Web Content Accessibility Guidelines (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

Die neuen oder verbesserten Funktionen lauten wie folgt:

| Funktion | Verbesserungen |
| -------------- | ----------------------------------- |
| [Aktivitäts-Stream](../cloud-guide/project/activity-stream.md) | Interagieren mit einer sortierbaren Liste von laufenden, ausstehenden oder historischen Aktionen. Wählen Sie eine Aktivität aus und zeigen Sie Protokolle an oder brechen Sie einen laufenden Build ab. |
| [Projekt- und Umgebungsübersichten](../cloud-guide/project/overview.md#project-overview) | Öffnen Sie das Projekt und sehen Sie sich die Übersicht der Projektdetails und die Umgebungsliste an. Die Umgebungsübersicht enthält weitere Details zum Umgebungsstatus, zum Programmzugriff und zu den letzten Aktivitäten. |
| [Integrationsformulare](../cloud-guide/integrations/overview.md) | Verwenden Sie einfache Formulare und Anleitungen, um Integrationen hinzuzufügen, z. B. Bitbucket- oder Slack-Benachrichtigungen. |
| [Projektliste](../cloud-guide/project/overview.md#cloud-console) | Die Ansicht _Alle Projekte_ listet alle Projekte auf, für die Sie über Zugriffsberechtigungen verfügen. Sie können auf **[!UICONTROL Show filters]** klicken und Ihre Projektliste nach Typ, Region oder Plan filtern. |
| [Optionen für die variable Sichtbarkeit](../cloud-guide/environment/variable-levels.md) | Begrenzen der Sichtbarkeit einer Variablen auf Projektebene oder Umgebungsebene während des Builds oder der Laufzeit. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Konsolenfragen

**_Wo finde ich die Snapshots-Funktion_**?

Bei [!DNL Starter] Projekten heißt die Funktion „Momentaufnahmen“ jetzt _Sicherungen_. Sie können ein manuelles Backup Ihrer [!DNL Starter] Umgebung über die [!DNL Cloud Console] erstellen oder einen Snapshot über die Cloud-CLI erstellen. Sie müssen über eine Administratorrolle für die Umgebung verfügen.

Wählen Sie in der Projekt-Navigationsleiste eine Umgebung aus. Die Umgebung muss aktiv sein. Wählen Sie die Registerkarte **[!UICONTROL Backups]** aus. Derzeit ist diese Option für Pro-Umgebungen nicht verfügbar.

**_Wo ist die Liste der konfigurierten Routen für die Umgebung_**?

Die Liste der konfigurierten Routen finden Sie auf der Registerkarte _Services_ für eine Umgebung.

Wählen Sie in der Projekt-Navigationsleiste eine Umgebung aus. Wählen Sie die Registerkarte **[!UICONTROL Services]** aus. Die **Router**-Übersicht zeigt die konfigurierten Routen an. Derzeit ist es nicht möglich, eine Route aus dem neuen [!DNL Cloud Console] hinzuzufügen.

## Kontomenü

In der oberen rechten Ecke befindet sich das Kontomenü. Klicken Sie auf den Abwärtspfeil für das Menü und wählen Sie **[!UICONTROL My Profile]** aus. In der Ansicht _Mein Profil_ können Sie Benutzerdetails und Anzeigeeinstellungen steuern, [Sicherheitsauthentifizierung](../cloud-guide/project/user-access.md#user-authentication-requirements), [API-Token](../cloud-guide/project/user-access.md#create-an-api-token) und [SSH-Schlüssel verwalten](../cloud-guide/development/secure-connections.md).
