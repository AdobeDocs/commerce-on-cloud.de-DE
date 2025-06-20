---
title: Fastly Services - Übersicht
description: Erfahren Sie, wie die Fastly-Services, die in Adobe Commerce zur Cloud-Infrastruktur enthalten sind, Ihnen bei der Optimierung und Sicherung der Inhaltsbereitstellung für Ihre Adobe Commerce-Sites helfen.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: 429b6762-0b01-438b-a962-35376306895b
source-git-commit: 3cef442321120d8ca813c760d2fd0435f4961235
workflow-type: tm+mt
source-wordcount: '1443'
ht-degree: 0%

---

# Fastly Services - Übersicht

>[!WARNING]
>
>Um die PCI-Konformität für Adobe Commerce-Sites, die auf der Cloud-Plattform bereitgestellt werden, aufrechtzuerhalten, richten Sie Fastly in Ihrer Starter-Hauptzweig-, Pro-Produktions- und Pro-Staging-Umgebung ein. Wenn Sie Adobe Commerce in einer Headless-Bereitstellung verwenden, empfehlen wir dringend, Fastly zum Zwischenspeichern von GraphQL-Antworten zu verwenden. Siehe [Caching mit Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) im *GraphQL-*.

Fastly bietet die folgenden Services an, um die Bereitstellung von Inhalten für Adobe Commerce in Cloud-Infrastrukturprojekten zu optimieren und zu sichern. Diese Services sind ohne zusätzliche Kosten in Adobe Commerce on Cloud Infrastructure enthalten.

- **Content Delivery Network (CDN)** - Lackbasierter Service, der Ihre Website-Seiten, Assets, CSS und mehr in von Ihnen eingerichteten Backend-Rechenzentren zwischenspeichert. Wenn Kunden auf Ihre Site und Ihre Stores zugreifen, erreichen die Anfragen Fastly, um zwischengespeicherte Seiten schneller zu laden. Der CDN-Service bietet die folgenden Funktionen:

