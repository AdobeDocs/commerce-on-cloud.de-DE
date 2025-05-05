---
title: Zugriff auf das Commerce Admin Panel
description: Erfahren Sie, wie Sie auf Ihr Commerce Admin-Bedienfeld zugreifen können.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Zugriff auf das Commerce Admin Panel

Benutzende mit administrativem Zugriff auf das Commerce-Admin-Bedienfeld können Benutzende hinzufügen, Store-Services konfigurieren, die Einrichtung und Anpassung von Stores abschließen und vieles mehr.

Bei einem neuen Projekt besteht der erste Schritt nach Erhalt der Begrüßungs-E-Mail darin, den Admin-Zugriff auf das Projekt zu sichern, indem das Passwort im Konto Lizenzinhaber geändert wird. Der Standardbenutzername für dieses Konto ist die E-Mail-Adresse des Lizenzinhabers.

Sie können eine Kennwortänderungsanfrage mit einer der folgenden Methoden senden:

- Suchen Sie die Begrüßungs-E-Mail, die an die E-Mail-Adresse des Lizenzinhabers gesendet wurde, und folgen Sie dem Link, um Ihr Kennwort zu ändern.

- Kopieren Sie die Store-URL aus dem [[!DNL Cloud Console]](../cloud-guide/project/overview.md) in einen Browser. Hängen Sie dann `/admin` an das Ende der URL an, um die Anmeldeseite zu öffnen. Klicken Sie auf **Kennwort vergessen?** Link zum Senden einer Anforderung zur Passwortänderung an die E-Mail-Adresse des Lizenzinhabers.

Nachdem Sie die Kennwortänderungsanfrage gesendet haben, überprüfen Sie Ihre E-Mail auf die Benachrichtigung zum Zurücksetzen des Kennworts. Wenn Sie die E-Mail nicht erhalten, überprüfen Sie Ihren Spam-Ordner.

>[!TIP]
>
>Wenn das Zurücksetzen des Kennworts fehlschlägt oder Sie sich nicht beim Admin-Bedienfeld anmelden können, kann ein Benutzer mit Administratorzugriff mithilfe von SSH eine Verbindung zum Projekt herstellen und mithilfe des `admin:user:create` CLI-Befehls einen Admin-Benutzer hinzufügen. Siehe [Erstellen, Bearbeiten oder Entsperren eines Administratorkontos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=de) im _Installationshandbuch_.

## Überwachen des Site-Status

Das [Site-Wide Analysis Tool](https://experienceleague.adobe.com/de/docs/commerce-operations/tools/site-wide-analysis-tool/intro) ist ein proaktives Self-Service-Tool und ein zentrales Repository, das detaillierte Systemeinblicke und Empfehlungen bietet, um die Sicherheit und Bedienbarkeit Ihrer Adobe Commerce-Installation sicherzustellen. Sie bietet rund um die Uhr eine Echtzeit-Leistungsüberwachung, Berichte und Beratung, um potenzielle Probleme zu identifizieren und den Site-Status, die Sicherheit und Anwendungskonfigurationen besser einsehen zu können. Dies trägt dazu bei, die Auflösungszeit zu reduzieren und die Stabilität und Leistung der Site zu verbessern. Sie können auf das Site-Wide Analysis Tool direkt über das [Admin-Bedienfeld“ zugreifen](https://experienceleague.adobe.com/de/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
