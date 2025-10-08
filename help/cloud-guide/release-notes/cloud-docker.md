---
title: Cloud Docker-Paket
description: Hier finden Sie eine Liste der neuesten Verbesserungen am Cloud Docker-Paket.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: c1f5e663716d3c9390e8ffb8bb2da6c7c67996b0
workflow-type: tm+mt
source-wordcount: '3790'
ht-degree: 0%

---

# Cloud Docker-Paket

Das [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) bietet Funktionen und Docker-Images, um Adobe Commerce in einer lokalen Cloud-Umgebung bereitzustellen. In diesen Versionshinweisen werden die neuesten Verbesserungen an diesem Paket beschrieben, das eine Komponente von [Cloud-Tools-Suite für Commerce](cloud-tools-suite.md) ist.

Das `magento/magento-cloud-docker`-Paket verwendet die folgende Versionssequenz: `<major>.<minor>.<patch>`

Die Versionshinweise umfassen Folgendes:

- ![neues Symbol](../../assets/new.svg) Neue Funktionen
- ![Fehlerbehebungssymbol](../../assets/fix.svg) Fehlerbehebungen und Verbesserungen

<!--Add release notes below-->

## v1.4.5 {#latest}

Veröffentlichungsdatum: 8. Oktober 2025

- ![neues Symbol](../../assets/new.svg) **ActiveMQ** - ActiveMQ-Unterstützung in Cloud-Docker mit Funktionstests hinzugefügt.<!-- MCLOUD-13771 -->

## v1.4.4

Veröffentlichungsdatum: 7. August 2025

- ![fix icon](../../assets/fix.svg) **PHP 8.4**—Hinzugefügte PHP 8.4-Tests.<!-- MCLOUD-13311 -->
- ![Fix icon](../../assets/fix.svg) **FTP-Erweiterung**-Fix für FTP-Erweiterung hinzugefügt.<!-- MCLOUD-13843 -->
- ![neues Symbol](../../assets/new.svg) **OpenSearch3 Bild**—Es wurde Unterstützung von OpenSearch3.<!-- MCLOUD-13766 --> hinzugefügt
- ![neues Symbol](../../assets/new.svg) **Opensearch3 tests**—PHP 8.4 Tests für Opensearch3.<!-- MCLOUD-13768 --> hinzugefügt
- ![neues Symbol](../../assets/new.svg) **Valkey** - Unterstützung für Valkey wurde hinzugefügt.<!-- MCLOUD-13558 -->

## v1.4.3

Veröffentlichungsdatum: 3. Juni 2025

- ![Fix-Symbol](../../assets/fix.svg) **Verbesserte Kompatibilität mit 2.4.8**-Aktualisierte Drittanbieterbibliotheken für bessere Kompatibilität mit 2.4.8<!-- MCLOUD-13707	 - -->

## v1.4.2

Veröffentlichungsdatum: 7. April 2025

- ![neues Symbol](../../assets/new.svg) **PHP 8.4** - `php-cli` 8.4 und `php-fpm` 8.4 Bilder hinzugefügt.


## v1.4.1

Veröffentlichungsdatum: 6. Februar 2025

- ![neues Symbol](../../assets/new.svg) **PHP 8.4**—Unterstützung für PHP 8.4 hinzugefügt.


## v1.4.0

Veröffentlichungsdatum: 7. Oktober 2024

- ![fix icon](../../assets/fix.svg) **Refactored code**—Die Unterstützung alter PHP-Versionen (7.4, 7.3, 7.2) und zugehöriger Bibliotheken und Bilder wurde entfernt.

## v1.3.7

Veröffentlichungsdatum: 8. April 2024

- ![new icon](../../assets/new.svg) **PHP** — Unterstützung für PHP 8.3- und PHP 8.3-Images hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **Nginx** — Das Bild Nginx v. 1.24 wurde hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **OpenSearch** - Bild hinzugefügt OpenSearch v. 2.12, 1.3.
- ![neues Symbol](../../assets/new.svg) **Composer** - Composer-Version wurde auf 2.2.23 aktualisiert.

## v1.3.6

Veröffentlichungsdatum: 31. Juli 2023

- ![neues Symbol](../../assets/new.svg) **Neue Service-Version hinzugefügt**—OpenSearch 2.5.
- ![neues Symbol](../../assets/new.svg) **Composer-Cache aktivieren** - Jetzt können Sie die Docker-Konfiguration erweitern, um Composer beim Starten des Docker-Containers das Löschen des Cache zu aktivieren. Siehe [Erweitern der Docker-Konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) im Handbuch _Cloud Docker for Commerce_.

## v1.3.5

Veröffentlichungsdatum: 10. März 2023

- ![neues Symbol](../../assets/new.svg) **ionCube** - Die ionCube-Erweiterung für das PHP 8.1-Image wurde hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **Neue Service-Versionen hinzugefügt**—OpenSearch 2.3 und 2.4, PHP 8.2, Lack 7.1.1.
- ![neues Symbol](../../assets/new.svg) **Erweiterte Unterstützung für PHP 8.2**—Es wurden Kompatibilitätsprobleme mit bestimmten PHP 8.2.x-Versionen behoben, um Commerce 2.4.6 zu unterstützen.
- ![Fix icon](../../assets/fix.svg) **Composer-Problem** - Es wurden Probleme behoben, die nach der Aktualisierung der Composer-Version in den Docker-Containern aufgetreten waren.

## v1.3.4

Veröffentlichungsdatum: 27. Oktober 2022

- ![neues Symbol](../../assets/new.svg) **Hinzugefügte neue Lackbilder** - Hinzugefügte Bilder für Lack 6.5, 7.0 und 7.1.<!-- MCLOUD-7879 -->

## v1.3.3

Veröffentlichungsdatum: 13. September 2022

