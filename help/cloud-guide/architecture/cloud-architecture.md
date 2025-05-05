---
title: Cloud-Architektur für Commerce
description: Erfahren Sie, wie sich Starter- und Pro-Projektarchitekturen bei Commerce in der Cloud-Infrastruktur unterscheiden.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Cloud-Architektur für Commerce

Adobe Commerce auf Cloud-Infrastruktur hat einen Starter- und einen Pro-Plan. Jeder Plan verfügt über eine einzigartige Architektur zur Unterstützung Ihres Adobe Commerce-Entwicklungs- und -Bereitstellungsprozesses. Sowohl der Starter-Plan als auch die Pro-Plan-Architektur stellen Datenbanken, Webserver und Caching-Server in mehreren Umgebungen für End-to-End-Tests bereit und unterstützen dabei die kontinuierliche Integration.

Zu Vergleichszwecken enthält jeder Plan die folgenden Infrastrukturfunktionen und unterstützten Produkte.

|          | Starter | Profi |
| -------- | --------------------| ------------------ |
| Kernfunktionen | <ul><li>[Alle Adobe Commerce-Funktionen](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=de)</li><li>PayPal-Onboarding-Tool</li><li>[Commerce-Berichte](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Alle Adobe Commerce-Funktionen](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=de)</li><li>PayPal-Onboarding-Tool</li><li>[Commerce-Berichte](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B-Modul](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infrastruktur und Bereitstellung | <ul><li>Kontinuierliche Cloud-Integrationstools mit unbegrenzten Benutzerzahlen</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) und zusätzliche Sicherheit mit großzügigen Bandbreitenbeschränkungen. Der Service Web Application Firewall (WAF) ist nur in Produktionsumgebungen verfügbar.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Performance Monitoring) auf 3 Verzweigungen: `master` und 2 Ihrer Wahl<br>Platform as a Service(PaaS)-Produktions-, Staging- und Entwicklungsumgebungen (insgesamt 4 aktive Umgebungen), optimiert für Adobe Commerce</li><li>Ausgangs-Filter (ausgehende Firewall)</li></ul> | <ul><li>Kontinuierliche Cloud-Integrationstools mit unbegrenzten Benutzerzahlen</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) und zusätzliche Sicherheit mit großzügigen Bandbreitenbeschränkungen. Der Service Web Application Firewall (WAF) ist nur in Produktionsumgebungen verfügbar.</li><li>[New Relic](../monitor/new-relic-service.md) Produktionsinfrastruktur + APM (Performance Monitoring) für Staging und Produktion. Die [Richtlinie für verwaltete Warnhinweise](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) für Adobe Commerce implementiert Best Practices für die Überwachung, um Sie proaktiv über Anwendungs- und Infrastrukturprobleme zu informieren, die die Site-Leistung beeinträchtigen.</li><li>PaaS-basierte (Integrations[Entwicklungs-)Umgebungen ](pro-architecture.md#integration-environment) Platform as a Service (2 aktive Umgebungen insgesamt), optimiert für Adobe Commerce</li><li>Infrastructure as a Service (IaaS) - dedizierte virtuelle Infrastruktur für Staging- und Produktionsumgebungen</li></ul> |
| Hochverfügbare Infrastruktur | | [Hochverfügbarkeitsarchitektur](pro-architecture.md#redundant-hardware) mit einem Drei-Server-Setup in der zugrunde liegenden Infrastructure as a Service (IaaS), um Zuverlässigkeit und Verfügbarkeit auf Unternehmensniveau zu bieten |
| Dedizierte Hardware | | Isolierte und dedizierte Hardware in der zugrunde liegenden Infrastruktur als Service (IaaS), um ein noch höheres Maß an Zuverlässigkeit und Verfügbarkeit zu bieten |
| 24x7 E-Mail-Support | Rund-um-die-Uhr-Überwachung und E-Mail-Unterstützung für das Kernprogramm und die Cloud-Infrastruktur | Rund-um-die-Uhr-Überwachung und E-Mail-Unterstützung für das Kernprogramm und die Cloud-Infrastruktur |
| Ein dedizierter technischer Kundenberater (CTA) | | Dedizierte technische Kontoverwaltung für die erste Startzeit, beginnend mit Ihrem Abonnement bis zum ersten Start Ihrer Site |
| Add-ons\* | <ul><li>[B2B-Modul](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Gegen Aufpreis erhältlich_

## Einstiegsprojekte

Die [Starter-Planarchitektur](starter-architecture.md) umfasst vier Umgebungen:

- **Integration**: Die Integrationsumgebung bietet zwei testbare Umgebungen. Jede Umgebung enthält eine aktive Git-Verzweigung, Datenbank, Webserver, Caching, einige Services, Umgebungsvariablen und Konfigurationen.

- **Staging**: Wenn Code und Erweiterungen Ihre Tests bestehen, können Sie Ihre `integration` Verzweigung mit der Staging-Umgebung zusammenführen, die Ihre Testumgebung vor der Produktion wird. Dazu gehören die `staging` aktive Verzweigung, Datenbank, Webserver, Caching, Drittanbieterdienste, Umgebungsvariablen, Konfigurationen und Services wie Fastly und New Relic.

- **Produktion** - Wenn der Code bereit und getestet ist, wird der gesamte Code zur Bereitstellung auf der Live-Produktions-Site zu `master` zusammengeführt. Diese Umgebung umfasst Ihre aktive `master`, die Datenbank, den Webserver, das Caching, die Services von Drittanbietern, Umgebungsvariablen und Konfigurationen.

- **Inaktiv** - Sie haben eine unbegrenzte Anzahl von inaktiven Verzweigungen.

## Pro Projekte

Die [Pro-Planarchitektur](pro-architecture.md) verfügt über eine globale `master` mit drei Umgebungen:

- **Integration**: Die Integrationsumgebung bietet eine testbare Umgebung, die eine Datenbank, einen Webserver, eine Zwischenspeicherung, einige Services, Umgebungsvariablen und Konfigurationen umfasst. Sie können Ihren Code entwickeln, bereitstellen und testen, bevor Sie ihn mit der Staging-Umgebung zusammenführen.

   - _Inaktiv_ - Je nach `integration`-Umgebung können Sie eine unbegrenzte Anzahl inaktiver Verzweigungen haben, jedoch nur eine aktive Verzweigung (ohne `integration` ).

- **Staging**: Die Staging-Umgebung dient dem Testen vor der Produktion und umfasst eine Datenbank, einen Webserver, eine Zwischenspeicherung, Drittanbieter-Services, Umgebungsvariablen, Konfigurationen und Services wie Fastly.

- **Produktion** - Die Produktionsumgebung umfasst eine Architektur mit drei Knoten und hoher Verfügbarkeit für Ihre Daten, Services, Caching und Speicher. Die Produktion ist Ihre Live-Umgebung des öffentlichen Speichers mit Umgebungsvariablen, Konfigurationen und Services von Drittanbietern.

## Unterstützte Software und Services

Adobe Commerce in Cloud-Infrastruktur verwendet:

- Betriebssystem: Debian GNU/Linux
- Webserver: nginx
- Datenbank: MySQL (MariaDB)
- Content Delivery Network (CDN): Fastly CDN

Sie können die folgenden Dienste konfigurieren:

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Siehe [Systemanforderungen](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=de) im _Installationshandbuch_ für empfohlene Versionen.

Das Fastly CDN-Modul wird für CDN- und Caching-Services in Staging- und Produktionsumgebungen verwendet. Siehe [Fastly-Services konfigurieren](../cdn/fastly.md).

Informationen zum Konfigurieren der Softwareversionen, die in Ihrer Implementierung verwendet werden sollen, finden Sie in den folgenden Konfigurationsdateien für Adobe Commerce in Cloud-Infrastrukturen:

- [Anwendungskonfiguration (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Umgebungskonfiguration (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Routen-Konfiguration (routes.yaml)](../routes/routes-yaml.md)
- [Service-Konfiguration (services.yaml)](../services/services-yaml.md)
