---
title: Technologie-Stack
description: Weitere Informationen finden Sie im Technologie-Stack, der die Commerce on Cloud-Infrastruktur bildet.
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Technologie-Stack

Stellen Sie sich Adobe Commerce in der Cloud-Infrastruktur als fünf funktionale Ebenen vor, wie unten dargestellt:

![Cloud-Stack](../../assets/CloudStack.svg)

1. [**Cloud-Infrastruktur**](pro-architecture.md): Wählen Sie entweder Amazon Web Services (AWS) oder Microsoft Azure als Infrastructure as a Service (IaaS)-Grundlage für Ihre Adobe Commerce in Cloud Infrastructure Pro-Projekten.

   Adobe analysiert routinemäßig die Nutzung Ihrer Virtual Compute Resource (vCPU) und weist automatisch Ressourcen zu, um Ihre langfristige Nutzung zu optimieren und das Risiko einer Überschreitung Ihrer maximalen jährlichen vCPU-Tagegelder zu minimieren. Wenn Sie für bestimmte Zeiträume einen erhöhten Site-Traffic erwarten, müssen Sie weiterhin ein Support-Ticket öffnen, um [eine temporäre Aktualisierung anzufordern](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=de).

1. [**Platform as a Service**](cloud-architecture.md): Jedes Adobe Commerce on Cloud Infrastructure-Projekt bietet eine Platform as a Service (PaaS)-Integrationsumgebung zum Entwickeln, Testen und Integrieren von Services.
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce in der Cloud-Infrastruktur bietet eine vorab bereitgestellte Infrastruktur, die PHP, MySQL (MariaDB), Redis, Message Queue Services ([!DNL RabbitMQ] oder [!DNL ActiveMQ]) und unterstützte Suchmaschinentechnologien umfasst.
1. [**Leistungs-Tools**](../monitor/new-relic-service.md): Mit den Leistungs-Tools von New Relic können Sie Anwendungen und Infrastrukturen debuggen, überwachen und verwalten, indem Sie Daten aus Ihrer Adobe Commerce in Cloud-Infrastrukturprojekten erfassen, analysieren und anzeigen.
1. [**Content Delivery Network (CDN), Web Application Firewall ([!DNL WAF]) und Image Optimization (IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) - Bietet sichere CDN-Services mit integriertem Schutz vor DDoS-Angriffen (Distributed Denial of Service) wie [!DNL Ping of Death], [!DNL Smurf] und anderen Internet Control Message Protocol (ICMP)-basierten Flutangriffen.
   * [Web Application Firewall (WAF)](../cdn/fastly-waf-service.md) - WAF-Services stellen die PCI-Compliance für Adobe Commerce-Storefronts in Produktionsumgebungen sicher und WAF-Richtlinien, die Ihre Adobe Commerce-Web-Anwendungen vor Injection-Angriffen, böswilliger Eingabe, Cross-Site-Scripting, Datenexfiltration, HTTP-Protokollverletzungen und anderen [[!DNL OWASP] Top 10-Sicherheitsbedrohungen](https://owasp.org/www-project-top-ten/) schützen.
   * [Bildoptimierung (IO)](../cdn/fastly-image-optimization.md): Bietet Bildbearbeitung und -optimierung in Echtzeit, um die Bildbereitstellung zu beschleunigen und die Wartung von Bildquellensätzen für responsive Web-Anwendungen zu vereinfachen. Fastly IO verlagert die Bildverarbeitung und verändert die Größe der Last, sodass Server Bestellungen und Konversionen effizient verarbeiten können.

Monolithische Anwendungen sind ressourcenintensiv, schwer skalierbar und schnell zu bedienen. Mit der Cloud-Infrastruktur erhalten Commerce-Kunden beispiellosen Zugriff auf SaaS-basierte Microservices, die reich, intelligent und leistungsstark sind. Siehe [Unterstützte Software und Services](cloud-architecture.md#supported-software-and-services).

Verwenden Sie das [Commerce-Handbuch „Erste &#x200B;](../../get-started/overview.md)&quot;, um Ihr neues Cloud-Programm einzurichten und mit der Verwaltung Ihres [!DNL Commerce]-Programms in einer Cloud-nativen Umgebung zu beginnen.