- **Cache-**: Speichern Sie Seiten, Assets, CSS und mehr Ihrer Website im Cache von Back-End-Rechenzentren, die Sie zur Reduzierung der Bandbreitenlast und der Kosten einrichten

   - Verwenden Sie [Fastly Custom VCL-Snippets](fastly-vcl-custom-snippets.md) (kompatibel mit Varnish 2.1), um die Art und Weise zu ändern, wie das Caching auf Anfragen reagiert

   - Einrichten [GeoIP-Service-Unterstützung](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [Erzwingen von unverschlüsselten Anforderungen an TLS](fastly-custom-cache-configuration.md#force-tls)

   - Einstellungen [Fastly Timeout anpassen](fastly-custom-cache-configuration.md#extend-fastly-timeout) um 503-Antworten auf Massenvorgangsanfragen zu verhindern

   - Erstellen [benutzerdefinierter Fehlerantwortseiten](fastly-custom-response.md)

- **Sicherheit** - Nachdem Sie die Fastly Services für Adobe Commerce Sites aktiviert haben, stehen zusätzliche Sicherheitsfunktionen zum Schutz Ihrer Sites und Ihres Netzwerks zur Verfügung:

   - [Web Application Firewall](fastly-waf-service.md) (WAF): Ein verwalteter Firewall-Service für Web-Anwendungen, der PCI-konformen Schutz bietet, um bösartigen Traffic zu blockieren, bevor er Ihre Produktions-Adobe Commerce auf Cloud-Infrastrukturstandorten und im Netzwerk beschädigen kann. Der WAF-Service ist nur für Pro- und Starter-Produktionsumgebungen verfügbar.

   - [Distributed Denial of Service (DDoS)-Schutz](#ddos-protection) Integrierter DDoS-Schutz vor häufigen Angriffen wie Ping of Death, Smurf-Angriffen und anderen ICMP-basierten Flutangriffen.

   - [SSL-/TLS-](fastly-configuration.md#provision-ssltls-certificates): Der Fastly-Service erfordert ein SSL-/TLS-Zertifikat, um sicheren Traffic über HTTPS bereitzustellen.

     Adobe Commerce stellt für jede Staging- und Produktionsumgebung ein Domain-validiertes Let&#39;s Encrypt SSL/TLS-Zertifikat bereit. Adobe Commerce schließt die Domain-Validierung und Zertifikatbereitstellung während des schnellen Einrichtungsprozesses ab.

- **Origin Cloaking**: Verhindert, dass Traffic die Fastly WAF umgeht, und verbirgt die IP-Adressen Ihrer Original-Server, um sie vor direktem Zugriff und DDoS-Angriffen zu schützen.

  „Origin Cloaking“ ist in Adobe Commerce in Cloud-Infrastruktur-Pro-Produktionsprojekten standardmäßig aktiviert. Um das Origin Cloaking in Adobe Commerce für Cloud-Infrastruktur-Starter-Produktionsprojekte zu aktivieren, reichen Sie ein [Adobe Commerce-Support-Ticket ein](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket). Wenn Sie Traffic haben, für den keine Zwischenspeicherung erforderlich ist, können Sie die Fastly-Service-Konfiguration so anpassen, dass Anfragen den Fastly[Cache umgehen können](fastly-vcl-bypass-to-origin.md).

- **[Bildoptimierung](fastly-image-optimization.md)** - Lädt die Bildverarbeitung und Größenanpassung auf den Fastly-Service ab, damit Server Bestellungen und Konversionen effizienter verarbeiten können.

- **[Fastly CDN- und WAF-Protokolle](../monitor/new-relic-service.md#new-relic-log-management)** - Für Adobe Commerce in Cloud Infrastructure Pro-Projekten können Sie den Service &quot;New Relic Logs“ verwenden, um Fastly CDN- und WAF-Protokolldaten zu überprüfen und zu analysieren.

## Fastly CDN-Modul für Magento 2

Fastly Services für Adobe Commerce auf Cloud-Infrastruktur verwenden das [Fastly CDN-Modul für Magento 2], das in den folgenden Umgebungen installiert ist: Pro Staging und Produktion, Starter Production (`master` Zweig).

Bei der ersten Bereitstellung oder dem Upgrade Ihres Adobe Commerce-Projekts installiert Adobe die neueste Version des Fastly CDN-Moduls in Ihren Staging- und Produktionsumgebungen. Wenn Fastly Modulaktualisierungen veröffentlicht, erhalten Sie Benachrichtigungen im Admin für Ihre Umgebungen. Adobe empfiehlt, dass Sie Ihre Umgebungen aktualisieren, um die neueste Version zu verwenden. Siehe [Schnelles Upgrade](fastly-configuration.md#upgrade-the-fastly-module).

## Fastly Service-Konto und Anmeldedaten

Adobe Commerce für Cloud-Infrastrukturprojekte erhält kein dediziertes Fastly-Konto. Der Fastly-Service wird in einem zentralen Konto verwaltet, das bei Adobe registriert ist, und das Verwaltungs-Dashboard ist nur für das Cloud-Support-Team zugänglich.

Stattdessen verfügt jede Staging- und Produktionsumgebung über eindeutige Fastly-Anmeldeinformationen (API-Token und Service-ID) zum Konfigurieren und Verwalten von Fastly-Services über den Commerce-Administrator. Die Fastly-API ist für die erweiterte Verwaltung des Fastly-Service verfügbar, der die Anmeldeinformationen benötigt, um diese Anfragen zu senden.

Während der Projektbereitstellung fügt Adobe Ihr Projekt zum Fastly-Service-Konto für Adobe Commerce in der Cloud-Infrastruktur hinzu und fügt die Fastly-Anmeldeinformationen zur Konfiguration für die Staging- und Produktionsumgebung hinzu. Siehe [Abrufen von Fastly-Anmeldeinformationen](fastly-configuration.md#get-fastly-credentials).

### Fastly-API-Token ändern

Senden Sie ein Adobe Commerce-Support-Ticket, um eine neue Anmeldedaten für Fastly-API[Token herauszugeben (wenn die Validierung fehlschlägt/abgelaufen ist](https://experienceleague.adobe.com/de/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials) oder wenn Sie glauben, dass es kompromittiert wurde.

Wenn Sie das neue Token erhalten, aktualisieren Sie Ihre Staging- oder Produktionsumgebung, um das neue Token zu verwenden.

**So ändern Sie die Anmeldedaten für das Fastly-API-Token**:

1. [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) Anfordern neuer Fastly-API-Anmeldeinformationen.

   Schließen Sie Ihre Adobe Commerce on Cloud Infrastructure-Projekt-ID und die Umgebungen ein, die neue Anmeldeinformationen erfordern.

1. Nachdem Sie das neue API-Token erhalten haben, aktualisieren Sie den Wert des API-Tokens in der Konfiguration [Fastly-Anmeldeinformationen](fastly-configuration.md#test-the-fastly-credentials) in der Admin-Datei oder in den [[!DNL Cloud Console] Umgebungsvariablen](../project/overview.md#configure-environment).

1. [Testen Sie die neue Berechtigung](fastly-configuration.md#test-the-fastly-credentials).

1. Nachdem Sie die Berechtigung aktualisiert haben, reichen Sie ein Adobe Commerce-Support-Ticket ein, um das alte API-Token zu löschen.

### Mehrere Fastly-Konten und zugewiesene Domains

Fastly ermöglicht es Ihnen nur, eine Apex-Domain und zugehörige Subdomains einem Fastly-Service und -Konto zuzuweisen. Wenn Sie bereits über ein Fastly-Konto verfügen, das denselben Apex und dieselbe Subdomain verknüpft, die für Ihre Adobe Commerce-Site verwendet werden, haben Sie die folgenden Optionen:

- Entfernen Sie die Apex- und Subdomains aus dem bestehenden Konto, bevor Sie Fastly Service-Anmeldeinformationen für Ihre Adobe Commerce in Cloud-Infrastrukturprojektumgebungen anfordern. Siehe [Arbeiten mit Domains] in der Fastly-Dokumentation.

  Verwenden Sie diese Option, um die Apex-Domain und alle Subdomains mit dem Fastly-Service-Konto für Adobe Commerce in der Cloud-Infrastruktur zu verknüpfen.

- Senden Sie ein Adobe Commerce-Support-Ticket, um die Domain-Delegierung anzufordern, damit Apex- und Subdomains mit verschiedenen Konten verknüpft werden können.

  Verwenden Sie diese Option, wenn Sie eine Apex-Domain mit mehreren Subdomains für Adobe Commerce- und Nicht-Adobe Commerce-Sites haben und diese Subdomains mit verschiedenen Fastly-Konten verknüpfen möchten.

#### Domain-Delegierung anfordern

*Szenario 1:*

Die Apex-Domain (`testweb.com` und `www.testweb.com`) ist mit einem vorhandenen Fastly-Konto verknüpft. Sie haben ein Adobe Commerce on Cloud Infrastructure-Projekt mit den folgenden Subdomains konfiguriert: `mcstaging.testweb.com` und `mcprod.testweb.com`. Sie möchten die Apex-Domain nicht in das Fastly-Service-Konto für Adobe Commerce in der Cloud-Infrastruktur verschieben.

Senden Sie ein [Fastly-Support]-Ticket, in dem Sie darum bitten, die Subdomains vom bestehenden Fastly-Konto an das Fastly-Konto für Adobe Commerce in der Cloud-Infrastruktur zu delegieren. Fügen Sie Ihre Adobe Commerce-Projekt-ID in das Ticket ein.

Nach Abschluss der Delegierung können Ihre Projekt-Subdomains zum Fastly-Service-Konto für Adobe Commerce in der Cloud-Infrastruktur hinzugefügt werden. Siehe [Abrufen von Fastly-Anmeldeinformationen](fastly-configuration.md#get-fastly-credentials).

*Szenario 2:*

Die Apex-Domain (`testweb.com` und `www.testweb.com`) ist mit dem Fastly-Service-Konto für Adobe Commerce in der Cloud-Infrastruktur verknüpft. Sie möchten die Fastly-Services für die `service.testweb.com` und `product-updates.testweb.com` Subdomains von einem anderen Fastly-Konto aus verwalten.

Senden Sie ein Adobe Commerce-Support-Ticket, in dem angefordert wird, dass die Subdomains vom Fastly-Service-Konto in Adobe Commerce auf der Cloud-Infrastruktur an das Fastly-Konto delegiert werden. Fügen Sie die Service-ID für das Fastly-Konto in das Ticket ein.

## DDoS-Schutz

Der DDOS-Schutz ist in den Fastly CDN-Service integriert. Sobald Sie Fastly Services für Ihre Adobe Commerce-Sites aktiviert haben, filtert Fastly den gesamten Web- und Admin-Traffic, um potenzielle Angriffe zu erkennen und zu blockieren.

- Bei Angriffen auf Ebene 3 oder 4 filtert der Fastly-Service den Traffic basierend auf Port und Protokoll heraus und überprüft nur HTTP- oder HTTPS-Anfragen. ICMP-, UDP- und andere netzwerkinitiierte Angriffe werden an unserem Netzwerkrand abgelegt. Dazu gehören Reflexions- und Amplifikationsangriffe, die UDP-Dienste wie SSDP oder NTP nutzen. Durch die Bereitstellung dieses Schutzniveaus verhindern wir effektiv mehrere häufige Angriffe wie Ping of Death, Schlumpf-Angriffe und andere ICMP-basierte Überschwemmungen.

  Verwaltet Angriffe auf TCP-Ebene schnell auf Cache-Ebene. Diese Strategie bietet die erforderliche Skalierbarkeit und den Kontext pro Client, um mit einem SYN-Flutangriff und seinen vielen Varianten, einschließlich TCP-Stack, Ressourcenangriffen und TLS-Angriffen innerhalb von Fastly-Systemen umzugehen.

- Fastly bietet auch Schutz vor Layer 7-Angriffen. Wenn in Ihrem Geschäft Leistungsprobleme auftreten und Sie einen Layer 7 DDoS-Angriff vermuten, reichen Sie ein Adobe Commerce-Support-Ticket ein. Adobe kann benutzerdefinierte Regeln erstellen und auf den Fastly-Service anwenden, um bösartige Anfragen basierend auf Header, Payload oder einer Kombination von Attributen, die den Angriffsdatenverkehr identifizieren, zu untersuchen und herauszufiltern. Siehe [Überprüfung auf DDoS]-Angriffe und [Wie Sie bösartigen Traffic blockieren] im *Adobe Commerce Help Center*.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Überprüfen auf DDoS-Angriffe]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html?lang=de

[Fastly CDN-Modul für Magento 2]: https://github.com/fastly/fastly-magento2

[Fastly Support-Ticket]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[Sperren von bösartigem Traffic]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html?lang=de

[Arbeiten mit Domains]: https://docs.fastly.com/en/guides/working-with-domains
