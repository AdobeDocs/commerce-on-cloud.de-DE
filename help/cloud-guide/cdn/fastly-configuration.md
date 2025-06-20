---
title: Fastly-Services konfigurieren
description: Erfahren Sie, wie Sie Fastly Services für Ihr Adobe Commerce-Projekt einrichten und konfigurieren.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: f9ce1e8b-4e9f-488e-8a4d-f866567c41d8
source-git-commit: 867abffd6cbed6e026c20b646ff641cc6ab40580
workflow-type: tm+mt
source-wordcount: '2063'
ht-degree: 0%

---

# Fastly-Services konfigurieren

Fastly ist für Adobe Commerce in Staging- und Produktionsumgebungen der Cloud-Infrastruktur erforderlich.

Fastly arbeitet mit Varnish zusammen, um schnelle Caching-Funktionen und ein Content Delivery Network (CDN) für statische Assets bereitzustellen. Fastly bietet auch eine Web Application Firewall (WAF), um Ihre Site- und Cloud-Infrastruktur zu sichern. Um Ihre Site und Cloud-Infrastruktur vor bösartigem Traffic und Angriffen zu schützen, leiten Sie den gesamten eingehenden Site-Traffic über Fastly.

>[!NOTE]
>
>Fastly ist in Integrationsumgebungen nicht verfügbar.

Führen Sie die folgenden Schritte aus, um Fastly früh in Ihrem Site-Entwicklungsprozess zu aktivieren, zu konfigurieren und zu testen, um einen sicheren Zugriff auf Ihre Site zu ermöglichen.

- Schnelle Anmeldedaten für Staging- und Produktionsumgebungen
- Schnelles CDN-Caching aktivieren
- Hochladen von Fastly VCL-Snippets
- DNS-Konfiguration aktualisieren, um Traffic an den Fastly-Service zu leiten
- Schnelles Caching testen

>[!NOTE]
>
>Nachdem Sie die anfängliche Fastly-Konfiguration aktiviert und überprüft haben, können Sie die Konfiguration anpassen. Sie können beispielsweise zusätzliche Optionen wie Bildoptimierung, Edge-Module und benutzerdefinierten VCL-Code aktivieren. Siehe [Anpassen der Cache-Konfiguration](fastly-custom-cache-configuration.md).

