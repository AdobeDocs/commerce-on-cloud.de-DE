---
title: Privater Link-Service
description: Erfahren Sie, wie Sie mit dem PrivateLink-Service eine sichere Verbindung zwischen einer privaten Cloud und der Adobe Commerce-Cloud-Plattform in derselben Region herstellen.
feature: Cloud, Iaas, Security
exl-id: 13a7899f-9eb5-4c84-b4c9-993c39d611cc
source-git-commit: 0e7f268de078bd9840358b66606a60b2a2225764
workflow-type: tm+mt
source-wordcount: '1616'
ht-degree: 0%

---

# Privater Link-Service

Adobe Commerce in der Cloud-Infrastruktur unterstützt die Integration mit dem Service [AWS PrivateLink](https://aws.amazon.com/privatelink/) oder [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/). Sie können PrivateLink verwenden, um eine sichere private Kommunikation zwischen Adobe Commerce in Cloud-Infrastrukturumgebungen mit Services und Anwendungen herzustellen, die auf externen Systemen gehostet werden. Sowohl auf die Adobe Commerce-Anwendung als auch auf externe Systeme muss über Virtual Private Cloud (VPC)-Endpunkte zugegriffen werden können, die auf derselben Cloud-Plattform (AWS oder Azure) in derselben Cloud-Region konfiguriert sind.

>[!TIP]
>
>PrivateLink eignet sich am besten zum Schützen von Verbindungen für Nicht-HTTP(S)-Integrationen, wie z. B. Datenbank- oder Dateiübertragungen. Wenn Sie beabsichtigen, Ihr Programm mit Adobe Commerce-APIs zu integrieren, finden Sie weitere Informationen unter Erstellen eines [Adobe](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/)API-Meshs in _API-Mesh für Adobe Developer App Builder_.

## Funktionen und Support

Die PrivateLink-Service-Integration für Adobe Commerce in Cloud-Infrastrukturprojekten umfasst die folgenden Funktionen und Unterstützung:

- Eine sichere Verbindung zwischen einer Virtual Private Cloud (VPC) des Kunden und der Adobe VPC auf derselben Cloud-Plattform (AWS oder Azure) innerhalb derselben Cloud-Region.
- Unterstützung für unidirektionale oder bidirektionale Kommunikation zwischen Endpunkt-Services, die bei Adobe und Kunden-VPCs verfügbar sind.
- Service-Aktivierung:

   - Öffnen der erforderlichen Ports in der Adobe Commerce in der Cloud-Infrastrukturumgebung
   - Herstellen der anfänglichen Verbindung zwischen dem Kunden und Adobe VPCs
   - Fehlerbehebung bei Verbindungsproblemen während der Aktivierung

## Einschränkungen

- Der Support für PrivateLink ist nur in Pro-Produktions- und Staging-Umgebungen verfügbar. Sie ist nicht in lokalen oder Integrationsumgebungen oder in Starter-Projekten verfügbar.
- Sie können keine SSH-Verbindungen mit PrivateLink herstellen. Siehe [SSH-Schlüssel aktivieren](secure-connections.md).
- Die Adobe Commerce-Unterstützung deckt keine Fehlerbehebung bei AWS PrivateLink-Problemen über die anfängliche Aktivierung hinaus ab.
- Kunden tragen die Kosten für die Verwaltung ihrer eigenen VPC.
- **HTTPS-Protokoll (Port 443)-Unterstützung durch Plattform:**
   - **Azure Private Link**: Sie können das HTTPS-Protokoll (Port 443) nicht verwenden, um in der Cloud-Infrastruktur eine Verbindung zu Adobe Commerce herzustellen, da der Ursprung [ Cloaking ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html) ist.
   - **AWS PrivateLink**: Verbindungen mit dem HTTPS-Protokoll (Port 443) werden unterstützt.
- PrivateDNS ist nicht verfügbar.

## PrivateLink-Verbindungstypen

Es stehen zwei PrivateLink-Verbindungstypen zur Verfügung (siehe folgendes Netzwerkdiagramm), um eine sichere Kommunikation zwischen Ihrem Store und externen Systemen zu gewährleisten, die außerhalb der Cloud-Umgebung gehostet werden.

![PrivateLink-Netzwerkdiagramm](../../assets/privatelink-architecture-diagram.png)

Wählen Sie einen der PrivateLink-Verbindungstypen aus, die für Ihre Adobe Commerce in Cloud-Infrastrukturumgebungen am besten geeignet sind:

- **Unidirektional PrivateLink**-Wählen Sie diese Konfiguration, um Daten sicher aus einem Adobe Commerce im Cloud-Infrastrukturspeicher abzurufen.
- **Bidirektionaler PrivateLink**-Wählen Sie diese Konfiguration, um sichere Verbindungen zu und von Systemen außerhalb der Adobe Commerce in der Cloud-Infrastrukturumgebung herzustellen. Die bidirektionale Option erfordert zwei Verbindungen:

   - Eine Verbindung zwischen der VPC des Kunden und der VPC von Adobe
   - Eine Verbindung zwischen der Adobe VPC und der Kunden-VPC

>[!TIP]
>
>Wenden Sie sich an Ihren Netzwerkadministrator oder Cloud Platform-Anbieter, um Hilfe bei der Auswahl des PrivateLink-Verbindungstyps zu erhalten, oder an die VPC-Einrichtung und -Administration. Siehe Cloud Platform PrivateLink-Dokumentation: [AWS PrivateLink](https://aws.amazon.com/privatelink/) oder [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/).

## Aktivierung von PrivateLink anfordern

>[!WARNING]
>
>Die Aktivierung von PrivateLink kann bis zu _fünf_ Werktage dauern. Die Bereitstellung unvollständiger oder ungenauer Informationen kann den Prozess verzögern.

### Voraussetzungen

![Überprüfen](../../assets/fix.svg) Ein Cloud-Konto (AWS oder Azure) in derselben Region wie die Adobe Commerce in der Cloud-Infrastrukturinstanz.

![check](../../assets/fix.svg) Ein VPC in der Kundenumgebung, das die Services hostet, mit denen eine Verbindung über PrivateLink hergestellt werden soll. Hilfe zur Einrichtung von VPC finden Sie in der AWS- oder Azure-Dokumentation oder wenden Sie sich an Ihren Netzwerkadministrator.

![check](../../assets/fix.svg) Für bidirektionale PrivateLink-Verbindungen müssen Sie die Endpunkt-Service-Konfiguration für Ihre Anwendung oder Ihren Service erstellen und einen Endpunkt in Ihrer VPC-Umgebung erstellen, bevor Sie die PrivateLink-Aktivierung anfordern. Siehe [Einrichten für bidirektionale PrivateLink-Verbindungen](#set-up-for-bidirectional-privatelink-connections).

Sammeln Sie die folgenden Daten, die für die Aktivierung von PrivateLink erforderlich sind:

- **Kunden-Cloud-Kontonummer** (AWS oder Azure) - Muss sich in derselben Region wie die Adobe Commerce in der Cloud-Infrastrukturinstanz befinden
- **Cloud-Region** - Geben Sie die Cloud-Region an, in der das Konto zu Verifizierungszwecken gehostet wird
- **Services und Kommunikations-Ports** - Adobe muss Ports öffnen, um die Service-Kommunikation zwischen VPCs zu ermöglichen, z. B. SQL-Port 3306, SFTP-Port 2222
- **Projekt-ID** - Geben Sie die Projekt-ID für Adobe Commerce in der Cloud-Infrastruktur an. Sie können die Projekt-ID und andere Projektinformationen mit dem folgenden [Cloud-CLI](../dev-tools/cloud-cli-overview.md)-Befehl abrufen: `magento-cloud project:info`
- **Verbindungstyp** - Geben Sie für den Verbindungstyp „unidirektional“ oder „bidirektional“ an
- **Endpunktdienst** - Geben Sie für bidirektionale PrivateLink-Verbindungen die DNS-URL für den VPC-Endpunktdienst an, mit dem Adobe eine Verbindung herstellen muss, z. B.: `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **Zugriff auf Endpunkt-Service gewährt** - Um eine Verbindung zu einem externen Service herzustellen, gewähren Sie dem Endpunkt-Service Zugriff auf den folgenden AWS-Kontoprinzipal: `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >Wenn kein Zugriff auf den Endpunkt-Service bereitgestellt wird, wird die bidirektionale PrivateLink-Verbindung zum Service in Ihrer VPC **nicht** hinzugefügt, wodurch die Einrichtung verzögert wird.

#### Zusätzliche Voraussetzungen speziell für die Aktivierung des privaten Azure-Links

- Geben Sie die Cluster-ID an. Melden Sie sich mithilfe von SSH bei der Remote-Instanz an und verwenden Sie den folgenden Befehl: `cat /etc/platform_cluster`
- Damit ein externer Service eine Verbindung zu Ihrem Adobe Commerce Pro-Cluster herstellen kann, benötigen Sie Folgendes:

   - Eine Liste der Ports auf Ihrem Pro-Cluster, die für den neuen externen privaten Endpunkt verfügbar gemacht werden sollen
   - Eine Liste der Azure-Abonnement-IDs für die privaten Endpunktverbindungen

- Um Ihren Adobe Commerce Pro-Cluster mit einem externen Service zu verbinden, benötigen Sie Folgendes:

   - Eine Liste der Ressourcen-IDs für die Ziel-Services. Die IDs des Diensts für externe private Links sehen etwa wie folgt aus:

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### Aktivierungs-Workflow

Im folgenden Workflow wird der Aktivierungsprozess für die PrivateLink-Integration mit Adobe Commerce in der Cloud-Infrastruktur beschrieben.

1. **Kunde** reicht ein Support-Ticket ein, um die Aktivierung von PrivateLink mit der Betreffzeile `PrivateLink support for <company>` anzufordern. Schließen Sie die [Daten, die für die Aktivierung erforderlich sind](#prerequisites) in das Ticket ein. Adobe verwendet das Support-Ticket, um die Kommunikation während des Aktivierungsprozesses zu koordinieren.

1. **Adobe** Ermöglicht den Zugriff des Kundenkontos auf den Endpunkt-Service in Adobe VPC.

   - Aktualisieren Sie die Konfiguration des Adobe-Endpunkt-Service, um Anfragen zu akzeptieren, die vom AWS- oder Azure-Kundenkonto initiiert wurden.
   - Aktualisieren Sie das Support-Ticket, um den Service-Namen für den Adobe VPC-Endpunkt anzugeben, mit dem eine Verbindung hergestellt werden soll, z. B. `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. **Kunde** fügt den Adobe-Endpunktdienst zu seinem Cloud-Konto (AWS oder Azure) hinzu, wodurch eine Verbindungsanfrage an Adobe Trigger wird. Anweisungen finden Sie in der Dokumentation zur Cloud-Plattform:

   - Informationen zu AWS finden Sie [Akzeptieren und Ablehnen von Verbindungsanfragen für Schnittstellenendpunkte].
   - Für Azure siehe [Verbindungsanfragen verwalten].

1. **Adobe** genehmigt die Verbindungsanfrage.

1. Nach der Genehmigung der Verbindungsanfrage **der Kunde** [überprüft die Verbindung](#test-vpc-endpoint-service-connection) zwischen seiner VPC und der Adobe VPC.

1. Zusätzliche Schritte zum Aktivieren bidirektionaler Verbindungen:

   - **Adobe** stellt den Adobe-Kontoprinzipal (Stammbenutzer für AWS- oder Azure-Konto) bereit und fordert Zugriff auf den VPC-Endpunktdienst des Kunden an.
   - **Kunde** Ermöglicht Adobe den Zugriff auf den Endpunkt-Service in der Kunden-VPC. Dabei wird davon ausgegangen, dass der Adobe-Kontoprinzipal Zugriff auf `arn:aws:iam::402592597372:root` hat, wie zuvor in der Voraussetzung **Endpoint Service-Zugriff erteilt** beschrieben.

      - Aktualisieren Sie die Konfiguration des Kundenendpunkt-Service, um Anfragen zu akzeptieren, die vom Adobe-Konto initiiert werden. Anweisungen finden Sie in der Dokumentation zur Cloud-Plattform:

         - Informationen zu AWS finden Sie [Hinzufügen und Entfernen von Berechtigungen für Ihren Endpunkt-Service].
         - Informationen zu Azure finden Sie unter [Verwalten einer privaten Endpunktverbindung]

      - Geben Sie Adobe den Namen des Endpunkt-Services für die Kunden-VPC.

   - **Adobe** fügt den Kundenendpunkt-Service zum Adobe-Plattformkonto (AWS Trigger oder Azure) hinzu, über das eine Verbindungsanfrage an VPC des Kunden gesendet wird.
   - **Kunde** genehmigt die Verbindungsanfrage von Adobe, um die Einrichtung abzuschließen.
   - **Kunde** [überprüft die Verbindung](#test-vpc-endpoint-service-connection) über die Adobe VPC.

## Testen der VPC Endpoint Service-Verbindung

Sie können die Telnet-Anwendung verwenden, um die Verbindung zum VPC-Endpunktdienst zu testen.

**So testen Sie die Verbindung zum VPC-Endpunktdienst**:

1. Wählen Sie im Projektstammverzeichnis die **Staging-** Produktionsumgebung aus, die für den Zugriff auf den PrivateLink-Endpunktdienst konfiguriert ist.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Führen Sie den folgenden CURL-Befehl aus:

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   Beispiel:

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   Beispiel für eine erfolgreiche Antwort:

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   Beispielantwort fehlgeschlagen:

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. Stellen Sie sicher, dass der Service auf VM wartet.

   ```bash
   netstat -na | grep <port>
   ```

1. Überprüfen Sie den Paketfluss.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   Überprüfen Sie die folgenden internen Einstellungen, um sicherzustellen, dass die Konfiguration gültig ist:

   - Einstellungen für Endpunkt- und Endpunkt-Services
   - Einstellungen für den Netzwerklastenausgleich (NLB)
   - Die Zielgruppen in NLB überprüfen, ob sie in Ordnung sind
   - Die netcat/curl-Endpunkt-URL jeder VM (siehe oben)

   In den folgenden Artikeln finden Sie Hilfe zur Fehlerbehebung bei Verbindungsproblemen:

   - [AWS: Fehlerbehebung bei Endpunkt-Service-Verbindungen]
   - [Amazon: Fehlerbehebung bei Verbindungsproblemen mit Azure Private Link]

   Wenn Sie die Fehler nicht beheben können, aktualisieren Sie das Adobe Commerce-Support-Ticket, um Hilfe beim Verbindungsaufbau anzufordern.

## PrivateLink-Konfiguration ändern

[Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) um eine vorhandene PrivateLink-Konfiguration zu ändern. Sie können beispielsweise Änderungen wie die folgenden anfordern:

- Entfernen Sie die PrivateLink-Verbindung aus der Adobe Commerce in der Cloud-Infrastruktur der Pro-Produktions- oder Staging-Umgebung.
- Ändern Sie die Kontonummer der Cloud-Plattform des Kunden für den Zugriff auf den Adobe-Endpunktdienst.
- Hinzufügen oder Entfernen von PrivateLink-Verbindungen von der Adobe VPC zu anderen Endpunkt-Services, die in der VPC-Kundenumgebung verfügbar sind.

## Einrichten für bidirektionale PrivateLink-Verbindungen

Die VPC des Kunden muss über die folgenden Ressourcen verfügen, um bidirektionale PrivateLink-Verbindungen zu unterstützen:

- Ein Netzwerklastenausgleich (NLB)
- Eine Endpunkt-Service-Konfiguration, die den Zugriff auf eine Anwendung oder einen Service über die Kunden-VPC ermöglicht
- Ein [Schnittstellenendpunkt] (AWS) oder [privater Endpunkt] (Azure), der es Adobe ermöglicht, eine Verbindung zu in VPC gehosteten Endpunktdiensten herzustellen

Wenn diese Ressourcen in der VPC des Kunden nicht verfügbar sind, müssen Sie sich bei Ihrem Cloud Platform-Konto anmelden, um die Konfiguration hinzuzufügen.

- Amazon VPC-Konsole - `https://console.aws.amazon.com/vpc/`
- Azure-Portal - `https://portal.azure.com`

In der Dokumentation Ihrer Cloud-Plattform finden Sie Anweisungen zum Einrichten von PrivateLink:

- **Dokumentation zu AWS PrivateLink**
   - [Erstellen eines Netzwerklastenausgleichs]
   - [Erstellen einer Endpunkt-Service-Konfiguration]
   - [Erstellen eines Schnittstellenendpunkts]
   - [Lebenszyklus des Schnittstellenendpunkts]

- **Azure PrivateLink-Dokumentation**
   - [Erstellen eines Lastenausgleichs]
   - [Arbeitsablauf für Azure Private Link]

<!--Link definitions-->

[Akzeptieren und Ablehnen von Verbindungsanfragen für Schnittstellenendpunkte]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Hinzufügen und Entfernen von Berechtigungen für den Endpunkt-Service]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon: Fehlerbehebung bei Verbindungsproblemen mit Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS: Fehlerbehebung bei Endpunkt-Service-Verbindungen]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Arbeitsablauf für Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[Erstellen eines Lastenausgleichs]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[Erstellen eines Netzwerklastenausgleichs]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[Erstellen einer Endpunkt-Service-Konfiguration]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[Erstellen eines Schnittstellenendpunkts]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[Schnittstellenendpunkt]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[Verwalten einer privaten Endpunktverbindung]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[Verbindungsanforderungen verwalten]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[Privater Endpunkt]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
