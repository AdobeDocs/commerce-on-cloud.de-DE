---
title: Staging- und Produktionstests
description: Erfahren Sie, wie Sie in Staging- und Produktionsumgebungen testen können.
exl-id: 39625c97-5eb0-4039-ac5f-ddaeb43156de
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Staging- und Produktionstests

Verwenden Sie nach einer erfolgreichen Migration Ihres Codes, Ihrer Dateien und Ihrer Daten zu Staging oder Produktion die Umgebungs-URLs, um Ihre Sites und Stores zu testen. Im Folgenden finden Sie Informationen zum Überprüfen von Protokollen, Testen von Fastly-Konfigurationen, Benutzerakzeptanztests (UAT) und mehr.

{{second-staging}}

## Protokolldateien

Wenn beim Testen Fehler bei der Bereitstellung oder andere Probleme auftreten, überprüfen Sie die Protokolldateien. Protokolldateien befinden sich im `var/log`.

Das Bereitstellungsprotokoll befindet sich in `/var/log/platform/<prodject-ID>/deploy.log`. Der Wert von `<project-ID>` hängt von der Projekt-ID ab und davon, ob es sich um eine Staging- oder eine Produktionsumgebung handelt. Bei einer Projekt-ID von `yw1unoukjcawe` wird beispielsweise der Staging-Benutzer `yw1unoukjcawe_stg` und der Produktions-Benutzer `yw1unoukjcawe`.