Während der Projektbereitstellung fügt Adobe Ihr Projekt zum [Fastly Service-Konto](fastly.md#fastly-service-account-and-credentials) für Adobe Commerce in der Cloud-Infrastruktur hinzu und erstellt Fastly-Kontoanmeldeinformationen für die Starter `master`- und Pro-Staging- und Produktionsumgebungen. Jede Umgebung verfügt über eindeutige Anmeldeinformationen.

Sie benötigen die Fastly-Anmeldedaten, um Fastly CDN-Services über den Adobe Commerce-Administrator zu konfigurieren und Fastly-API-Anfragen zu senden.

## Fastly Admin Dashboard Access

Mit Adobe Commerce in der Cloud-Infrastruktur können Sie nicht direkt auf das Fastly Admin Dashboard zugreifen.

Sie müssen Adobe Commerce Admin verwenden, um die Fastly-Konfiguration für Ihre Umgebungen zu überprüfen und zu aktualisieren. Wenn Sie ein Problem nicht mit den Fastly-Funktionen in Admin beheben können, reichen Sie ein [Adobe Commerce-Support-Ticket ein](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

## Abrufen von Fastly-Anmeldedaten

Verwenden Sie die folgenden Methoden, um die Fastly-Service-ID und das API-Token für Ihre Umgebung zu finden und zu speichern:

**So zeigen Sie Ihre Fastly-Anmeldedaten an**:

>[!NOTE]
>
>Geben Sie Ihr API-Token nicht in Support-Tickets, öffentlichen Foren oder öffentlichen Orten frei. Außerdem sollten API-Token niemals in Code-Repositorys übertragen werden - Repositorys sollten nur unveränderliche Dateien ohne vertrauliche Informationen enthalten.
>
>Der Adobe Commerce-Support hat bereits Zugriff auf die erforderlichen Schlüssel, sodass Sie bei der Hilfestellung Ihr API-Token nicht angeben müssen.
>
>Wenn Ihr API-Token jemals öffentlich freigegeben oder an ein Support-Ticket angehängt wird, wird es als gefährdet betrachtet. In solchen Fällen muss Adobe ein neues Token für Sie generieren.
>
>Verwandt: [Fehler bei der Validierung der Fastly-Anmeldedaten](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials#solution)

Die Methode zum Anzeigen von Anmeldeinformationen unterscheidet sich bei Pro- und Starter-Projekten.

- Freigegebenes Verzeichnis mit IaaS-Mount: Bei Pro-Projekten wird SSH verwendet, um eine Verbindung zu Ihrem Server herzustellen und die Fastly-Anmeldeinformationen aus der `/mnt/shared/fastly_tokens.txt`-Datei abzurufen. Staging- und Produktionsumgebungen verfügen über eindeutige Anmeldeinformationen. Sie müssen die Anmeldeinformationen für jede Umgebung abrufen.

- Lokaler Arbeitsbereich: Verwenden Sie in der Befehlszeile die `magento-cloud`-CLI, um Umgebungsvariablen [aufzulisten und zu überprüfen](../environment/variables-cloud.md#viewing-environment-variables) Fastly.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console] - Überprüfen Sie die folgenden Umgebungsvariablen in der [Umgebungskonfiguration](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Wenn Sie die Fastly-Anmeldedaten für die Staging- oder Produktionsumgebungen nicht finden können, wenden Sie sich an Ihren technischen Adobe-Kundenberater (CTA).

## Schnelles Caching aktivieren

Sie benötigen die folgenden Komponenten, um Fastly-Services zu aktivieren und zu konfigurieren:

- Neueste Version des Moduls [Fastly CDN for Magento 2](fastly.md#fastly-cdn-module-for-magento-2), das in den Staging- und Produktionsumgebungen installiert ist. Siehe [Schnelles Upgrade](#upgrade-the-fastly-module).

- [Fastly-Anmeldedaten](#get-fastly-credentials) für Adobe Commerce in Staging- und Produktionsumgebungen der Cloud-Infrastruktur

**So aktivieren Sie das schnelle CDN-Caching in der Staging- und Produktionsumgebung**:

{{admin-login-step}}

1. Klicken Sie auf **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** und erweitern Sie **Vollständiger Seitencache**.

   ![Erweitern Sie auf Fastly](../../assets/cdn/fastly-menu.png)

1. Entfernen Sie im Abschnitt _Caching_ die Auswahl aus **Systemwert verwenden** und wählen Sie dann **Fastly CDN** aus der Dropdown-Liste aus.

   ![Schnell wählen](../../assets/cdn/fastly-enable-admin.png)

1. Erweitern Sie **Fastly Configuration** und wählen [Caching-Optionen](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. Klicken Sie nach dem Konfigurieren der Caching **Optionen oben** der Seite auf „Konfiguration speichern“.

1. Löschen Sie den Cache gemäß der Benachrichtigung.

1. Fahren Sie mit der Konfiguration von Fastly fort, indem Sie zurück zu **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System** > **Fastly Configuration**.

### Testen von Fastly-Anmeldedaten

1. Navigieren Sie in der Admin **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** > **Fastly Configuration**.

1. Fügen Sie bei Bedarf die Werte **Fastly-Service** ID und **API-Token** für Ihre Projektumgebung hinzu.

   ![Fastly Anmeldedaten-Administrator](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Wählen Sie nicht den Link aus, um das Fastly-API-Token zu erstellen. Adobe Verwenden Sie stattdessen die von [ bereitgestellten „Fastly-Anmeldeinformationen (Service-ID und API-Token](#get-fastly-credentials).

1. Klicken Sie **Testanmeldeinformationen**.

1. Wenn der Test erfolgreich ist, klicken Sie **Konfiguration speichern** und löschen Sie dann den Cache.

   Wenn der Test fehlschlägt, stellen Sie sicher, dass die richtigen Werte für Service-ID und API-Token mit den Anmeldeinformationen für die aktuelle Umgebung übereinstimmen.

   Wenn der Test erneut fehlschlägt, reichen Sie ein Adobe Commerce-Support-Ticket ein oder wenden Sie sich an Ihren Adobe-Kundenbetreuer. Geben Sie bei Pro-Projekten die URLs für Ihre Produktions- und Staging-Sites an. Geben Sie für Einstiegsprojekte die URLs für Ihre `Master`- und Staging-Site an.

>[!NOTE]
>
>Anweisungen zum Ändern der Fastly-API-Token-Anmeldeinformationen für eine Staging- oder Produktionsumgebung finden Sie unter [Ändern der Fastly-Anmeldeinformationen](fastly.md#change-fastly-api-token).

### VCL zu Fastly hochladen

Laden Sie nach dem Aktivieren des Fastly-Moduls den Standard[VCL-Code](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) auf die Fastly-Server hoch. Dieser Code bietet eine Reihe von VCL-Snippets, die die Konfigurationseinstellungen angeben, um das Caching und andere Fastly CDN-Services für Ihre Adobe Commerce in der Cloud-Infrastruktur zu aktivieren.

>[!NOTE]
>
>Fastly-Caching-Services funktionieren erst, wenn Sie den ersten Upload des Fastly-VCL-Codes auf die Staging- und Produktions-Sites von Adobe Commerce abgeschlossen haben.

**Hochladen der Fastly-VCL**:

1. Klicken Sie im Abschnitt _Fastly_ auf **VCL in Fastly hochladen** wie in der folgenden Abbildung dargestellt.

   ![Laden Sie eine Magento-VCL in Fastly hoch](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Aktualisieren Sie nach Abschluss des Uploads den Cache entsprechend der Benachrichtigung oben auf der Seite.

## Bereitstellen von SSL-/TLS-Zertifikaten

Adobe stellt ein Domain-validiertes Let&#39;s Encrypt SSL/TLS-Zertifikat zur Verfügung, das sicheren HTTPS-Traffic von Fastly bereitstellt. Adobe stellt ein Zertifikat für jede Pro-Produktions-, Staging- und Starter-Produktionsumgebung bereit, um alle Domains in dieser Umgebung zu sichern. Ausführliche Informationen zum bereitgestellten Zertifikat finden Sie unter [Adobe SSL (TLS)-Zertifikate für Adobe Commerce in der Cloud-Infrastruktur](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html).

>[!NOTE]
>
>Sie können Ihr eigenes TLS- oder SSL-Zertifikat bereitstellen, anstatt das von Adobe bereitgestellte Let&#39;s Encrypt-Zertifikat zu verwenden. Dieser Prozess erfordert jedoch zusätzliche Arbeit bei der Einrichtung und Wartung. Um diese Option auszuwählen, reichen Sie ein Adobe Commerce-Support-Ticket ein oder arbeiten Sie mit Adobe zusammen, um Ihrer Adobe Commerce in Cloud-Infrastrukturumgebungen benutzerdefinierte, gehostete Zertifikate hinzuzufügen.

Um die SSL-/TLS-Zertifikate für Adobe Commerce-Umgebungen zu aktivieren, führt Adobe Automation die folgenden Schritte aus:

- Validiert den Domain-Besitz
- Stellt ein SSL-/TLS-Zertifikat zur Verschlüsselung bereit, das die angegebenen Top-Level- und Subdomains für Ihre Stores abdeckt
- Lädt das Zertifikat in die Cloud-Umgebung hoch, wenn die Site live ist

Für diese Automatisierung müssen Sie die DNS-Konfiguration für Ihre Site aktualisieren, um Informationen zur Domain-Validierung bereitzustellen. Verwenden Sie **eine** der folgenden Methoden:

- **DNS-Validierung**-Aktualisieren Sie Ihre DNS-Konfiguration für Live-Sites mit CNAME-Einträgen, die auf den Fastly-Service verweisen.
- **ACME Challenge CNAME-Einträge** - Aktualisieren Sie Ihre DNS-Konfiguration mit den von Adobe für jede Domain in Ihrer Umgebung bereitgestellten ACME Challenge CNAME-Einträgen.

>[!TIP]
>
>Wenn Sie über eine nicht aktive Produktions-Domain verfügen, verwenden Sie die CNAME-Einträge der ACME-Herausforderung für die Domain-Validierung. Durch frühzeitiges Hinzufügen der Einträge zu Ihrer DNS-Konfiguration kann Adobe das SSL-/TLS-Zertifikat mit den richtigen Domains bereitstellen, bevor die Site gestartet wird. Bevor Sie mit der Produktion beginnen, müssen Sie diese Platzhalterdatensätze durch die von Adobe bereitgestellten CNAME-Datensätze ersetzen.

Nach Abschluss der Domain-Validierung stellt Adobe das TLS/SSL-Zertifikat für die Verschlüsselung bereit und lädt es in Live-Staging- oder Produktionsumgebungen hoch. Dieser Vorgang kann bis zu 12 Stunden dauern. Es wird empfohlen, die DNS-Konfigurationsaktualisierungen mehrere Tage im Voraus abzuschließen, um Verzögerungen bei der Site-Entwicklung und dem Site-Launch zu vermeiden.

## DNS-Konfiguration mit Entwicklungseinstellungen aktualisieren

Während des anfänglichen Fastly-Einrichtungsprozesses können Sie die folgenden URLs verwenden, um das Fastly-Caching in Staging- und Produktionsumgebungen zu konfigurieren und zu testen:

- Für Pro Staging und Produktion:

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Nur für die Erstproduktion:

   - `mcprod.<your-domain>.com`

Diese standardmäßigen Vorproduktions-URLs sind verfügbar, nachdem Ihr Projekt bereitgestellt wurde. Der Wert für `"your-domain"` ist der Domain-Name, den Sie während des Onboarding-Prozesses angegeben haben.

>[!NOTE]
>
>Sie können keine benutzerdefinierte Domain für eine Nicht-Produktionsumgebung in Starter-Projekten angeben.

Aktualisieren Sie Ihre DNS-Konfiguration, um Traffic von Ihren Store-URLs zum Fastly-Service zu leiten. Wenn Sie die Konfiguration aktualisieren, stellt Adobe automatisch die erforderlichen SSL-/TLS-Zertifikate bereit und lädt sie in Ihre Cloud-Umgebungen hoch. Diese Bereitstellung kann bis zu 12 Stunden dauern.

>[!NOTE]
>
>Wenn Sie bereit sind, Ihre Produktions-Site zu starten, müssen Sie die DNS-Konfiguration erneut aktualisieren, um Ihre Produktions-Domains auf den Fastly-Service zu verweisen und zusätzliche Konfigurationsaufgaben durchzuführen. Siehe [Checkliste starten](../launch/checklist.md).

**Voraussetzungen:**

- Aktivieren Sie das Fastly-Modul.
- Laden Sie den Standard-Fastly-VCL-Code hoch.
- Geben Sie eine Liste der Top-Level- und Subdomains für jede Umgebung an Adobe weiter oder senden Sie ein Adobe Commerce-Support-Ticket.
- Warten Sie auf die Bestätigung, dass die angegebenen Domains zu Ihren Cloud-Umgebungen hinzugefügt wurden.
- Fügen Sie bei Einstiegsprojekten die Domains zu Ihrer Fastly-Service-Konfiguration hinzu. Siehe [Verwalten von Domains](fastly-custom-cache-configuration.md#manage-domains).
- Informationen zum Aktualisieren der DNS-Konfiguration finden Sie bei Ihrer [DNS-](https://lookup.icann.org/)) nach der richtigen Methode für Ihren Domain-Dienst.

**So aktualisieren Sie Ihre DNS-Konfiguration für die Entwicklung**:

1. Verweisen Sie Vorproduktions-URLs auf den Fastly-Service, indem Sie CNAME-Einträge hinzufügen: `prod.magentocloud.map.fastly.net`, z. B.:

   | Domain oder Subdomain | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Wenn die CNAME-Einträge live sind, stellt Adobe Zertifikate bereit und lädt die SSL-/TLS-Zertifikate hoch.

   >[!NOTE]
   >
   >Wenn Sie planen, Apex-Domains (`your-domain.com`) für Ihre Produktions-Website zu verwenden, müssen Sie DNS-Adresseinträge (A-Einträge) so konfigurieren, dass sie auf die Fastly-Server-IP-Adressen verweisen. Siehe [Aktualisieren der DNS-Konfiguration mit Produktionseinstellungen](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Fügen Sie CNAME-Einträge mit ACME-Challenge für die Domain-Validierung und die Vorbereitstellung von SSL-/TLS-Produktionszertifikaten hinzu, z. B.:

   | Domain oder Subdomain | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >Die ACME-Challenge-Datensätze in diesem Beispiel sind Platzhalter, die nicht für die Bereitstellung Ihrer Adobe Commerce-Staging- und Produktions-Sites vorgesehen sind. Erhalten Sie die richtigen Datensatzinformationen für die ACME-Herausforderung für Ihr Projekt, indem Sie sich an Adobe wenden.

   Nach dem Hinzufügen der CNAME-Einträge validiert Adobe die Domains und stellt das SSL-/TLS-Zertifikat für die Umgebung bereit. Wenn Sie die DNS-Konfiguration aktualisieren, um Traffic von diesen Domains zum Fastly-Service zu leiten, lädt Adobe das Zertifikat in die Umgebung hoch.

1. Aktualisieren Sie die Adobe Commerce-Basis-URL.

   - Verwenden Sie SSH, um sich bei der Produktionsumgebung anzumelden.

     ```bash
     magento-cloud ssh
     ```

   - Verwenden Sie die Cloud-CLI, um die Basis-URL für Ihren Store zu ändern.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Als Alternative zur Verwendung der Cloud-CLI können Sie die Basis-URL von [Admin](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html) aktualisieren

1. Starten Sie den Webbrowser neu.

1. Testen Sie Ihre Website.

## Schnelles Caching testen

Nachdem Sie die Änderungen an der DNS-Konfiguration vorgenommen haben, verwenden Sie das [cURL](https://curl.se/)-Befehlszeilen-Tool, um zu überprüfen, ob der Fastly-Cache funktioniert.

**Überprüfen der Antwort-Header**:

1. Verwenden Sie in einem Terminal den folgenden `curl`-Befehl, um Ihre Live-Site-URL zu testen:

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Wenn Sie keine statische Route festgelegt oder die DNS-Konfiguration für die Domains auf Ihrer Live-Site abgeschlossen haben, verwenden Sie das `--resolve`-Flag, das die DNS-Namensauflösung umgeht.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. Überprüfen Sie in der Antwort die [Kopfzeilen](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers), um sicherzustellen, dass Fastly funktioniert. In der Antwort sollten die folgenden eindeutigen Kopfzeilen angezeigt werden:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Wenn die Kopfzeilen nicht die richtigen Werte aufweisen, finden Sie unter [Beheben von Fehlern in den Antwort-Kopfzeilen](fastly-troubleshooting.md#curl) Hilfe zur Fehlerbehebung.

## Upgrade des Fastly-Moduls

Fastly aktualisiert das Fastly CDN für Magento 2-Modul, um Probleme zu beheben, die Leistung zu steigern und neue Funktionen bereitzustellen.
Wir empfehlen Ihnen, das Fastly-Modul in Ihren Staging- und Produktionsumgebungen auf die [neueste Version) ](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Nachdem Sie das Modul aktualisiert haben, müssen Sie den VCL-Code hochladen, um die Änderungen auf die Fastly-Service-Konfiguration anzuwenden.

>[!WARNING]
>
> Wenn Sie den Standard-Fastly-VCL-Code mit einer benutzerdefinierten Version angepasst haben, überschreibt das Upgrade des Fastly-Moduls Ihre Änderungen. Wenn Sie benutzerdefinierte VCL-Snippets mit eindeutigen Namen hinzugefügt haben, bleiben diese Änderungen während des Aktualisierungsprozesses erhalten. Als Best Practice empfiehlt es sich, die Staging-Umgebung zu aktualisieren und die Änderungen zu validieren, bevor Sie Änderungen auf die Produktionsumgebung anwenden.

**So überprüfen Sie die Version des Fastly CDN-Moduls für Magento 2**:

1. Wechseln Sie in das Stammverzeichnis Ihrer Cloud-Umgebung.

1. Verwenden Sie Composer, um die installierte Version zu überprüfen.

   ```bash
   composer show *fastly*
   ```

1. Wenn die [neueste Version](https://github.com/fastly/fastly-magento2/releases) nicht installiert ist, führen Sie die Schritte zum Upgrade des Fastly-Moduls aus.

**So aktualisieren Sie das Fastly-Modul**:

1. Verwenden Sie in Ihrer lokalen Integrationsumgebung die folgenden Modulinformationen, um [das Fastly-Modul zu aktualisieren](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Pushen Sie Ihre Aktualisierungen in die Staging-Umgebung.

1. Melden Sie sich bei der Admin für Ihre Staging-Umgebung an, um [den VCL-Code hochzuladen](#upload-vcl-to-fastly).

1. [Überprüfen der Fastly](fastly-troubleshooting.md#verify-or-debug-fastly-services)Services auf der Adobe Commerce-Staging-Site.

Nachdem Sie die Fastly-Services auf der Staging-Site überprüft haben, wiederholen Sie den Upgrade-Prozess in der Produktionsumgebung.

>[!TIP]
>
> Wenn Sie Probleme mit Fastly-Services in Ihren Adobe Commerce-Umgebungen haben, finden Sie weitere Informationen im [Adobe Commerce Fastly-](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html).
