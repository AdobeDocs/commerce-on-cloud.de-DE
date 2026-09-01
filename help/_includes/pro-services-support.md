---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# Pro Services Support und Kundenverfügbarkeit

## Pro Services-Support

Gehen Sie wie folgt vor, um ein Pro-Service-Upgrade in der Staging- oder Produktionsumgebung anzufordern und abzuschließen:

1. **Um (Services[&#x200B; nur in `Staging` und `Production` Umgebungen zu installieren &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml) zu aktualisieren** senden Sie ein [Adobe Commerce-Support-Ticket](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket).

   Geben Sie im Ticket die erforderlichen Service-Änderungen an, schließen Sie die aktualisierten `.magento.app.yaml`- und `.magento/services.yaml`-Dateien ein und notieren Sie sich die PHP-Zielversion.

   PHP-Version, Composer-Updates, Erweiterungen und Umgebungseinstellungen sind Self-Service-Änderungen. Adobe muss möglicherweise den New Relic-Agenten aktualisieren, um die PHP-Versionskompatibilität zu gewährleisten. Siehe [PHP-Einstellungen](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings) in _Anwendungskonfiguration_.

   >[!IMPORTANT]
   >
   >Verwenden Sie bei der Auswahl des Felds **[!UICONTROL Environment]** im Ticketformular die Umgebungsbenennung von Adobe. Wählen Sie beispielsweise Staging aus, auch wenn Sie diese Umgebung intern **dev** aufrufen. Sie können Ihren internen Namen in der Beschreibung angeben, aber das Feld [!UICONTROL Environment] muss die Nomenklatur von Adobe verwenden.

1. **Bestätigen Sie den Upgrade** Zeitplan durch den zweiteiligen Prozess von Adobe: Sie bestätigen zuerst das angeforderte Datum und die angeforderte Uhrzeit und senden es dann an den Support zur endgültigen Bestätigung an das Infrastruktur-Team.

   Produktionsänderungen (nur Pro) erfordern mindestens zwei Werktage Vorankündigung, Wochenenden ausgeschlossen. Beispielsweise muss das Cloud-Infrastruktur-Team ein Upgrade vom Montag bis zum vorhergehenden Mittwoch bestätigen. Bei Spitzennachfrage ist mit zusätzlicher Vorlaufzeit zu rechnen. Um Verzögerungen zu vermeiden, beantworten Sie die ursprüngliche Anfrage mindestens 48 Stunden vor dem Fenster. Das Upgrade gilt erst als geplant, wenn Sie eine endgültige Bestätigung erhalten.

   >[!NOTE]
   >
   >Bereitstellen von Wartungsfenstern in UTC. Staging-Upgrades sind nicht im Voraus geplant und werden in der Regel am selben Tag wie die Anfrage abgeschlossen.
   >
   >Stellen Sie nach einem RabbitMQ-Upgrade die Umgebung erneut bereit, um die Nachrichtenwarteschlangen neu zu initialisieren.

1. **Validieren Sie das Upgrade** in einer Staging- oder Integrationsumgebung, bevor Sie es in der Produktionsumgebung planen.

   Probleme, die durch Drittanbietermodule, benutzerdefinierten Code oder die Kompatibilität mit Abhängigkeiten verursacht werden, treten häufig während der erneuten Bereitstellung auf, die auf ein Service-Upgrade folgt. Zur Validierung mehrerer Service-Upgrades auf einmal ist eine angemessene Reihenfolge Valley oder Redis, dann RabbitMQ, dann OpenSearch, dann MariaDB. Dies ist keine erforderliche Sequenz. Datenbank-Upgrades haben die größte Auswirkung auf den Betrieb und verdienen höchste Vorsicht.

   Adobe übernimmt keine Garantie für die genaue Dauer eines Produktionswartungsfensters im Voraus, da der Zeitpunkt von der Umgebung und den beteiligten Services abhängt. Nutzen Sie bei der Planung des Produktionsfensters die Zeit, die für das Staging-Upgrade benötigt wird.

1. **Stellen Sie die Umgebung erneut**, nachdem Adobe das Service-Upgrade abgeschlossen hat, damit die Änderung wirksam wird, auch wenn sich die Version der Adobe Commerce-Anwendung nicht ändert.

   Wenn das Upgrade OpenSearch umfasst, planen Sie auch eine vollständige Neuindizierung. Adobe kann keine Ausfallzeiten für ein Service-Upgrade garantieren. Planen Sie daher ein Wartungsfenster, in dem Zeit für die Neubereitstellung, Neuindizierung bei Bedarf und die Validierung der Storefront und des Administrators bleibt, bevor Sie die Site erneut öffnen.

## Kundenverfügbarkeit bei Upgrades

**Ein Mitarbeiter Ihres Teams oder Implementierungspartners muss für die Dauer des geplanten Produktions-Upgrades online verfügbar sein.** Durch die Planung während einer Zeit mit geringem Traffic ist das Upgrade nicht praktisch erledigt. Adobe verwaltet die Aktualisierung der Cloud-Infrastruktur, kann jedoch das Anwendungsverhalten, Integrationen, benutzerdefinierten Code oder Unternehmens-Workflows nicht validieren.

Der verfügbare Vertreter muss in der Lage sein,

- **Überwachen** der Storefront und kritischer Geschäftstransaktionen während und nach dem Upgrade.
- **Beantworten Sie** Fragen des Adobe-Supports oder des Cloud-Infrastruktur-Teams.
- **Bestätigen** dass Integrationen, Erweiterungen, Anpassungen, Cron-Aufträge, Warteschlangen und andere kundenspezifische Funktionen erwartungsgemäß funktionieren.
- **Validieren** geschäftskritische Workflows, wie z. B. Checkout, Katalogansichten, Suche, Anmeldung und Bestellabwicklung.
- **Bericht** unerwartetes Verhalten sofort melden, während der Upgrade-Kontext und die Protokolle weiterhin verfügbar sind.

>[!TIP]
>
>Für Pro-Projekte erfordern Service-Upgrades in der Produktion auch eine vorzeitige Planung und einen zweiteiligen Bestätigungsprozess mit Adobe-Support. Siehe [Pro Services-Support](#pro-services-support).

### Wartungsmodus

**Der Wartungsmodus ist kein Ersatz für die Kundenverfügbarkeit.** Der Wartungsmodus blockiert den Zugriff auf die Storefront, validiert jedoch nicht Anwendungsdienste, Integrationen, Warteschlangen, Cron-Aufträge, Checkout oder andere kundenspezifische Funktionen.

Wenn für die geplanten Arbeiten der Wartungsmodus erforderlich ist, koordinieren Sie die Verwendung mit dem Adobe-Support und befolgen Sie die Anweisungen für dieses Upgrade. Vergewissern Sie sich anschließend, dass die Storefront und die kritischen Workflows normal funktionieren, bevor Sie den Abschluss der Arbeit in Betracht ziehen.
