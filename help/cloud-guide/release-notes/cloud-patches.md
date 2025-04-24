---
title: Cloud-Patches für Commerce
description: Hier finden Sie eine Liste der neuesten Verbesserungen am Cloud-Patches-Paket.
recommendations: noDisplay, catalog
last-substantial-update: 2025-04-24T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: db8d20cdf999657c059202131111e71faf50b8e9
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 0%

---

# Cloud-Patches für Commerce

Das [Cloud Patches](https://github.com/magento/magento-cloud-patches)-Paket enthält eine Reihe erforderlicher Patches, die die Integration aller Adobe Commerce-Versionen in Cloud-Umgebungen verbessern und die schnelle Bereitstellung wichtiger Fehlerbehebungen unterstützen.

Das Paket Cloud-Patches für Commerce ist eine Abhängigkeit für das Paket ECE-Tools und wird installiert und aktualisiert, wenn Sie das Paket ECE-Tools installieren oder aktualisieren. Sie können Cloud-Patches für Commerce auch als eigenständiges Paket verwenden und verwalten, um Patches auf ein Adobe Commerce-Projekt anzuwenden, das sich nicht auf der Cloud-Plattform befindet. In diesen Versionshinweisen werden die neuesten Verbesserungen an diesem Paket beschrieben.

>[!TIP]
>
>Um sicherzustellen, dass Ihr Projekt über alle erforderlichen Patches verfügt, aktualisieren Sie auf die [neueste Version von ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>Anweisungen [ Anwenden von Patches auf ](../development/apply-patches.md) Projekte finden Sie unter „Anwenden von Patches“.

Das `magento/magento-cloud-patches`-Paket verwendet die folgende Versionssequenz: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.6 {#latest}

Veröffentlichungsdatum: 24. April 2025

- ![neues Symbol](../../assets/new.svg) **Aktualisierter Patch für Commerce 2.4.4 auf 2.4.7** - Dies ist ein aktualisierter Patch für [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08), der in Version 1.1.4 veröffentlicht wurde<!-- MCLOUD-13240 -->

## v1.1.5

Veröffentlichungsdatum: 15. April 2025

- ![neues Symbol](../../assets/new.svg) **Patch für B2B 1.5.2 hinzugefügt**—Behebung des Problems ACP2E-3833 mit B2B-Modul 1.5.2 und MariaDB 10.6<!-- MCLOUD-13605	-->

## v1.1.4

Veröffentlichungsdatum: 13. Februar 2025

- ![neues Symbol](../../assets/new.svg) **Patch für Commerce 2.4.4 bis 2.4.7 hinzugefügt** - Dieses Update aktualisiert Patches [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## v1.1.3

Veröffentlichungsdatum: 6. Februar 2025

- ![neues Symbol](../../assets/new.svg) **PHP 8.4**—Unterstützung für PHP 8.4.<!-- MCLOUD-13149	 - --> hinzugefügt

## v1.1.2

Veröffentlichungsdatum: 5. November 2024

- ![Fix-Symbol](../../assets/fix.svg) **Patch für Commerce 2.4.4 bis 2.4.7 hinzugefügt** - Dieses Update behebt eine [CVE-2024-45115](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) Sicherheitslücke für Adobe Commerce bei der Verwendung des B2B-Moduls.<!-- MCLOUD-12980 - -->

## v1.1.1

Veröffentlichungsdatum: 5. November 2024

- ![Fix icon](../../assets/fix.svg) **Patch für Commerce 2.4.4 bis 2.4.7 hinzugefügt** - Dieses Update behebt eine kritische [CVE-2024-34102](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting-Schwachstelle.<!-- MCLOUD-12980 - -->

## v1.1.0

Veröffentlichungsdatum: 7. Oktober 2024

- ![fix icon](../../assets/fix.svg) **Refactored code**—Die Unterstützung alter PHP-Versionen (7.4, 7.3, 7.2) und der zugehörigen Bibliotheken wurde entfernt.<!-- MCLOUD-9278 - -->
- ![Fix icon](../../assets/fix.svg) **Aktualisierte Monolog-Version** - Unterstützung für Monolog 3.6.<!-- MCLOUD-12855 - --> hinzugefügt
- ![Fix icon](../../assets/fix.svg) **Patch für Anwendungsserver** - Löst ein bekanntes Problem mit dem GraphQL-Anwendungsserver. Insbesondere enthielt die `CatalogGraphQl\\Model\\Config\\AttributeReader` in Version 2.4.7 einen Fehler, der dazu führen konnte, dass GraphQL-Anfragen Antworten basierend auf veralteter Attributkonfiguration abrufen konnten.<!-- ACPT-1876 -->

## v1.0.27

Veröffentlichungsdatum: 21. Mai 2024

- **Unterstützung für PHP 8.3** - Dieser Patch löst Kompatibilitätsfehler zwischen PHP 8.3 und der Composer-Paketversion.

## v1.0.26

Veröffentlichungsdatum: 8. April 2024

- ![new icon](../../assets/new.svg) **PHP** — Unterstützung für PHP 8.3 hinzugefügt.

## v1.0.25

Veröffentlichungsdatum: 16. Januar 2024

- **Cache-**: Dieser Patch verbessert die Effizienz des Layout-Caches und reduziert so die Speichernutzung für Adobe Commerce-Versionen 2.4.4 und höher erheblich.<!-- MCLOUD-11514 -->
- **Verbesserungen bei CRON-Aufträgen** - Dieser Patch behebt das Problem, dass verpasste Aufträge unnötigerweise auf Cron-Auftragssperren für Adobe Commerce-Versionen 2.4.4 und höher warten.<!-- MCLOUD-11329 -->

## v1.0.24

Veröffentlichungsdatum: 15. September 2023

- **Leistungsverbesserung** - Dieser Patch behebt ein Problem, das die Leistung beeinträchtigt, indem die Anzahl der Ladevorgänge derselben Bereitstellungskonfigurationen für Adobe Commerce 2.4.6 auf 2.4.6-p1 reduziert wird<!-- MCLOUD-10604 -->

## v1.0.23

Veröffentlichungsdatum: 31. Juli 2023

- **Patch: MCLOUD-10604** wurde entfernt. Dieser Patch wurde in QPT verschoben.<!-- MCLOUD-10736 -->

## v1.0.22

Veröffentlichungsdatum: 19. Juni 2023

- **Verbesserter QPT-CLI-Assistent/-**: Dem QPT-CLI-Assistenten/-Ausgang wurde eine Warnung hinzugefügt, die Sie daran erinnert, Patch-Details und -Anforderungen zu überprüfen, wenn Abhängigkeiten bestehen.<!-- ACP2E-1963 -->
- **Patches für Commerce 2.4.6:** hinzugefügt
   - Fehlerkorrektur - Die `regexp cache tag` Validierung funktioniert jetzt.<!-- MCLOUD-10226 -->
   - Verbesserte Leistung durch Reduzierung der Anzahl der Ladevorgänge derselben Bereitstellungskonfigurationen.<!-- MCLOUD-10604 -->
- **Patches für Commerce 2.3.7 bis 2.4.6 hinzugefügt** - Es wurde ein Problem behoben, das dazu führte, dass für die `catalog_product_entity_*` Tabellen nicht um 1, sondern um einen zufälligen Wert inkrementiert wurde.<!-- MCLOUD-10032 -->
- **Patches für Commerce 2.4.0 bis 2.4.6 hinzugefügt** - Es wurde ein Fehler behoben, der diese `The file can't be deleted. Warning!unlink: No such file or directory` angab. Dieser Fehler trat auf, wenn der JS/CSS-Cache aus der Admin-Instanz geleert wurde.<!-- MCLOUD-10279 -->

## v1.0.21

Veröffentlichungsdatum: 10. März 2023

- **Erweiterte Unterstützung für PHP 8.2**—Es wurden Kompatibilitätsprobleme mit bestimmten PHP 8.2.x-Versionen behoben, um Commerce 2.4.6 zu unterstützen.

## v1.0.20

Veröffentlichungsdatum: 27. Oktober 2022

- **Patch zur Verbesserung des L2-Cache hinzugefügt** - Dieser Patch behebt ein Problem beim Leeren des lokalen L2-Cache für Commerce Version 2.4.0 und 2.4.1.<!-- MCLOUD-7845 -->

## v1.0.19

Veröffentlichungsdatum: 13. September 2022

- **Verbesserte Unterstützung für PHP 8.1**—Es wurden Kompatibilitätsprobleme mit bestimmten PHP 8.1.x-Versionen behoben.

## v1.0.18

Veröffentlichungsdatum: 11. August 2022

Kritischer Patch für Adobe Commerce 2.4.5:

- **Problem mit Bestellungen mit Braintree Payments** - Dieser Patch löst ein wichtiges Problem, das Administratoren daran hindert, neue Bestellungen oder Neubestellungen aufzugeben.<!-- MCLOUD-9137 -->

Siehe [Der Administrator kann keine Bestellung/Neuanordnung erstellen, wenn Braintree Payment aktiviert ist](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## v1.0.17

Veröffentlichungsdatum: 24. Mai 2022

Es wurden Einschränkungen für Sicherheits-Patches in der `patches.json`-Datei behoben.

## v1.0.16

Veröffentlichungsdatum: 31. März 2022

Kritischer Patch für Adobe Commerce 2.3.3-p1 und neuere Versionen:

Patches wurden aktualisiert, um eine **kritische** Sicherheitslücke zu beheben, die zu einer nicht authentifizierten Ausführung von Remote-Code führt.<!-- MCLOUD-8479 -->

Siehe [Adobe-Sicherheitsbulletin APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.15

Veröffentlichungsdatum: 10. März 2022

- **Unterstützung von PHP 8.1** - Unterstützung für PHP 8.1 wurde hinzugefügt und die Unterstützung für PHP 7.0 und 7.1 wurde eingestellt.
- **Patch für Adobe Commerce 2.3.3 hinzugefügt** - Feste Währung wird auf der Produktseite angezeigt.

## v1.0.14

Veröffentlichungsdatum: 13. Februar 2022

Kritischer Patch für Adobe Commerce 2.3.3-p1 und neuere Versionen:

Es wurde ein Patch hinzugefügt, um eine **kritische** Sicherheitslücke zu beheben, die zu einer nicht authentifizierten Ausführung von Remote-Code führte.<!-- MCLOUD-8461 -->

Siehe [Adobe-Sicherheitsbulletin APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.13

Veröffentlichungsdatum: 25. Oktober 2021

- **Monolog aktualisieren** - Die Mindestversion, die für das `monolog` erforderlich ist, wurde auf `^2.3` aktualisiert.<!-- ACMP-1263 -->
- **Inkompatible PHP-Methode**—Behobene inkompatible PHP-Methode für Adobe Commerce-Versionen 2.4.3 und 2.3.7-p1.<!-- AC-384 -->
- **PHP-Fehler** - Es wurde ein `PHP error 'Undefined variable: errorMessage' ...` Fehler behoben, der beim Anwenden eines Patches auftrat.<!-- ACP2E-138 -->

## v1.0.12

Veröffentlichungsdatum: 12. August 2021

Kritischer Patch für Adobe Commerce 2.4.3 und 2.3.7-p1:

- **Problem mit API-Ratenbegrenzung** - Dieser Patch korrigiert ein standardmäßiges Ratenlimit, das Web-APIs daran hinderte, Anfragen mit mehr als 20 Elementen in einem Array zu verarbeiten. Dieser Patch erhöht den Standardwert des Ratenlimits. Siehe die Versionshinweise zu Adobe Commerce [2.4.3](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

Veröffentlichungsdatum: 29. Juli 2021

- **Es wurde ein Problem behoben, das durch die Anwendung des Patches für die mehrschichtige B2B-Navigation verursacht wurde** - Für Kunden, die den Patch für die mehrschichtige B2B-Navigation angewendet haben, wird mit dieser Korrektur ein `Undefined offset` behoben, der nach dem Wechsel der Store-Ansicht auf der Suchseite angezeigt wird.<!--MCLOUD-5287-->

- **PayPal Checkout-Patch** - Behebt ein Problem mit Adobe Commerce 2.3.7 bei PayPal Express, bei dem der zuvor aufgegebene Bestellpreis angezeigt wird.<!--MC-42674-->

- **Unterstützung der Patch-Kategorie** - Es wurde Unterstützung für die Verarbeitung von Patch-Kategorien und Ursprungsquellen hinzugefügt, die Qualitäts-Patches zugewiesen wurden. Mit diesen Kategorien können Kunden Filter und Sortierung verwenden, um Patches schneller zu finden, wenn sie das [Quality Patches Tool](https://github.com/magento/quality-patches) und das Site-Wide Analysis Tool (SWAT) verwenden. <!--MC-38577-->

## v1.0.10

Veröffentlichungsdatum: 10. Mai 2021

- **Kompatibilität mit Adobe Commerce 2.3.7**—Behobener Konflikt zwischen Composer-Abhängigkeiten für die Installation auf Adobe Commerce 2.3.7.<!--MC-42131-->
- **Es wurde ein Problem behoben, das durch mehrmaliges Anwenden eines gebündelten Patches** wurde. Wenn ein gebündelter Patch (einer, der andere veraltete Patches enthält) mehrmals angewendet wurde, konnten die eingeschlossenen veralteten Pakete zurückgesetzt werden. Alle Patches werden jetzt nur noch einmal angewendet. Beim erneuten Versuch, dasselbe Paket anzuwenden, wird eine Meldung angezeigt, dass der Patch bereits angewendet wurde.<!--MC-41912-->
- **Patch für die B2B-** - Es wurde ein weiteres Problem behoben, das verhinderte, dass die mehrschichtige Navigation alle Produktoptionen anzeigte, wenn der Benutzer den freigegebenen B2B-Katalog aktiviert hatte.<!--MCLOUD-7742-->

## v1.0.9

Veröffentlichungsdatum: 1. Februar 2021

- **Patch für die B2B-**-Navigation: Es wurde das Problem behoben, das verhinderte, dass die mehrschichtige Navigation alle Produktoptionen anzeigte, wenn der freigegebene B2B-Katalog aktiviert war.<!--MCLOUD-6923-->
- **Kompatibilität mit PHP 7.4** - Es wurde ein Kompatibilitätsproblem mit Cloud-Patches mit PHP 7.4 behoben<!--MCLOUD-7367-->
- **Veraltete Patches werden sichtbar** - Es wurde ein Problem mit Cloud-Patches behoben, bei dem veraltete Patches in der Patches-Tabelle angezeigt wurden, nachdem ein Ersatz-Patch angewendet wurde, der den gesamten Inhalt des veralteten Patches enthält. Dies kann passieren, wenn Sie einen Patch angewendet haben, der mehrere andere Patches kombiniert hat.<!--MC-40626-->
- **Stille Fehler beim Anwenden von Patches** - Es wurde ein Problem mit Cloud-Patches behoben, bei dem der Befehl `git apply` in einigen Umgebungen im Hintergrund keine Patches angewendet hat.<!--MC-40529-->

## v1.0.8

Veröffentlichungsdatum: 14. Oktober 2020

- **Kompatibilitätsaktualisierungen für magento/magento-cloud-patches** - Die `symfony`- und `semver`-Versionsbeschränkungen in der `composer.json` wurden aus Gründen der Kompatibilität mit Adobe Commerce 2.4.1 und höheren Versionen aktualisiert.<!--MCLOUD-7111-->

## v1.0.7

Veröffentlichungsdatum: 14. Oktober 2020

- **Redis-Patches für Adobe Commerce 2.3.0 auf 2.3.5, 2.4.0** - Die Redis-Patches wurden aktualisiert, um das Hinzufügen von Produkten zu einer Kategorie bei der Implementierung eines Level-2-Caches zu unterstützen. <!--MCLOUD-6659-->

- **Braintree VBE-Patch**: Es wurde ein Problem behoben, das einen Fehler verursachte, wenn ein Administrator versuchte, einen Braintree-Vergleichsbericht anzuzeigen. <!--MCLOUD-6684-->

- Jetzt verwendet der Befehl `ece-patches apply` den Unix-`patch`-Befehl, um Patches anzuwenden, wenn Git nicht auf dem Host-System verfügbar ist. <!--MCLOUD-7069-->

## v1.0.6

Veröffentlichungsdatum:

- **Redis-Patches für Adobe Commerce 2.3.0 - 2.3.4** - Optimieren Sie die Kommunikation und verbessern Sie die Leistung
   - Verringern des Umfangs der Netzwerkübertragungen zwischen Redis und Adobe Commerce
   - Beheben von Laufzeitbedingungen bei Redis-Lade- und -Schreibvorgängen
   - Neuschreiben des Basis-Cache-Adapters zur Behandlung von Speicherfehlern
   - Verringern Sie den Verbrauch von Redis CPU<!--MCLOUD-6139-->

- **Redis-Patches für Adobe Commerce 2.3.0 - 2.3.5** - Verbesserung der Leistung und Behebung von Fehlern
   - Behebung der Cache-Sperrimplementierung, um unendliche Sperren zu verhindern
   - Den aktuellen Sperrmechanismus verbessern
   - Implementieren von signierten Sperren, um zu verhindern, dass parallele Anforderungen entsperrt werden
   - Behebung des folgenden Fehlers, der beim Redis-Schreibvorgang auftritt: `OOM command not allowed when used memory > maxmemory`
   - Fehlerkorrektur - Die Verarbeitung für sauberen Cache wird durch `cat_p` Tag korrigiert, das während Produktaktualisierungen ausgeführt wird<!--MCLOUD-6110-->

- Es wurde ein Fehler behoben, der beim Anwenden des erforderlichen `amzn/amazon-pay-module`-Patches auf Adobe Commerce in Cloud-Infrastrukturprojekten mit Adobe Commerce v2.2.6 oder 2.3.5, die dieses Modul nicht enthalten, zu einem Fehler führte. Der Patch-Prozess überspringt jetzt den `amzn/amazon-pay-module` Patch, wenn das Modul nicht installiert ist.<!--MCLOUD-6588-->

## v1.0.5

Veröffentlichungsdatum: 26. Juni 2020

- **Leistungsverbesserungen von Redis** - Fügt den Adobe Commerce-Versionen 2.3.3 und 2.3.4 Redis-Optimierungsfunktionen hinzu. Diese Fehlerbehebungen waren in der Adobe Commerce-Version 2.3.5 enthalten.<!--MCLOUD-5771-->

- **New Relic Log Enricher** - Fügt die Monolog ProcessorInterface hinzu, die zur Unterstützung der in den Cloud-Komponenten von Commerce Version 1.0.4 eingeführten Verbesserungen der New Relic-Protokollierungsfunktionen erforderlich ist. Dieser Patch ist erforderlich, um Adobe Commerce 2.1.x bereitzustellen. Wenn der Patch nicht angewendet wird, schlägt der Build während des `di:compile` fehl.<!--MCLOUD-6029-->

## v1.0.4

Veröffentlichungsdatum: 12. Mai 2020

- **Amazon Pay Checkout** - Es wurde ein Problem mit dem Amazon Pay Payment-Widget behoben, das Kunden daran hinderte, die Zahlungsmethode im Schritt _Überprüfen und Zahlungen_ während des Checkout-Prozesses zu ändern.<!--MCLOUD-5930-->

- **Produktanzeige auf Kategorieseite** - Behebt ein Problem, das verhinderte, dass Produkte auf der Kategorieseite in der Ansicht _Alle Seiten anzeigen_ angezeigt wurden<!--MCLOUD-5684-->

- **Bild-Upload in Page Builder** - Es wurde ein Problem mit der Page Builder-Benutzeroberfläche behoben, das manchmal den folgenden Fehler beim Hochladen von Bildern in die Bildergalerie verursachte: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Unnötige Sitemap-Generierungswarnungen unterdrücken**: Fügt einen Wiederholungsversuch hinzu, wenn während der Sitemap-Generierung Fehler auftreten, und überspringt die E-Mail-Benachrichtigung des Kunden, wenn Fehler automatisch behoben werden können.<!--MCLOUD-3025-->

- **Verbesserung der Site** - Behebt ein Leistungsproblem mit der `Magento\Framework\App\DeploymentConfig\Reader::load`, bei dem es in regelmäßigen Abständen zu langen Ladezeiten kam, die die Site-Leistung beeinträchtigten. <!--MCLOUD-5650-->

- Die Patch-Zuweisung für Zahlungsmethoden-Patches wurde aktualisiert, um die Zahlungsmodule anstelle des Magento-Basispakets (magento/magento2-base) auszuwählen, sodass die Zahlungsmodule nur angewendet werden, wenn die Zahlungsmodule vorhanden sind.<!--MCLOUD-5666-->

- Patches wurden aus Gründen der Kompatibilität mit Magento Open Source aktualisiert.<!--MCLOUD-5701-->

## v1.0.3

Veröffentlichungsdatum: 28. April 2020

- Es wurde eine Fehlerbehebung für den Patch „FPC wird während der Bereitstellung deaktiviert“ hinzugefügt, um Adobe Commerce 2.3.5 zu unterstützen.

## v1.0.2

Veröffentlichungsdatum: 27. Februar 2020

Diese Version umfasst die folgenden Patches und wichtigen Fehlerbehebungen:

- **Kompatibilitätsaktualisierungen für magento/magento-cloud-patches**

   - Die `symfony`- und `semver`-Versionsbeschränkungen in der `composer.json` wurden aus Gründen der Kompatibilität mit Adobe Commerce 2.4 und höheren Versionen aktualisiert.<!--MAGECLOUD-5127-->

   - Einschränkungen in `composer.json` zur Kompatibilität mit `ece-tools` Versionen 2002.0.22 und höher 2002.0.x wurden aktualisiert.

- **PayPal Express Checkout** - Dieser Patch wurde am 12. Februar 2020 veröffentlicht und behebt ein Problem, das Bestellungen betrifft, die mit dem PayPal Express Checkout aufgegeben wurden. Dabei gibt die Lieferadresse für die Bestellung eine Länderregion an, die manuell in das Textfeld eingegeben wurde, anstatt sie aus dem Dropdown-Menü auf der Versandseite auszuwählen. Die vollständige Patch-Beschreibung finden Sie auf der Patch-Download-Seite.

- **Fehlerbehebung bei der Anwendungsbereitstellung** - Es wurde ein Patch hinzugefügt, um ein Problem zu beheben, das den vollständigen Seiten-Cache während des Bereitstellungsprozesses deaktiviert hat. Dieser Patch gilt für Adobe Commerce 2.3.2 und neuere Versionen.

- **Bereichsparameter für die Async/Bulk-API** - Dieser Patch wurde aktualisiert, um einen Syntaxfehler in der `composer.json`-Datei zu beheben. Dieser Patch gilt für Magento Open Source 2.3.1 und 2.3.2. Die vollständige Patch-Beschreibung finden Sie auf der Patch-Download-Seite.

## v1.0.1

Veröffentlichungsdatum: 6. Februar 2020

Wir haben alle Magento Open Source 2.x-Patches von der Seite „Software-Downloads“ in die Version Magento/Magento-Cloud-Patches v1.0.1 aufgenommen. Wenn Sie zuvor Patches in Ihr Projekt kopiert haben, entfernen Sie diese, um Konflikte zu vermeiden.

Diese Version umfasst die folgenden Patches und wichtigen Fehlerbehebungen:

- **Beheben von Cron-Deadlocks und Verbessern der Cron-Sperrung**—

   - Es wurde ein Problem behoben, bei dem einige Cron-Aufträge aufgrund eines falschen Statuswerts in der `cron_schedule`-Tabelle nicht ausgeführt wurden. Jetzt verwenden wir das Adobe Commerce-Sperrframework , um den Cron-Auftragsstatus zu überprüfen und zu aktualisieren, anstatt die `cron_schedule`-Tabelle zu verwenden. Cron-Aufträge, die mit einem Fehlerstatus beendet wurden, werden während des nächsten Cron-Durchgangs erneut versucht, anstatt 24 Stunden zu warten.

   - Fügt einen Vorgang _Wiederholen_ hinzu, um Deadlocks bei Aktualisierungen der Daten in der `cron_schedule` zu vermeiden.

- **`magento/magento-cloud-patches` wurde aktualisiert, um alle verfügbaren Patches für Magento Open Source 2.x einzuschließen** - Das Paket „magento/magento-cloud-patches“ wurde aktualisiert, um alle Magento Open Source 2.x-Patches einzuschließen, die auf der Seite „Software-Downloads“ verfügbar sind. Wenn Sie zuvor Magento Open Source-Patches in Ihr Adobe Commerce on Cloud-Infrastrukturprojekt kopiert haben, entfernen Sie sie, um Konflikte zu vermeiden.<!--MAGECLOUD-4606-->

- **Fehlerbehebung bei der Katalogpaginierung in Elasticsearch** - Elasticsearch hat den Katalogpaginierungs-Patch aus Magento/Magento-Cloud-Patches v1.0 durch eine wirksamere Fehlerbehebung ersetzt.<!--MAGECLOUD-4847-->

- **Page Builder-**: In Cloud-Patches für Commerce 1.0.0 wurden Page Builder-Patches gebündelt, um eine bekannte RCE-Schwachstelle (Remote Code Execution) in Page Builder zu beheben. Die erste Fehlerbehebung basiert auf Adobe Commerce 2.3.3. Wir haben diese Patches mit einer stabileren Implementierung aktualisiert, die auf Adobe Commerce 2.3.4 basiert und mehrere Optimierungen zur Behebung des Problems enthält.<!--MAGECLOUD-4884-->

  Wenn Sie das Paket magento/magento-cloud-patches 1.0.0 haben, sind Sie weiterhin vor den Page Builder RCE-Schwachstellen geschützt. Wenn Sie auf 1.0.1 oder höher aktualisieren, haben Sie eine bessere Implementierung der gleichen Fehlerbehebung.

## v1.0.0

Veröffentlichungsdatum: 14. November 2019

Dies ist die erste Version des [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)-Pakets, das eine neue Abhängigkeit für die `ece-tools`-Paketversion 2002.0.22 oder höher darstellt.

Diese Version umfasst die folgenden Patches und wichtigen Fehlerbehebungen:

- **Page Builder-Sicherheits-Patches für die Versionen 2.3.1.x und 2.3.2.x** - Behebt ein Problem in der Page Builder-Vorschau, das es nicht authentifizierten Benutzenden ermöglicht, auf einige Vorlagenmethoden zuzugreifen, die zum Trigger der beliebigen Codeausführung über das Netzwerk (RCE) verwendet werden können, was zu globalen Informationslecks führt. Dieses Problem kann auftreten, wenn nicht unterstützte Versionen von Page Builder mit Adobe Commerce 2.3.1 und 2.3.2.<!--MAGECLOUD-4649--> verwendet werden

- **MSI-Patches**: Behebt Probleme, die bei der Verwendung von standardmäßigen Inventareinstellungen zur Verwaltung von Lagern zu Indexfehlern und Leistungsproblemen führten.<!--MAGECLOUD-4428-->

- **Abwärtskompatibilität neuer Mail-Schnittstellen**-Behebt ein Abwärtsinkompatibilitätsproblem, das durch die in Adobe Commerce v2.3.3 eingeführte `Magento\Framework\Mail\EmailMessageInterface` PHP-Schnittstelle verursacht wird. Im Rahmen dieses Patches erbt die neue `EmailMessageInterface` von der alten `MessageInterface`, und die Adobe Commerce-Kernmodule werden so zurückgesetzt, dass sie von `MessageInterface` abhängen.<!--MAGECLOUD-4422-->

- **Katalogpaginierung funktioniert nicht in Elasticsearch 6.x** - Behebt ein kritisches Problem bei der Suchergebnisseitenbildung, das sich auf Kundinnen und Kunden auswirkt, die Elasticsearch 6.x als Katalogsuchmaschine verwenden.<!--MAGECLOUD-4448-->
