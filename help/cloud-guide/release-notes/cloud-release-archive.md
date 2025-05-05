---
title: Archiv mit Versionshinweisen für ECE-Tools
description: Erfahren Sie mehr über archivierte Verbesserungen für ECE-Tools.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# Archiv mit Versionshinweisen für ECE-Tools

>[!NOTE]
>
>Diese Versionshinweise enthalten Informationen und Aktualisierungen für `ece-tools` Version 2002.0.22 und höher. Siehe [Versionshinweise für Cloud Tools Suite](cloud-tools-suite.md), um die neuesten Aktualisierungen für `ece-tools` und andere Cloud-Pakete zu erhalten.

## v2002.0.22

Die `ece-tools` Version 2002.0.22 ändert die Struktur des `ece-tools`-Pakets, um die Freigabe von `Adobe Commerce on cloud infrastructure` Patches von der ECE-Tools-Version zu entkoppeln. Ab dieser Version werden Patches und kritische Fehlerbehebungen mithilfe des [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)-Pakets bereitgestellt, das eine neue Abhängigkeit für das `ece-tools`-Paket darstellt. Wir haben diese Änderungen vorgenommen, um die Komplexität bei der Planung von Versionsaktualisierungen und der Arbeit mit Community-Beiträgen zu reduzieren.

