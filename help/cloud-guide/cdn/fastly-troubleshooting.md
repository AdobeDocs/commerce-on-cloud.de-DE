---
title: Schnelle Fehlerbehebung
description: Erfahren Sie, wie Sie Probleme mit dem Fastly CDN-Modul und den Services für Adobe Commerce beheben und verwalten können.
feature: Cloud, Configuration, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Schnelle Fehlerbehebung

Verwenden Sie die folgenden Informationen, um das Fastly CDN-Modul für Magento 2 in Ihren Adobe Commerce in Cloud-Infrastrukturprojektumgebungen zu beheben und zu verwalten. Sie können beispielsweise die Werte der Antwort-Header und das Caching-Verhalten untersuchen, um Service- und Leistungsprobleme von Fastly zu beheben.

In Pro-Produktions- und Staging-Umgebungen können Sie [New Relic-Protokolle](../monitor/log-management.md) verwenden, um Fastly CDN- und WAF-Protokolldaten anzuzeigen und zu analysieren, um Fehler und Leistungsprobleme zu beheben.

>[!NOTE]
>
>Informationen zum Einrichten und Konfigurieren von Fastly finden Sie unter [Schnelles Einrichten](fastly.md).

## Fastly-Service-ID suchen

Sie benötigen die Fastly-Service-ID, um Fastly vom Administrator zu konfigurieren oder Fastly-API-Anfragen für erweiterte Fastly-Konfiguration und Fehlerbehebung zu senden.

