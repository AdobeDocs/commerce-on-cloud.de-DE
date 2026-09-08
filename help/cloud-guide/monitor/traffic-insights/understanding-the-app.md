---
title: Grundlegendes zur App
description: Erfahren Sie, wie Adobe Commerce Traffic Insights funktioniert, wie Sie es mit Filtern steuern, wie die Daten gemessen werden und wie Datenbeschränkungen und -leistung auftreten.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Grundlegendes zur App

Die [!DNL Adobe Commerce Traffic Insights]-App visualisiert Raw Fastly Content Delivery Network (CDN)-Zugriffsprotokolle in einem Bild des Edge-Traffics eines Stores. Die Diagramme sind in die folgenden Registerkarten gruppiert:

- **Bandbreite** - Wie die Traffic-Bandbreite im Laufe der Zeit auf Domains, Inhalte und Ressourcentypen sowie Cloud-Projekte verteilt wird.
- **Leistung des vollständigen Seiten-Cache** - Wie effizient wird die dynamische Storefront HTML für Produktdetailseiten (PDP), Produktlistenseiten (PLP) und Seiten des Content-Management-Systems (CMS) am Edge zwischengespeichert.
- **Bots-Aktivitäts- und Anforderungsanalyse** - Traffic aufgeschlüsselt nach bekannten Bot-Agenten, Geolokalisierung, IPs/Subnetzen, URLs und Fastly Web Application Firewall (WAF)-Signalen der nächsten Generation.

Eine vierte In-App-Registerkarte **Dokumentation** enthält Konzeptnotizen und das [Ermittlungs-Playbook](investigation-playbook.md).

## Für wen ist dieser Leitfaden geeignet?

- **Site Operators und Site Reliability Engineering (SREs) untersuchen** CDN-Bandbreitenüberdeckung, Traffic-Spitzen oder die Ursprungslast.
- **Entwickler** Optimieren der FPC-Abdeckung (Full Page Cache) und der Trefferquote oder Implementieren von VCL-Regeln (Fastly Varnish Configuration Language).
- **Administratoren und Sicherheitsingenieure** Erkennung und Abwehr unerwünschter Bots, Scraper und bösartigen automatisierten Datenverkehrs.

Es wird davon ausgegangen, dass Sie mit [!DNL Adobe Commerce on Cloud Infrastructure], Fastly CDN-Konzepten und der grundlegenden New Relic-Navigation vertraut sind.

## Funktionsweise

Wählen Sie oben auf der Seite in den Plattformsteuerelementen ein Konto und einen Zeitbereich aus. Eine optionale **Projekt-ID** kann Diagramme weiter auf bestimmte Cloud-Projekte eingrenzen. Wenn Sie in einem Master-Konto oder einer Partnerschaft ein Konto in der Dropdown-Liste anzeigen können, bedeutet dies nicht, dass Sie es abfragen können. Wenn ein Diagramm einen Berechtigungsfehler meldet, wechseln Sie zu einem Konto, auf das Sie Zugriff auf die New Relic Query Language (NRQL) haben.

Sie wenden weiterhin Filter an, um einen umfassenden Überblick in eine zielgerichtete Untersuchung zu verwandeln. Klicken Sie auf einen Wert in einer Facettenspalte, z. B. Bot, IP, Subnetz, Land oder Inhaltstyp, um einen [globalen Filter“ ](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use). Aktive Filter werden oben im Raster angezeigt und gelten für jedes Widget auf jeder Registerkarte gleichzeitig. Um den Umfang zu erweitern, entfernen Sie einen Filter.

**exemplarisch** - Stellen Sie sich ein Szenario vor, in *die* Gesamtbandbreite“ über dem vertraglichen Zuschlag liegt, und Sie möchten wissen, wer für diese Entwicklung verantwortlich ist:

1. Öffnen Sie die Registerkarte **Bots-Aktivität und Anforderungsanalyse** und lesen Sie **Bandbreitenstruktur** um zu sehen, wie viel Traffic automatisiert im Vergleich zu organischem Traffic erfolgt.
1. Wenn Bots mehr Traffic zu haben scheinen, öffnen Sie **Bekannte Bots nach Bandbreite** und klicken Sie auf den schwersten benannten Bot, z. B. einen Scraper. Dadurch wird ein neuer Filter hinzugefügt, was bedeutet, dass jetzt jedes Widget auf diesen Bot angewendet wird.
1. Lesen Sie **Details zur Wirkung von bekannten Bots** für die entsprechende Anfragerate, den Status-Mix und die FPC-Trefferrate.
1. Um zu sehen, wo der Bot seinen Ursprung hat, überprüfen Sie **Bandbreite nach Land**. Informationen zum Abrufen des Bots finden Sie unter **URLs nach Bandbreite**.
1. Wenn sich der Traffic auf ein Netzwerk konzentriert, klicken Sie auf **Statistiken nach IP-Subnetzen**, um einen Akteur zu bestätigen, der über Adressen in einem einzigen Block rotiert.
1. Jetzt verfügen Sie über das Wer, Was und Wo, um eine zielgerichtete Minderung zu schreiben. Fahren Sie mit dem [Playbook für Ermittlungen](investigation-playbook.md) fort, um zu erfahren, wie Sie vorgehen.

Dieselbe Filtermethode funktioniert bei jeder Ausgangsfacette: bei einem verdächtigen Land, einer einzelnen IP-Adresse, einem Inhaltstyp oder einem URL-Pfadsegment.

## Wie die Daten gemessen werden

Wenn Sie einige Messoptionen verstehen, können Sie den Zahlen leichter vertrauen und sie leichter interpretieren.

- **Bandbreite (BW)** ist die Gesamtzahl der Bytes, die das CDN für die übereinstimmenden Anfragen bereitgestellt hat, wobei **Antwort-Header und Hauptteil)**. Es handelt sich dabei um die Kostenkennzahl, die mit dem Vertragsabzug verrechnet wird.
- **Anfragen (erforderlich)** ist die Anzahl der verschiedenen Anfragen, jedoch wird bei aktivierter Fastly[Abschirmung](https://www.fastly.com/documentation/guides/concepts/shielding/) eine einzelne Anfrage **zweimal** einmal in jedem der folgenden Schritte protokolliert:
  - Innenabschirmung [Point of Presence (POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - EDGE POP
    Dies geschieht, sofern die Antwort nicht direkt aus dem lokalen POP-Cache kommt oder der Shield selbst als POP für den Absenderstandort fungiert. Um eine doppelte Zählung dieser `HIT,MISS` und `MISS,MISS` zu vermeiden, werden die Abfragen der App mit [`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount) über dem `request_id` Feld aggregiert. Dadurch wird ein **Näherungswert** mit einer erwarteten Fehlermarge von **~5 %**, keine exakte Zählung zurückgegeben.
- **CDN-Netzwerksegmente** werden unterschiedlich komprimiert. Die an den Client gesendete Antwort wird komprimiert, der Shield-to-POP-Traffic jedoch [nicht komprimiert](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) um [Edge Side Includes (ESI)-](https://www.fastly.com/documentation/reference/vcl/statements/esi/) beizubehalten. Eine niedrige Cache-Trefferquote erhöht daher das interne Segment stärker als das Client-seitige, da nicht zwischengespeicherte Inhalte wiederholt mit voller, unkomprimierter Größe über den Schild gezogen werden müssen. Diese Komprimierung ist der Grund, warum das **CDN Network Segment Bandwidth**-Widget und die FPC-Trefferquote zwei Ansichten derselben zugrunde liegenden Kosten sind.

## Datenbeschränkungen und Leistung

- **30-tägige Aufbewahrung** - Schnelle CDN-Protokolle werden in New Relic für **30 Tage** gemäß dem Abonnementplan aufbewahrt. Jedes Fenster, das Sie auswählen, muss innerhalb der letzten 30 Tage fallen. Verwenden Sie für eine längerfristige **Gesamtbandbreite** die direkte Fastly-Integration im [!DNL Adobe Commerce admin] Panel, **Dashboard > Fastly > Bandbreite > Summe**, beachten Sie jedoch, dass sie Berichte pro Service-ID ausgibt. Daher müssen die Daten pro Umgebung erfasst und aggregiert werden, um sie mit dem Vertragszuschlag zu vergleichen.
- **60-Sekunden-Abfragelimit** - Die NRQL jedes Diagramms hat eine [60-Sekunden-Ausführungsgrenze](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration). Bei Konten mit sehr hohem Traffic-Aufkommen kann es vorkommen, dass ein Widget beim Überprüfen zu vieler Protokolldatensätze eine Zeitüberschreitung aufweist. Verringern Sie in diesem Fall den Zeitraum und laden Sie die Diagramme neu. Sie können sie für kleinere Registerkarten erneut erweitern.