- ![neues Symbol](../../assets/new.svg) **Unterstützung für Apple M1 (ARM64)** - Es wurden Änderungen an Docker-Images hinzugefügt, um die Unterstützung für die Apple M1 (ARM64)-Architektur zu aktivieren.<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![fix icon](../../assets/fix.svg) **Mailhog**: Fehlerkorrektur - Der Mailhog-Service kann jetzt im Entwicklermodus E-Mails abrufen.<!-- MCLOUD-8643 -->
- ![Fix icon](../../assets/fix.svg) **init-docker.sh** - Der Validator der Service-Versionen im `init-docker.sh`-Skript wurde korrigiert.<!-- MCLOUD-8765 -->

## v1.3.2

Veröffentlichungsdatum: 31. März 2022

- ![neues Symbol](../../assets/new.svg) **Elasticsearch 7.10 hinzugefügt**<!-- MCLOUD-8548 -->

## v1.3.1

Veröffentlichungsdatum: 10. März 2022

- ![neues Symbol](../../assets/new.svg) **Unterstützung von PHP 8.1**—Unterstützung für PHP 8.1 hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **OpenSearch** - Es wurden Bilder der OpenSearch-Versionen 1.1 und 1.2 hinzugefügt.
- ![neues Symbol](../../assets/new.svg) **Composer 2.1**—Setzen Sie Composer 2.1.x standardmäßig in PHP 8.x-Bildern.
- ![Neues Symbol](../../assets/new.svg) **Verbesserungen an PHP-**—

   - PHP 8.1-Bilder hinzugefügt
   - Aktualisierung von xDebug Version 3.1.2
   - Upgrade von xmlrpc 1.0.0RC3

- ![Fix-Symbol](../../assets/fix.svg) **Elasticsearch- und OpenSearch-**: -Verbesserungen in Elasticsearch und OpenSearch-Dockerfiles; entfernt das Elasticsearch 5.2-Image.
- ![fix icon](../../assets/fix.svg) **Sodium extension** - Aktiviert die `sodium` standardmäßig in allen PHP-Bildern.
- ![Fix icon](../../assets/fix.svg) **Composer-Cache-Volume** - Fester Pfad für Composer-Cache-Volume, auf dem Composer-Pakete zwischengespeichert werden.
- ![fix icon](../../assets/fix.svg) **Speicherbegrenzung in nginx**—Feste Speicherbegrenzung im NGINX-Bild.

## v1.3.0

Veröffentlichungsdatum: 25. Oktober 2021

