---
title: Cloud-Komponenten für Commerce
description: Hier finden Sie eine Liste der neuesten Verbesserungen am Cloud-Komponentenpaket.
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: dcf71ffbdafae46e6a02735c090c33a8fe248bc6
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Cloud-Komponenten für Commerce

Das [Cloud-Komponenten](https://github.com/magento/magento-cloud-components)-Paket bietet erweiterte Adobe Commerce-Kernfunktionen für Sites, die in der Cloud-Infrastruktur bereitgestellt werden. Dieses Paket ist eine Abhängigkeit für das ECE-Tools-Paket. In diesen Versionshinweisen werden die neuesten Verbesserungen an diesem Paket beschrieben, das eine Komponente von [Cloud-Tools-Suite für Commerce](cloud-tools-suite.md) ist.

Das `magento/magento-cloud-components`-Paket verwendet die folgende Versionssequenz: `<major>.<minor>.<patch>`

Die Versionshinweise umfassen Folgendes:

- ![neues Symbol](../../assets/new.svg) Neue Funktionen
- ![Fehlerbehebungssymbol](../../assets/fix.svg) Fehlerbehebungen und Verbesserungen

<!--Add release notes below-->

## v1.1.2 {#latest}

Veröffentlichungsdatum: 3. Juni 2025

- ![Fix-Symbol](../../assets/fix.svg) **Verbesserte Kompatibilität mit 2.4.8**-Aktualisierte Drittanbieterbibliotheken für bessere Kompatibilität mit 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.1

Veröffentlichungsdatum: 6. Februar 2025

- ![neues Symbol](../../assets/new.svg) **PHP 8.4**—Unterstützung für PHP 8.4.<!-- MCLOUD-13148	 - --> hinzugefügt
- ![Fix icon](../../assets/fix.svg) **Fix for Cache Warm-up** - Es wurde ein Problem mit Kategorie-URLs während des Cache-Warm-ups behoben.<!-- MCLOUD-12454 - -->


## v1.1.0

Veröffentlichungsdatum: 7. Oktober 2024

- ![fix icon](../../assets/fix.svg) **Refactored code**—Die Unterstützung der alten PHP-Versionen 7.4, 7.3, 7.2 und der zugehörigen Bibliotheken wurde entfernt.<!-- MCLOUD-9278 - -->
- ![Fix icon](../../assets/fix.svg) **Aktualisierte Monolog-Version** - Unterstützung für Monolog 3.6.<!-- MCLOUD-12855 - --> hinzugefügt

## v1.0.14

Veröffentlichungsdatum: 8. April 2024

- ![new icon](../../assets/new.svg) **PHP**—Unterstützung für PHP 8.3 hinzugefügt.

## v1.0.13

Veröffentlichungsdatum: 10. März 2023

- ![neues Symbol](../../assets/new.svg) **Erweiterte Unterstützung für PHP 8.2**—Es wurden Kompatibilitätsprobleme mit bestimmten PHP 8.2.x-Versionen behoben, um Commerce 2.4.6 zu unterstützen.

## v1.0.12

Veröffentlichungsdatum: 13. September 2022

- ![Fix icon](../../assets/fix.svg) **Errors on warmup** - Es wurde ein Problem behoben, bei dem versucht wurde, [warmup](../environment/variables-post-deploy.md#warm_up_pages) auszuführen, wenn die Sichtbarkeit der Seite in der Admin-**[&#128279;](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) auf** Nicht sichtbareingestellt war, was zu `ERROR: Warming up failed: <link to page>` Fehlern im Bereitstellungsprotokoll führte.<!-- MCLOUD-9134 -->

## v1.0.11

Veröffentlichungsdatum: 4. August 2022

- ![fix icon](../../assets/fix.svg) **Hinzugefügte Unterstützung für Symfony 5.4-** - Fehlerbehebungen für die Kompatibilität mit Symfony 5.4.<!-- AC-3550 -->

## v1.0.10

Veröffentlichungsdatum: 10. März 2022

- ![neues Symbol](../../assets/new.svg) **Unterstützung von PHP 8.1**—Unterstützung für PHP 8.1 wurde hinzugefügt und die Unterstützung für PHP 7.1 wurde eingestellt.

## v1.0.9

Veröffentlichungsdatum: 25. Oktober 2021

- ![Fix icon](../../assets/fix.svg) **Update Monolog** - Die Mindestversion, die für das `monolog`-Paket erforderlich ist, wurde auf `^2.3` aktualisiert.<!-- ACMP-1263 -->

## v1.0.8

Veröffentlichungsdatum: 29. Juli 2021

- ![Fix icon](../../assets/fix.svg) **Entfernte nachfolgende Schrägstriche aus automatisch generierten URLs** - Entfernte die nachfolgenden Schrägstriche aus Kategorieseiten-URLs, die während der Cache-Aufwärmung generiert wurden.<!--MCLOUD-7192-->

## v1.0.7

Veröffentlichungsdatum: 9. September 2020

- ![neues Symbol](../../assets/new.svg) **Verbesserungen bei der Protokollierung** - Verringern Sie die Größe der `cache.log` Datei, um die Leistung zu verbessern.<!--MCLOUD-6859-->

- ![fix icon](../../assets/fix.svg) Es wurde ein Typfehler in den Cache-Konfigurationswerten behoben, der dazu führte, dass der `php bin/magento cache:evict` CLI-Befehl fehlschlug.

## v1.0.6

Veröffentlichungsdatum: 5. August 2020

- ![neues Symbol](../../assets/new.svg) **Verbesserung der Redis-Leistung** - Der `./bin/magento cache:evict`-Befehl wurde hinzugefügt, um abgelaufene Redis-Schlüssel zu entfernen, wodurch die Redis-Speicherauslastung reduziert wird, um die Leistung zu verbessern.<!--MCLOUD-6023-->

- ![Fehlerbehebungssymbol](../../assets/fix.svg) Unterstützung für *New Relic-Protokolle im Kontext* entfernt, um ein Leistungsproblem zu beheben.<!--MCLOUD-6422-->

## v1.0.5

Veröffentlichungsdatum: 25. Juni 2020

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das in magento/magento-cloud-components Version 1.0.4 eingeführt wurde und dazu führte, dass der Cache-Vorgang während der Bereitstellungsphase fehlschlug und somit der Bereitstellungsprozess unterbrochen wurde.

## v1.0.4

Veröffentlichungsdatum: 25. Juni 2020

- ![neues Symbol](../../assets/new.svg) **Implementierte New Relic-Protokolle im Kontext** - Von Adobe Commerce generierte Anwendungsprotokolle werden jetzt in New Relic in Spuren angezeigt, um die Fehlerbehebungsfunktionen zu verbessern.<!--MCLOUD-6029-->

- ![neues Symbol](../../assets/new.svg) **Verbesserte Protokollierung** Es wurde eine Protokollierung hinzugefügt, um Cache-Invalidierung und vollständige Neuindizierungsereignisse zu verfolgen.<!--MCLOUD-6157-->

## v1.0.3

Veröffentlichungsdatum: 27. Februar 2020

- ![Fix icon](../../assets/fix.svg) Es wurde ein Kompatibilitätsproblem behoben, um `ece-tools` Versionen 2002.0.x zu unterstützen, die ältere PHP-Versionen verwenden.

## v1.0.2

Veröffentlichungsdatum: 6. Februar 2020

- ![neues Symbol](../../assets/new.svg) Die Funktionalität der `WARM_UP_PAGES` Umgebungsvariablen wurde erweitert, um das Vorausfüllen des Cache für bestimmte Produktseiten zu unterstützen. Eine ausführliche Beschreibung der [ finden Sie ](../environment/variables-post-deploy.md#warm_up_pages) Thema „Variablen nach der Bereitstellung“<!--MAGECLOUD-4444-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, bei dem eine ungültige Speicher-URL dazu führte, dass der Hook nach der Bereitstellung fehlschlug, wenn die `WARM_UP_PAGES`-Funktion zum Auffüllen des Caches verwendet wurde. Dieses Problem trat nur auf, wenn die URL-Neuschreibungen deaktiviert waren.<!-- MAGECLOUD-4094 -->

## v1.0.1

Veröffentlichungsdatum: 23. Juli 2019

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das die [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)-Funktion beeinträchtigte, welche eine standardmäßige Store-URL verwendet. Wenn der `config:show:default-url`-Befehl jetzt keine Basis-URL abrufen kann, wird die URL aus der Variablen MAGENTO_CLOUD_ROUTES verwendet.<!-- MAGECLOUD-3866 -->

## v1.0.0

Veröffentlichungsdatum: 12. Juni 2019

Dies ist die erste Version des [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components)-Pakets, das eine neue Abhängigkeit für `ece-tools` Paketversion 2002.0.20 und höher ist.

- ![neues Symbol](../../assets/new.svg) Es wurde die Möglichkeit hinzugefügt, Regex-Muster zu verwenden, um die Umgebungsvariable **WARM_UP_PAGES** zu konfigurieren, um einzelne Seiten, mehrere Domains und mehrere Seiten zwischenzuspeichern. Siehe [Variablen nach der Bereitstellung](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