Wenn Fastly in Ihrer Projektumgebung aktiviert ist, können Sie die Service-ID vom Administrator abrufen. Siehe [Abrufen von Fastly-Anmeldeinformationen](fastly-configuration.md#get-fastly-credentials).

Entwickler und Benutzer mit erweiterter VCL können benutzerdefinierte VCL verwenden, um die Service-ID mithilfe der Fastly-Variablen-`req.service_id` abzurufen. Sie können beispielsweise die `req.service_id` zur benutzerdefinierten Protokollierungsanweisung in Ihrer VCL hinzufügen, um den Wert der Service-ID zu erfassen:

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

Sie können dieselbe VCL für Produktions- und Staging-Umgebungen verwenden. Siehe [`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/) in der _Fastly-_.

## Probleme mit Site-Performance, Bereinigung und Cache

Verwenden Sie die folgende Liste, um Probleme im Zusammenhang mit der Fastly-Service-Konfiguration für Ihre Adobe Commerce in einer Cloud-Infrastrukturumgebung zu identifizieren und zu beheben.

- **Das Menü „Store“ wird nicht angezeigt oder**. Möglicherweise verwenden Sie einen Link oder einen temporären Link direkt zum ursprünglichen Server, anstatt die Live-Site-URL zu verwenden, oder Sie haben `-H "host:URL"` in einem [cURL-Befehl verwendet](#check-live-site-through-fastly). Wenn Sie Fastly auf den Ursprungs-Server umgehen, funktioniert das Hauptmenü nicht und es werden falsche Kopfzeilen angezeigt, die das Caching auf der Browser-Seite ermöglichen.

- **Die obere Navigation funktioniert nicht** - Die obere Navigation beruht auf der Edge Side Includes (ESI)-Verarbeitung, die aktiviert wird, wenn Sie die standardmäßigen Magento Fastly-VCL-Snippets hochladen. Wenn die Navigation nicht funktioniert, laden [ die Fastly-VCL hoch ](fastly-configuration.md#upload-vcl-to-fastly) überprüfen Sie die Site erneut.

- **Geo-location/GeoIP funktioniert nicht** - Die standardmäßigen Magento-Fastly-VCL-Snippets hängen den Ländercode an die URL an. Wenn der Länder-Code nicht funktioniert, laden [ die Fastly-VCL hoch ](fastly-configuration.md#upload-vcl-to-fastly) überprüfen Sie die Website erneut.

- **Seiten werden nicht zwischengespeichert** - Standardmäßig speichert Fastly Seiten nicht mit dem `Set-Cookies`-Header zwischen. Adobe Commerce setzt Cookies auch auf zwischenspeicherbaren Seiten (TTL > 0). Die standardmäßige Magento Fastly-VCL entfernt diese Cookies auf zwischenspeicherbaren Seiten. Wenn Seiten nicht zwischengespeichert werden, laden [ die Fastly-VCL hoch ](fastly-configuration.md#upload-vcl-to-fastly) überprüfen Sie die Site erneut.

  Dieses Problem kann auch auftreten, wenn ein Seitenblock in einer Vorlage als nicht Cache-fähig markiert ist. In diesem Fall ist das Problem höchstwahrscheinlich auf ein Drittanbietermodul oder eine Erweiterung zurückzuführen, das bzw. die die Adobe Commerce-Kopfzeilen blockiert oder entfernt. Informationen zum Beheben des Problems finden Sie unter [X-Cache enthält nur MISS, keinen HIT](#x-cache-contains-only-miss-no-hit).

- **Bereinigungsanfragen schlagen fehl** - gibt beim Senden einer Bereinigungsanfrage schnell den folgenden Fehler zurück:

  ```text
  The purge request was not processed successfully.
  ```

  Dieses Problem kann durch eines der folgenden Probleme verursacht werden:

   - Ungültige Fastly-Anmeldeinformationen in der Fastly-Service-Konfiguration für die Adobe Commerce in der Cloud-Infrastrukturprojektumgebung
   - Ungültiger Code in einem benutzerdefinierten VCL-Code

  Informationen zum Beheben des Problems finden Sie unter [Fehler beim Bereinigen des Fastly-Cache in ](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) Cloud“ im Adobe Commerce-Hilfezentrum.

## 503 Fehler von Fastly

Wenn Fastly 503 Zeitüberschreitungsfehler zurückgibt, überprüfen Sie die Fehlerprotokolle und die Fehlerseite 503, um die Grundursache zu identifizieren.

>[!NOTE]
>
>Wenn die Zeitüberschreitung bei der Ausführung von Massenvorgängen auftritt, können Sie [die Zeitüberschreitung bei Fastly für den Administrator erweitern](fastly-custom-cache-configuration.md#extend-fastly-timeout).

Wenn Sie einen 503-Fehler erhalten, überprüfen Sie das Fehlerprotokoll für die Produktions- oder Staging-Umgebung und das PHP-Zugriffsprotokoll, um das Problem zu beheben.

**Überprüfen der Fehlerprotokolle**:

- [Fehlerprotokoll](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  Dieses Protokoll enthält alle Fehler aus der Anwendung oder PHP-Engine, zum Beispiel `memory_limit` oder `max_execution_time exceeded`. Wenn Sie keine Fastly-bezogenen Fehler finden, überprüfen Sie das PHP-Zugriffsprotokoll.

- PHP-Zugriffsprotokoll

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  Durchsuchen Sie das Protokoll nach HTTP-200-Antworten nach der URL, die den 503-Fehler zurückgegeben hat. Wenn Sie die Antwort 200 finden, bedeutet dies, dass Adobe Commerce die Seite fehlerfrei zurückgegeben hat. Dies deutet darauf hin, dass das Problem möglicherweise nach dem Intervall aufgetreten ist, das den in der Fastly-Service-Konfiguration festgelegten `first_byte_timeout` überschreitet.

Wenn ein 503-Fehler auftritt, gibt Fastly den Grund auf der Fehler- und Wartungsseite zurück. Möglicherweise wird der Grund nicht angezeigt, wenn Sie Code für eine [benutzerdefinierte Antwortseite“ ](fastly-custom-response.md). Um den Ursachen-Code auf der Standardfehlerseite anzuzeigen, können Sie den HTML-Code für die benutzerdefinierte Fehlerseite entfernen.

**So überprüfen Sie die Fehlerseite von Fastly 503**:

{{admin-login-step}}

1. Klicken Sie auf **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie im rechten Bereich **Vollständiger Seitencache**.

1. Erweitern Sie im Abschnitt **Fastly** den Eintrag **Benutzerdefinierte synthetische Seiten** wie in der folgenden Abbildung dargestellt.

   ![Benutzerdefinierte 503-Fehlerseite](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Klicken Sie **HTML festlegen**.

1. Entfernen Sie den benutzerdefinierten Code. Sie können sie in einem Textprogramm speichern, um sie später wieder hinzuzufügen.

1. Klicken Sie **Hochladen**, um Ihre Aktualisierungen an Fastly zu senden.

1. Klicken **oben auf** Seite auf „Konfiguration speichern“.

1. Öffnen Sie erneut die URL, die den 503-Fehler verursacht hat. gibt schnell eine Fehlerseite mit dem Grund zurück, wie im folgenden Beispiel gezeigt.

   ![Fastly-Fehler](../../assets/cdn/fastly-503-example.png)

## Apex und Subdomains, die bereits einem Fastly-Konto zugeordnet sind

Wenn die Apex-Domain und Subdomains für Ihr Adobe Commerce in Cloud-Infrastrukturprojekt bereits mit einem bestehenden Fastly-Konto mit zugewiesener Service-ID verknüpft sind, können Sie erst starten, wenn Sie Ihre Fastly-Konfiguration aktualisieren:

- Aktualisieren Sie die Apex- und Subdomain-Konfiguration auf dem vorhandenen Fastly-Konto. Siehe [Mehrere Fastly-Konten und zugeordnete Domains](fastly.md#multiple-fastly-accounts-and-assigned-domains).

- [Aktivieren und Konfigurieren von Fastly](fastly-configuration.md#enable-fastly-caching) und Abschließen der [DNS-Konfiguration](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Fastly-Services überprüfen oder debuggen

Sie können Leistungs- oder Caching-Probleme für eine Adobe Commerce auf einer Cloud-Infrastruktur-Site beheben, indem Sie die Site-URLs testen und die in der Antwort zurückgegebenen Header-Werte überprüfen.

### Live-Site über Fastly überprüfen

Verwenden Sie die Fastly-API, um die von Ihrer Live-Site zurückgegebenen `Fastly-Magento-VCL-Uploaded`- und `X-Cache`-Antwortkopfzeilen zu überprüfen.

Fastly API-Anfragen werden über die Fastly-Erweiterung weitergeleitet, um eine Antwort von Ihren Ursprungs-Servern zu erhalten. Wenn die Antwort falsche Kopfzeilen zurückgibt, testen Sie die [ursprünglichen Server direkt](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**Überprüfen der Antwort-Header**:

1. Verwenden Sie in einem Terminal den folgenden `curl`-Befehl, um Ihre Live-Site-URL zu testen:

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   Wenn Sie keine statische Route festgelegt oder die DNS-Konfiguration für die Domains auf Ihrer Live-Site abgeschlossen haben, verwenden Sie das `--resolve`-Flag, das die DNS-Namensauflösung umgeht.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >Um diesen Befehl mit der Option `--resolve` verwenden zu können, müssen Sie TLS mit Fastly über ein SSL-/TLS-Zertifikat aktiviert haben und die IP-Adresse des Cache-Knotens finden.

1. Überprüfen Sie in der Antwort die [Kopfzeilen](#check-cache-hit-and-miss-response-headers), um sicherzustellen, dass Fastly funktioniert. In der Antwort sollten die folgenden eindeutigen Kopfzeilen angezeigt werden:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Wenn die Kopfzeilen nicht die richtigen Werte aufweisen, lesen Sie die folgenden Informationen:

- [VCL-Upload überprüfen](#fastly-vcl-has-not-been-uploaded)

- [X-Cache enthält nur MISS, kein HIT](#x-cache-contains-only-miss-no-hit)

### Umgehen des Fastly-Cache zur Überprüfung von Adobe Commerce-Sites

Wenn der Fastly-Service falsche Kopfzeilen zurückgibt, können Sie ein VCL-Fragment erstellen, mit dem Sie Anfragen senden können, die den Fastly-Cache umgehen. Siehe [Umgehen des Fastly-Cache](fastly-vcl-bypass-to-origin.md).

Nachdem Sie den VCL-Ausschnitt hinzugefügt haben, verwenden Sie cURL-Befehle, um von der angegebenen IP-Adresse aus Anfragen an den Ursprungs-Server zu senden. Überprüfen Sie dann die Antworten auf Fehler.

### Überprüfen der Antwort-Header für Cache-Treffer und -Fehler

Stellen Sie sicher, dass die zurückgegebene Antwort die folgenden Informationen enthält:

- Enthält die `X-Magento-Tags`

- Der Wert des `Fastly-Module-Enabled`-Headers ist entweder `Yes` oder die Versionsnummer des in der Projektumgebung installierten Fastly for CDN Magento 2 -Moduls

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) ist größer als 0

- [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) Einstellung ist `cache`

Der folgende Auszug aus der cURL-Befehlsausgabe zeigt die richtigen Werte für die `Pragma`-, `X-Magento-Tags`- und `Fastly-Module-Enabled`-Kopfzeilen:

```
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>Detaillierte Informationen zu Treffern und Fehlern finden Sie unter [Grundlegendes zu Cache-HIT- und Fehlerkopfzeilen mit abgeschirmten Diensten](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) in der Fastly-Dokumentation.

### Beheben von Fehlern in Antwort-Headern

Dieser Abschnitt enthält Vorschläge zur Behebung von Fehlern, die beim Überprüfen von Antwort-Headern mithilfe der Fastly-API zurückgegeben wurden.

#### Fastly-Modul ist nicht aktiviert

Wenn das Fastly-Modul nicht aktiviert ist (`Fastly-Module-Enabled: no`) oder die Kopfzeile fehlt, [verwenden Sie SSH, um sich ](../development/secure-connections.md#connect-to-a-remote-environment) Projekt anzumelden. Führen Sie dann den folgenden Befehl aus, um den Modulstatus zu überprüfen.

```bash
php bin/magento module:status Fastly_Cdn
```

Verwenden Sie basierend auf dem zurückgegebenen Status die folgenden Anweisungen, um die Fastly-Konfiguration zu aktualisieren.

- `Module does not exist` - Wenn das Modul nicht vorhanden ist [installieren und konfigurieren](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) das Fastly CDN-Modul für Magento 2 in einer Integrationsverzweigung. Aktivieren und konfigurieren Sie das Modul nach Abschluss der Installation. Siehe [Schnelles Setup](fastly-configuration.md).

- `Module is disabled` - Wenn das Fastly-Modul deaktiviert ist, aktualisieren Sie die Umgebungskonfiguration auf einer `integration` Verzweigung in Ihrer lokalen Umgebung, um sie zu aktivieren. Übertragen Sie die Änderungen dann in die Staging- und Produktionsumgebung. Siehe [Erweiterungen verwalten](../store/extensions.md#install-an-extension).

  Wenn Sie [Konfigurationsverwaltung](../store/store-settings.md#configure-store) verwenden, überprüfen Sie den Status des Fastly CDN-Moduls in der `app/etc/config.php`-Konfigurationsdatei, bevor Sie Änderungen in die Produktions- oder Staging-Umgebung pushen.

  Wenn das Modul in der `config.php`-Datei nicht aktiviert (`Fastly_CDN => 0`) ist, löschen Sie die Datei und führen Sie den folgenden Befehl aus, um `config.php` mit den neuesten Konfigurationseinstellungen zu aktualisieren.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCL wurde nicht hochgeladen

Wenn die Fastly-VCL nicht hochgeladen wurde (`Fastly-Magento-VCL-Uploaded`: `false`), verwenden Sie die Option *VCL hochladen* in der Admin-Liste, um sie hochzuladen. Siehe [Hochladen von Fastly VCL-Snippets](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache enthält nur MISS, kein HIT

Wenn die `X-Cache`-Kopfzeile `HIT` (`HIT, HIT` oder `HIT, MISS`) enthält, bedeutet dies, dass Fastly den zwischengespeicherten Inhalt erfolgreich zurückgibt.

Wenn die `X-Cache`-Kopfzeile `MISS, MISS` ist und keine `HIT` enthält, führen Sie den `curl` erneut aus, um sicherzustellen, dass die Seite nicht kürzlich aus dem Cache gelöscht wurde.

Wenn Sie dasselbe Ergebnis erhalten, verwenden Sie die [`curl` Befehle ](#check-live-site-through-fastly) überprüfen Sie die [Antwort-Header](#check-cache-hit-and-miss-response-headers):

- `Pragma` ist `cache`
- `X-Magento-Tags` vorhanden
- `Cache-Control: max-age` ist größer als 0

Wenn das Problem weiterhin besteht, wird diese Kopfzeile wahrscheinlich von einer anderen Erweiterung zurückgesetzt. Wiederholen Sie das folgende Verfahren in der Staging-Umgebung, indem Sie alle Erweiterungen deaktivieren und jede Erweiterung erneut aktivieren, um zu bestimmen, welche Erweiterung die Header zurücksetzt. Nachdem Sie die Erweiterung identifiziert haben, die das Problem verursacht, müssen Sie sie in der Produktionsumgebung deaktivieren.

**So identifizieren Sie eine Erweiterung, die Antwortkopfzeilen zurücksetzt:**

{{admin-login-step}}

1. Navigieren Sie zu **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **Erweitert**.

1. Suchen Sie im *Ausgabe von Modulen deaktivieren* im rechten Bereich nach allen Erweiterungen und deaktivieren Sie sie.

1. Klicken Sie **Konfiguration speichern**.

1. Klicken Sie auf **System** > **Tools** > **Cache-Verwaltung**.

1. Klicken Sie auf **Leeren des Magento-Cache**.

1. Führen Sie die folgenden Schritte für jede Erweiterung aus, die möglicherweise Probleme mit Fastly-Kopfzeilen verursacht:

   - Aktivieren Sie jeweils eine Erweiterung, speichern Sie die Konfiguration und leeren Sie den Adobe Commerce-Cache.

   - Führen Sie die [`curl` Befehle aus](#check-live-site-through-fastly) um die [Antwort-Header](#check-cache-hit-and-miss-response-headers) zu überprüfen.

   Wiederholen Sie diesen Vorgang für jede Erweiterung. Wenn die Fastly-Antwort-Header nicht mehr angezeigt werden, haben Sie die Erweiterung identifiziert, die Probleme mit Fastly verursacht.

Nachdem Sie die Erweiterung identifiziert haben, die die Fastly-Kopfzeilen zurücksetzt, wenden Sie sich an den Entwickler der Erweiterung, um weitere Hilfe zu erhalten. Wir können keine Fehlerbehebungen oder Aktualisierungen bereitstellen, damit Erweiterungen von Drittanbietern mit der Fastly-Zwischenspeicherung funktionieren.

## Rollback Fastly-Konfiguration

Wenn benutzerdefinierte VCL-Snippet-Aktualisierungen oder andere Fastly-Konfigurationsänderungen dazu führen, dass ein Adobe Commerce auf der Cloud-Infrastruktur-Site Fehler verursacht oder zurückgibt, verwenden Sie den Befehl Fastly API [aktivieren](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5), um zu einer früheren VCL-Version zurückzukehren. Sie können die VCL-Version nicht vom Administrator zurücksetzen.

**Zurücksetzen der VCL-Version**:

1. Um eine Liste der verfügbaren VCL-Versionen für einen Service abzurufen, führen Sie den folgenden Befehl aus

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. Führen Sie den folgenden Befehl aus, um die aktive VCL-Version in eine angegebene Version zu ändern.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Weitere Informationen zur Verwendung der Fastly-API zur Überprüfung und Verwaltung von VCL finden Sie unter [Verwalten von VCL mithilfe der API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