Verwenden Sie beim Zugriff auf Protokolle in Produktions- oder Staging-Umgebungen SSH, um sich bei jedem der drei Knoten anzumelden und die Protokolle zu finden. Sie können auch die [New Relic-Protokollverwaltung](../monitor/log-management.md) verwenden, um aggregierte Protokolldaten von allen Knoten anzuzeigen und abzufragen. Siehe [Protokolle anzeigen](log-locations.md#application-logs).

## Code-Basis überprüfen

Überprüfen Sie, ob Ihre Code-Basis in Staging- und Produktionsumgebungen ordnungsgemäß bereitgestellt wurde. Die Umgebungen sollten über identische Code-Basen verfügen.

## Konfigurationseinstellungen überprüfen

Überprüfen Sie die Konfigurationseinstellungen über das Admin-Bedienfeld, einschließlich Basis-URL, Basis-Admin-URL, Einstellungen für mehrere Sites und mehr. Wenn Sie zusätzliche Änderungen vornehmen müssen, schließen Sie die Änderungen in Ihrer lokalen Git-Verzweigung ab und übertragen Sie sie in die `master` Verzweigung in Integration, Staging und Produktion.

## Überprüfen der Fastly-Zwischenspeicherung

[Fastly konfigurieren](../cdn/fastly-configuration.md) erfordert sorgfältige Aufmerksamkeit zum Detail: Verwenden der richtigen Fastly Service-ID- und Fastly API-Token-Anmeldeinformationen, Hochladen des Fastly VCL-Codes, Aktualisieren der DNS-Konfiguration und Anwenden der SSL-/TLS-Zertifikate auf Ihre Umgebungen. Nachdem Sie diese Einrichtungsaufgaben abgeschlossen haben, können Sie das schnelle Caching in Staging- und Produktionsumgebungen überprüfen.

**So überprüfen Sie die Fastly-Service-Konfiguration**:

1. Melden Sie sich bei Admin für Staging und Produktion an, indem Sie die URL mit `/admin` oder die [aktualisierte Admin-URL](../environment/variables-admin.md#admin-url) verwenden.

1. Navigieren Sie zu **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System**. Scrollen Sie und klicken Sie auf **Vollständiger Seitencache**.

1. Stellen Sie sicher **dass der Wert für** Caching-Anwendung“ auf _Fastly CDN)_.

1. Testen Sie die Fastly-Anmeldedaten.

   - Klicken Sie **Fastly Configuration**.

   - Überprüfen Sie, ob die Werte für die Fastly Service ID- und Fastly API Token-Anmeldeinformationen vorliegen. Siehe [Abrufen von Fastly-Anmeldeinformationen](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - Klicken Sie **Testanmeldeinformationen**.

   >[!WARNING]
   >
   >Stellen Sie sicher, dass Sie die richtige Fastly Service-ID und das richtige API-Token in Ihren Staging- und Produktionsumgebungen eingegeben haben. Fastly-Anmeldeinformationen werden pro Service-Umgebung erstellt und zugeordnet. Wenn Sie die Staging-Anmeldeinformationen in Ihre Produktionsumgebung eingeben, können Sie Ihre VCL-Snippets nicht hochladen, das Caching funktioniert nicht ordnungsgemäß und Ihre Caching-Konfiguration verweist auf den falschen Server und speichert.

**So überprüfen Sie das Fastly-Caching**:

1. Suchen Sie mithilfe des `dig`-Befehlszeilendienstprogramms nach Kopfzeilen, um Informationen über die Site-Konfiguration zu erhalten.

   Sie können eine beliebige URL mit dem Befehl `dig` verwenden. In den folgenden Beispielen werden Pro-URLs verwendet:

   - Staging: `dig https://mcstaging.<your-domain>.com`
   - Produktion: `dig https://mcprod.<your-domain>.com`

   Weitere `dig` finden Sie unter Fastly&#39;s [Testing before changing DNS](https://docs.fastly.com/en/guides/working-with-domains).

1. Verwenden Sie `cURL`, um die Antwort-Header-Informationen zu überprüfen.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   Siehe [Überprüfen von Antwort-](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers)) für Details zur Überprüfung der Kopfzeilen.

1. Nachdem Sie live sind, verwenden Sie `cURL` , um Ihre Live-Site zu überprüfen.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## Vollständige UAT-Tests

Abschließen der Benutzerakzeptanztests (UAT) für Staging und Produktion. Die folgenden Tests sind eine schnelle Liste möglicher Aufgaben und Bereiche, die als Händler und Kunde getestet werden sollten. Ihre Liste kann länger sein und zusätzliche Tests für benutzerdefinierte Module, Erweiterungen und Integrationen von Drittanbietern enthalten. Verwenden Sie beim Testen Desktops, Laptops und Mobilgeräte.

Wenn Probleme auftreten, speichern Sie Ihre Reproduktionsschritte, Fehlermeldungen, seltsamen Bildschirmaufnahmen und Links. Verwenden Sie diese Informationen, um Probleme im Code der Integrationsumgebung und in Konfigurationen oder Umgebungseinstellungen zu untersuchen und zu beheben.

<table>
<tr>
<td style="width:150px">Benutzerverwaltung</td>
<td>
<ul>
<li>Erstellen und Bearbeiten von Kundenkonten, Überprüfen von E-Mails</li>
<li>Erstellen von Administratorrollen für Händler</li>
<li>Erstellen von Händlerkonten mit bestimmten Rollen</li>
<li>Zugriff auf Händlerkonto pro Rolle testen</li>
</ul>
</td>
</tr>
<tr>
<td>Kataloge und Produkte</td>
<td>
<ul>
<li>Erstellen eines Katalogs mit zugehörigen Produkten</li>
<li>Erstellen Sie Produkte für Ihre Storefront, einschließlich aller Produktarten: einfach, konfigurierbar, gebündelt</li>
<li>Hinzufügen von Produktbildern, Farbfeldern, Videos und anderen Medienoptionen</li>
<li>Konfigurieren von Preisen, Rabatten und Preisregeln </li>
<li>Erweiterte Funktionen konfigurieren, einschließlich Preisspannen, vorgestellter Produkte, Verfügbarkeitsdaten</li>
<li>Ändern des Bestands und Überprüfen der korrekten Werte Anzeige und Änderung pro Erhöhung und abgeschlossenem Kauf</li>
</ul>
</td>
</tr>
<tr>
<td>Warenkörbe und Checkout</td>
<td>
<ul>
<li>Nach Produkten suchen und Filteroptionen auswählen</li>
<li>Fügen Sie Produkte aus Suchergebnissen, Kategorieseiten und Produktseiten in den Warenkorb</li>
<li>Alle Produktarten testen</li>
<li>Anzeigen des Warenkorbs und Ändern des Inhalts durch Entfernen oder Ändern von Beträgen </li>
<li>Zur Kasse gehen, um die Bestellbeträge mit dem Warenkorb und den Produktinformationen zu vergleichen</li>
<li>Überprüfen, ob die Steuer für den Warenkorb korrekt berechnet wird</li>
<li>Kauf mit verschiedenen Optionen abschließen: Gutschein hinzufügen, Versand auswählen, Versand- und Rechnungsinformationen und Zahlungsinformationen eingeben</li>
<li>Überprüfen von Zahlungs-Gateways und -optionen während des Checkouts</li>
<li>Auf Benachrichtigungen auf dem Bildschirm, im Kundenkonto aufgeführte Bestellungen und E-Mail-Benachrichtigungen prüfen</li>
<li>Testen von Gast- und Kunden-Checkout</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>Erstellen einer Bestellung für einen Kunden</li>
<li>Suchen und Anzeigen von Bestellungen</li>
<li>Ändern einer Bestellung durch Hinzufügen und Entfernen von Produkten, Ändern von Beträgen, Ändern von Versand- und Rechnungsinformationen</li>
<li>Verwalten einer Rückerstattung</li>
<li>Stornieren einer Bestellung</li>
<li>Couponcodes und Rabatte anwenden</li>
</ul>
</td>
</tr>
<tr>
<td>Site-Inhalte</td>
<td>
<ul>
<li>Überprüfen, ob alle Designs und Assets korrekt geladen wurden</li>
<li>Überprüfen der korrekten CSS-Anzeige, einschließlich der Größe responsiver Medien</li>
<li>Prüfen Sie die Geschäftsbedingungen, die Rückerstattungsrichtlinie und andere Richtlinieninformationen.</li>
<li>Überprüfen Sie Kontaktinformationen, Links und mehr über Ihr Unternehmen</li>
<li>Nach Produkten und Inhalten suchen, Filterung der Ergebnisse überprüfen</li>
<li>Überprüfen des Fußzeilenblocks und der oberen Navigationsblöcke</li>
<li>Testen der 404- und Wartungsseiten</li>
</ul>
</td>
</tr>
<tr>
<td>Erweiterungen</td>
<td>
<ul>
<li>Überprüfen Sie alle Erweiterungseinstellungen, insbesondere für alle Steuer-, Versand- und Zahlungsmodule (Beispiel: Bestellung an Lager- und Finanzverwaltungssystem gesendet).</li>
<li>Testen aller angepassten Modul- und installierten Erweiterungsinteraktionen</li>
<li>Überprüfen Sie die Daten auf alle Interaktionen, die abgeschlossen werden sollen (Zahlungen, Bestellungen, E-Mail-Benachrichtigungen).</li>
<li>Überprüfen der Konfigurationen pro Umgebung für Ihre Erweiterungen</li>
<li>Überprüfen der Funktionsabhängigkeit zwischen Modulen und Erweiterungen</li>
<li>Überprüfen Sie alle Aktionen als Händler und Kunde</li>
</ul>
</td>
</tr>
<tr>
<td>Integrationen von Drittanbietern</td>
<td>
<ul>
<li>Stellen Sie sicher, dass Daten korrekt in Adobe Commerce gespeichert werden und vom Drittanbieterdienst exportiert, per Push übertragen oder aufgerufen werden können (Beispiel: Bestellungen werden im Auftragsverwaltungssystem von Drittanbietern angezeigt)</li>
<li>Überprüfen von Konfigurationen und Interaktionen pro Integration</li>
<li>Durchführen von Roundtrip-Tests mit Ursprung in Adobe Commerce und Ihrem Drittanbieterdienst</li>
<li>Überprüfen, ob die Authentifizierung abgeschlossen ist</li>
<li>Überprüfen Sie, ob protokollierte Probleme vorliegen, um Code-Integrationen oder Fehlermeldungen in Control Panels zu aktualisieren.</li>
</ul>
</td>
</tr>
<tr>
<td>Backend-Tests</td>
<td>
<ul>
<li>Testen und Löschen des Cache </li>
<li>Durchführen von Neuindizierungen und Überprüfen der Ergebnisse</li>
<li>Überprüfen Sie Cron-Aufträge, prüfen Sie sie auf etwaige cron_schedule-Fehler</li>
<li>Überprüfen auf Probleme mit Shell-Skripten</li>
<li>Überprüfen auf protokollierte Probleme: Anwendungsprotokolle, PHP-Protokolle, MySQL-Protokolle, E-Mail-Protokolle</li>
</ul>
</td>
</tr>
</table>

## Belastungs- und Belastungstests

Vor dem Start sollten Sie umfassende Traffic- und Leistungstests für Ihre Staging- und Produktionsumgebungen durchführen. Erwägen Sie Leistungstests für Ihre Frontend- und Backend-Prozesse.

Bevor Sie mit dem Testen beginnen, geben Sie ein Ticket mit Support ein, in dem Sie die zu testenden Umgebungen, die von Ihnen verwendeten Tools und den Zeitrahmen angeben. Aktualisieren Sie das Ticket mit Ergebnissen und Informationen, um die Leistung zu verfolgen. Wenn Sie den Test abgeschlossen haben, fügen Sie Ihre aktualisierten Ergebnisse hinzu und notieren Sie, dass der Tickettest mit einem Datum- und Zeitstempel abgeschlossen ist.

Überprüfen Sie die [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)-Optionen im Rahmen Ihres Bereitschaftsprozesses vor der Markteinführung.

Um optimale Ergebnisse zu erzielen, verwenden Sie die folgenden Tools:

- [Anwendungs-Leistungstest](../environment/variables-post-deploy.md#ttfb_tested_pages) - Testen Sie die Anwendungs-Leistung, indem Sie die `TTFB_TESTED_PAGES` Umgebungsvariable konfigurieren, um die Reaktionszeit der Site zu testen.
- [Siege](https://www.joedog.org/siege-home/) - Traffic-Gestaltungs- und Testsoftware, die Ihr Geschäft an die Grenze bringt. Treten Sie mit einer konfigurierbaren Anzahl simulierter Clients auf Ihre Website. Siege unterstützt einfache Authentifizierung, Cookies, HTTP-, HTTPS- und FTP-Protokolle.
- [Jmeter](https://jmeter.apache.org) - Hervorragende Belastungstests, um die Leistung bei Traffic-Spitzen wie bei Flash-Verkäufen zu messen. Erstellen Sie benutzerdefinierte Tests, die für Ihre Site ausgeführt werden.
- [New Relic](../monitor/new-relic-service.md) (bereitgestellt) - Hilft beim Auffinden von Prozessen und Bereichen der Site, was zu einer langsamen Leistung führt, wobei die pro Aktion aufgezeichnete Zeit wie die Übertragung von Daten, Abfragen, Redis und mehr verwendet wird.
- [WebPageTest](https://www.webpagetest.org) und [Pingdom](https://www.pingdom.com) - Echtzeit-Analyse der Ladezeit Ihrer Site-Seiten mit verschiedenen Ursprungsorten. Pingdom kann eine Gebühr verlangen. WebPageTest ist ein kostenloses Tool.

## Funktionstests

Sie können das Magento Functional Testing Framework (MFTF) verwenden, um Funktionstests für Adobe Commerce über die Cloud Docker-Umgebung abzuschließen. Siehe [Anwendungstests](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing) im Handbuch _Cloud Docker für Commerce_.

## Einrichten des Sicherheits-Scan-Tools

Es gibt ein kostenloses Sicherheits-Scan-Tool für Ihre Websites. Informationen zum Hinzufügen Ihrer Sites und Ausführen des Tools finden Sie unter [Security Scan Tool](../launch/overview.md#set-up-the-security-scan-tool).
