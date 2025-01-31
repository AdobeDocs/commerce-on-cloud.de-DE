---
title: Bereitstellung von Commerce in Cloud Manager
description: Erfahren Sie, wie Sie einen technischen Berater für Adobe-Kunden vorbereiten, um Ihr Adobe Commerce-on-Cloud-Infrastrukturprojekt bereitzustellen.
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Voraussetzungen für die Bereitstellung von Commerce auf Cloud

Beginnen wir mit der Initialisierung Ihres Commerce-Projekts in der Cloud-Infrastruktur!

Bevor Adobe Ihre Commerce in Cloud-Projektumgebungen bereitstellt, sollten Sie die folgenden Strategien in Betracht ziehen und Antworten für Ihre erste Beratung mit Ihrem Adobe-Account-Team vorbereiten. Verwenden Sie die folgenden Abschnitte als Checkliste, um Sie bei der Vorbereitung Ihres Gesprächs mit einem technischen Kundenberater für die Bereitstellung eines Cloud-Projekts zu unterstützen:

## Domain-Definition

**Frage 1**: _Welche Domain(s) möchten Sie für den Site-Launch verwenden?_

Bereiten Sie sich auf die Integration der Fastly- und nginx-Services vor, indem Sie Ihre Top-Level-Domains und Subdomains für die Pro Staging- und Produktionsumgebungen definieren. Nach der Ersteinrichtung können Sie nur Domains hinzufügen, indem Sie ein Adobe Commerce-Support-Ticket senden. Es wird daher empfohlen, Ihre Domain-Informationen vorzubereiten.

Beispiele für Produktions- und Staging-Domains:

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

Weitere [ zu mehreren oder eindeutigen Domains finden Sie unter](../cloud-guide/store/multiple-sites.md)Einrichten mehrerer Websites oder Stores _im Handbuch zu_ Commerce in Cloud-Infrastrukturen.

Siehe [Mehrere Fastly-Konten und zugewiesene Domains](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"}, wenn Sie über ein vorhandenes Fastly-Konto verfügen, das denselben Apex und dieselbe Subdomain verknüpft, die auf Ihrer Adobe Commerce-Site verwendet werden.

## Transaktions-E-Mail-Domain

**Frage 2**: _Welche Domain(s) möchten Sie für Transaktions-E-Mails verwenden?_

Adobe Commerce in Cloud Manager verwendet den SMTP-Proxy-Service (Simple Mail Transfer Protocol) SendGrid, der die Authentifizierung ausgehender E-Mails und die Überwachung der Reputation bereitstellt. SendGrid sendet Transaktions-E-Mails in Ihrem Namen, sodass Domain-Informationen erforderlich sind.

Beispiel für die SendGrid-Domain: `example@your-store.com`

Siehe [SendGrid-E](../cloud-guide/project/sendgrid.md)Mail-Dienst) im Handbuch _Commerce_ Cloud-Infrastruktur für weitere Anleitungen zu Transaktions-E-Mails und Domain-Einstellungen.

## Speicherzuweisung

**Frage 3**: _Wie viel von Ihrem vertraglich vereinbarten Speicher möchten Sie für den Datei-Upload und für die Datenbank bereitstellen?_

Adobe Commerce in der Cloud-Infrastruktur verwendet Speicher für die Dateistruktur, die Suchindizierung und die Datenbank. Sie können den Speicher nach Bedarf unterteilen, beginnend mit 10 GB für jede Partition.

Die Menge an Speicher, die Sie für Ihr Cloud-Projekt vertraglich gebunden haben, stellt den Gesamtspeicher für jede Umgebung dar. Wenn Sie beispielsweise 50 GB Speicherplatz erworben haben, verfügen Sie über 50 GB Speicherplatz für jede Umgebung. Es gibt 50 GB separaten Speicher für die Produktions-, Staging- bzw. Integrationsumgebung.

Sie können Ihre vertraglich vereinbarte Lagermenge jederzeit erhöhen. Für Pro-Produktions- und Staging-Umgebungen müssen Sie ein Adobe Commerce-Support-Ticket einreichen, um die Speicherplatzzuweisung zu ändern. Eine Vergrößerung der Pro-Produktions- und Staging-Umgebungen kann nur in bestimmten Intervallen erfolgen. Abhängig von Ihrer aktuellen Speicherplatznutzung empfiehlt das Support-Team möglicherweise, die Speicherplatzzuweisung um mindestens 10 GB zu erhöhen. Nach der Zuweisung kann die Speichererhöhung für Pro Staging und Produktion **nicht** rückgängig gemacht werden.

Siehe [Verwalten von Festplattenspeicher](../cloud-guide/storage/manage-disk-space.md) im Handbuch _Commerce on Cloud Infrastructure_.

## Cloud Service-Region

**Frage 4**: _Welche Cloud Service-Region liegt am bequemsten in Ihrer Nähe?_

Wählen Sie entweder Amazon Web Services (AWS) oder Microsoft Azure as a Service (IaaS) als Grundlage für Ihre Adobe Commerce on Cloud Infrastructure Pro-Projekte. Jeder Dienstleister ist in mehreren Regionen tätig und bietet mehrere Verfügbarkeitszonen. Wählen Sie eine Region aus, die Ihrem Standort entspricht, und reduzieren Sie das Latenzpotenzial und die höheren Kosten.

Siehe [Karte der Cloud-Regionen von Adobe Commerce](../cloud-guide/overview.md).

## Verbindungsdienst

**Frage 5**: _Benötigen Sie einen PrivateLink-Service? Wenn ja, in welcher Region ist die PrivateLink-Verbindung?_

Adobe Commerce in der Cloud-Infrastruktur unterstützt die Integration mit dem AWS PrivateLink- oder Azure Private Link-Service. Obwohl dieser Service optional ist, wird PrivateLink verwendet, um eine sichere, private Kommunikation zwischen Cloud-Infrastrukturumgebungen mit Services und Anwendungen herzustellen, die auf externen Systemen gehostet werden.

Es ist wichtig, Ihre Cloud Service-Strategie (AWS oder Azure) zu berücksichtigen, damit die Adobe Commerce-Instanz in derselben Region zugänglich ist. Siehe [PrivateLink-](../cloud-guide/development/privatelink-service.md) im Handbuch _Commerce on Cloud Infrastructure_, um weitere Informationen zur regionalen Barrierefreiheit zu erhalten.

## Target-Site-Launch

**Frage 6**: _Welches ist das voraussichtliche Zieldatum für die Markteinführung?_

Der Start einer Site erfordert iterative Konfiguration und Tests, um den Erfolg des Site-Starts sicherzustellen. Das Festlegen eines Zieldatums hilft Ihnen und Ihrem Adobe-Account-Team bei der Vorbereitung auf die endgültigen Aktivitäten vor dem Start, zu denen auch ein Aufruf zur Koordinierung der letzten Schritte gehört.

Siehe die [Launch-Site-Übersicht](../cloud-guide/launch/overview.md) im Handbuch zu _Commerce_ Cloud-Infrastruktur, um den vollständigen Prozess zu überprüfen und eine Kopie der Launch-Checkliste herunterzuladen.

>[!TIP]
>
> Werfen Sie einen kurzen Blick auf das Cloud-Portal und greifen Sie auf Ihr neues Cloud-Projekt zu.
>
>**Nächster Schritt**: [Onboarding bei Commerce](onboarding.md)
