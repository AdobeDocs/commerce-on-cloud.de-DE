---
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# ECE-Tools-Fehlercodes

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Kritische Fehler

Kritische Fehler weisen auf ein Problem mit der Projektkonfiguration von Commerce in der Cloud-Infrastruktur hin, das zu Bereitstellungsfehlern führt, z. B. falsche, nicht unterstützte oder fehlende Konfiguration für erforderliche Einstellungen. Bevor Sie bereitstellen können, müssen Sie die Konfiguration aktualisieren, um diese Fehler zu beheben.

### Build-Phase

| Fehlercode | Build-Schritt | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 2 |  | Schreiben in die `./app/etc/env.php`-Datei nicht möglich | Das Bereitstellungsskript kann die erforderlichen Änderungen an der `/app/etc/env.php`-Datei nicht vornehmen. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 3 |  | Konfiguration ist nicht in der `schema.yaml` definiert | Die Konfiguration ist nicht in der `./vendor/magento/ece-tools/config/schema.yaml` definiert. Überprüfen Sie, ob der Name der Konfigurationsvariablen korrekt und definiert ist. |
| 4 |  | Die `.magento.env.yaml` konnte nicht analysiert werden | Das `./.magento.env.yaml` Dateiformat ist ungültig. Verwenden Sie einen YAML-Parser, um die Syntax zu überprüfen und Fehler zu beheben. |
| 5 |  | Die `.magento.env.yaml` kann nicht gelesen werden | Die `./.magento.env.yaml` kann nicht gelesen werden. Überprüfen Sie die Dateiberechtigungen. |
| 6 |  | Die `.schema.yaml` kann nicht gelesen werden | Die `./vendor/magento/ece-tools/config/magento.env.yaml` kann nicht gelesen werden. Überprüfen Sie die Dateiberechtigungen und stellen Sie erneut bereit (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | In die `./app/etc/config.php` Datei kann nicht geschrieben werden | Das Implementierung Skript kann keine erforderlichen Änderungen an der `/app/etc/config.php` Datei vornehmen. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 8 | validate-config | Die Datei kann nicht `composer.json` gelesen werden. | Die `./composer.json` Datei kann nicht gelesen werden. Überprüfen Sie die Dateiberechtigungen. |
| 9 | validate-config | In der `composer.json` Datei fehlt der erforderliche Abschnitt für das automatische Laden | `autoload` Erforderlich Abschnitt fehlt in der `composer.json` Datei. Vergleichen Sie den Abschnitt &quot;Autoload&quot; mit der `composer.json` Datei im Cloud-Vorlage und fügen Sie die fehlende Konfiguration hinzu. |
| 10 | validate-config | Die `.magento.env.yaml` enthält eine Option, die im Schema nicht deklariert ist, oder eine Option, die mit einem ungültigen Wert oder Schritt konfiguriert wurde | Die `./.magento.env.yaml` enthält eine ungültige Konfiguration. Ausführliche Informationen finden Sie im Fehlerprotokoll. |
| 11 | refresh-modules | Befehl fehlgeschlagen: `/bin/magento module:enable --all` | Versuchen Sie, `composer update` lokal auszuführen. Übertragen Sie dann die aktualisierte `composer.lock`-Datei per Push. Überprüfen Sie außerdem die `cloud.log` , um weitere Informationen zu erhalten. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 12 | apply patches | Patch konnte nicht angewendet werden |  |
| 13 | set-report-dir-nesting-level | Schreiben in die Datei `/pub/errors/local.xml` nicht möglich |  |
| 14 | copy-sample-data | Beispieldatendateien konnten nicht kopiert werden |  |
| 15 | compile-di | Befehl fehlgeschlagen: `/bin/magento setup:di:compile` | Weitere Informationen finden Sie hier `cloud.log` . Fügen Sie `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` hinzu, um eine detailliertere Befehlsausgabe zu erhalten. |
| 16 | dump-autoload | Befehl fehlgeschlagen: `composer dump-autoload` | Der `composer dump-autoload` ist fehlgeschlagen. Weitere Informationen finden Sie in der `cloud.log`. |
| 17 | Ballenpresse | Der Befehl zum Ausführen von `Baler` für die JavaScript-Bündelung ist fehlgeschlagen | Überprüfen Sie die Umgebungsvariable `SCD_USE_BALER` , um sicherzustellen, dass das Modul Baler für die JS-Bündelung konfiguriert und aktiviert ist. Wenn Sie das Modul Ballenpresse nicht benötigen, stellen Sie `SCD_USE_BALER: false` ein. |
| 18 | compress-static-content | Erforderliches Dienstprogramm nicht gefunden (Zeitüberschreitung, Bash) |  |
| 19 | deploy-static-content | `/bin/magento setup:static-content:deploy` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 20 | compress-static-content | Statische Inhaltskomprimierung fehlgeschlagen | Weitere Informationen finden Sie hier `cloud.log` . |
| 21 | backup-data: static-content | Statischer Inhalt konnte nicht in das `init` kopiert werden | Weitere Informationen finden Sie in der `cloud.log`. |
| 22 | Backup-data: writable-dirs | Fehlgeschlagen, um einige beschreibbare Verzeichnisse in das `init` Verzeichnis zu kopieren | Fehlgeschlagen, um beschreibbare Verzeichnisse in den `./init` Ordner zu kopieren. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 23 |  | Logger-Objekt kann nicht erstellt werden |  |
| 24 | Backup-data: static-Inhalte | Fehlgeschlagen, um das `./init/pub/static/` Verzeichnis zu bereinigen | Fehlgeschlagen, um den Ordner zu bereinigen `./init/pub/static` . Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 25 |  | Composer-Paket wurde nicht gefunden | Wenn Sie die Adobe Commerce-Anwendungsversion direkt aus dem GitHub-Repository installiert haben, überprüfen Sie, ob die Umgebungsvariable `DEPLOYED_MAGENTO_VERSION_FROM_GIT` konfiguriert ist. |
| 26 | validate-config | Entfernen Sie die Magento Braintree-Modulkonfiguration, die in Adobe Commerce und Magento Open Source 2.4 und höheren Versionen nicht mehr unterstützt wird. | Die Unterstützung für das Braintree-Modul ist in Magento 2.4.0 und höher nicht mehr enthalten. Entfernen Sie die __ CONFIG STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL aus dem Variablenabschnitt der `.magento.app.yaml`. Verwenden Sie für die Braintree-Zahlungsunterstützung stattdessen eine offizielle Erweiterung aus der Commerce Marketplace. |

### Staging-Bereitstellung

| Fehlercode | Bereitstellungsschritt | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 101 | Pre-deploy: Cache | Falsche Cache-Konfiguration (fehlender Port oder Host) | In der Cache-Konfiguration fehlen die erforderlichen Parameter `server` oder `port`. Weitere Informationen finden Sie in der `cloud.log`. |
| 102 |  | Schreiben in die `./app/etc/env.php`-Datei nicht möglich | Das Bereitstellungsskript kann die erforderlichen Änderungen an der `/app/etc/env.php`-Datei nicht vornehmen. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 103 |  | Konfiguration ist nicht in der `schema.yaml` definiert | Konfiguration ist nicht in der `./vendor/magento/ece-tools/config/schema.yaml` Datei definiert. Überprüfen Sie, ob der Konfigurationsname Variable korrekt und definiert ist. |
| 104 |  | Die `.magento.env.yaml` konnte nicht analysiert werden | Die Konfiguration ist nicht in der `./vendor/magento/ece-tools/config/schema.yaml` definiert. Überprüfen Sie, ob der Name der Konfigurationsvariablen korrekt ist und ob er definiert ist. |
| 105 |  | Die `.magento.env.yaml` kann nicht gelesen werden | Die `./.magento.env.yaml` kann nicht gelesen werden. Dateiberechtigungen überprüfen. |
| 106 |  | Die `.schema.yaml` kann nicht gelesen werden |  |
| 107 | Pre-deploy: clean-redis-cache | Bereinigung des Redis-Cache fehlgeschlagen | Fehler beim Löschen des Redis-Cache. Vergewissern Sie sich, dass die Redis-Cache-Konfiguration korrekt ist und dass der Redis-Service verfügbar ist. Siehe [Einrichten des Redis-](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/redis). |
| 140 | Pre-deploy: clean-valkey-cache | Bereinigung des Valley-Cache fehlgeschlagen | Fehler beim Löschen des Valley-Cache. Vergewissern Sie sich, dass die Valley-Cache-Konfiguration korrekt ist und dass der Valley-Service verfügbar ist. Siehe [Einrichten des Valkey-](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/valkey). |
| 108 | Pre-Deploy: set-production-mode | `/bin/magento maintenance:enable` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 109 | validate-config | Falsche Datenbankkonfiguration | Überprüfen Sie, ob die `DATABASE_CONFIGURATION` Umgebungsvariable korrekt konfiguriert ist. |
| 110 | validate-config | Falsche Sitzungskonfiguration | Überprüfen Sie, ob die `SESSION_CONFIGURATION` Umgebungsvariable korrekt konfiguriert ist. Die Konfiguration muss mindestens den `save` Parameter enthalten. |
| 111 | validate-config | Falsche suchen Konfiguration | Prüfen Sie, ob das `SEARCH_CONFIGURATION` Umgebung Variable korrekt konfiguriert ist. Die Konfiguration muss mindestens den `engine` Parameter enthalten. |
| 112 | validate-config | Falsche Ressourcenkonfiguration | Überprüfen Sie, ob die `RESOURCE_CONFIGURATION` Umgebungsvariable korrekt konfiguriert ist. Die Konfiguration muss mindestens `connection` Parameter enthalten. |
| 113 | validate-config:elasticsuite-integrität | ElasticSuite ist installiert, aber der Elasticsearch-Service ist nicht verfügbar | Überprüfen Sie, ob die `SEARCH_CONFIGURATION` Umgebungsvariable korrekt konfiguriert ist, und stellen Sie sicher, dass der Elasticsearch-Service verfügbar ist. |
| 114 | validate-config:elasticsuite-integrität | ElasticSuite ist installiert, aber es wird eine andere Suchmaschine verwendet | ElasticSuite ist installiert, aber es ist eine andere Suchmaschine konfiguriert. Aktualisieren Sie die Umgebungsvariable `SEARCH_CONFIGURATION` , um Elasticsearch zu aktivieren, und überprüfen Sie die Konfiguration des Elasticsearch-Services in der `services.yaml`. |
| 115 |  | Ausführung der Datenbankabfrage fehlgeschlagen |  |
| 116 | install-update: setup | `/bin/magento setup:install` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log` und im `install_upgrade.log` . Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 117 | install-update: config-import | `app:config:import` fehlgeschlagen | Weitere Informationen finden Sie hier `cloud.log` . Für eine detailliertere Befehlsausgabe fügen Sie die `VERBOSE_COMMANDS: '-vvv'` Option zur `.magento.env.yaml` Datei hinzu. |
| 118 |  | Erforderliches Dienstprogramm nicht gefunden (Zeitüberschreitung, Bash) |  |
| 119 | install-update: deploy-static-content | `/bin/magento setup:static-content:deploy` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 120 | compress-static-content | Statische Inhaltskomprimierung fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. |
| 121 | deploy-static-content:generate | Die bereitgestellte Version kann nicht aktualisiert werden | Die `./pub/static/deployed_version.txt` kann nicht aktualisiert werden. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 122 | clean-static-content | Bereinigung von statischen Inhaltsdateien fehlgeschlagen |  |
| 123 | install-update: split-db | `/bin/magento setup:db-schema:split` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 124 | clean-view-preprocessing | Bereinigung des `var/view_preprocessed` Ordners fehlgeschlagen | Der `./var/view_preprocessed` kann nicht bereinigt werden. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 125 | install-update: reset-password | Fehler beim Aktualisieren der `/var/credentials_email.txt` | Fehler beim Aktualisieren der `/var/credentials_email.txt`. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 126 | install-update: update | `/bin/magento setup:upgrade` Befehl fehlgeschlagen | Weitere Informationen finden Sie unter `cloud.log` `install_upgrade.log` Für eine detailliertere Befehlsausgabe fügen Sie die `VERBOSE_COMMANDS: '-vvv'` Option zur `.magento.env.yaml` Datei hinzu. |
| 127 | clean-cache | `/bin/magento cache:flush` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Um eine detailliertere Befehlsausgabe zu erhalten, fügen Sie die Option `VERBOSE_COMMANDS: '-vvv'` zur `.magento.env.yaml` hinzu. |
| 128 | disable-maintenance-mode | `/bin/magento maintenance:disable` fehlgeschlagen | Weitere Informationen finden Sie in der `cloud.log`. Fügen Sie `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` hinzu, um eine detailliertere Befehlsausgabe zu erhalten. |
| 129 | install-update: reset-password | Kennwortvorlage kann nicht gelesen werden |  |
| 130 | install-update: cache_type | Befehl fehlgeschlagen: `php ./bin/magento cache:enable` | Der Befehl `php ./bin/magento cache:enable` wird nur ausgeführt, wenn Adobe Commerce installiert wurde, `./app/etc/env.php` Datei jedoch zu Beginn der Bereitstellung fehlte oder leer war. Weitere Informationen finden Sie in der `cloud.log`. Fügen Sie `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` hinzu, um eine detailliertere Befehlsausgabe zu erhalten. |
| 131 | install-update | Der Wert des `crypt/key`-Schlüssels ist nicht in der `./app/etc/env.php`-Datei oder der `CRYPT_KEY` Cloud-Umgebungsvariablen vorhanden | Dieser Fehler tritt auf, wenn die `./app/etc/env.php`-Datei zu Beginn der Adobe Commerce-Bereitstellung nicht vorhanden ist oder der `crypt/key` nicht definiert ist. Wenn Sie die Datenbank aus einer anderen Umgebung migriert haben, rufen Sie den Wert des Verschlüsselungsschlüssels aus dieser Umgebung ab. Fügen Sie dann den Wert zur Cloud[Umgebungsvariablen „CRYPT_KEY](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#crypt_key) in Ihrer aktuellen Umgebung hinzu. Siehe [Adobe Commerce-Verschlüsselungsschlüssel](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview#gather-credentials). Wenn Sie die Datei `./app/etc/env.php` versehentlich entfernt haben, verwenden Sie den folgenden Befehl, um sie aus den Sicherungsdateien einer früheren Bereitstellung wiederherzustellen: `./vendor/bin/ece-tools backup:restore` CLI-Befehl.“ |
| 132 |  | Verbindung zum Elasticsearch-Service nicht möglich | Überprüfen Sie, ob gültige Elasticsearch-Anmeldeinformationen vorhanden sind und ob der Dienst ausgeführt wird |
| 137 |  | Es kann keine Verbindung zum OpenSearch-Dienst hergestellt werden. | Überprüfen Sie, ob gültige OpenSearch-Anmeldeinformationen vorhanden sind, und vergewissern Sie sich, dass der Dienst ausgeführt wird |
| 133 | validate-config | Entfernen Sie die Magento Braintree-Modulkonfiguration, die in Adobe Commerce oder Magento Open Source 2.4 und höheren Versionen nicht mehr unterstützt wird. | Die Unterstützung für das Braintree-Modul ist nicht mehr in Adobe Commerce oder Magento Open Source 2.4.0 und höher enthalten. Entfernen Sie die __ CONFIG STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL aus dem Variablenabschnitt der `.magento.app.yaml`. Verwenden Sie für den Braintree-Support stattdessen eine offizielle Braintree Payments-Erweiterung aus der Commerce Marketplace. |
| 134 | validate-config | Für Adobe Commerce und Magento Open Source 2.4.0 muss der Elasticsearch-Dienst installiert sein | Installieren des Elasticsearch-Service |
| 138 | validate-config | Für Adobe Commerce und Magento Open Source 2.4.4 muss OpenSearch oder der Elasticsearch-Service installiert sein | OpenSearch-Service installieren |
| 135 | validate-config | Die Suchmaschine muss für Adobe Commerce und Magento Open Source auf Elasticsearch eingestellt sein >= 2.4.0 | Überprüfen Sie die Variable SEARCH_CONFIGURATION für die Option `engine` . Wenn sie konfiguriert ist, entfernen Sie die Option oder setzen den Wert auf „elasticsearch“. |
| 136 | validate-config | Die Split-Datenbank wurde ab Adobe Commerce und Magento Open Source 2.5.0 entfernt. | Wenn Sie eine geteilte Datenbank verwenden, müssen Sie zu einer einzelnen Datenbank zurückkehren oder zu dieser migrieren oder einen alternativen Ansatz verwenden. |
| 139 | validate-config | Falsche Suchmaschine | Diese Adobe Commerce- oder Magento Open Source-Version unterstützt OpenSearch nicht. Verwenden Sie die Versionen 2.3.7-p3, 2.4.3-p2 oder höher |

### Phase nach der Bereitstellung

| Fehlercode | Schritt Post Bereitstellung | Fehler Beschreibung (Titel) | Empfohlene Maßnahme |
| - | - | - | - |
| 201 | is-deploy-failed | Bereitstellungsphase fehlgeschlagen |  |
| 202 |  | Die `./app/etc/env.php`-Datei kann nicht beschrieben werden | Das Bereitstellungsskript kann die erforderlichen Änderungen an der `/app/etc/env.php`-Datei nicht vornehmen. Überprüfen Sie Ihre Dateisystemberechtigungen. |
| 203 |  | Konfiguration ist nicht in der `schema.yaml` definiert | Die Konfiguration ist nicht in der `./vendor/magento/ece-tools/config/schema.yaml` definiert. Überprüfen Sie, ob der Name der Konfigurationsvariablen korrekt ist und ob er definiert ist. |
| 204 |  | Die `.magento.env.yaml` konnte nicht analysiert werden | Das `./.magento.env.yaml` Dateiformat ist ungültig. Verwenden Sie einen YAML-Parser, um die Syntax zu überprüfen und Fehler zu beheben. |
| 205 |  | Die `.magento.env.yaml` kann nicht gelesen werden | Dateiberechtigungen überprüfen. |
| 206 |  | Die `.schema.yaml` kann nicht gelesen werden |  |
| 207 | Aufwärmen | Einige Aufwärmseiten konnten nicht vorgeladen werden |  |
| 208 | Time-to-First-Byte | Zeit bis zum ersten Byte (TTFB) konnte nicht getestet werden |  |
| 227 | clean-cache | `/bin/magento cache:flush` Befehl fehlgeschlagen | Weitere Informationen finden Sie hier `cloud.log` . `VERBOSE_COMMANDS: '-vvv'` hinzufügen für `.magento.env.yaml` eine detailliertere Befehlsausgabe. |

### Allgemein

| Fehlercode | Allgemeine Schritte | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 243 |  | Konfiguration ist nicht in der `schema.yaml` definiert | Überprüfen Sie, ob der Name der Konfigurationsvariablen korrekt ist und ob er definiert ist. |
| 244 |  | Die `.magento.env.yaml` konnte nicht analysiert werden | Das `./.magento.env.yaml` Dateiformat ist ungültig. Verwenden Sie einen YAML-Parser, um die Syntax zu überprüfen und Fehler zu beheben. |
| 245 |  | Die `.magento.env.yaml` kann nicht gelesen werden | Die `./.magento.env.yaml` kann nicht gelesen werden. Dateiberechtigungen überprüfen. |
| 246 |  | Die `.schema.yaml` kann nicht gelesen werden |  |
| 247 |  | Es kann kein Modul für das Ereignis generiert werden | Weitere Informationen finden Sie in der `cloud.log`. |
| 248 |  | Modul kann nicht für das Ereignis aktiviert werden | Weitere Informationen finden Sie in der `cloud.log`. |
| 249 |  | Das Modul „AdobeCommerceWebhookPlugins“ konnte nicht generiert werden | Weitere Informationen finden Sie in der `cloud.log`. |
| 250 |  | Das AdobeCommerceWebhookPlugins-Modul konnte nicht aktiviert werden | Weitere Informationen finden Sie hier `cloud.log` . |

## Warnung Fehler

Warnung Fehler weisen auf eine Herausforderung mit der Konfiguration von Commerce für Cloud-Infrastruktur-Projekt hin, z. B. falsche, veraltete, nicht unterstützte oder fehlende Konfigurationseinstellungen für optionale Funktionen, die sich auf den Websitebetrieb auswirken können. Obwohl eine Warnung nicht Implementierung Fehler verursacht, sollten Sie die Warnmeldungen überprüfen und die Konfiguration aktualisieren, um sie zu beheben.

### Build Schritt

| Fehlercode | Build-Schritt | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 1001 | validate-config | Die Datei app/etc/config.php existiert nicht |  |
| 1002 | validate-config | Die .Die Datei /build_options.ini wird nicht mehr unterstützt |  |
| 1003 | validate-config | Der Modulabschnitt fehlt in der freigegebenen Konfigurationsdatei |  |
| 1004 | validate-config | Die Konfiguration ist mit dieser Version von Magento nicht kompatibel |  |
| 1005 | validate-config | SCD-Optionen ignoriert |  |
| 1006 | validate-config | Der konfigurierte Status ist nicht ideal |  |
| 1007 | Ballenpresse | Ballenpresse JS-Bündelung kann nicht verwendet werden |  |

### Bereitstellen Schritt

| Fehlercode | Bereitstellungsschritt | Fehler Beschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 2001 | Pre-deploy:cache | Der Cache ist für einen Redis-Service konfiguriert, der nicht verfügbar ist. Konfiguration wird ignoriert. |  |
| 2032 | Pre-deploy:cache | Der Cache ist für einen Valley-Service konfiguriert, der nicht verfügbar ist. Konfiguration wird ignoriert. |  |
| 2002 | validate-config | Der konfigurierte Status ist nicht ideal |  |
| 2003 | validate-config | Der Wert der Verzeichnisverschachtelungsebene für die Fehlerberichterstattung wurde nicht konfiguriert |  |
| 2004 | validate-config | Ungültige Konfiguration in der .Datei /pub/errors/local.xml. |  |
| 2005 | validate-config | Admin-Daten werden nur während der Erstinstallation zum Erstellen eines Admin-Benutzers verwendet. Alle Änderungen an den Admin-Daten werden während des Upgrades ignoriert. | Nach der ersten Installation können Sie Admin-Daten aus der Konfiguration entfernen. |
| 2006 | validate-config | Admin-Benutzer wurde nicht erstellt, da keine Admin-E-Mail festgelegt wurde | Nach der Installation können Sie manuell einen Admin-Benutzer erstellen: Verwenden Sie ssh, um eine Verbindung zu Ihrer Umgebung herzustellen. Führen Sie dann den Befehl `bin/magento admin:user:create` aus. |
| 2007 | validate-config | PHP-Version auf empfohlene Version aktualisieren |  |
| 2008 | validate-config | Solr-Unterstützung ist in Adobe Systems Commerce und Magento Open Source 2.1 veraltet. |  |
| 2009 | validate-config | Solr wird von Adobe Commerce und Magento Open Source 2.2 oder höher nicht mehr unterstützt. |  |
| 2010 | validate-config | Der Elasticsearch-Service wird auf Infrastrukturebene installiert, er wird jedoch nicht als Suchmaschine verwendet. | Erwägen Sie, den Elasticsearch-Service aus der Infrastrukturschicht zu entfernen, um die Ressourcennutzung zu optimieren. |
| 2011 | validate-config | Die Version des Elasticsearch-Services auf der Infrastrukturschicht ist nicht mit der aktuellen Version des Elasticsearch/Elasticsearch-Moduls kompatibel, das von Ihrer Adobe Commerce-Anwendung verwendet wird. |  |
| 2012 | validate-config | Die aktuelle Konfiguration ist mit dieser Version von Adobe Commerce nicht kompatibel |  |
| 2013 | validate-config | SCD-Optionen wurden ignoriert, da der Bereitstellungsprozess in der Build-Phase nicht ausgeführt wurde |  |
| 2014 | validate-config | Die Konfiguration enthält veraltete Variablen oder Werte |  |
| 2015 | validate-config | Umgebungskonfiguration ist ungültig |  |
| 2016 | validate-config | JSON-Typkonfiguration kann nicht dekodiert werden |  |
| 2017 | validate-config | Die aktuelle Konfiguration ist mit dieser Version von Adobe Commerce nicht kompatibel |  |
| 2018 | validate-config | Einige Services wurden eingestellt |  |
| 2019 | validate-config | Die Konfigurationsoption MySQL-Suche ist veraltet | Verwenden Sie stattdessen Elasticsearch . |
| 2029 | validate-config | Die Split-Datenbank ist seit Adobe Commerce und Magento Open Source 2.4.2 veraltet und wird in 2.5 entfernt. | Wenn Sie eine geteilte Datenbank verwenden, sollten Sie mit der Planung beginnen, zu einer einzelnen Datenbank zurückzukehren oder zu ihr zu migrieren oder einen alternativen Ansatz verwenden. |
| 2020 | install-update | Adobe Commerce-Installation abgeschlossen, aber die `app/etc/env.php`-Konfigurationsdatei fehlte oder war leer. | Erforderliche Daten werden aus Umgebungskonfigurationen und aus der Datei &quot;.magento.env.yaml“ wiederhergestellt. |
| 2021 | install-update:db-connection | Für aufgeteilte Datenbanken, die für benutzerdefinierte Verbindungen verwendet werden |  |
| 2022 | install-update:db-connection | Sie haben zu einer Datenbankkonfiguration gewechselt, die nicht mit der Slave-Verbindung kompatibel ist. |  |
| 2023 | install-update:split-db | Das Aktivieren einer geteilten Datenbank wird übersprungen. |  |
| 2024 | install-update:split-db | In der Variable SPLIT_DB fehlt die Konfiguration für Verbindungsarten, die aufgeteilt werden sollen. |  |
| 2025 | install-update:split-db | Slave-Verbindung nicht festgelegt. |  |
| 2026 | pre-deploy:writable-dirs wiederherstellen | Fehlgeschlagen, um einige während der Build Phase generierte Daten in den bereitgestellten Verzeichnissen wiederherzustellen | Weitere Informationen finden Sie hier `cloud.log` . |
| 2027 | validate-config:image-mode-variable | Moduswert für die Umgebungsvariable MAGE_MODE wird nicht unterstützt | Entfernen Sie die Umgebungsvariable MAGE_MODE oder ändern Sie ihren Wert in „production“. Adobe Commerce in der Cloud-Infrastruktur unterstützt nur den „Produktions“-Modus. |
| 2028 | Remote-Speicher | Der Remotespeicher konnte nicht aktiviert werden. | Anmeldedaten für Remote-Speicher überprüfen. |
| 2030 | validate-config | Elasticsearch- und OpenSearch-Services werden beide auf Infrastrukturebene installiert. Adobe Commerce und Magento Open Source 2.4.4 und höher verwenden standardmäßig OpenSearch | Erwägen Sie, den Elasticsearch- oder OpenSearch-Service aus der Infrastrukturschicht zu entfernen, um die Ressourcennutzung zu optimieren. |

### Phase nach der Bereitstellung

| Fehlercode | Schritt nach der Bereitstellung | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 3001 | validate-config | Die Debug-Protokollierung ist in Adobe Commerce aktiviert | Aktivieren Sie keine Debug-Protokollierung für Ihre Produktionsumgebungen, um Speicherplatz zu sparen. |
| 3002 | Anlauf | Geschäft URLs können nicht abgerufen werden |  |
| 3003 | Aufwärmen | Store-URL kann nicht abgerufen werden |  |
| 3004 | Sicherung | Backup-Dateien können nicht erstellt werden |  |

### Allgemein

| Fehlercode | Allgemeine Schritte | Fehlerbeschreibung (Titel) | Vorgeschlagene Aktion |
| - | - | - | - |
| 4001 |  | Die Anzahl der Systemprozessoren kann nicht abgerufen werden: |  |
