---
title: New Relic-Überwachung
description: Erfahren Sie, wie Sie auf Ihr New Relic-Dashboard zugreifen und Daten aus Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt analysieren können.
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# New Relic-Überwachung

New Relic verbindet und überwacht Ihre Infrastruktur und [!DNL Commerce] mit PHP-Agenten. Nachdem eine Cloud-Umgebung eine Verbindung zu New Relic hergestellt hat, können Sie sich bei Ihrem New Relic-Konto anmelden, um die vom Agenten erfassten Daten zu überprüfen.

Wählen Sie auf der _APM und Services_-Seite die Option **Zusammenfassung** aus, um Transaktionsdaten zu Ihrer Anwendung anzuzeigen. In dieser Ansicht können Sie potenzielle Fehler identifizieren und den Gesamtzustand Ihrer Anwendung und Services überprüfen.

![Übersichtsseite zum Cloud-Projekt New Relic](../../assets/new-relic/dashboard.png)

In dieser Ansicht können Sie Transaktionen verfolgen, die auf langsame Antworten oder Engpässe, Anwendungsdurchsatz, Web-Fehler und mehr stoßen.

Überprüfen der getrackten Daten:

- **Am zeitaufwendigsten** - Bestimmen Sie den Zeitaufwand, indem Sie Anforderungen parallel verfolgen. Beispielsweise könnten Sie die höchste Transaktionszeit in Produkt- und Kategorieansichten haben. Wenn eine Kundenkontenseite plötzlich einen hohen Zeitaufwand verzeichnet, kann Ihre Anwendung durch einen Aufruf oder eine Leistungsverzögerung bei Abfragen beeinträchtigt werden.

- **Höchster Durchsatz** - Identifizieren Sie Seiten, die am meisten Treffer erzielen, basierend auf der Größe und Häufigkeit der übertragenen Bytes.

Alle erfassten Daten geben Aufschluss über die Zeit, die mit Aktionen verbracht wurde, die Daten, Abfragen oder _Redis_-Daten übertragen. Wenn Abfragen Probleme verursachen, stellt New Relic Informationen zum Nachverfolgen und Reagieren auf diese Probleme bereit.

>[!TIP]
>
>Weitere Informationen zur Verwendung dieser Daten zur Fehlerbehebung bei Leistungsproblemen von Anwendungen finden Sie unter [Fehlerbehebung bei der Leistung mit New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html) im _Adobe Commerce Help Center_.

## Überwachen der Leistung mit verwalteten Warnhinweisen

Adobe stellt die Warnhinweisrichtlinie _Managed Alerts for Adobe Commerce_ zur Verfolgung von Leistungsmetriken bereit. Die Richtlinie umfasst eine Sammlung von Warnhinweisen, die Schwellenwerte und Trigger-Warnungen festlegen, sowie kritische Benachrichtigungen, wenn Infrastruktur- oder Anwendungsprobleme die Site-Performance beeinträchtigen. Die Richtlinie verfolgt die folgenden Metriken in Produktionsumgebungen:

| Metrik | Datenerfassung | Verfügbarkeit |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] | APM | Pro und Starter |
| Verwendung von CPU | NRI | Profi |
| Speicherplatz | NRI | Profi |
| Fehlerrate | APM | Pro und Starter |
| Speichernutzung | NRI | Profi |
| MariaDB-Abfragelast | NRI | Profi |
| Redis-Speicher | NRI | Profi |

Wenn ein Trigger in der Website-Infrastruktur oder in den Anwendungsbedingungen einen Warnschwellenwert erzeugt, sendet New Relic Warnbenachrichtigungen, damit Sie das Problem proaktiv beheben können. Unter [Verwaltete Warnhinweise für Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) im _Adobe Commerce-_ finden Sie Details zu Warnschwellen und Schritten zur Fehlerbehebung, um die Probleme zu beheben, die den Warnhinweis ausgelöst haben.

>[!TIP]
>
>Verwenden Sie für Staging- und Integrationsumgebungen von Pro und Starter-Umgebungen [Statusbenachrichtigungen](../integrations/health-notifications.md), um den Festplattenspeicher zu überwachen.

