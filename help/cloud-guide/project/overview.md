---
title: Cloud-Infrastrukturprojekt
description: Lesen Sie einen Überblick über die Adobe Commerce in Cloud [!DNL Cloud Console] Infrastruktur und erfahren Sie, wie Sie auf die Kontoeinstellungen zugreifen können.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 8eed04c7-6469-45a4-aa89-dc594c977264
source-git-commit: 00b1b6578c226a304697963d17ba349ea17da260
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Cloud-Infrastrukturprojekt

Das Adobe Commerce on Cloud-Infrastrukturprojekt umfasst den gesamten Code in Git-Verzweigungen, zugehörigen Umgebungen und Skripten zur Bereitstellung der [!DNL Commerce]. Umgebungen enthalten Dienste zur Unterstützung der [!DNL Commerce], einschließlich einer Datenbank, eines Webservers und eines Caching-Servers.

Adobe bietet eine [!DNL Cloud Console] und Entwickler-Tools zur vollständigen Verwaltung aller Aspekte Ihres Projekts. Als Kontoinhaber haben Sie vollen Zugriff auf alle Umgebungen.

## [!DNL Cloud Console]

Die [!DNL Cloud Console] bietet interaktive Methoden zum Erstellen, Verwalten und Bereitstellen von Commerce-Code in einem benutzerfreundlichen Format. [Melden Sie sich bei an [!DNL Cloud Console]](https://console.adobecommerce.com) um Ihre Projektliste anzuzeigen. Sie können nur Projekte sehen, für die Sie als Admin oder für bestimmte Umgebungstypen über Zugriffsberechtigungen verfügen. Wenn Sie Adobe-Lösungspartner sind, werden möglicherweise mehrere Projekte für von Ihnen unterstützte Clients angezeigt.

>[!TIP]
>
>Wenn keine Projekte angezeigt werden, wenden Sie sich an den [Kontoinhaber oder Projektadministrator](../project/user-access.md) der mit dem Projekt verknüpft ist und beantragen Sie Zugriff. Erstmalige Benutzer finden weitere Informationen unter [Onboarding-Thema](../../get-started/onboarding.md#cloud-console) im _Erste Schritte_.

Die Ansicht _Alle Projekte_ listet alle Projekte auf, für die Sie über Zugriffsberechtigungen verfügen. Sie können auf **[!UICONTROL Show filters]** klicken und Ihre Projektliste nach Typ, Region oder Plan filtern.

![Projektliste](../../assets/ui-allprojects-list.png)

### Projektübersicht

Wenn Sie ein Projekt in der Liste _Alle Projekte_ auswählen, wird die Projektübersicht geöffnet. In der Projektübersicht wird immer eine Projekt-Navigationsleiste angezeigt, die einen Umgebungsselektor und eine Konfigurationsschaltfläche enthält:

![Projektnavigation](../../assets/project-nav.png)

Die Projektübersicht zeigt, sofern keine Umgebung ausgewählt ist, eine Zusammenfassung der Projektdetails im Vorschaubereich an:

- Projektname
- Region, Projekt-ID
- Planung, zugewiesener Speicher, Umgebungen, Benutzer
- Storefront-URL mit **[!UICONTROL Set a custom domain]** Schaltfläche

Und in der Hauptprojektübersicht:

- Die Ansicht Umgebungen zeigt eine Liste oder Baumstrukturansicht der Umgebungen ![aktive Verzweigung](../../assets/icon-active.png){width="32"} (aktiv) und ![inaktive Verzweigung](../../assets/icon-inactive.png){width="32"} (inaktiv) an.
- [Aktivitäts-Stream](activity-stream.md) zeigt laufende, ausstehende und aktuelle Aktivitäten für das Projekt an.
<!-- - Apps & Services—Shows a topology of service containers -->

Bei **Starter**-Projekten gibt es eine Hierarchie von Verzweigungen, die von `master` (Produktion) beginnt. Jede Verzweigung, die Sie erstellen, wird als untergeordnete Elemente der `master` Verzweigung angezeigt. Adobe empfiehlt, zunächst eine `staging` Verzweigung und dann eine `integration` Verzweigung für die Entwicklung zu erstellen. Siehe [Starter-Architektur](../architecture/starter-architecture.md).

Für **Pro** gibt es eine Hierarchie von Verzweigungen, die von `production` bis `staging` bis `integration` reicht. Das ![Dediziertes Symbol](../../assets/icon-dedicated.png){width="32"} zeigt an, dass die Verzweigung in einer dedizierten Umgebung bereitgestellt wird. Alle Verzweigungen, die Sie erstellen, werden als untergeordnete Elemente des `integration` Zweigs angezeigt. Siehe [Pro-Architektur](../architecture/pro-architecture.md).

![Pro Umgebungsliste](../../assets/pro-environments.png)

### Umgebungsübersicht

Wenn Sie eine Umgebung in der Projekt-Navigationsleiste auswählen, ändern sich die Übersicht und die Navigationsleiste, sodass sie sich auf die ausgewählte Umgebung konzentrieren. Die Navigationsleiste enthält Verzweigungssteuerelemente (Verzweigung, Zusammenführen und Synchronisieren) und eine Konfigurationsschaltfläche:

![Umgebung ausgewählt](../../assets/environment-selected.png)

Die Umgebungsübersicht zeigt eine Zusammenfassung der Umgebungsdetails im Vorschaubereich an:

- Umgebungsname, Typ
- Region, Projekt-ID
- Datum und Uhrzeit der letzten Aktivität, einschließlich Backup
- HTTP-Zugriff und Suchmaschinenstatus
- Maschinenname der Umgebung zugewiesen
- Umgebungsstatus (aktiv oder inaktiv)
- Storefront-URL mit **[!UICONTROL Set a custom domain]** Schaltfläche

Und in der Hauptübersicht zur Umgebung:

- [Aktivitäts-Stream](activity-stream.md) bildet die wichtigste Umgebungsübersicht und zeigt laufende, ausstehende und aktuelle Aktivitäten für die ausgewählte Umgebung an.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- [Registerkarte Sicherungen](../storage/snapshots.md#create-a-manual-backup) enthält eine Liste der gespeicherten Sicherungen, den Verlauf der Sicherungsaktionen und die Schaltfläche Sichern .

### Zugriff auf Storefront

Jede aktive Umgebung verfügt über eine Storefront. Wählen Sie in der oberen Navigationsleiste eine Umgebung aus und klicken Sie auf die URL in der Umgebungsübersicht . Außerdem gibt es eine **[!UICONTROL URLs]** Liste auf der rechten Seite über der Aktivitätsliste.

Die Web-Zugriffs-URL kann Folgendes enthalten:

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **Eindeutige ID** = 7 zufällige alphanumerische Zeichen
- **Projekt-ID** = Projekt-ID mit 13 Zeichen
- **Region** = Name der AWS- oder Azure-Region, siehe &quot;[&#x200B; IP-Adressen](regional-ip-addresses.md)

Die Pro-Produktions- und Staging-Umgebungen umfassen drei Knoten, auf die Sie über die folgenden Links zugreifen können:

- Load-Balancer-URLs:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- Direkter Zugriff auf einen der drei redundanten Server:

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  Die Produktions-URL wird vom Content Delivery Network (CDN) verwendet.

## Einstellungen

Öffnen Sie _Bedienfeld_ Einstellungen“, indem Sie auf das Symbol ![Projektsymbol konfigurieren](../../assets/icon-configure.png){width="36"} (Konfigurieren) rechts in der Projektnavigation klicken.

### Projekteinstellungen

**[!UICONTROL Project Settings]** wird ein Menü mit Steuerelementen auf Projektebene zum Verwalten von Benutzern, Variablen und mehr erweitert:

| Option | Beschreibung |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| Allgemein | Verwalten Sie die Zeitzone für die Verwendung mit der Planung von Sicherungen oder Wartungsarbeiten. |
| Zugriff | Verwalten Sie [Benutzerzugriff](user-access.md) auf Projekt- und Umgebungstypen. |
| Zertifikate | Zeigt eine Liste der mit dem Projekt verknüpften SSL-Zertifikate an. |
| Schlüssel bereitstellen | Fügen Sie den öffentlichen Schlüssel zum Projekt-Code-Repository hinzu und zeigen Sie ihn an. |
| Domains | Fügen Sie dem Projekt einen Domain-Namen hinzu. Siehe [Verwalten von Domains](../cdn/fastly-custom-cache-configuration.md#manage-domains). |
| Integrationen | Hinzufügen und Verwalten von [Integrationen](../integrations/overview.md) wie Statusbenachrichtigungen und Webhooks. |
| Variablen | Fügen Sie [Variablen auf Projektebene](../environment/variable-levels.md) hinzu, die zum Zeitpunkt des Builds und der Laufzeit in allen Umgebungen verfügbar sind. |

{style="table-layout:auto"}

### Umgebungseinstellungen

Klicken Sie auf **[!UICONTROL Environments]** und wählen Sie eine bestimmte Umgebung aus der Liste aus, um Steuerelemente zum Verwalten von Site-Einstellungen, Umgebungsvariablen und mehr zu erhalten:

| Option | Beschreibung |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Allgemein | Konfigurieren von Anzeigename, Umgebungstyp und übergeordneter Umgebung.<br>Umschalten zwischen Umgebungseinstellungen: |
|           | **Ausgehende E-Mails aktivieren**: Senden Sie [ausgehende E-Mails](outgoing-emails.md) aus der Umgebung mithilfe des SMTP-Protokolls. |
|           | **Vor Suchmaschinen verstecken**: Blockieren Sie Suchmaschinen-Indexer und -Crawler von der Website. |
|           | **HTTP-Zugriffssteuerung**: Aktivieren Sie die Sicherheitskonfiguration für den [!DNL Cloud Console] mithilfe einer Anmelde- und IP-Adresszugriffssteuerung. |
|           | Status ist `active` oder `inactive`. Der Großteil Ihrer Arbeit befindet sich in einer aktiven Umgebung. Sie können die Umgebung deaktivieren oder löschen. |
| Variablen | Anzeigen, Erstellen und Verwalten von [Variablen auf &#x200B;](../environment/variable-levels.md) zur Laufzeit. |
| Domains | Zeigen Sie eine Liste der [konfigurierten Routen](../routes/routes-yaml.md) an. |

{style="table-layout:auto"}

>[!WARNING]
>
>**VERWENDEN SIE** HTTP-Zugriffssteuerungs-Methode zum Schützen von Pro-Staging- und Produktionsumgebungen. Dadurch wird die Fastly-Zwischenspeicherung unterbrochen. Verwenden Sie stattdessen die Funktion [Blockieren](../cdn/fastly-vcl-blocking.md), die im Fastly CDN für Adobe Commerce verfügbar ist, um den Zugriff zu blockieren oder die Zugriffssteuerung mit [Fastly Basic Auth](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) zu implementieren.

## Fastly und New Relic Anmeldedaten

Ihr Projekt umfasst [Fastly](../cdn/fastly.md) und [New Relic](../monitor/new-relic-service.md). Die Projektdetails enthalten Informationen zu Ihrem Projektplan und wichtige Lizenzen und Token für diese Integrationen. Nur der Lizenzinhaber hat anfänglichen Zugriff auf die Anmeldeinformationen und Services. Geben Sie diese Anmeldeinformationen bei Bedarf an technische Ressourcen und Entwicklerressourcen weiter.

- [Fastly](https://www.fastly.com/) bietet Content Delivery (CDN), Bildoptimierung und Sicherheits-Services (DDoS und WAF) für Ihre Adobe Commerce in Cloud-Infrastrukturprojekten. Siehe [Abrufen von Fastly-Anmeldeinformationen](../cdn/fastly-configuration.md#get-fastly-credentials).

- [New Relic](../monitor/new-relic-service.md) stellt Anwendungsmetriken und Leistungsinformationen für Staging- und Produktionsumgebungen bereit.

Verwenden Sie die [Cloud-CLI](../dev-tools/cloud-cli-overview.md) um Ihre Integrations-Token, IDs und mehr zu überprüfen:

```bash
magento-cloud subscription:info services
```