- ![Fix icon](../../assets/fix.svg) **Workflow zum Verbessern des Entwicklermodus** - Zuvor mussten Sie den Modus im Build- und Bereitstellungsschritt angeben. Jetzt bestimmt die Option `--mode` im `build` Schritt den Modus im späteren `deploy`. Das Festlegen des Modus nach der Bereitstellung ist nicht mehr erforderlich. Siehe [Entwicklermodus](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![Fix icon](../../assets/fix.svg) **Verbesserungen für schreibgeschützte Dateisysteme**—<!-- ACMP-1106 -->
   - Fehlerkorrektur - Der PHP-Container für die Mail-Konfiguration wird jetzt gestartet.
   - Kann Umgebungsvariablen in INI-Dateien verwenden.
   - Stellen Sie sicher, dass PHP-Einstiegspunkte keine Schreibberechtigung benötigen.
- ![Fix icon](../../assets/fix.svg) **Update Node** - Aktualisieren Sie die gebündelte Node-Version; bei der Installation von Node in PHP-CLI-Images wird jetzt die aktuelle LTS-Version verwendet.<!-- ACMP-1539 -->
- ![fix icon](../../assets/fix.svg) **update symfony**—Die Abhängigkeiten von Symfony wurden aktualisiert, damit sie mit Adobe Commerce 2.4.4.<!-- ACMP-1533 --> kompatibel sind

## v1.2.4

Veröffentlichungsdatum: 29. Juli 2021

- ![neues Symbol](../../assets/new.svg) **Neuer `Zookeeper`-Container** - Ein [ZooKeeper-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container) wurde hinzugefügt, um die Sperranbieterkonfiguration für Projekte zu verwalten, die nicht in Adobe Commerce in der Cloud-Infrastruktur bereitgestellt werden.<!--MCLOUD-8000-->

- ![neues Symbol](../../assets/new.svg) **Unterstützung für Composer 2.0.** hinzugefügt - Composer Version 2.0 zur Composer-Konfigurationsdatei hinzugefügt, um Upgrades von Composer 1.0 zu unterstützen, das sich dem Ende des Lebenszyklus nähert.<!--MCLOUD-8003-->

## v1.2.3

Veröffentlichungsdatum: 14. Juni 2021

- ![neues Symbol](../../assets/new.svg) **Hinzugefügt PHP 8.0**—PHP wurde auf Version 8.0 aktualisiert, sodass Sie alle neuen Funktionen und Optimierungen nutzen können, die PHP 8.0 enthält.<!--MCLOUD-7941-->
- ![neues Symbol](../../assets/new.svg) **Aktualisiert auf Varnish 6.6 und Elasticsearch 7.11.** - Die folgenden Links enthalten Versionsinformationen zu [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) und Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![neues Symbol](../../assets/new.svg) **Hinzugefügte `ioncube` Erweiterung für PHP 7.4 Image**—Die `ioncube` Erweiterung wurde dem PHP 7.4 Image erneut hinzugefügt, nachdem sie ursprünglich vom PHP 7.3 auf PHP 7.4 Upgrade ausgeschlossen worden war. *[Eingegeben von mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![neues Symbol](../../assets/new.svg) **Es wurde eine Dateisynchronisierungsoption hinzugefügt:`manual-native`** - Die `manual-native` Dateisynchronisierungsoption bietet manuelle Kontrolle über die Synchronisierung, was die beste Leistung für macOS- und Windows-Umgebungen bietet. Erfahren Sie mehr über die Verwendung der `manual-native`-Option [Entwicklermodus](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) und [Synchronisieren von Daten in einer Docker-](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options))<!--MCLOUD-7977-->
- ![neues Symbol](../../assets/new.svg) **Löschungen von Volumes aus `up`- und `down`-Befehlen wurden entfernt** - Die `--volume` Option wurde aus den `bin/magento-docker up`- und `bin/magento-docker down`-Befehlen entfernt und durch den neuen `bin/magento-docker init`-Befehl mit einer Datenverlustwarnung ersetzt. Durch diese Änderung wird ein versehentlicher Datenverlust verhindert. *[Eingegeben von joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![Fix icon](../../assets/fix.svg) **Der `CN` für das generierte Zertifikat wurde aktualisiert** - Der hartcodierte `CN` wurde aus der Dockerdatei entfernt. Dieser Wert hat einen Zertifikatfehler (`NET::ERR_CERT_INVALID`) erstellt, der dazu geführt hat, dass die Option `--host` für den `ece-docker build:compose`-Befehl ignoriert wurde.<!--MCLOUD-7934-->

## v1.2.2

Veröffentlichungsdatum: 20. April 2021

- ![neues Symbol](../../assets/new.svg) **Aktualisierte `host.docker.internal`, um plattformunabhängig zu sein** - Sie können jetzt dieselben Docker Compose-Skripte für Ubuntu, Windows und macOS erstellen. Die Verwendung von Xdebug auf Ubuntu erfordert keine separate Umgebungsvariable mehr. [Fehlerbehebung eingereicht von Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![neues Symbol](../../assets/new.svg) **Aktualisiert init-docker.sh** - Das `mounts`-Objekt wurde zur `MAGENTO_CLOUD_APPLICATION` Umgebungsvariablen hinzugefügt. [Fehlerbehebung eingereicht von Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![neues Symbol](../../assets/new.svg) **Aktualisiert init-docker.sh**—Das `init-docker.sh`-Skript wurde mit PHP 7.4 und Cloud Docker 1.2.1 aktualisiert. [Fehlerbehebung eingereicht von Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![neues Symbol](../../assets/new.svg) **Natrium standardmäßig aktiviert**—Aktiviert die `sodium` PHP-Erweiterung standardmäßig in PHP Docker-Bildern.<!--MCLOUD-7548-->
- ![neues Symbol](../../assets/new.svg) **`custom-registry`Option** - Dem `--custom-registry`-Befehl wurde eine `php ./vendor/bin/ece-docker build:compose` Option zur Verwendung Ihrer eigenen Bildregistrierung hinzugefügt.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![neues Symbol](../../assets/new.svg) **Alte Elasticsearch-Versionen entfernt** - Die Elasticsearch-Versionen 1.7 und 2.4 wurden aus den Elasticsearch-Images entfernt.<!--MCLOUD-7504-->
- ![neues Symbol](../../assets/new.svg) **Automatisches Generieren von NGINX-Zertifikaten** - Entfernt die vorhandenen Zertifikate aus dem NGINX-Bild. Die NGINX-Zertifikate werden jetzt bei jeder neuen Bereitstellung automatisch generiert, um die Sicherheit zu verbessern.<!--MCLOUD-7396-->
- ![fix icon](../../assets/fix.svg) **Enabled`opcache.validate_timestamps`** - Aktiviert die `opcache.validate_timestamps` PHP-Einstellung standardmäßig im Entwicklermodus. Durch Aktivierung dieser Einstellung wurde das Problem behoben, dass Änderungen am Dateisystem in Docker nicht erkannt wurden.<!--MCLOUD-7466-->
- ![Fix icon](../../assets/fix.svg) **Behobene`build:custom:compose`** - Der `build:custom:compose`-Befehl wurde korrigiert, der einen Fehler auslöst, wenn Dateien während des Build-Prozesses nicht überschrieben werden können. Das Auslösen eines Fehlers verhindert Situationen, in denen `docker-compose up` die falschen Dateien verwenden könnten.<!--MCLOUD-7457-->
- ![Fix icon](../../assets/fix.svg) **Option &quot;`--sync_engine="native"` korrigiert** - Es wurde das Problem behoben, dass im Produktionsmodus (`--mode="production"`) mit der Option `--sync_engine="native"` keine Einträge für lokale Ordner in der `docker.composer.yml` erstellt wurden.<!--MCLOUD-7254-->
- ![Fehlerbehebung](../../assets/fix.svg) **Fehler bei der Validierung der Service-Version** - Service-Versionen für [!DNL RabbitMQ], Elasticsearch und andere Services wurden zur `type` in der `MAGENTO_CLOUD_RELATIONSHIP`-Variablen hinzugefügt. Durch Hinzufügen dieser Versionen zur `relationships`-Variablen wurden die Validierungsfehler behoben, die während der Bereitstellungsphase auftraten.<!--MCLOUD-7572-->

## v1.2.1

Veröffentlichungsdatum: 21. Dezember 2020

- ![neues Symbol](../../assets/new.svg) **NGINX-Befehlsoptionen** - Es wurden Befehlsoptionen zum Erstellen hinzugefügt, um die Anzahl der NGINX-`worker_processes` und NGINX-`worker_connections` für TLS und Web-Services zu ändern. Der `worker_process`-Parameter behält die Möglichkeit, den Wert auf `auto` festzulegen. Beispiele: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![neues Symbol](../../assets/new.svg) **TLS-Befehlsoption** - Es wurde eine Build-Befehlsoption hinzugefügt, um eine Konfiguration ohne den TLS-Service zu erstellen. Beispiel: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![neues Symbol](../../assets/new.svg) **NGINX-Speicherverbrauch** - Verringert den Speicherverbrauch durch den NGINX-Prozess für TLS und Web-Services.<!--MCLOUD-7259-->

- ![neues Symbol](../../assets/new.svg) **Blackfire** - Blackfire PHP-Erweiterung ist im Cloud Docker-Image standardmäßig deaktiviert.

- ![fix icon](../../assets/fix.svg) **PHP-FPM container**—Die Konsistenzprüfung des PHP-FPM-Containers wurde korrigiert, indem die `WEB_PORT` von `80` auf `8080` geändert wurde.<!--MCLOUD-7232-->

- ![Fix-Symbol](../../assets/fix.svg) **Ungültige Volume-** - Es wurde ein Fehler mit ungültiger Volume-Benennung im Entwicklermodus behoben.<!--MCLOUD-7442-->

- ![Fix icon](../../assets/fix.svg) **NGINX Upstream Port** - Das Docker NGINX 1.19-Image wurde aktualisiert und verwendet nun Port 8080, um eine Endlosschleife zu vermeiden. [Fehlerbehebung eingereicht von Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Veröffentlichungsdatum: 9. November 2020

- ![neues Symbol](../../assets/new.svg) **Container-Updates—**

   - ![neues Symbol](../../assets/new.svg) **PHP-FPM-Container**—Unterstützung für die gnupg PHP-Erweiterung hinzugefügt. [Fehlerbehebung eingereicht von G Arvind von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![fix icon](../../assets/fix.svg) **Datenbank-Container** - Die Konsistenzprüfung des Datenbank-Containers wurde korrigiert, indem das erforderliche Datenbankkennwort zum Konsistenzprüfungsbefehl hinzugefügt wurde.<!--MCLOUD-7122-->

   - ![Neues Symbol](../../assets/new.svg) **Elasticsearch-Container**

      - Es wurde Unterstützung für Elasticsearch 7.9 zur Kompatibilität mit kommenden Adobe Commerce-Versionen hinzugefügt.<!--MCLOUD-7190-->

      - **Elasticsearch-Plug-in**-Konfiguration - Es wurde Unterstützung hinzugefügt, um die Elasticsearch-Plug-in-Konfigurationsinformationen aus der `services.yaml`-Datei zum Generieren der `docker-compose.yaml`-Datei für eine Cloud Docker-Umgebung für Commerce zu verwenden. Siehe [Elasticsearch-Plug-ins](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Unterstützung für Elasticsearch-Plug**—Folgende Elasticsearch-Plug-ins werden jetzt unterstützt: `analysis-icu`, `analysis-phonetic`, `analysis-stempel` und `analysis-nori`. Die Plug-ins `analysis-icu` und `analysis-phonetic` werden standardmäßig installiert. Sie können die `analysis-stempel` und `analysis-nori`-Plug-ins nach Bedarf hinzufügen oder entfernen.<!--MCLOUD-2789-->

   - ![neues Symbol](../../assets/new.svg) **CLI-Container**

      - **Befehle in Docker-PHP-Containern ausführen** - Jetzt können Sie die Cloud Docker-CLI verwenden, um Befehle in PHP-Containern in Ihrer Docker-Umgebung auszuführen, ohne PHP auf dem Host installieren zu müssen. Der folgende Befehl erstellt beispielsweise die Konfiguration: `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Siehe [Cloud Docker-CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli). [Fehlerbehebung eingereicht von G Arvind von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - OpenSSH-Client zu PHP CLI-Containern hinzugefügt. Jetzt können Sie die ssh-agent-Weiterleitung für Composer verwenden, wenn die `composer.json`-Datei private Git-Repositorys enthält, für die ein ssh-Client Composer-Befehle verwenden muss.<!--MCLOUD-6008-->

   - ![Fix icon](../../assets/fix.svg) **TLS-Container** - Jetzt basiert der [TLS-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) auf dem `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker-Image anstelle des CentOS-Images. Durch diese Änderung werden Probleme behoben, die beim Senden von HTTPS-Anfragen zwischen Containern in der Cloud Docker-Umgebung zu Fehlern führten.<!--MCLOUD-6469-->

   - ![neues Symbol](../../assets/new.svg) **Test-Container** - Es wurde ein Test-Container für Anwendungstests hinzugefügt und die `--with-test`-Option zum Docker-`build:compose`-Befehl hinzugefügt, um den Container nur beim Testen in der Docker-Umgebung zu erstellen. Siehe [Anwendungstests](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![Neues Symbol](../../assets/new.svg) **FPM-XDEBUG-Container**

      - ![neues Symbol](../../assets/new.svg) **Konfigurieren von Xdebug unter Linux** - Dem `--set-docker-host`-Befehl wurde die Option `ece-docker build:compose` hinzugefügt, um den `host.docker.internal` im Xdebug-Container zu konfigurieren. Diese Option ist erforderlich, um Xdebug auf Linux-Systemen verwenden zu können. Siehe [Konfigurieren von Xdebug für Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-6430-->

      - ![Fix icon](../../assets/fix.svg) Die Xdebug-Variablenkonfiguration für den Docker-EINSTIEGSPUNKT wurde korrigiert, um `uninitialized "with_xdebug" variable` Fehler in den Protokollen zu beheben. [Fehlerbehebung eingereicht von Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![neues Symbol](../../assets/new.svg) **Docker-Konfigurationsänderungen**

   - **MailHog-**: Jetzt können Sie die folgenden `ece-docker build:compose`-Befehlsoptionen verwenden, um MailHog zu deaktivieren und Ports anzugeben: `--no-mailhog`, `--mailhog-http-port` und `--mailhog-smtp-port`. Siehe [Einrichten von E-Mails](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Für Cloud Docker für Commerce 1.2.0 und höher stellt Adobe jetzt Docker-Images für jede Patch-Version bereit, und der Docker-Konfigurationsgenerator erstellt die Docker-Konfiguration mit einer angegebenen Patch-Version, anstatt die neueste zu verwenden. Zuvor erstellte der Docker-Konfigurations-Generator die Konfiguration mit der neuesten Patch-Version, was das mit einer früheren Version erstellte Cloud Docker für Commerce-Umgebungen beschädigen könnte.<!--MCLOUD-7093-->

   - **Angeben benutzerdefinierter Bilder und Versionen in der benutzerdefinierten Cloud-Docker-Konfiguration** - Der `build:custom:compose`-Befehl wurde mit Optionen zum Angeben benutzerdefinierter Bilder und Versionen beim Generieren einer benutzerdefinierten Docker Compose-Konfigurationsdatei (`docker-compose.yaml`) aktualisiert. Siehe [Erstellen einer benutzerdefinierten Docker-Compose-Konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/). <!--MCLOUD-7089-->

   - Die Docker-Host-Konfiguration wurde aktualisiert, um Port 443 anzuzeigen, damit der Zugriff auf Adobe Commerce (`https://magento2.docker`) von allen CLI-Containern aus ermöglicht wird. Sie können den Standard-Port ändern, indem Sie die Option `--tls-port` hinzufügen, wenn Sie die Docker-Konfigurationsdatei generieren.<!--MCLOUD-6806-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass der Build von Cloud Docker für Commerce fehlschlug, wenn die `app/etc/env.php` vorhanden war.<!--MCLOUD-6732-->

- ![Fehlerbehebungssymbol](../../assets/fix.svg) Die Build-Konfiguration wurde aktualisiert, um benannte Volumes durch reguläre Volumes zu ersetzen, um Probleme bei der Bereitstellung von Cloud Docker für Commerce unter Linux oder dem Windows-Subsystem für Linux (WSL2) zu vermeiden.<!--MCLOUD-6732-->

- ![Fix-Symbol](../../assets/fix.svg) Die Funktionstests von Cloud Docker für Commerce wurden aktualisiert, um Composer 2.0 zu unterstützen.<!--MCLOUD-7183-->

## v1.1.2

Veröffentlichungsdatum: 9. September 2020

- ![neues Symbol](../../assets/new.svg) Unterstützung für Elasticsearch 7.7 hinzugefügt<!--MCLOUD-6219-->

## v1.1.1

Veröffentlichungsdatum: 5. August 2020

- ![Fehlerbehebung](../../assets/fix.svg) **E-Mail-Konfiguration aktualisiert** Die standardmäßige Cloud-Docker-Konfiguration für Commerce wurde aktualisiert, um den MailHog-Service anstelle von SendMail zu unterstützen. Siehe [Einrichten von E-Mails](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![Fix icon](../../assets/fix.svg) PS-Bibliothek wurde in der Cloud Docker-Umgebungskonfiguration wiederhergestellt, um `ps:  command not found` Fehler zu beheben.<!--MCLOUD-6621-->

- ![Fix-Symbol](../../assets/fix.svg) Die standardmäßige Cloud Docker-Konfiguration für Commerce wurde aktualisiert, um die automatische Bereitstellung des Datenbank-Einstiegspunkts und von MariaDB-Volumes zu entfernen und `Cannot create container for service db` zu beheben, die beim Starten der Cloud Docker-Umgebung auftreten können.

  Jetzt können Sie die Cloud Docker-Umgebung so konfigurieren, dass die Datenbankordner gemountet werden, indem Sie die folgenden Optionen zum `ece-docker build:compose`-Befehl hinzufügen: `--with-entry-point` und `with-mariadb-conf`. Siehe [Service-Konfigurationsoptionen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![Neues Symbol](../../assets/new.svg) **CLI-Befehlsaktualisierungen**

| Aktion | Befehl |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Hinzufügen eines Einstiegspunkts zum Datenbank-Container, um die Datenbank aus einer Sicherung wiederherzustellen | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Hinzufügen eines MariaDB-Konfigurationsvolumes | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Veröffentlichungsdatum: 25. Juni 2020

- ![neues Symbol](../../assets/new.svg) **Unterstützung für die Split Database Performance Solution hinzugefügt** - Jetzt können Sie einen Store mithilfe der Split Database Performance Solution in der Cloud Docker-Umgebung konfigurieren und bereitstellen.<!--MCLOUD-3740-->

- ![neues Symbol](../../assets/new.svg) **Unterstützung für die Bereitstellung von Adobe Commerce und Magento Open Source** - Jetzt können Sie Cloud Docker für Commerce verwenden, um eine lokale Entwicklungsumgebung für Projekte bereitzustellen, die nicht in Adobe Commerce auf der Cloud-Infrastruktur gehostet werden.<!--MCLOUD-5667-->

- ![neues Symbol](../../assets/new.svg) **Blackfire.io-Unterstützung**—Es wurde Unterstützung für die Verwendung der [Blackfire.io-Erweiterung](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/) für automatisierte Leistungstests hinzugefügt. [Fehlerbehebung eingereicht von Adarsh Manickam von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![neues Symbol](../../assets/new.svg) **Container-Aktualisierungen**

   - **Varnish** - Jetzt ist Varnish der Standard-Cache, wenn Sie Adobe Commerce in einer Cloud Docker-Umgebung mit einer unterstützten Version der Cloud-Anwendungsvorlage bereitstellen. Siehe [Lackbehälter](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - Es wurde die `--no-varnish` Option zum Überspringen der Installation des Varnish-Service hinzugefügt, wenn Sie die Cloud Docker-Konfigurationsdatei generieren.<!--MCLOUD-2634-->

   - ![neues Symbol](../../assets/new.svg) **Datenbank**

      - Unterstützung für die MySQL-Datenbank wurde hinzugefügt. Jetzt können Sie die Cloud Docker-Umgebung entweder mit MariaDB oder mit MySQL konfigurieren. Siehe [Service-Konfigurationsoptionen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - Es wurde die Möglichkeit hinzugefügt, die Inkrement- und Offset-Einstellungen für die Datenbankreplikation festzulegen, wenn Sie die Docker-Compose-Datei generieren. Siehe [Service-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![Neues Symbol](../../assets/new.svg) **PHP-FPM**

      - Es wurde Unterstützung für PHP 7.4 hinzugefügt [Fix eingereicht von Mohanela Murugan von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Es wurde die Möglichkeit hinzugefügt, eine `php.ini`-Datei in das Stammverzeichnis des Projekts in die Cloud Docker-Umgebung zu kopieren und benutzerdefinierte PHP-Einstellungen auf die PHP-FPM- und CLI-Container anzuwenden. Siehe [PHP-Einstellungen anpassen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings). [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - Eine Container-Konsistenzprüfung wurde hinzugefügt. [Fehlerbehebung eingereicht von Visanth Sampath von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![Fix icon](../../assets/fix.svg) **Node.js** - Die Standardversion von Node.js wurde von Version 8 auf Version 10 aktualisiert, um die Sicherheit zu verbessern. Node.js Version 8 ist veraltet und wird nicht mehr mit Fehlerbehebungen oder Sicherheitspatches aktualisiert. [Fehlerbehebung eingereicht von Mohan Elamurugan von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![Neues Symbol](../../assets/new.svg) **Elasticsearch**

      - Hinzugefügte Unterstützung für Elasticsearch 6.8, 7.2, 7.5 und 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Es wurde die Möglichkeit hinzugefügt, die Elasticsearch-Container-Konfiguration [&#128279;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) anzupassen, wenn Sie die Docker Compose-Konfigurationsdatei generieren.<!--MCLOUD-3059-->

      - Die `--no-es` Option wurde zu den Service-Konfigurationsoptionen für die Generierung der Docker Compose-Konfigurationsdatei hinzugefügt. Verwenden Sie diese Option, um die Elasticsearch-Container-Installation zu überspringen und stattdessen die MySQL-Suche zu verwenden. Diese Option wird nur für Adobe Commerce-Versionen 2.3.5 und früher unterstützt.<!--MCLOUD-3766-->

   - ![neues Symbol](../../assets/new.svg) **FPM-XDEBUG-Container** - Es wurde eine Service-Konfigurationsoption hinzugefügt, um Xdebug zum Debugging von PHP in Ihrer Cloud Docker-Umgebung zu installieren und zu konfigurieren. Siehe [Konfigurieren von xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-4098-->

- ![neues Symbol](../../assets/new.svg) **Docker-Konfigurationsänderungen**

   - Es wurden Konsistenzprüfungen für die Container der Dienste PHP-FPM, Redis, Elasticsearch und MySQL Docker hinzugefügt.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Der standardmäßige Dateisynchronisierungsmodus wurde im Entwicklermodus in `native` geändert.<!--MCLOUD-3890 -->

   - Beim Generieren der `docker-compose.yml`-Datei wurden Versionsinformationen zum generischen Docker-Service-Container-Bild hinzugefügt.<!--MCLOUD-3878-->

   - Verbesserte Fähigkeit, große Antworten aus dem Upstream PHP-FPM Container zu verarbeiten, indem der `fastcgi_buffers` Wert für den Nginx Server erhöht wird.<!--MCLOUD-5980-->

   - Verbesserte Leistung bei der mutagenen Dateisynchronisierung durch Hinzufügen einer zweiten Synchronisierungssitzung zum Synchronisieren von Dateien im `vendor`. Durch diese Änderung wird verhindert, dass Mutagene während des Dateisynchronisierungsprozesses hängen bleiben. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![Neues Symbol](../../assets/new.svg) **CLI-Befehlsaktualisierungen**

| Aktion | Befehl |
| -------- | --------------- |
| Redis-Cache löschen | `bin/magento-docker flush-redis` |
| Lackcache löschen | `bin/magento-docker flush-varnish` |
| Standardinstallation des Lackes überspringen | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Anpassen der Elasticsearch-Optionen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Elasticsearch-Konfiguration entfernen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Konfigurieren des DB-Containers mit MySQL Version 5.6 oder 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Benutzerdefinierte Basis-URL angeben | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Container für die Xdebug-Konfiguration hinzufügen](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![fix icon](../../assets/fix.svg) Die Konfiguration der Synchronisation mutagener Dateien wurde korrigiert, um zu verhindern, dass mutagen veraltete Sitzungen erstellt. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![fix icon](../../assets/fix.svg) Es wurde ein Konfigurationsproblem behoben, das zu Syntaxfehlern im Docker Compose-Protokoll beim Starten des PHP-FPM-Containers führte. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurden Volume-Konfliktfehler behoben, die manchmal bei der Verwendung mehrerer Docker-Umgebungen auftraten. [Fehlerbehebung eingereicht von G Arvind von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![Fix-Symbol](../../assets/fix.svg) Es wurde ein Problem behoben, das dazu führte, dass der `ece-docker build:compose`-Befehl fehlschlug, wenn die Konfiguration Blackfire.io enthielt. [Fehlerbehebung eingereicht von G Arvind von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![fix icon](../../assets/fix.svg) Die PHP-CLI-Bildkonfiguration wurde aktualisiert, um Speicherfehler zu verhindern, die bei der Installation mehrerer Pakete mit Cloud Docker für Commerce aufgetreten sind. [Fehlerbehebung eingereicht von Mohan Elamurugan von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![Fix-Symbol](../../assets/fix.svg) Es wurde Unterstützung für mehrere MySQL-Benutzer in der Cloud Docker-Umgebung hinzugefügt. In früheren Versionen schlug der `build:compose` fehl, wenn in der `magento.app.yaml` mehrere Datenbankbenutzer angegeben waren. [Fehlerbehebung eingereicht von G Arvind von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![Fix-Symbol](../../assets/fix.svg) Die `rsyslog` wurden aus dem Cloud Docker für Commerce PHP-Container entfernt, um Kompatibilitätsprobleme zu beheben, die zu Warnbenachrichtigungen während der Bereitstellung führten. Cloud Docker verwendet nicht das Dienstprogramm rsyslog.<!--MCLOUD-6173-->

## v1.0.0

Veröffentlichungsdatum: 5. Februar 2020

- ![neues Symbol](../../assets/new.svg) **Separates Paket zur Bereitstellung von`Cloud Docker for Commerce`** erstellt - Quellcode zur Bereitstellung von Cloud Docker für Commerce aus dem `ece-tools`-Repository in das [neue `magento-cloud-docker`-Repository](https://github.com/magento/magento-cloud-docker) verschoben, um die Code-Qualität zu erhalten und unabhängige Versionen bereitzustellen. Das neue Paket ist eine Abhängigkeit für ECE-Tools v2002.1.0 und höher.

  Wenn Sie die ECE-Tools aktualisieren, aktualisieren Sie auch das `magento/magento-cloud-docker`-Paket auf Version 1.0.0. Wenn Sie Cloud Docker für Commerce mit einer früheren `ece-tools`-Version (2002.0.x) verwendet haben, überprüfen Sie die [Abwärtsinkompatibilitäten](backward-incompatible-changes.md) und aktualisieren Sie Ihr Projekt nach Bedarf als Skripte, Befehle und Prozesse.

- ![neues Symbol](../../assets/new.svg) **Versionierung der Docker-Images hinzugefügt** - Sie müssen jetzt das `magento/magento-cloud-docker`-Paket aktualisieren, um die aktualisierten Images zu erhalten.<!--MAGECLOUD-4737-->

- ![neues Symbol](../../assets/new.svg) **Container-Aktualisierungen**—

   - ![Neues Symbol](../../assets/new.svg) **PHP-FPM-Container**—

      - ![neues Symbol](../../assets/new.svg) **Hinzugefügte Node.js-Unterstützung** - Das PHP-FPM-Image wurde aktualisiert, um die Funktionen node, npm und grunt-cli im PHP-Container zu unterstützen.<!--MAGECLOUD-3953-->

      - ![neues Symbol](../../assets/new.svg) **Unterstützung für [ionCube hinzugefügt](https://www.ioncube.com/)** - Die standardmäßige Docker-Konfiguration wurde aktualisiert, um ionCube in der lokalen Docker-Entwicklungsumgebung zu unterstützen.<!--MAGECLOUD-4354-->

   - ![Neues Symbol](../../assets/new.svg) **Web-Container**—

      - ![neues Symbol](../../assets/new.svg) **NGINX-Konfiguration anpassen** - Es wurde die Möglichkeit hinzugefügt, eine benutzerdefinierte `nginx.conf` in der Cloud Docker für Commerce-Umgebung zu mounten. Siehe [Web-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![neues Symbol](../../assets/new.svg) **Automatisch generierte NGINX-Zertifikate** - Die Docker-Konfigurationsdatei enthält jetzt die Konfiguration zum automatischen Generieren von NGINX-Zertifikaten für den Web-Container.<!--MAGECLOUD-4258-->

   - ![neues Symbol](../../assets/new.svg) **Neuer Selenium-Container** - Ein [Selenium-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container) wurde hinzugefügt, um Adobe Commerce-Anwendungstests mithilfe des Magento Functional Testing Framework (MFTF) zu unterstützen.<!--MAGECLOUD-4040-->

   - ![neues Symbol](../../assets/new.svg) Unterstützung der **[!DNL RabbitMQ]-** - Die [!DNL RabbitMQ]-Container-Konfiguration wurde aktualisiert, um [!DNL RabbitMQ] Version 3.8 zu unterstützen.<!--MAGECLOUD-4674-->

   - ![Fix-Symbol](../../assets/fix.svg) **Persistenter Datenbank-Container** - Das `magento-db: /var/lib/mysql` Datenbankvolume bleibt jetzt bestehen, nachdem Sie die Docker-Konfiguration angehalten und entfernt haben, und wird beim Neustart der Docker-Konfiguration wiederhergestellt. Jetzt müssen Sie das Datenbankvolume manuell löschen. Siehe [Datenbank-Container].<!--MAGECLOUD-3978-->

   - ![neues Symbol](../../assets/new.svg) **TLS-Container**—

      - ![neues Symbol](../../assets/new.svg) **Das Container-Basisbild wurde aktualisiert, um das offizielle Bild zu verwenden**—Das [Cloud TLS-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container)-Bild basiert jetzt auf dem offiziellen `debian:jessie` Docker-Bild.—<!--MAGECLOUD-4163-->

      - ![neues Symbol](../../assets/new.svg) **Es wurde Unterstützung für den [Pound TLS Termination Proxy]** hinzugefügt - Die [Pound-Konfigurationsdatei](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) fügt die folgenden ENV-Variablen hinzu, um die Docker-Konfiguration für den TLS-Container anzupassen:

         - **`TimeOut`** (Time to First Byte) - Setzt den Wert TTFB (Time to First Byte) für die Zeitüberschreitung. Der Standardwert ist 300 Sekunden.

         - **`RewriteLocation`** - Bestimmt, ob der Proxy „pound“ den Speicherort standardmäßig auf die Anfrage-URL umschreibt. Die Standardeinstellung ist `0`, um zu verhindern, dass durch die Umschreibung Umleitungen zu externen Websites wie einer externen SSO-Site unterbrochen werden. [Fehlerbehebung eingereicht von Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![neues Symbol](../../assets/new.svg) Der Zeitüberschreitungswert in der TLS-Container-Konfiguration wurde von 15 auf 300 Sekunden erhöht. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![Neues Symbol](../../assets/new.svg) **Lackcontainer**—

      - ![neues Symbol](../../assets/new.svg) **Das Container-Basisbild wurde aktualisiert, um das offizielle Bild zu verwenden**—Der [Cloud-Lackierungs-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) basiert jetzt auf dem offiziellen `centos` Docker-Bild.<!--MAGECLOUD-4163-->

      - ![neues Symbol](../../assets/new.svg) **Verbesserte standardmäßige Zeitüberschreitungskonfiguration**-`.first_byte_timeout` und `.between_bytes_timeout`-Konfiguration zum Lackierungs-Container hinzugefügt. Beide Zeitüberschreitungswerte sind standardmäßig auf `300s` (5 Minuten) eingestellt. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![Fehlerbehebungssymbol](../../assets/fix.svg) **Fehler während Xdebug-Sitzungen überspringen** - Die Konfiguration des Klarlack-Containers wurde aktualisiert, sodass `pass` auf Anfragen zurückgegeben werden, die empfangen werden, wenn Xdebug aktiviert ist. In früheren Versionen konnten Sie Xdebug nicht verwenden, wenn die Docker-Umgebung Varnish enthielt. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![neues Symbol](../../assets/new.svg) **Docker-Konfigurationsänderungen**—

   - ![neues Symbol](../../assets/new.svg) **Bereitstellungen und Volumes für Ihr Projekt verwalten** Die Möglichkeit zum Verwalten von Bereitstellungen und Volumes beim Starten einer Docker-Umgebung für die lokale Entwicklung wurde hinzugefügt. Siehe [Freigeben von Projektdaten].<!--MAGECLOUD-3248-->

   - ![neues Symbol](../../assets/new.svg) **Unterstützung für den Netzwerk-Bridge-Modus** - Es wurde Unterstützung für den Netzwerk-Bridge-Modus hinzugefügt, um Verbindungen zwischen Docker-Containern über das lokale Netzwerk zu aktivieren.<!--MAGECLOUD-4165-->

   - ![neues Symbol](../../assets/new.svg) **Cron-Container standardmäßig deaktiviert** - Um die Leistung zu verbessern, ist der Cron-Container beim Erstellen der Docker-Umgebung nicht mehr standardmäßig konfiguriert. Sie können die `--with-cron`-Option im Docker-Build-Befehl verwenden, um Ihrer Umgebung einen Cron-Container hinzuzufügen. Siehe [Verwalten von Cron-Aufträgen](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/).<!--MAGECLOUD-5181-->

   - ![neues Symbol](../../assets/new.svg) **Beenden der Synchronisierung großer Backup-Dateien** - Es wurden DB-Dumps und Archivdateien - ZIP, SQL, GZ und BZ2 - zur Ausschlussliste in den `dist/docker-sync.yml` und `dist/mutagen.sh` Dateien hinzugefügt. Das Synchronisieren großer Dateien (>1 GB) kann zu Inaktivität führen, und Backup-Dateien müssen normalerweise nicht synchronisiert werden, da sie neu generiert werden können.<!--MAGECLOUD-3979-->

- ![Neues Symbol](../../assets/new.svg) **Befehlsänderungen**—

   - ![Fix-Symbol](../../assets/fix.svg) Die `./bin/docker`-Datei wurde umbenannt, um ein Problem zu `./bin/magento-docker`, das dazu führte, dass einige Docker-Umgebungen nicht mehr funktionierten, da die `./bin/docker`-Datei vorhandene Docker-Binärdateien überschreibt. Dies ist eine [abwärtsinkompatible Änderung](backward-incompatible-changes.md) die Aktualisierungen Ihrer Skripte und Befehle erfordert.<!-- MAGECLOUD-4038 -->

   - ![neues Symbol](../../assets/new.svg) **Es wurde eine Service-Konfigurationsoption hinzugefügt, um den Datenbank-Port für den Host verfügbar zu machen** - Verwenden Sie die `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>`-Option, um den Datenbank-Port für den Host beim Erstellen der `docker-compose.yml`-Datei verfügbar zu machen: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![neues Symbol](../../assets/new.svg) **neuer Post-Bereitstellungsbefehl**—Zuvor wurden die in der `.magento.app.yaml`-Datei definierten Post-Bereitstellungs-Hooks automatisch ausgeführt, nachdem Sie Adobe Commerce mithilfe des `cloud-deploy`-Befehls in einem Cloud Docker-Container bereitgestellt hatten. Jetzt müssen Sie einen separaten `cloud-post-deploy`-Befehl ausführen, um die Hooks nach der Bereitstellung auszuführen. Siehe die aktualisierten Launch-Anweisungen für [Entwickler](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) und [&#128279;](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/)Produktionsmodus.<!--MAGECLOUD-3996-->

   - ![neues Symbol](../../assets/new.svg) Die Option `--rm` wurde hinzugefügt, um Befehle für die Build- und Bereitstellungs-Container zu `./bin/magento-docker`. Dadurch wird der Container entfernt, nachdem die Aufgabe abgeschlossen ist.<!--MAGECLOUD-4205-->

   - ![neues Symbol](../../assets/new.svg) **Aktualisierungen `build:compose` Befehls**—

      - ![neues Symbol](../../assets/new.svg) Dem `--sync-engine="native"`-Befehl wurde die Option `docker-build` hinzugefügt, um die Dateisynchronisierung zu deaktivieren, wenn Sie die Docker Compose-Konfigurationsdatei im Entwicklermodus generieren. Verwenden Sie diese Option bei der Entwicklung auf Linux-Systemen, für die keine Dateisynchronisierung für die lokale Docker-Entwicklung erforderlich ist. Siehe [Synchronisieren von Daten in der Docker-Umgebung](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![neues Symbol](../../assets/new.svg) Die Standardeinstellung für die Dateisynchronisierung wurde von `docker-sync` in `native` geändert. [Fehlerbehebung eingereicht von Mathew Beane von Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![neues Symbol](../../assets/new.svg) **Validierungsverbesserungen**—

   - ![neues Symbol](../../assets/new.svg) Es wurde eine Validierung zum Bereitstellungsprozess für lokale Docker-Entwicklungsumgebungen hinzugefügt, um zu überprüfen, ob die Cloud-Umgebungskonfiguration den zum Entschlüsseln der Datenbank erforderlichen Verschlüsselungsschlüssel enthält. Jetzt erhalten Sie eine Fehlermeldung im Protokoll, wenn die Umgebungskonfiguration keinen Wert für den Verschlüsselungsschlüssel angibt.<!--MAGECLOUD-4423-->

   - ![neues Symbol](../../assets/new.svg) Es wurde eine Container-Konsistenzprüfung zum Elasticsearch-Service hinzugefügt, um sicherzustellen, dass der Service bereit ist, bevor die Build- und Bereitstellungsverarbeitung fortgesetzt wird. Wenn die Konsistenzprüfung einen Fehler zurückgibt, wird der Container automatisch neu gestartet.<!--MAGECLOUD-4456-->