- ![neues Symbol](../../assets/new.svg) **Änderungen am ECE-Tools-Paket**

   - ![neues Symbol](../../assets/new.svg) Die Adobe Commerce-Patches wurden aus dem Paket `ece-tools` in ein neues Paket [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) Composer verschoben.

   - ![neues Symbol](../../assets/new.svg) Die `composer.json` für das Paket `ece-tools` wurde aktualisiert, um eine Abhängigkeit für das Paket `magento/magento-cloud-patches` v1.0.0 hinzuzufügen.

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass der `ece-tools`-Patch-Prozess beim Anwenden von Patch-Sets auf reinen Sicherheitsversionen, beginnend mit Version 2.3.2-p2 und höher, fehlschlug. Dieses Problem wurde durch das neue Versionierungsschema eingeführt, das für [Nur-Sicherheits-Patches](https://experienceleague.adobe.com/de/docs/commerce-operations/release/notes/security-patches/overview) übernommen wurde<!--MAGECLOUD-4661-->

- ![Fix-Symbol](../../assets/fix.svg) **Patches und kritische**: Aktualisieren Sie Ihre Cloud-Umgebungen mit `ece-tools` Version 2002.0.22, um die folgenden Patches und kritischen Fehlerbehebungen anzuwenden. Diese Patches sind im Paket `magento/magento-cloud-patches` v1.0.0 enthalten.

   - ![Fix icon](../../assets/fix.svg) **Page Builder-Sicherheits-Patches für die Versionen 2.3.1.x und 2.3.2.x**-Behebt ein Problem in der Page Builder-Vorschau, das es nicht authentifizierten Benutzenden ermöglicht, auf einige Vorlagenmethoden zuzugreifen, die zum Trigger der beliebigen Codeausführung über das Netzwerk (RCE) verwendet werden können, was zu globalen Informationslecks führt. Dieses Problem kann auftreten, wenn nicht unterstützte Versionen von Page Builder mit Adobe Commerce 2.3.1 und 2.3.2.<!--MAGECLOUD-4649--> verwendet werden

   - ![Fix icon](../../assets/fix.svg) **MSI patches**-Fixes: Behebt Probleme, die bei der Verwendung von standardmäßigen Inventareinstellungen zur Bestandsverwaltung zu Indexfehlern und Leistungsproblemen führten.<!--MAGECLOUD-4428-->

   - ![Fix icon](../../assets/fix.svg) **Abwärtskompatibilität neuer Mail-Schnittstellen**-Behebt ein Abwärtsinkompatibilitätsproblem, das durch die in Adobe Commerce v2.3.3 eingeführte `Magento\Framework\Mail\EmailMessageInterface` PHP-Schnittstelle verursacht wird. Im Rahmen dieses Patches erbt die neue `EmailMessageInterface` von der alten `MessageInterface`, und die Adobe Commerce-Kernmodule werden so zurückgesetzt, dass sie von `MessageInterface` abhängen.<!--MAGECLOUD-4422-->

   - ![Fix icon](../../assets/fix.svg) **Katalogpaginierung funktioniert nicht in Elasticsearch 6.x**-Behebt ein kritisches Problem mit der Suchergebnisseitenbildung, das Kunden betrifft, die Elasticsearch 6.x als Katalogsuchmaschine verwenden.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![neues Symbol](../../assets/new.svg) **Docker-Updates**—

   - ![neues Symbol](../../assets/new.svg) **Neue Docker-** - Unterstützt von Version 2.3.3 und höher<!-- MAGECLOUD-3345 -->

      - PHP-Version 7.3.<!-- MAGECLOUD-4017 -->

      - Lackcache 6.2.0<!-- MAGECLOUD-4017 -->

   - ![neues Symbol](../../assets/new.svg) Es wurde Unterstützung zum Anwenden einer benutzerdefinierten Hook-Konfiguration hinzugefügt, die in `.magento.app.yaml` in der Docker-Umgebung angegeben ist. Zuvor unterstützte die Docker-Umgebung nur die standardmäßige Hook-Konfiguration.<!-- MAGECLOUD-3505-->

   - ![neues Symbol](../../assets/new.svg) Docker-ENV-Dateien werden während des Docker-Builds nicht mehr generiert, und der `docker:config:convert`-Befehl wird nicht mehr unterstützt. Die entsprechenden Daten werden jetzt in der `docker-compose.yml` gespeichert.<!-- MAGECLOUD-3816-->

   - ![neues Symbol](../../assets/new.svg) **Aktualisiertes PHP-Bild**-Node.js wurde zum PHP Docker-Image hinzugefügt, um Node-, npm- und grunt-cli-Funktionen zu unterstützen.<!-- MAGECLOUD-3953 -->

- ![neues Symbol](../../assets/new.svg) **Aktualisierungen von Umgebungsvariablen**-

   - ![neues Symbol](../../assets/new.svg) Die Bereitstellungsvariable **LOCK_PROVIDER** wurde hinzugefügt, um den Sperranbieter zu konfigurieren, der den Start doppelter Cron-Aufträge und Cron-Gruppen verhindert. Siehe die Variablenbeschreibung im Thema [Variablen bereitstellen](../environment/variables-deploy.md#lock_provider).<!-- MAGECLOUD-4052 -->

   - ![neues Symbol](../../assets/new.svg) Die Umgebungsvariable **CONSUMERS_WAIT_FOR_MAX_MESSAGES** wurde hinzugefügt, um zu konfigurieren, wie Verbraucher Nachrichten aus der Nachrichtenwarteschlange verarbeiten, wenn sie die Umgebungsvariable `CRON_CONSUMERS_RUNNER` zum Verwalten von Cron-Aufträgen verwenden. Siehe die Variablenbeschreibung im Thema [Variablen bereitstellen](../environment/variables-deploy.md#consumers_wait_for_max_messages).<!-- MAGECLOUD-4071 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu Datenbankblockierungsfehlern führen kann, wenn der `consumers_runner` Cron-Auftrag mehrere Instanzen desselben Verbrauchers auf verschiedenen Knoten startet. Wenn Sie nun die Variable [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) in Ihrer Umgebung aktiviert haben, verwendet der `consumers_runner` die Option `single-thread` , um eine Instanz jedes Consumers nur auf einem Knoten zu starten.<!-- MAGECLOUD-3913 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das die [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)-Funktion beeinträchtigte, welche eine standardmäßige Store-URL verwendet. Wenn der `config:show:default-url`-Befehl jetzt keine Basis-URL abrufen kann, wird die URL aus der Variablen MAGENTO_CLOUD_ROUTES verwendet.<!-- MAGECLOUD-3866 -->

- ![neues Symbol](../../assets/new.svg) Die vom `module:refresh`-Befehl zurückgegebenen Protokollierungsinformationen wurden aktualisiert. Jetzt können Sie eine detaillierte Liste der aktivierten Module in der `cloud.log`-Datei sehen.<!-- MAGECLOUD-2514 -->

- ![neues Symbol](../../assets/new.svg) Verbesserte Versionskompatibilitätsvalidierung und Warnbenachrichtigungen bei Kompatibilitätsproblemen zwischen Adobe Commerce-Version und installierten Services wie Elasticsearch, [!DNL RabbitMQ], Redis und DB.<!-- MAGECLOUD-3535 -->

- ![neues Symbol](../../assets/new.svg) Unterstützung für RabitMQ Version 3.8.<!-- MAGECLOUD-4674--> hinzugefügt

- ![neues Symbol](../../assets/new.svg) Interaktive Validierungen für die Service-Kompatibilität wurden aktualisiert, um unterstützte Versionen für die neuen Versionen Adobe Commerce 2.3.3 und 2.2.10 widerzuspiegeln. Siehe [Systemanforderungen](https://experienceleague.adobe.com/de/docs/commerce-operations/installation-guide/system-requirements) im _Installationshandbuch_ für empfohlene Versionen.<!-- MAGECLOUD-4018 -->

- ![Fix-Symbol](../../assets/fix.svg) Die Protokollmeldung, die zurückgegeben wird, wenn der Cron-Auftragsverwaltungsprozess in der Bereitstellungsphase versucht, einen Cron-Auftrag zu stoppen, der bereits abgeschlossen wurde, wurde verbessert, um klarzustellen, dass dieses Problem kein Fehler ist. Protokollebene von `INFO` in `DEBUG` geändert.<!-- MAGECLOUD-3653-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem beim Ausführen des `setup:upgrade`-Befehls behoben, durch den der Bereitstellungsprozess nicht unterbrochen wurde, wenn während der `app:config:import`-Aufgabe ein Fehler auftrat.<!-- MAGECLOUD-3806 -->

- ![neues Symbol](../../assets/new.svg) `debug` Die standardmäßige Protokollebene für den Datei-Handler wurde geändert, um die Anzahl der Details in dem im [!DNL Cloud Console] angezeigten Protokoll zu reduzieren und gleichzeitig detaillierte Informationen für das Debugging bereitzustellen.<!-- MAGECLOUD-3871 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das während des Builds zu einem Fehler bei der statischen Inhaltsbereitstellung führte. Nach einer Installation und `ece-tools` Konfigurations-Dump ist ein Fehler aufgetreten, wenn für den Admin-Benutzer kein Gebietsschema in der `config.php` angegeben war. Jetzt gibt es in der `config.php`-Datei ein Standardgebietsschema für den Admin-Benutzer.<!-- MAGECLOUD-3957 -->

- ![Fehlerbehebung](../../assets/fix.svg) Es wurde ein `Undefined index error` behoben, das auftritt, wenn ein `magento-cloud` CLI-Befehl in einer Umgebung fehlschlägt, die nicht mit einer sicheren URL (https) konfiguriert ist. Jetzt verwendet das Paket ECE-Tools die Basis-URL (http), wenn die sichere URL nicht verfügbar ist.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![neues Symbol](../../assets/new.svg) **Docker-Updates**—

   - ![neues Symbol](../../assets/new.svg) Sie können jetzt Funktionstests mit dem `ece-tools`-Paket in der Docker-Umgebung durchführen. Siehe [Anwendungstests](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/).<!-- MAGECLOUD-3129/3684 -->

   - ![neues Symbol](../../assets/new.svg) Es wurde Unterstützung für die Konfiguration von PHP-Modulen mithilfe der `.magento.app.yaml` hinzugefügt. Alle [PHP-Erweiterungen, die in der `.magento.app.yaml`-Datei angegeben sind](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions) werden in den Docker-PHP-Containern verfügbar.<!-- MAGECLOUD-3357 -->

   - ![neues Symbol](../../assets/new.svg) Es sind neue Befehle verfügbar, um das Docker-Befehlszeilenerlebnis zu verbessern. Siehe den [`bin/magento-docker` Abschnitt der Docker-Referenz](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![neues Symbol](../../assets/new.svg) Es wurde die Möglichkeit hinzugefügt, Mutagen.io zu verwenden, um Dateien während der Entwicklung zwischen dem lokalen Host und Docker zu synchronisieren.<!-- MAGECLOUD-3559 -->

   - ![Fix-Symbol](../../assets/fix.svg) Der Standardpfad bei Verwendung der Docker-Umgebung wurde korrigiert. Wenn Sie jetzt SSH verwenden, um sich beim Docker-Container anzumelden, befinden Sie sich erwartungsgemäß am Projektstamm im `/app`.<!-- MAGECLOUD-3582 -->

   - ![Fix icon](../../assets/fix.svg) Die Natriumbibliothek wurde von Version 1.0.11 auf Version 1.0.18 aktualisiert und die PHP-Erweiterung wurde aktualisiert.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Kunden von Adobe Commerce auf Cloud-Infrastrukturen müssen [ein Adobe Commerce-Support-Ticket einreichen](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket), um das libnatrium-Paket in Pro-Produktions- und Staging-Umgebungen zu aktualisieren, bevor sie ein Upgrade auf Adobe Commerce 2.3.2 durchführen. Derzeit ist kein Upgrade von Starter-Umgebungen auf Adobe Commerce 2.3.2 möglich.

   - ![Fix-Symbol](../../assets/fix.svg) Die Plug-ins `analysis-icu` und `analysis-phonetic` Elasticsearch wurden zu allen Docker-Images hinzugefügt.<!-- MAGECLOUD-3446 -->

   - ![Fix-Symbol](../../assets/fix.svg) Verbesserte Validierungen: Bei Verwendung von Optionen für den `docker:build`-Befehl müssen Sie bei Verwendung einer Option einen Wert angeben. Außerdem wurde bei Verwendung des `docker:build run`-Befehls eine Validierung für die Knotenversion hinzugefügt.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![neues Symbol](../../assets/new.svg) **Aktualisierungen von Umgebungsvariablen**—

   - ![neues Symbol](../../assets/new.svg) Es wurde Unterstützung für Datenbanktabellen-Präfixe mithilfe der Umgebungsvariablen [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) hinzugefügt.<!-- MAGECLOUD-2901 -->

   - ![neues Symbol](../../assets/new.svg) Die Variable **FORCE_UPDATE_URLS** deploy wurde hinzugefügt, um Basis-URLs bei der Bereitstellung in Pro und Starter Produktions- und Staging-Umgebungen zu aktualisieren. Siehe die Definition im Inhalt [Variablen bereitstellen](../environment/variables-deploy.md#force_update_urls).<!-- MAGECLOUD-3602 -->

   - ![neues Symbol](../../assets/new.svg) Nach der Bereitstellung wurde die Variable **TTFB_TESTED_PAGES** hinzugefügt, um _Time to First Byte_-Seitentests zu konfigurieren, um die Anwendungsleistung auf Sites zu überprüfen, die in der Cloud-Infrastruktur bereitgestellt sind. Siehe die Variablenbeschreibung in [Variablen nach der Bereitstellung](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit Multi-Thread-SCD behoben, das zu zufälligen Fehlern bei der Bereitstellung statischer Inhalte führte. Als Problemumgehung mussten Sie die Variable **SCD_THREADS** auf `1` setzen. Sie können die Anzahl jetzt nach Bedarf erhöhen. Siehe die Definitionen in [Bereitstellungsvariablen](../environment/variables-deploy.md#scd_threads) und [Build-Variablen](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![Fix-Symbol](../../assets/fix.svg) Sie können die Umgebungsvariable **WARM_UP_PAGES** so konfigurieren, dass einzelne Seiten, mehrere Domains und mehrere Seiten zwischengespeichert werden. Siehe die erweiterte Definition im Inhalt [Variablen nach der Bereitstellung](../environment/variables-post-deploy.md#warm_up_pages).<!-- MAGECLOUD-3258 -->

- ![Fix-Symbol](../../assets/fix.svg) Die `pub/static/.htaccess`-Datei wurde zur Ausschlussliste hinzugefügt. [Fehlerbehebung eingereicht von Björn Kraus von PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Fehler behoben, bei dem alle Validierungsmeldungen als `Critical` angezeigt wurden, wenn mindestens ein Validator der kritischen Ebene einen Fehler zurückgab.<!-- MAGECLOUD-3178 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu einem Bereitstellungsfehler führte, wenn die Basis-URL nicht in der Datenbank vorhanden war.<!-- MAGECLOUD-3075 -->

- ![neues Symbol](../../assets/new.svg) Ein neuer **`env:config:show`-Befehl** zum `ece-tools`-Paket hinzugefügt, das Umgebungsdienste, Routen oder Variablen anzeigt. Siehe [Services, Routen und Variablen](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables). [Funktion eingereicht von Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu einem kritischen Fehler beim Versuch führte, Adobe Commerce 2.2.6 oder früher mit `ece-tools` Developer nach der Shell-Umgestaltung zu installieren.<!-- MAGECLOUD-3665 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass Adobe Commerce 2.1.x- und 2.2.x-Installationen fehlschlugen, wobei ein Warnhinweis zur Verwendung einer veralteten Version von Carbon ausgegeben wurde.<!-- MAGECLOUD-3704 -->

- ![Fix-Symbol](../../assets/fix.svg) Die `cloud.log` Protokollebene für Shell-Ausgaben wurde von `info` auf `debug` verringert.<!-- MAGECLOUD-3277 -->

- ![Fix-Symbol](../../assets/fix.svg) Dem `ece-tools db-dump`-Befehl wurde die Option `--remove-definers (-d)` hinzugefügt, um Definieren aus der Dump-Datei zu entfernen.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, durch das die `env.php`-Datei während einer Bereitstellung überschrieben wurde, was zum Verlust benutzerdefinierter Konfigurationen führte. Diese Aktualisierung stellt sicher, dass Adobe Commerce in der Cloud-Infrastruktur die `env.php` bei jeder Bereitstellung aktualisiert, während benutzerdefinierte Konfigurationen beibehalten werden.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![neues Symbol](../../assets/new.svg) **Docker-Updates**—

   - ![neues Symbol](../../assets/new.svg) Jetzt unterstützt die Docker-Umgebung die Cron-Konfiguration, die in der [crons-Eigenschaft der Datei &quot;.magento.app.yaml“ definiert ](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property). <!-- MAGECLOUD-3150 -->

   - ![neues Symbol](../../assets/new.svg) **Neuer Docker-Container** - Ein [TLS-Terminations-Proxy-Container wurde hinzugefügt](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) um die SSL-Beendigung von Varnish über HTTPS zu erleichtern.<!-- MAGECLOUD-2890 -->

   - ![neues Symbol](../../assets/new.svg) **Neues Docker-** - Ein Node.js-Bild wurde hinzugefügt, um Gulp und andere Funktionen wie Jasmine JS Unit Testing zu unterstützen.<!-- MAGECLOUD-3345 -->

   - ![neues Symbol](../../assets/new.svg) **Docker-Build-**: Jetzt können Sie die Docker-Umgebung im [Produktions- oder Entwicklermodus) ](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode). Der Entwicklermodus unterstützt die aktive Entwicklung mit vollständigen, schreibbaren Dateisystemberechtigungen.<!-- MAGECLOUD-3152/3511 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass die Docker-Bereitstellung mit einem `Name or service not known` Fehler fehlschlug, wenn der Cache für einen Service konfiguriert ist, der nicht verfügbar ist. Jetzt können Sie einen Dienst aus der [`.magento/services.yaml`-Datei entfernen](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/service/services-yaml). Der Docker-Konfigurations-Generator aktualisiert den Service in der `docker/config.php.dist`-Datei automatisch.<!-- MAGECLOUD-3369 -->

   - ![neues Symbol](../../assets/new.svg) Interaktive Validierungen für die Service-Kompatibilität hinzugefügt. Wenn ein angeforderter Dienst jetzt mit der Adobe Commerce-Version oder anderen Diensten inkompatibel ist, fordert der _interaktive Modus_ den Benutzer mit einer Meldung und einer Auswahl auf, fortzufahren. Siehe die [Service-Versionen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers), die für Docker verfügbar sind. Verwenden Sie die Option `-n` , um die Interaktivität für CICD-Zwecke zu überspringen.<!-- MAGECLOUD-3251 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit dem Befehl Docker compose `db-dump` behoben, durch das vorhandene Dumps gelöscht wurden.<!-- MAGECLOUD-3366 -->

   - ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, bei dem Redis-`session`, -`default` und -`page_cache` Cache-Speicher derselben Datenbank-ID zugewiesen wurden.<!-- MAGECLOUD-3172 -->

- ![neues Symbol](../../assets/new.svg) **Aktualisierungen von Umgebungsvariablen**—

   - ![neues Symbol](../../assets/new.svg) Die neue Umgebungsvariable **ELASTICSUITE\_CONFIGURATION** behält Ihre benutzerdefinierten Service-Einstellungen zwischen Bereitstellungen bei. Siehe die Definition im Inhalt [Variablen bereitstellen](../environment/variables-deploy.md#elasticsuite_configuration).<!-- MAGECLOUD-3205 -->

   - ![neues Symbol](../../assets/new.svg) Die Umgebungsvariable **SCD_MAX_EXECUTION_TIMEOUT** wurde hinzugefügt, damit Sie die Zeit bis zum Abschluss der statischen Inhaltsbereitstellung aus der `.magento.env.yaml` erhöhen können. Siehe die Definition in [Variablen bereitstellen](../environment/variables-deploy.md#scd_max_execution_time) den [Variablen erstellen](../environment/variables-build.md#scd_max_execution_time) und den [globalen Variablen](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![neues Symbol](../../assets/new.svg) Die Umgebungsvariable **MAGENTO_CLOUD_LOCKS_DIR** wurde hinzugefügt, um den Pfad zum Bereitstellungspunkt für den Sperranbieter in der Cloud-Infrastruktur zu konfigurieren. Der Sperranbieter verhindert den Start doppelter Cron-Aufträge und Cron-Gruppen. Diese Variable wird ab Adobe Commerce-Version 2.2.5 unterstützt und automatisch konfiguriert. Siehe die Definition in [Cloud-Variablen](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![Fehlerbehebung](../../assets/fix.svg) Die Standardwerte der Umgebungsvariablen **SCD_THREADS** wurden geändert, um den optimalen Wert automatisch basierend auf der erkannten CPU-Thread-Anzahl zu bestimmen. Siehe die aktualisierten Definitionen in den [Bereitstellungsvariablen](../environment/variables-deploy.md#scd_threads) und [Build-Variablen](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![Fehlerbehebungssymbol](../../assets/fix.svg) Es wurde ein Problem mit einem Patch für den DB-Isolationsmechanismus behoben, das beim Upgrade auf Adobe Commerce in der Cloud-Infrastrukturversion 2002.0.16 einen Fehler verursachte.<!-- MAGECLOUD-3383 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Patch hinzugefügt, der _Google-_ durch &quot;_&quot;_. Siehe den DevBlog-Artikel [Einstellung und Aktualisierung von Google-Bilddiagrammen für M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde eine Validierung für die Variable [SEARCH_CONFIGURATION“ ](../environment/variables-deploy.md#search_configuration). Die Bereitstellung schlägt fehl, wenn die Option „Engine“ nicht festgelegt ist und `_merge` nicht erforderlich ist.<!-- MAGECLOUD-3470 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, durch das vertrauliche Daten nach einem Ausnahmefehler angezeigt wurden. Jetzt werden die sensiblen Informationen entsprechend maskiert.<!-- MAGECLOUD-3525 -->

- ![fix icon](../../assets/fix.svg) Die Fehlertoleranzeinstellungen des Magento Open Source-Packages wurden verbessert. Wenn Adobe Commerce keine Daten aus der Redis-`slave` lesen kann, wird ein Lesevorgang aus der Redis-`master` durchgeführt. Siehe [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>Die `ece-tools` Version 2002.0.17 enthält einen wichtigen Sicherheitspatch. Siehe [Technische Ressourcen: Magento Open Source Patches](https://magento.com/tech-resources/download#download2288).

- ![neues Symbol](../../assets/new.svg) **Service-Updates** - Wird von den folgenden Adobe Commerce-Versionen unterstützt: 2.2.8 und höher 2.2.x, 2.3.1 und höher 2.3.x

   - Unterstützung für Elasticsearch-Version 6.x.<!-- MAGECLOUD-3196 --> wurde hinzugefügt

   - Redis Version 5.0 wird nun unterstützt.

- ![neues Symbol](../../assets/new.svg) **Neue Docker-Images** - Folgende Services wurden zum Docker-Build hinzugefügt:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![neues Symbol](../../assets/new.svg) **Neue Umgebungsvariable** - Zuvor gab es ein hartcodiertes Timeout für die SCD-Komprimierung. Jetzt können Sie das SCD-Komprimierungs-Timeout mithilfe der **SCD_COMPRESSION_TIMEOUT**-Umgebungsvariable konfigurieren. Siehe die Definitionen im [Build-Variablen](../environment/variables-build.md#scd_compression_timeout) und im [Deploy-Variablen](../environment/variables-deploy.md#scd_compression_timeout)-Inhalt.<!-- MAGECLOUD-2870 -->

- ![Fix-Symbol](../../assets/fix.svg) Dem Installationsbefehl wurde die Option &quot;`--use-rewrites`&quot; hinzugefügt, damit Webserver-Neuschreibungen für generierte Links in der Storefront und Administratorzugriff verwendet werden, um die Sicherheit und das Kundenerlebnis zu verbessern.<!-- MAGECLOUD-3246 -->

- ![Fix-Symbol](../../assets/fix.svg) Der `var/log/install_upgrade.log`-Datei wurden Zeitstempel hinzugefügt, damit sie die Installations- und Aktualisierungsereignisse anzeigt.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![neues Symbol](../../assets/new.svg) **Docker-Updates**—

   - Jetzt ist die in der Docker-Umgebung generierte Standard-Service-Konfiguration mit der Standardkonfiguration in der Cloud-Vorlage identisch.<!-- MAGECLOUD-3025 -->

   - Sie können E-Mails aus Ihrer Docker-Umgebung mithilfe des `sendmail`-Services senden.<!-- MAGECLOUD-2907 -->

   - Es wurde die Möglichkeit hinzugefügt[ „Xdebug konfigurieren](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) um in der Cloud Docker-Umgebung zu debuggen.<!-- MAGECLOUD-2891 -->

   - Fehlerkorrektur - Beim Generieren der `docker-compose.yml`-Datei tritt jetzt kein Fehler mehr mit Webservice-Berechtigungen auf.<!-- MAGECLOUD-2883 -->

- ![neues Symbol](../../assets/new.svg) **Upgrade-Verbesserung** - Es wurde eine Validierung hinzugefügt, um zu bestätigen, dass die `autoload`-Eigenschaft in der `composer.json`-Datei die erforderlichen Konfigurationsänderungen enthält, bevor ein Upgrade auf Adobe Commerce v2.3 durchgeführt wird. Siehe [Upgrade-Version](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 -->

- ![neues Symbol](../../assets/new.svg) Der Komprimierungsprozess bei der Bereitstellung statischer Inhalte umfasst jetzt alle Assets - nativ generiert oder angepasst - und findet während der Erstellungsphase am Anfang des [`build:transfer` Abschnitts statt](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property). Zuvor wurde der Komprimierungsprozess ausgeführt, bevor eine benutzerdefinierte Minimierung und Bündelung statischer Assets angewendet wurde. [Fehlerbehebung eingereicht von Rafael Garcia Lepper von Tryzens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Datenbankverbindungsfehler behoben, der während der Bereitstellung unmittelbar nach der Konfiguration einer zusätzlichen Datenbank- und Dienstbeziehung aufgetreten war. Außerdem behebt diese Fehlerbehebung ein Problem, das während des Konfigurationsprozesses von Commerce Reporting for Starter aufgetreten ist. Zunächst einmal ist dieses Upgrade ein „Muss“ für die Verwendung von Commerce-Berichten.<!-- MAGECLOUD-3035 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Validierungsproblem mit der Datenbankkonfiguration behoben, das zum Fehlschlagen des Bereitstellungsprozesses führte.<!-- MAGECLOUD-3003 -->

- ![fix icon](../../assets/fix.svg) Die Einschränkung wurde mit der entsprechenden Version des `symfony/yaml`-Pakets aktualisiert, das mit [PHP-Konstanten](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) verwendet werden soll. Das konstante Parsen funktioniert nicht, wenn eine `symfony/yaml` Paketversion vor 3.2 verwendet wird. [Fehlerbehebung eingereicht von Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![neues Symbol](../../assets/new.svg) **Umgebungs-Konfigurationsprüfung**—Es wurde eine Validierung hinzugefügt, um die PHP-Version zu überprüfen und Benutzer zu warnen, wenn sie nicht die neueste empfohlene Version verwenden.<!--MAGECLOUD-2903-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem bei der Verarbeitung falsch formatierter JSON-Variablen behoben. Wenn nun eine JSON-Variable einen Syntaxfehler verursacht, wird in der `cloud.log`-Datei eine Warnung angezeigt und die Bereitstellung wird unter Verwendung der Standardvariablen fortgesetzt.<!-- MAGECLOUD-2851 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Verbindungsfehler behoben, der während der Bereitstellung unmittelbar nach der Deaktivierung des Redis-Service auftrat.<!-- MAGECLOUD-2747 -->

- ![neues Symbol](../../assets/new.svg) **Protokollierungsänderungen** - Die [Protokollebene](../environment/log-handlers.md#log-levels) wurde für die folgenden Build- und Bereitstellungsprozess-Ereignisse von `Info` auf `Notice` aktualisiert:<!--MAGECLOUD-2925-->

   - Beginn und Ende des Prozesses zur Abstimmung der installierten Module in `composer.json` mit den freigegebenen Konfigurationseinstellungen in der `app/etc/config.php`

   - Beginn und Ende des Validierungsprozesses der Konfiguration

   - Beginn und Ende des `setup:di:compile` zum Generieren von Klassen

- ![neues Symbol](../../assets/new.svg) **Neue Umgebungsvariablen**—

   - **[RESOURCE_CONFIGURATION](../environment/variables-deploy.md#resource_configuration)** Bereitstellungsvariable: Verwenden Sie diese Variable, um einer Datenbankverbindung einen Ressourcennamen zuzuordnen.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION global variable](../environment/variables-global.md#x_frame_configuration)** - Verwenden Sie diese Variable, um die `X-Frame-Options` Header-Konfiguration zum Rendern einer Adobe Commerce-Seite in einem `<frame>`, einem `<iframe>` oder einer `<object>` zu ändern.<!-- MAGECLOUD-3048 -->

- ![fix icon](../../assets/fix.svg) **Umgebungsvariablenaktualisierungen** - Die folgenden Umgebungsvariablen wurden geändert:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** - Es wurde die Möglichkeit hinzugefügt, den Cache für bestimmte Seiten auf allen Domains, die für einen Adobe Commerce-Store definiert sind, vorab zu laden. Wenn Ihre Site zuvor mit mehreren Domains konfiguriert war, konnte der Cache für die angegebenen Seiten in nicht standardmäßigen Domains nicht vorab vom Post-Deploy-Prozess geladen werden und der folgende Fehler wurde im Post-Deploy-Protokoll zurückgegeben: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** - Die Dokumentation und die `.magento.env.yaml`-Beispieldatei wurden mit den richtigen Standardwerten für die SCD-Komprimierungsstufe aktualisiert. Siehe die Definitionen im [Build-Variablen](../environment/variables-build.md#scd_compression_level) und im [Deploy-Variablen](../environment/variables-deploy.md#scd_compression_level)-Inhalt.<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** - Diese Umgebungsvariable wird nicht mehr unterstützt. Verwenden Sie die [SCD_MATRIX](../environment/variables-build.md#scd_matrix), um die Design-Konfiguration zu steuern.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** - Der Validierungsprozess wurde korrigiert, um ein Problem zu vermeiden, das auftrat, wenn die SCD_MATRIX einen Design-Wert ignorierte, der verschiedene Zeichenfälle enthielt. Siehe die Definitionen im [Build-Variablen](../environment/variables-build.md#scd_matrix) und im [Deploy-Variablen](../environment/variables-deploy.md#scd_matrix)-Inhalt.<!-- MAGECLOUD-2904 -->

   - **ADMIN-Variablen**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - Die Sicherheit bei der Verwaltung von Anmeldeinformationen für Admin-Benutzende mithilfe von Umgebungsvariablen wurde verbessert. Sie können die Umgebungsvariablen ADMIN_EMAIL, ADMIN_USERNAME und ADMIN_PASSWORD nicht mehr verwenden, um Admin-Anmeldeinformationen während Upgrades zu überschreiben. Wenn Sie nicht auf das Admin-Bedienfeld zugreifen können, verwenden Sie die Funktion _Kennwort vergessen_ oder den `admin:user:create` CLI-Befehl, um einen neuen Admin-Benutzer zu erstellen. Siehe [Zugriff auf Ihr Admin-Bedienfeld](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/start/onboarding#admin).

      - ADMIN_EMAIL ist beim Upgrade oder Anwenden von Patches nicht mehr erforderlich.

## v2002.0.15

- ![neues Symbol](../../assets/new.svg) **Docker-Updates**—

   - Jetzt verwendet der Docker-Generator die in den `.magento.app.yaml`- und `.magento/services.yaml`-Konfigurationsdateien angegebenen Services beim Erstellen [ Docker-Umgebung](https://developer.adobe.com/commerce/cloud-tools/docker/configure/). Sie können mithilfe von Build-Parametern eine andere Service-Version auswählen.<!-- MAGECLOUD-2888 -->

   - PHP 7.2-Image hinzugefügt - Unterstützung für PHP 7.2 in Cloud Docker hinzugefügt; [Launch-Docker-Konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) wurde aktualisiert, um die `docker:build --php`-Option aufzunehmen, die die mit Ihrer Adobe Commerce-Version kompatible PHP-Version angibt.<!-- MAGECLOUD-2799 -->

   - Ein [Cron-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli/#cron-container) basierend auf dem PHP-CLI-Image wurde hinzugefügt.<!-- MAGECLOUD-2565 -->

   - Folgende Services wurden zum Docker-Build hinzugefügt:

      - [!DNL RabbitMQ] 3.5 und 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 und 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 und 4.0<!-- MAGECLOUD-2886 -->

- ![neues Symbol](../../assets/new.svg) **Mit PHP-Konstanten konfigurieren**—Unterstützung für [PHP-Konstanten](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) in der `.magento.env.yaml` Konfigurationsdatei hinzugefügt.<!-- MAGECLOUD- 2575 -->

- ![neues Symbol](../../assets/new.svg) **neue Umgebungsvariable** - Standardmäßig ist nur in der Produktionsumgebung Google Analytics aktiviert. Sie können Google Analytics in den Staging- und Integrationsumgebungen mithilfe der Umgebungsvariablen [ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics) aktivieren<!--MAGECLOUD-2879-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, durch das benutzerdefinierte Cron-Konfigurationen nach einer erneuten Bereitstellung aus der `env.php`-Datei entfernt wurden. Jetzt bleiben benutzerdefinierte Cron-Konfigurationen sicher in der `env.php`-Datei.<!-- MAGECLOUD-2923 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurden Inkonsistenzen in den Meldungen und [Protokollebenen](../environment/log-handlers.md#log-levels) für die Build-, Bereitstellungs- und Post-Bereitstellungsphase behoben. Erhöhte Anfang und Ende der Protokollierungsebenen von **info** bis **notice** für alle Phasen und Unterphasen. Beginn und Ende der Protokollmeldungen wurden hinzugefügt, sofern zutreffend.<!-- MAGECLOUD-2919 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit Cron-Prozessen behoben, das den Start der Phase nach der Bereitstellung verhinderte, wenn es konfiguriert war. Wenn Sie nun den Post-Deploy-Hook aktiviert haben, werden die Cron-Prozesse zu Beginn der Post-Deploy-Phase wieder aktiviert.<!-- MAGECLOUD-2862 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das die erfolgreiche Installation von Adobe Commerce beim Angeben einer benutzerdefinierten Datenbankkonfiguration verhinderte. Zuvor wurde im Installationsprozess die Datenbankkonfiguration aus der Variablen [MAGENTO_CLOUD_RELATIONSHIP verwendet](../environment/variables-cloud.md) auch wenn Sie in der Umgebungsvariablen [DATABASE_CONFIGURATION benutzerdefinierte Verbindungsinformationen angegeben haben](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![Fix-Symbol](../../assets/fix.svg) Der `config:dump`-Befehl wurde korrigiert, sodass er jedes Website-Gebietsschema im `system` Abschnitt der `config.php`-Datei enthält.<!--MAGECLOUD-2740-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu _Warm-up_-Fehlern während der Phase nach der Bereitstellung führte, indem die Referenz der Quell-Basis-URL korrigiert wurde.<!--MAGECLOUD-2797-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, durch das Dateien während des `setup:di:compile`-Prozesses nicht ordnungsgemäß generiert wurden, was sich auf das Amazon Pay-Modul auswirkte.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![neues Symbol](../../assets/new.svg) **Idealer Status überprüfen** - Der `ideal-state`-Assistent überprüft jetzt bei jeder Bereitstellung die aktuelle Konfiguration und liefert klare Anweisungen zum Aktualisieren der Konfiguration, um eine schnellere Bereitstellung ohne Ausfallzeiten zu erzielen.<!--MAGECLOUD-2372-->

- ![Fix icon](../../assets/fix.svg) **PCI Compliance** - Die Messaging-Protokolle für Adobe Commerce in der Cloud-Infrastruktur wurden so aktualisiert, dass bei der Verbindung mit Messaging-Services von Drittanbietern Transport Layer Security (TLS) Version 1.2 erforderlich ist. Wenn Sie einen Nachrichten-Service verwenden, der TLS Version 1.2 nicht unterstützt, müssen Sie Ihren Service aktualisieren. Andernfalls wird die folgende Fehlermeldung angezeigt, wenn Ihre Adobe Commerce-Anwendung versucht, eine Verbindung zum Nachrichtenserver herzustellen, um eine E-Mail zu senden: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![neues Symbol](../../assets/new.svg) **Bereitstellungsverbesserung** - Es wurde eine Validierung hinzugefügt, die Kunden warnt, wenn in einer Staging- oder Produktionsumgebung `dev`-, `debug`- oder `debug_logging` aktiviert sind, um Leistungsprobleme zu vermeiden, die durch übermäßige Protokollierungsaktivitäten verursacht werden.<!--MAGECLOUD-2517-->

- ![Fix-Symbol](../../assets/fix.svg) **Bereitstellungskorrekturen**—

   - Jetzt ist der Wartungsmodus zu Beginn der Bereitstellungsphase aktiviert und am Ende deaktiviert. Wenn die Bereitstellung fehlschlägt, bleibt die Site im Wartungsmodus, bis die Bereitstellungsprobleme behoben sind. Zuvor wurde die Site auch dann in den Produktionsmodus zurückversetzt, wenn die Bereitstellung fehlgeschlagen war.<!--MAGECLOUD-2603-->

      - Die Validierungsprüfungen der Bereitstellungsphase wurden überarbeitet, um die Fehlerebene für die folgenden Bereitstellungsprobleme von `CRITICAL` auf `WARNING` herabzustufen, sodass die Bereitstellung abgeschlossen ist. Zuvor schlug die Bereitstellung aufgrund dieser Probleme fehl.

      - Die Umgebungskonfiguration enthält falsche Werte für Bereitstellungs- oder Cloud-Variablen.

   - Die Elasticsearch-Version in der Cloud-Infrastruktur ist nicht kompatibel mit der Version des Elasticsearch/Elasticsearch-Moduls, das von Adobe Commerce in der Cloud-Infrastruktur unterstützt wird. Siehe den Artikel zur Fehlerbehebung bei [Elasticsearch ](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) in der Knowledgebase für den Adobe Commerce-Support.<!--MAGECLOUD-2600-->

   - Fehlerkorrektur - Bei den freigegebenen Konfigurationseinstellungen in der `app/etc/config.php`-Datei tritt jetzt kein Fehler mehr auf, der zu `recursion detected` während der Bereitstellung führte.<!--MAGECLOUD-2173-->

- ![Fix-Symbol](../../assets/fix.svg) **Cron-bezogene**)—

   - Es wurde ein Cron-Zeitplanungsproblem behoben, das die Ausführung von Aufträgen verhinderte, wenn Sie eine andere Cron-Häufigkeit als den Standardwert (1 Minute) angeben.<!--MAGECLOUD-2602-->

   - Es wurde ein Problem in der Bereitstellungsphase behoben, das dazu führte, dass Cron-Aufträge während der Bereitstellung weiterhin ausgeführt wurden, was zu Datenbanksperren und anderen kritischen Problemen führen konnte. Jetzt werden alle Cron-Aufträge gestoppt, bevor die Bereitstellungsphase beginnt, und nach Abschluss der Bereitstellung neu gestartet.&lt;!—MAGECLOUD—2537—>

   - Fehlerkorrektur - Der Cron-Auftrags-Workflow in Version 2.2.x entsperrt jetzt eingefrorene Cron-Aufträge, damit sie vor dem Beginn der Bereitstellung angehalten werden können. Zuvor führte ein eingefrorener Cron-Auftrag dazu, dass die Bereitstellung blockiert wurde.<!--MAGECLOUD-2501-->

- ![fix icon](../../assets/fix.svg) Das Format der vom `vendor/bin/ece-tools config:dump`-Befehl generierten `config.php`-Datei wurde geändert, sodass eine kurze Array-Syntax und ein Einzug mit vier Leerzeichen verwendet werden, um den Adobe Commerce-Codierungsstandards zu entsprechen.<!--MAGECLOUD-2527-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Bereitstellungsfehler behoben, der auftrat, wenn die `.magento.env.yaml` `{{ base_url }}`- und `{{ unsecure_base_url }}`-Platzhalter für Web-Konfigurationen anstelle der Standard-URL-Konfiguration für ein Adobe Commerce in einem Cloud-Infrastrukturprojekt enthält./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![neues Symbol](../../assets/new.svg) **Bereitstellung ohne Ausfallzeiten aktivieren** Jetzt stellt Adobe Commerce in der Cloud-Infrastruktur Anforderungen mit den erforderlichen Datenbankänderungen während der Bereitstellung in die Warteschlange und wendet die Änderungen an, sobald die Bereitstellung abgeschlossen ist. Anfragen können bis zu 5 Minuten lang aufbewahrt werden, um sicherzustellen, dass keine Sitzungen verloren gehen. Siehe [Optionen zur Bereitstellung statischer Inhalte, um Bereitstellungsausfälle in der Cloud zu reduzieren](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![neues Symbol](../../assets/new.svg) **Docker Compose für Cloud** - Folgende Verbesserungen am [Docker-Setup- und -](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) vorgenommen:

   - Es wurde ein Befehl `docker:config:convert` hinzugefügt, um PHP-Konfigurationsdateien in das Docker-ENV-Format zu konvertieren, um die Umgebungskonfiguration zu vereinfachen. Jetzt kopieren Sie die PHP-Konfigurationsdateien in das Docker-Verzeichnis und konvertieren sie in Docker ENV-Dateien. Siehe [Docker starten](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - Der Installationsprozess von Adobe Commerce in der Cloud-Infrastruktur unterstützt jetzt die Bereitstellung sowohl in schreibgeschützten als auch in schreibgeschützten Dateisystemen, um das Cloud-Dateisystem genauer zu emulieren. Siehe [Konfigurieren von Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).&lt;!—MAGECLOUD—2357—>

   - **Redis-Service-Unterstützung** Ein Redis-Image wurde hinzugefügt, das in einem Docker-Container bereitgestellt und automatisch für Ihre Docker-Installation konfiguriert wird.&lt;!—MAGECLOUD—2442—>

   - Jetzt verfügen Sie über die DB-Dump-Funktion bei Verwendung des Cloud Docker [Datenbank-Containers](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#database-container). Außerdem können Sie [Dateien freigeben](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container) zwischen einem Hostcomputer und einem Container mithilfe des `docker/mnt` Verzeichnisses.<!-- MAGECLOUD-2577 -->

   - **Unterstützung für den Varnish-Service**- Es wurde ein Varnish-Image hinzugefügt, das automatisch in einem Docker-Container bereitgestellt wird. Nach der Bereitstellung können Sie Varnish manuell gemäß den Best Practices von Adobe Commerce konfigurieren. Siehe [Konfigurieren und Verwenden von ](https://experienceleague.adobe.com/de/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish).&lt;!—MAGECLOUD—2358—>

   - Sicherer Site-Zugriff - Es wurde SSL-Unterstützung für den Zugriff auf Ihren Adobe Commerce-Store und Ihr Admin-Panel hinzugefügt.&lt;!—MAGECLOUD—2360—>

- ![Fix icon](../../assets/fix.svg) **Verbesserte Unterstützung von Adobe Commerce auf Cloud-Infrastruktur-Erweiterungen**- Die Mindestanforderung für die Version für das Paket guzzlehttp/guzzle in der Datei &quot;[composer.json“ von Adobe Commerce in der Cloud-Infrastruktur wurde ](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/overview) Version 6.2 heruntergestuft, sodass das Paket `ece-tools` mit weiteren Erweiterungen kompatibel ist.<!--MAGECLOUD-2205-->

- ![neues Symbol](../../assets/new.svg) **Anwenden benutzerdefinierter Änderungen auf Ihr Adobe Commerce-Programm während der Build-Phase** - Die Build-Phase wird in zwei separate Prozesse aufgeteilt, sodass Sie mithilfe von Erweiterungspunkten benutzerdefinierte Änderungen auf den generierten statischen Inhalt anwenden können, bevor Sie das Programm für die Bereitstellung verpacken. Der _build:generate_-Prozess generiert Code, wendet Patches an und generiert statischen Inhalt. Der _build:transfer_-Prozess überträgt den generierten Code und den statischen Inhalt an das endgültige Ziel. Siehe [Anwendungs-Hooks](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![Fix icon](../../assets/fix.svg) **Umgebungs-Konfigurationsprüfungen** - Verbesserte Validierung der Umgebungskonfiguration, um Kundinnen und Kunden vor Versionsinkompatibilitäten und Konfigurationsfehlern zu warnen, bevor Adobe Commerce in der Cloud-Infrastruktur erstellt und bereitgestellt wird.

   - Es wurde eine versionsspezifische Validierung hinzugefügt, um nicht unterstützte oder veraltete Umgebungsvariablen und -werte zu identifizieren.<!--MAGECLOUD-2183-->

   - Es wurde eine Elasticsearch-Kompatibilitätsprüfung hinzugefügt, um Benutzende vor Problemen mit der Elasticsearch-Konfiguration zu warnen. Die Bereitstellung schlägt jetzt fehl, wenn die Elasticsearch Service-Version auf dem Server mit Adobe Commerce inkompatibel ist. Zuvor war die Bereitstellung erfolgreich, selbst wenn die Elasticsearch-Version inkompatibel war, was nach der Site-Bereitstellung zu Produktkatalogproblemen führte.<!--MAGECLOUD-2389-->

     Sie können die Inkompatibilität beheben, indem Sie [ein Support-Ticket](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) senden, um Elasticsearch auf eine kompatible Version zu aktualisieren, oder die Adobe Commerce-Konfiguration ändern, um eine kompatible Version des Elasticsearch-PHP-Clients anzugeben.

      - Aktualisieren Sie Elasticsearch für Adobe Commerce Version 2.1.x auf 2.2.2 auf Version 2.4.

      - Aktualisieren Sie Elasticsearch für Adobe Commerce Version 2.2.3 und höher auf Version 5.2.

      - Wenn Sie Elasticsearch 1.x oder 2.x haben und nicht aktualisieren möchten, aktualisieren Sie die Adobe Commerce Elasticsearch PHP Client-Versionsanforderung in composer.json auf `"elasticsearch/elasticsearch": "~2.0"`.

   - Die Validierung von Umgebungsvariablen wurde verbessert, um Konfigurationseinstellungen zu identifizieren, die während der Build-, Bereitstellungs- und Nachbereitstellungsphase Konflikte verursachen können. Beispielsweise wird während des Installations- und Upgrade-Prozesses eine Warnmeldung angezeigt, wenn die globale Einstellung für die Bereitstellung statischer Inhalte mit den Einstellungen in der Build- oder Bereitstellungsphase in Konflikt steht.<!--MAGECLOUD-2156-->

- ![fix icon](../../assets/fix.svg) **Umgebungsvariablenaktualisierungen** - Die folgenden Umgebungsvariablen wurden geändert:

   - **[SKIP_HTML_MINIFICATION global variable](../environment/variables-global.md#skip_html_minification)** - Der Standardwert wurde in `true` geändert, um die On-Demand-Minimierung von HTML-Inhalten zu aktivieren, wodurch Ausfallzeiten bei der Bereitstellung in Staging- und Produktionsumgebungen minimiert werden. Diese Konfiguration ist für Bereitstellungen ohne Ausfallzeiten erforderlich.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES_deploy](../environment/variables-deploy.md#clean_static_files)** - Es wurde die Möglichkeit hinzugefügt, die bereinigte statische Dateiverarbeitung für statische Inhalte zu verwalten, die während der Build-Phase auf der Grundlage der Umgebungsvariableneinstellung von CLEAN_STATIC_FILES generiert wurden. Zuvor wurden statische Inhaltsdateien, die während der Build-Phase generiert wurden, immer bereinigt.<!--MAGECLOUD-1506-->

- ![Fix icon](../../assets/fix.svg) **Logging** - Folgende Änderungen wurden vorgenommen, um die Protokollmeldungen zu verbessern und die Protokollgröße zu reduzieren:

   - Protokolleinträge für Bereitstellungsfehler enthalten jetzt die Befehlsausgabe aus den Vorgängen, die die Fehler verursachen, selbst wenn in der Umgebungskonfiguration keine Protokollierung auf Debugging-Ebene angegeben ist. Siehe [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - Es wurde eine Protokollierung für Bereitstellungsfehler hinzugefügt, die auftreten, wenn generierte Factories, die für einige Erweiterungen erforderlich sind, nicht korrekt generiert werden können, da sich das Dateisystem in einem schreibgeschützten Zustand befindet.<!--MAGECLOUD-2209-->

   - Die Größe des Bereitstellungsprotokolls wurde verringert und Formatierungsprobleme wurden behoben, die durch Einrichtungsbefehle verursacht wurden, die die interaktive Fortschrittsleiste verwenden.<!--MAGECLOUD-2402-->

   - Beseitigt unnötige Ausführlichkeit und aktualisiert die Prioritätsstufen für einige Protokollanweisungen.<!--MAGECLOUD-2227-->

- ![Fix-Symbol](../../assets/fix.svg) **Cron-spezifische**)—

   - Die standardmäßigen Cron-Auftragskonfigurationseinstellungen für die Verlaufslebensdauer wurden von 3D (4320 Min.) auf 1H (60 Min.) geändert, um Leistungsprobleme und Bereitstellungsfehler zu vermeiden, die auftreten können, wenn die Cron-Warteschlange zu schnell gefüllt wird.<!--MAGECLOUD-2427-->

   - Der Cron-Job-Management-Prozess wurde während der Bereitstellungsphase verbessert, um Datenbanksperren und andere kritische Probleme zu vermeiden. Jetzt werden alle Cron-Aufträge während der Bereitstellungsphase angehalten und nach Abschluss der Bereitstellung neu gestartet.<!--MAGECLOUD-2445-->

   - Es wurde ein Problem mit dem Sperrmechanismus für die Planung von Verbrauchern behoben, die von Cron-Aufträgen in Adobe Commerce-Versionen 2.2.0 und höher gestartet wurden, um zu verhindern, dass Cron-Aufträge doppelte Verbraucher starten.<!--MAGECLOUD-2464-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit dem [statischen Inhaltskomprimierungsprozess](../environment/variables-intro.md) (`gzip`) behoben, das `not overwritten`- und `no such file or directory` beim Referenzieren der komprimierten Datei während des Bereitstellungsprozesses verursachte.<!-- MAGECLOUD-2182-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das verhinderte, dass der `php ./vendor/bin/ece-tools config:dump`-Befehl redundante Abschnitte aus der `config.php`-Datei während des Dump-Prozesses entfernte, wenn das Speichergebietsschema nicht angegeben war. Jetzt können Sie Ihre Konfigurationsdateien einfach zwischen Umgebungen verschieben. Generieren Sie nach dem Update auf `ece-tools` Version 2002.0.13 ältere `config.php` mit dem verbesserten `config:dump`. Siehe [Konfigurationsverwaltung für Store-Einstellungen](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure-store/store-settings).<!--MAGECLOUD-2444-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das während der Bereitstellungsphase zu einem Fehler führte, wenn die Routenkonfiguration in der `.magento/routes.yaml`-Datei von einer [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/)-Domain zu einer `www`-Domain umleitet.<!--MAGECLOUD-2556-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit der Option `_merge` für die [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) behoben, das zu falschen Zusammenführungsergebnissen führte, wenn Sie den Parameter `engine` nicht in die aktualisierte `.magento.env.yaml`-Konfigurationsdatei einschlossen. Jetzt überschreibt der Zusammenführungsvorgang korrekt nur die Werte, die Sie in der aktualisierten `.magento.env.yaml` angeben, ohne dass Sie den `engine` festlegen müssen.<!--MAGECLOUD-2520-->

- ![Fix icon](../../assets/fix.svg) Es wurde ein Redis-Konfigurationsproblem behoben, das fälschlicherweise die Sitzungssperre für Adobe Commerce in Cloud-Infrastrukturversionen 2.2.1 und höher aktiviert hatte, was zu langsamen Leistungseinbußen und Zeitüberschreitungen führen konnte. Jetzt ist die Sitzungssperre standardmäßig deaktiviert. Das Problem wurde durch eine Änderung im Standardverhalten des `disable_locking`-Parameters verursacht, der in Version 1.3.4 des Redis-Sitzungs-Handler-Pakets eingeführt wurde. Siehe [colinmollenhour/php-redis-session-abstract package](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![neues Symbol](../../assets/new.svg) **Docker Compose für Cloud** - Es wurde ein Befehl `docker:build` hinzugefügt, um eine [Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)-Konfiguration aus dem Cloud `ece-tools`-Repository zu generieren.<!-- MAGECLOUD-2250 -->

- ![neues Symbol](../../assets/new.svg) **Gebietsschema ändern** - Jetzt können Sie das Speichergebietsschema ändern, ohne den Konfigurationsprozess exportieren und importieren zu müssen. Während sich die Anwendung in der Produktion befindet und SCD_ON_DEMAND aktiviert ist, sind die Optionen „Store“ und „Admin Locale“ verfügbar.<!-- MAGECLOUD-2019 -->

- ![neues Symbol](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Sitemap und Robots** - Erstellt einen [Workflow](../store/robots-sitemap.md), um eine `robots.txt`-Datei hinzuzufügen und eine `sitemap.xml`-Datei für eine einzelne Domain-Konfiguration zu generieren, ohne dass die Infrastruktur geändert werden muss.

- ![neues Symbol](../../assets/new.svg) **Assistenten** - Es wurden zwei [Assistenten](../deploy/smart-wizards.md) hinzugefügt, die Sie bei der Cloud-Konfiguration unterstützen:<!-- MAGECLOUD-1910 -->

   - `ideal-state` - Konfigurieren Sie den idealen Status für minimale Bereitstellungsausfälle

   - `master-slave` - Konfigurieren des Lastenausgleichs für Datenbank und Redis

- ![neues Symbol](../../assets/new.svg) **Modul aktualisieren** - Ein Cloud-Befehl wurde hinzugefügt - `module:refresh` -, um Module zu aktivieren, die deaktiviert oder nicht explizit aktiviert waren, ähnlich wie dies bei einem Build automatisch geschieht.<!-- MAGECLOUD-1521 -->

- ![neues Symbol](../../assets/new.svg) Es wurde die Möglichkeit hinzugefügt, die Konfiguration für Services mithilfe der Option `_merge` in [CACHE](../environment/variables-deploy.md#cache_configuration)-, [SESSION](../environment/variables-deploy.md#session_configuration)-, [QUEUE](../environment/variables-deploy.md#queue_configuration)- und [SEARCH](../environment/variables-deploy.md#search_configuration)-Konfigurationen zusammenzuführen oder zu überschreiben<!-- MAGECLOUD-2105 -->

- ![neues Symbol](../../assets/new.svg) **Beispieldatei für die Umgebungskonfiguration** - Wir haben dem Paket ECE-Tools eine `.magento.env.yaml` Beispieldatei hinzugefügt, die eine detaillierte Beschreibung und mögliche Werte für jede Umgebungsvariable enthält.<!-- MAGECLOUD-1908 -->

   - Wir haben auch eine tiefe Validierung für die `.magento.env.yaml`-Konfiguration hinzugefügt, die Fehler im Bereitstellungsprozess verhindert, die durch unerwartete Werte verursacht werden. Wenn ein Fehler auftritt, erhalten Sie jetzt eine detaillierte Fehlermeldung, die mit folgendem beginnt: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![neues Symbol](../../assets/new.svg) Die folgenden [**Umgebungsvariablen“**](../environment/variables-intro.md):

   - Jetzt können Sie mit der neuen Umgebungsvariablen [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) mehrere Gebietsschemata für jedes Design definieren, wodurch die Anzahl der bereitzustellenden Design-Dateien reduziert wird.<!-- MAGECLOUD-1501 -->

   - Die Umgebungsvariable [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) wurde hinzugefügt, um die Datenbankverbindungen für die Bereitstellung anzupassen.<!-- MAGECLOUD-2047 -->

   - Die neue Variable [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) überschreibt die minimale Protokollierungsebene für alle Ausgabestreams, ohne Änderungen am Code vorzunehmen.<!-- MAGECLOUD-2129 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu Ausfallzeiten zwischen der Bereitstellungs- und der Nachbereitstellungsphase führte. Die Phase nach der Bereitstellung beginnt nun _sofort_ nachdem die Bereitstellungsphase beendet wurde.

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, durch das die erfolgreichen Cron-Aufträge, nämlich die mit `status = success`, nicht aus dem Zeitplan gelöscht wurden.<!-- MAGECLOUD-2268 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit dem `post_deploy`-Hook behoben, durch den der Cache in der Bereitstellungsphase anstelle der Phase nach der Bereitstellung des Projekts gelöscht wurde.<!-- MAGECLOUD-2113 -->

- ![Fix icon](../../assets/fix.svg) Fehlerkorrektur: Bei der Verwendung von SCD mit mehreren Gebietsschemata wird nicht mehr dieselbe `js-translation.json`-Datei für jedes Gebietsschema generiert.<!-- MAGECLOUD-2034 -->

- ![fix icon](../../assets/fix.svg) Der `db:dump`-Befehl im `ece-tools`-Paket wurde optimiert, um Sperren von Tabellen zu vermeiden und die Geschwindigkeit zu erhöhen.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>Die ECE-Tools Version 2002.0.11 ist für die Kompatibilität mit Version 2.2.4 erforderlich.

- ![neues Symbol](../../assets/new.svg) **Konfigurieren von schreibgeschützten Verbindungen zu Nicht-Master-Knoten** - Diese Version bietet die Möglichkeit, eine schreibgeschützte Verbindung zu einem Nicht-Master-Knoten zu konfigurieren, um schreibgeschützten Traffic zu empfangen (für [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) und für <!--MAGECLOUD-143 -->

- ![neues Symbol](../../assets/new.svg) **Konfigurationsassistent** - Es wurde ein Assistent hinzugefügt, mit dem Sie Ihre Konfiguration für die statische Inhaltsbereitstellung überprüfen können. Siehe [Intelligente Assistenten](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![neues Symbol](../../assets/new.svg) **Unterstützung für Symfony Console**—Unterstützung für Symfony Console 4 mit Adobe Commerce 2.3.<!-- MAGECLOUD-1966 --> hinzugefügt

- ![Fix icon](../../assets/fix.svg) **Cron Scheduling Optimization** - Verbesserte Warteschlangenverwaltung und Protokollierung, um Probleme mit Cron zu debuggen.<!-- MAGECLOUD-1607 -->

- ![Fix-Symbol](../../assets/fix.svg) Die Bereitstellungsvalidierung schlägt fehl, wenn ein `ADMIN_EMAIL`- oder `ADMIN_USERNAME` mit einem vorhandenen Administratorkonto übereinstimmt.<!-- MAGECLOUD-1221 -->

- ![Fix-Symbol](../../assets/fix.svg) SOLR-Unterstützung für Version 2.2.x wurde entfernt. Die Versionen 2.1.x behalten die Möglichkeit, SOLR.<!-- MAGECLOUD-1282 --> zu aktivieren

- ![fix icon](../../assets/fix.svg) Die erste Installation der Staging- und Produktionsumgebungen eines PRO-Projekts enthält jetzt verschiedene Indexpräfixe für das Elasticsearch, um mögliche Konflikte zu vermeiden und dabei die Datensätze zu identifizieren, die zu jeder Umgebung gehören.<!-- MAGECLOUD-1489 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das die Build-Phase für die Legacy-Architektur während der Bereitstellung statischer Inhalte unterbrach.<!-- MAGECLOUD-2021 -->

- ![Fix icon](../../assets/fix.svg) **Cron-spezifische Verbesserungen** - Überarbeitete die Cron-Implementierung:<!-- MAGECLOUD-1607 -->

   - Es wurde ein Problem behoben, das dazu führte, dass die Cron-Warteschlange schnell gefüllt wurde. Jetzt räumt es die veralteten Cron-Jobs auf zuverlässigere Weise.

   - Die Sequenz von Cron-Aufträgen wurde neu organisiert, sodass alle Aufträge in separaten Threads vor der allgemeinen Gruppe gestartet werden.

   - Die Protokollierung wurde verbessert, um Cron-Probleme besser zu debuggen.

   - **HINWEIS** - Diese Version behandelt viele Probleme im Zusammenhang mit Cron. Wenn Sie derzeit einige Cron-bezogene Patches in _m2-Hotfixes verwenden_ entfernen Sie diese.

- ![Fix-Symbol](../../assets/fix.svg) **SCD-spezifische Verbesserungen**—

   - Sie können die `VERBOSE_COMMANDS` und die `SCD_COMPRESSION_LEVEL` Umgebungsvariablen sowohl während der _build_- als auch während der de_ploy-Phase verwenden.<!-- MAGECLOUD-1819 -->

   - Es wurde ein Problem behoben, das dazu führte, dass die Bereitstellung mit einem zufälligen Fehler fehlschlug, wenn ein unerwarteter Wert für die `SCD_COMPRESSION_LEVEL` Umgebungsvariable auftrat. Die Konfigurationsvalidierung wurde verbessert, sodass aussagekräftige Benachrichtigungen bereitgestellt werden können. Siehe [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) für zulässige Werte.<!-- MAGECLOUD-2043 -->

   - Das Verhalten des Konfigurationsflusses der `SCD_COMPRESSION_LEVEL`-Umgebungsvariablen wurde korrigiert, damit die Überschreibungen wie erwartet funktionieren.<!-- MAGECLOUD-2044 -->

   - Es wurde ein Problem behoben, das die Konfiguration der `SCD_THREADS` Umgebungsvariable in der `.magento.env.yaml` Datei _deploy_ stage verhinderte<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![neues Symbol](../../assets/new.svg) **Statische Inhaltsbereitstellung (SCD)** - Es gibt einen neuen, alternativen Bereitstellungsprozess, um statische Inhalte auf Anfrage (auf Anfrage) zu generieren. Dadurch werden Ausfallzeiten reduziert und die Cache-Handhabung durch Generieren der kritischsten Assets verbessert.<!-- MAGECLOUD-1285 -->

   - **Neue Umgebungsvariable** - Die globale Umgebungsvariable `SCD_ON_DEMAND` wurde hinzugefügt, um bei Bedarf statische Inhalte zu generieren.<!-- MAGECLOUD-1738 -->

   - **Hook nach der Bereitstellung** - Es wurde ein `post_deploy` Hook für die `.magento.app.yaml` hinzugefügt, der den Cache löscht und den Cache vorlädt (erwärmt), _nachdem_ Container beginnt, Verbindungen zu akzeptieren. Sie ist nur für Pro-Projekte verfügbar, die Staging- und Produktionsumgebungen im [!DNL Cloud Console] enthalten, sowie für Starter-Projekte. Dies ist zwar nicht erforderlich, funktioniert aber zusammen mit der Umgebungsvariablen `SCD_ON_DEMAND`.<!-- MAGECLOUD-1788 -->

- ![neues Symbol](../../assets/new.svg) **Optimierung** - Optimiertes Verschieben oder Kopieren von Dateien während der Bereitstellung, um die Bereitstellungsgeschwindigkeit zu verbessern und die Belastung des Dateisystems zu verringern.<!-- MAGECLOUD-1842 -->

- ![neues Symbol](../../assets/new.svg) **Bereitstellungsprotokollierung** Es wurde die Möglichkeit hinzugefügt, die Handler Syslog und Graylog Extended Log Format (GELF) für die Ausgabe von Protokollen während des Bereitstellungsprozesses zu aktivieren. Siehe [Protokollierungs-Handler](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![neues Symbol](../../assets/new.svg) Die folgenden [**Umgebungsvariablen“**](../environment/variables-intro.md):

   - `CRYPT_KEY` - Stellen Sie beim Verschieben einer Datenbank einen kryptografischen Schlüssel für eine andere Umgebung bereit.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION` - _Global_ Umgebungsvariable, die das Kopieren der statischen Ansichtsdateien in das `var/view_preprocessed` überspringt und bei Bedarf minimierten HTML generiert.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND` - _Global_ Umgebungsvariable, um bei Bedarf statische Inhalte zu generieren.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - Sie können die Seiten auflisten, die zum Vorausfüllen des Caches verwendet werden sollen. Verfügbar in den neuen [Variablen nach der Bereitstellung](../environment/variables-post-deploy.md).

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, bei dem ein lokal angewendeter Patch die Bereitstellung auf einer -Instanz unterbrochen hatte. Jetzt kann ECE-Tools erkennen, dass ein Patch angewendet wurde.<!-- MAGECLOUD-982 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Konflikt zwischen der JavaScript-Bundle- und der GZIP-Funktion behoben. Jetzt funktionieren diese Funktionen ordnungsgemäß zusammen.<!-- MAGECLOUD-1735 -->

- ![fix icon](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass ECE-Tools CLI-Befehle bei der Verwendung früherer PHP 7.0.x-Versionen fehlschlugen.<!-- MAGECLOUD-1744 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das die Bereitstellung statischer Inhalte mit der kompakten Strategie in mehreren Threads verhinderte.<!-- MAGECLOUD-1822 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem mit der Redis-Sitzungssperre behoben, das zu einer Admin-Anmeldeverzögerung führte. Außerdem ist die Fehlerbehebung für 2.1.x.<!-- MAGECLOUD-1853 --> verfügbar

## v2002.0.9

>[!NOTE]
>
>Sie müssen [das Metapaket für Adobe Commerce auf Cloud-Infrastruktur aktualisieren](../dev-tools/install-package.md#update-the-metapackage) um dieses und alle zukünftigen Updates zu erhalten.

- ![neues Symbol](../../assets/new.svg) **ece-tools** - Das `ece-tools` unterstützt jetzt Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![neues Symbol](../../assets/new.svg) **Redis-Konfiguration** - Sie können jetzt [Redis-](../environment/variables-deploy.md#cache_configuration) und den Standard-Cache sowie den Redis-Sitzungsspeicher mit einer Umgebungsvariablen konfigurieren.<!-- MAGECLOUD-1552 -->

- ![Neues Symbol](../../assets/new.svg) **Service-Verbesserungen für Suche, AMQP und Redis** - Wir haben den Service-Konfigurationsfluss vereinheitlicht, sodass er sich jetzt für alle Services gleich verhält. Das manuelle Bearbeiten der `env.php` zum Konfigurieren von Diensten wird nicht mehr unterstützt. Sie müssen stattdessen Umgebungsvariablen oder die `.magento.env.yaml`-Datei verwenden.<!-- MAGECLOUD-1437 -->

- ![Fix-Symbol](../../assets/fix.svg) **Umgebungsvariablen**—

   - Die Verwendung von `env:STATIC_CONTENT_THREADS` ist veraltet und wird in einer zukünftigen Version entfernt. Verwenden Sie stattdessen [SCD_THREADS](../environment/variables-deploy.md#scd_threads).<!-- MAGECLOUD-1507 -->

   - Die `STATIC_CONTENT_EXCLUDE_THEMES` Umgebungsvariable wird nicht mehr unterstützt. Sie müssen stattdessen die Umgebungsvariable `SCD_EXCLUDE_THEMES` verwenden.<!-- MAGECLOUD-1640 -->

- ![Fix icon](../../assets/fix.svg) **Logging** - Wir haben die Protokollierung um integrierte Patch-Vorgänge vereinfacht.<!-- MAGECLOUD-1674 -->

- ![Fix-Symbol](../../assets/fix.svg) Die Unterstützung des `developer`-Modus und die `APPLICATION_MODE` Umgebungsvariable wurden entfernt, da sie unerwartetes Verhalten verursachten.<!-- MAGECLOUD-1615 -->

- ![fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das zu Fehlern bei der Bereitstellung statischer Inhalte im Zusammenhang mit Redis führte. Jetzt wird die Bereitstellung statischer Multithread-Inhalte wie vorgesehen ausgeführt.<!-- MAGECLOUD-1630 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das Benutzer daran hinderte, Änderungen an Konfigurationsfeldern in Admin zu speichern, die nach dem Ausführen des `app:config:dump`-Befehls als sensibel markiert wurden.<!-- MAGECLOUD-1175 -->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde Unterstützung für eine frühere Version von `symfony/yaml` hinzugefügt, um Konflikte mit einigen Paketen zu beheben, die noch nicht mit der neuesten Version kompatibel sind.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>In dieser Version haben wir `vendor/magento/ece-patches` mit `vendor/magento/ece-tools` zusammengeführt. Sie müssen das `vendor/magento/ece-patches` nicht mehr separat aktualisieren.

**Neue Funktionen:**

- **Verbesserte Protokollierung**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Wir haben die Protokollierung verbessert, um bessere Erklärungen zu liefern, wenn der Build- oder Bereitstellungsprozess eine Umgebungsvariable überschreibt.
   - Sie können jetzt den Installations- und Upgrade-Fortschritt in Echtzeit anzeigen. Verfolgen Sie die `install_update.log`, um den Fortschritt anzuzeigen. Beispiel:

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Neuer Cron-Befehl** - Sie können jetzt bestimmte stecken gebliebene Cron-Aufträge entsperren, anstatt sie alle mit dem [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451)-Befehl zu stoppen und neu zu starten. In Version 2.1.<!-- MAGECLOUD-1367 --> nicht verfügbar

- **Einheitliche Konfigurationsdatei** - Sie können jetzt Build- und Bereitstellungsphasen mithilfe einer [`.magento.env.yaml`](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml) Datei konfigurieren.<!-- MAGECLOUD-1369 -->

- **Backup-Konfigurationsdateien**: Der Bereitstellungsprozess erstellt jetzt nach der Bereitstellung automatisch ein Backup der `app/etc/env.php`- und `app/etc/config.php`-Konfigurationsdateien. Wir haben auch einen [neuen CLI-Befehl](https://support.magento.com/hc/en-us/articles/360033182871) hinzugefügt, um diese Konfigurationsdateien aus einer Sicherung wiederherzustellen.<!-- MAGECLOUD-1372 -->

- **Fehlerbehebung bei Validierungsfehlern** - Der Befehl, den Sie zum Beheben von Validierungsfehlern verwenden müssen, wenn `config.php` nicht genügend Daten für die statische Inhaltsbereitstellung enthält, wurde geändert. Zuvor wurden Sie in der Fehlermeldung angewiesen, `bin/magento app:config:dump` auszuführen. Jetzt müssen Sie `php ./vendor/bin/ece-tools config:dump` ausführen.<!-- MAGECLOUD-1491 -->

- **Neue Umgebungsvariablen** [ - Sie können jetzt Umgebungsvariablen verwenden, um benutzerdefinierte (](../environment/variables-deploy.md#search_configuration)- und [AMQP-basierte](../environment/variables-deploy.md#queue_configuration)-Services mit Ihrer Site zu verbinden.<!-- MAGECLOUD-1410 -->

- Wir haben smartes Patchen implementiert. Jetzt wendet das Paket Patches an, die nicht auf der Adobe Commerce-Version für die Cloud-Infrastruktur basieren, sondern auf der gepatchten Paketversion.<!--MAGECLOUD-1090-->

**Gelöste Probleme:**

- Es wurde ein Protokollierungsproblem behoben, das zu Build-Fehlern führte.<!-- MAGECLOUD-1162 -->

- Es wurde ein Problem behoben, das zu Timeout-Ausnahmen bei der Ausführung von Bereitstellungen im interaktiven Modus führte.<!-- MAGECLOUD-1389 -->

- Es wurde ein Problem behoben, das zu Fehlern bei der Verwendung der Kompaktstrategie für die Erstellung statischer Inhalte führte. In Version 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485--> nicht verfügbar

- Es wurde ein Problem behoben, das verhinderte, dass das Bereitstellungsskript Staging- und Produktionsumgebungen ordnungsgemäß identifizierte.<!-- MAGECLOUD-1493 -->

- Es wurde ein Problem behoben, das dazu führte, dass Netzwerkprobleme die Datenbankverbindungen unterbrachen und während des Installations- und Aktualisierungsprozesses zu Fehlern führten.<!-- MAGECLOUD-1520 -->

- Es wurde ein Problem behoben, das den Export der Konfigurationsdateien mit `app:config:dump` mehrmals verhinderte. In Version 2.1.<!--  MAGECLOUD-1567  --> nicht verfügbar

- Es wurde ein Redis-Sitzungs _Sperrungsproblem behoben_ das zu einer Anmeldeverzögerung _Admin_ führte. In Version 2.1.<!--  MAGECLOUD-1582  --> nicht verfügbar

- Es wurde ein Implementierungsproblem im Zusammenhang mit der Versionierung behoben, das einen Konflikt mit anderen Composer-basierten Patchmodulen verursachte.<!-- MAGECLOUD-1450 -->

- Es wurde ein Problem behoben, das beim Import zu Problemen mit dem PHP-Speicher führte.<!-- MAGECLOUD-1310 -->

- Patch entfernt; Fehlerbehebung in `colinmollenhour/credis` v1.6, um die Unterstützung für Adobe Commerce auf Cloud-Infrastruktur 2.2.1 zu aktivieren. In Version 2.1.<!-- MAGECLOUD-1033 --> nicht verfügbar

## v2002.0.7

**Gelöste Probleme:**

- Wir haben `var/view_preprocessed` Symlink entfernt, um ein Problem zu beheben, das zu JavaScript-Minimierungskonflikten führte.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Gelöste Probleme:**

- Es wurde ein Problem behoben, das zu `gzip`-Fehlern führte, wenn ein Datei- oder Verzeichnisname Leerzeichen enthielt.<!-- MAGECLOUD-1413 -->

- Es wurde ein Problem behoben, das verhinderte, dass Bereitstellungsskripte Modulabhängigkeiten ordnungsgemäß erkennen und aktivieren konnten.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Neue Funktionen:**

- **Konfigurieren eines Cron-Verbrauchers mit einer Umgebungsvariablen** - Sie können jetzt Cron-Verbraucher mit der neuen `CRON_CONSUMERS_RUNNER`-Umgebungsvariablen konfigurieren.

- **Konfigurationsüberprüfung**: Wir suchen jetzt während des Build-/Bereitstellungsprozesses nach kritischen Komponenten und halten den Prozess an, wenn der Scan fehlschlägt, was unnötige Ausfallzeiten aufgrund des Wartungsmodus der Site verhindert.

- **Benachrichtigungen erstellen/bereitstellen** - Wir haben eine Konfigurationsdatei hinzugefügt, mit der Sie [Slack- und/oder E-Mail-Benachrichtigungen einrichten](../environment/set-up-notifications.md) für Build-/Bereitstellungsaktionen in allen Ihren Umgebungen einrichten können.

- **Statische Inhaltskomprimierung**: Wir komprimieren jetzt statische Inhalte mit [gzip](https://www.gnu.org/software/gzip/) während der Build- und Bereitstellungsphase. Diese Komprimierung in Verbindung mit der Fastly-Komprimierung trägt dazu bei, die Größe Ihres Stores zu reduzieren und die Bereitstellungsgeschwindigkeit zu erhöhen. Bei Bedarf können Sie die Komprimierung mithilfe einer [Build-Option](../environment/variables-build.md) oder [Bereitstellungsvariable](../environment/variables-deploy.md) deaktivieren. Weitere Informationen finden Sie unter den folgenden Themen:

   - [Anwendungsumgebungsvariablen](../application/variables-property.md)

   - [Leistung bei der Bereitstellung statischer Inhalte](../deploy/static-content.md)

   - [Bereitstellungsprozess](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **Konfigurationsverwaltung**: Wir generieren jetzt während der Build-Phase automatisch eine `app/etc/config.php`-Datei in Ihrem Git-Repository, falls diese noch nicht vorhanden ist. Die automatisch generierte Datei enthält nur eine Liste von Modulen und Erweiterungen. Wenn die Datei bereits vorhanden ist, wird die Erstellungsphase wie gewohnt fortgesetzt. Wenn Sie [Konfigurationsverwaltung](../store/store-settings.md) zu einem späteren Zeitpunkt folgen, aktualisieren die Befehle die Datei, ohne dass zusätzliche Schritte erforderlich sind. Weitere Informationen finden [ unter ](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) .

- **Datenbank-Dumps** - Wir haben einen `magento/ece-tools` CLI-Befehl zum Erstellen von Datenbank-Dumps in allen Umgebungen hinzugefügt. Bei Pro-Plan-Produktionsumgebungen werden mit diesem Befehl nur Speicherauszüge von einem von drei Hochverfügbarkeits-Knoten ausgegeben, sodass Produktionsdaten, die während des Speicherauszugs auf einen anderen Knoten geschrieben werden, möglicherweise nicht kopiert werden. Es wird empfohlen, die Anwendung in den Wartungsmodus zu versetzen, bevor ein Datenbank-Dump in Produktionsumgebungen durchgeführt wird. Weitere Informationen finden [ unter ](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/develop/storage/snapshots)Backup-Verwaltung“.

- **Cron-Intervallbeschränkungen aufgehoben** - Das standardmäßige Cron-Intervall für alle Umgebungen in den Regionen us-3, eu-3 und ap-3 beträgt 1 Minute. Das standardmäßige Cron-Intervall in allen anderen Regionen beträgt 5 Minuten für Pro Integration-Umgebungen und 1 Minute für Pro Staging- und Produktionsumgebungen. Um Ihre vorhandenen Cron-Aufträge zu ändern, bearbeiten Sie Ihre Einstellungen in `.magento.app.yaml` oder erstellen Sie ein Support-Ticket für Produktions-/Staging-Umgebungen. Weitere Informationen finden [ unter „Einrichten ](../application/crons-property.md#set-up-cron-jobs) Cron-Aufträgen“.

**Gelöste Probleme:**

- Es wurde ein Problem behoben, das zu langen Bereitstellungszeiten führte, da der Bereitstellungsprozess den `cache-clean` vor der Bereitstellung statischer Inhalte aufrief.<!-- MAGECLOUD-1327 -->

- Es wurde ein Problem behoben, das während des Schritts zur Erzeugung statischer Inhalte bei der Bereitstellung in Produktionsumgebungen zu Fehlern führte.<!-- MAGECLOUD-1322 -->

- Es wurde ein Problem behoben, das einige `magento/ece-tools`-Befehle daran hinderte, die Ausgabe in `stderr` zu protokollieren.<!-- MAGECLOUD-1264 -->

- Es wurde ein Problem behoben, das verhinderte, dass Basis-URL-Werte in `env.php` in verzweigten Verzweigungen aktualisiert wurden.<!-- MAGECLOUD-1242 -->

- Es wurde ein Problem behoben, das dazu führte, dass der Befehl `magento setup:install` ein unsicheres Präfix (`http://`) zu sicheren Basis-URLs hinzufügte.<!-- MAGECLOUD-1171 -->

- Es wurde ein Problem behoben, das verhinderte, dass Patch-Fehler Bereitstellungsfehler verursachten.<!-- MAGECLOUD-1170 -->

- Es wurde ein Problem behoben, das verhinderte, dass `ece-tools` die Ausführung stoppte und eine Ausnahme auslöste, wenn keine Patches angewendet werden konnten.<!-- MAGECLOUD-1152 -->

- Es wurde ein Problem behoben, das nach der Aktivierung der HTML-Minimierung in Admin zu Fehlern beim Laden der Storefront führte.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Gelöste Probleme:**

- Sie können jetzt hängengebliebene Cron[Aufträge ](https://support.magento.com/hc/en-us/articles/360033099451) einem CLI-Befehl in allen Umgebungen über SSH-Zugriff manuell zurücksetzen. Der Bereitstellungsprozess setzt Cron-Aufträge automatisch zurück.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Gelöste Probleme:**

- Es wurde ein Problem behoben, das dazu führte, dass Seiten eine Zeitüberschreitung aufwiesen, da Redis zu lange zum Lesen/Schreiben benötigte. Sie können jetzt den `disable_locking`-Parameter in Redis-Konfigurationen verwenden, um dieses Problem zu vermeiden.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Gelöste Probleme:**

- Der [!DNL RabbitMQ]-Konfigurationsprozess ruft jetzt automatisch alle erforderlichen Parameter ab.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Neue Funktionen:**

- Adobe Commerce in der Cloud-Infrastruktur unterstützt jetzt Bereiche [statische Strategien zur Inhaltsbereitstellung](https://experienceleague.adobe.com/de/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy). Wir haben den Parameter `–s` mit der Standardeinstellung `quick` für die Strategie zur statischen Inhaltsbereitstellung hinzugefügt. Sie können die Umgebungsvariable [SCD_STRATEGY](../environment/variables-deploy.md) verwenden, um diese Strategien anzupassen und mit Ihren Build- und Bereitstellungsaktionen zu verwenden. Diese Variable unterstützt die Optionen `standard`, `quick` oder `compact`. Wenn Sie `compact` auswählen, überschreiben wir den `STATIC_CONTENT_THREADS` mit `1`, was die Bereitstellung verlangsamen kann, insbesondere in Produktionsumgebungen. In Version 2.1.<!--- MAGECLOUD-1057 --> nicht verfügbar

- Wir haben eine Protokolldatei für Umgebungen erstellt, um Aktionen zu erfassen, zu kompilieren, zu erstellen und bereitzustellen. Die `var/log/cloud.log`-Datei befindet sich im Stammverzeichnis der Anwendung.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Gelöste Probleme:**

- Das `ece-tools` wurde überarbeitet, um es mit Adobe Commerce auf Cloud-Infrastruktur 2.2.0 und höher kompatibel zu machen.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- Es wurde ein Problem behoben, das verhinderte, dass `ece-tools` die Ausführung stoppte und eine Ausnahme auslöste, wenn keine Patches angewendet werden konnten.<!-- MAGECLOUD-1186 -->

- Es wurde ein Problem behoben, das dazu führte, dass Ausnahmen ausgelöst wurden, wenn die Kompilierung der Abhängigkeitsinjektion (di) während Builds übersprungen wurde.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- Es wurde ein Problem behoben, das dazu führte, dass der Bereitstellungsprozess benutzerdefinierte Redis-Konfigurationen in der `env.php`-Datei überschrieb.<!-- MAGECLOUD-1019 -->

- Es wurde ein Problem behoben, das zu Umleitungsschleifen führte, da die standardmäßige sichere Verwaltung deaktiviert war.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Dieses Paket ist nicht mehr mit anderen Versionen von Adobe Commerce in Cloud-Infrastrukturen kompatibel und **sollte nicht** verwendet werden.

### Erstmalige Veröffentlichung

Erste Version von `ece-tools` für Adobe Commerce auf Cloud-Infrastruktur 2.2.0.