>[!PREREQUISITES]
>
>- **New Relic-Anmeldeinformationen** - Anmeldeinformationen zum Anmelden beim New Relic-Konto für Ihr Cloud-Projekt
>- **Aktive New Relic-Integration**: Stellen Sie sicher, dass Ihre Cloud-Umgebung mit New Relic verbunden ist
>- **Workflow-Benachrichtigung** - Konfigurieren Sie mindestens einen [Workflow](#set-up-a-workflow-for-notifications), um die Warnhinweise zu erhalten

**So überprüfen Sie die Richtlinie Verwaltete Warnhinweise für Adobe Commerce**:

1. Melden Sie sich bei Ihrem [New Relic-Konto an](https://login.newrelic.com/login).

1. Suchen Sie die Richtlinie _Managed Alerts for Adobe Commerce_:

   - Klicken Sie im Navigationsmenü des Explorers auf **[!UICONTROL Alerts & AI]**.

   - Klicken _unter &quot;_&quot; auf **[!UICONTROL Alert Conditions & Policies]**.

   - Vergewissern Sie sich, dass Ihr Konto oben in der Ansicht _Warnhinweisbedingungen und -richtlinien_ ausgewählt ist.

   - Wählen Sie in _Liste_ die Richtlinie **Verwaltete Warnhinweise für Adobe Commerce** aus.

     ![Generierte Warnhinweisrichtlinien](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Wenn die Richtlinie _Verwaltete Warnhinweise für Adobe Commerce_ nicht verfügbar ist, finden Sie weitere Informationen unter [Verwaltete Warnhinweise für Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) im _Adobe Commerce-Hilfe-Center_.

1. Klicken Sie auf die Registerkarte **[!UICONTROL Alert conditions]** , um die in der Richtlinie definierten Warnhinweisbedingungen zu überprüfen.

## Erstellen von Warnmeldungsrichtlinien

Ändern Sie keine Warnhinweise, die in der Richtlinie Verwaltete Warnhinweise für Adobe Commerce enthalten sind. Adobe aktualisiert und verbessert die Warnhinweisbedingungen in dieser Richtlinie im Laufe der Zeit, wodurch alle Anpassungen, die Sie zur Richtlinie hinzufügen, überschrieben werden.

Anstatt einen vorhandenen Warnhinweis zu ändern, können Sie eine Warnhinweisrichtlinie erstellen. Kopieren Sie dann die Warnhinweisbedingungen in die neue Richtlinie.

>[!TIP]
>
>Weitere Informationen [ Warnhinweisen, Warnhinweisrichtlinien und Workflows finden ](https://docs.newrelic.com/docs/alerts/overview/) in der Dokumentation _2}New Relicunter „Einführung in Warnhinweise“._

## Einrichten eines Workflows für Benachrichtigungen

Sie können jetzt einen _Workflow_ einrichten, der früher als Benachrichtigungskanal bezeichnet wurde, um Benachrichtigungen über die Leistung Ihrer Site basierend auf gefilterten Daten zu erhalten, z. B. eine Warnmeldungsrichtlinie. Benachrichtigungen über Leistungsprobleme werden an alle Workflows gesendet, die mit einer Warnmeldungsrichtlinie verbunden sind, wenn Bedingungen auf dem Anwendungs- oder Infrastruktur-Trigger einen Warnhinweis erzeugen. Sie erhalten auch Benachrichtigungen, wenn ein Problem quittiert und geschlossen wird.

New Relic bietet Vorlagen zum Konfigurieren verschiedener Arten von Workflow-Benachrichtigungen, einschließlich E-Mail, Slack, PagerDuty, Webhooks und mehr.

**Workflow konfigurieren**:

1. Melden Sie sich bei Ihrem [New Relic-Konto an](https://login.newrelic.com/login).

1. Erstellen Sie einen Workflow.

   - Klicken Sie im Navigationsmenü des Explorers auf **[!UICONTROL Alerts & AI]**.

   - Klicken Sie im linken Navigationsbereich unter _Anreichern und_ Benachrichtigen“ auf **[!UICONTROL Workflows]**.

   - Klicken Sie auf der rechten Seite auf **[!UICONTROL Add a workflow]**.

     ![New Relic - Workflow hinzufügen](../../assets/new-relic/add-a-workflow.png)

   - Geben _auf der Seite „Workflow konfigurieren_ einen Namen für den Workflow ein.

   - Wählen _im Abschnitt_ Daten filtern“ **[!UICONTROL Managed Alerts for Adobe Commerce]** aus der Dropdown-Liste **[!UICONTROL Policy]** aus.

   - Wählen _im Abschnitt „Benachrichtigen_ einen Kanal aus und befolgen Sie die Anweisungen.

   - Klicken Sie auf **[!UICONTROL Test workflow]** , um Ihre Konfiguration zu überprüfen.

1. Klicken Sie auf **[!UICONTROL Activate workflow]**.

Siehe die New Relic-Dokumentation zu [Workflows](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Die Warnhinweise in der Richtlinie Verwaltete Warnhinweise für Adobe Commerce verfügen über standardmäßige Workflows, die Adobe-Teams benachrichtigen, die Kunden von Adobe Commerce in Cloud-Infrastrukturen unterstützen. Ändern Sie nicht die Konfiguration für diese Standardkanäle und entfernen Sie keine ihnen zugewiesenen Warnmeldungsrichtlinien.
