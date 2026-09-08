---
title: Adobe Commerce Traffic Insights
description: Erfahren Sie mehr über das Tool Adobe Commerce Traffic Insights und wie Sie damit den Traffic in Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt besser verstehen können.
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Traffic-Erkenntnisse

Adobe Commerce Traffic Insights ist eine New Relic One-App, die [!DNL Adobe Commerce on Cloud Infrastructure] Fastly CDN-Traffic visualisiert. Er liest die Fastly CDN-Zugriffsprotokoll-Zeilen, die bereits als `Log`-Ereignisse in New Relic bereitgestellt werden, und rendert einen kuratierten Satz von Diagrammen, die sich auf ein von Ihnen ausgewähltes New Relic-Konto und den Plattformzeitbereich beziehen. Dadurch wird der Edge-Traffic eines Stores visualisiert, ohne dass NRQL, die Abfragesprache von New Relic, manuell geschrieben wird.

## Was es Ihnen bei der Untersuchung hilft

Traffic Insights soll Ihnen bei der Lösung von drei häufigen Problemen helfen:

- **CDN-Bandbreitenüberdeckung** - Traffic-Tendenz oberhalb des Vertragslimits. Ordnen Sie das Volumen einer bestimmten Domain, einem bestimmten Inhaltstyp, einer bestimmten URL oder einem bestimmten Projekt zu, indem Sie es großen Medien, großen Dateien, nicht Cache-fähigen 404-Seiten oder einem ineffizienten Cache zuordnen.
- **Laden von Such-Bots und Crawler** - Eine Suchmaschine oder KI-Crawler, die einen unverhältnismäßig hohen Anteil an Anfragen erzeugt und die Cache-Effizienz und die Ursprungslast beeinträchtigt. Finden Sie heraus, welche benannten Bots am aktivsten sind und was genau sie abrufen.
- **Bösartige Skripte und Scraper** — Abkratzung, Berechtigungsfüllung, Kartentests, Erstellung gefälschter Konten oder Missbrauch der Ebene 7. Aufdecken der Fastly WAF-Signale der nächsten Generation und der IPs, Subnetze und Länder hinter verdächtigem Traffic.

In jedem Fall identifiziert die App das *, was und* des Traffics. Bewerkstelligen Sie diese Informationen mit Fastly VCL-Regeln, Bildoptimierung, Cache-Optimierung, Ratenbegrenzung oder dem Adobe Add[on „Erweiterte &#x200B;](../../cdn/advanced-security.md)&quot; in Ihrer Commerce- und Fastly-Konfiguration. Das [Playbook für Ermittlungen](investigation-playbook.md) behandelt jede dieser Fragen.

## Zugriff auf die App

- **Direkter Link:** [Adobe Commerce Traffic Insights](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47).
- **Auf dem New Relic One-Startbildschirm** (one.newrelic.com) — Sobald das Konto die App abonniert hat, wird es als eigene Kachel **Adobe Commerce Traffic Insights** auf der Startseite angezeigt.
- **In der oberen Suchleiste (Schnellsuche)** - Suchen Sie nach `Adobe Commerce Traffic Insights` und wählen Sie diese aus den Ergebnissen aus.
- **Für einen schnelleren Zugriff anheften** - Verwenden Sie das Stern- oder Anheft-Steuerelement auf der Kachel oder Seitenkopfzeile der App, um es den Favoriten oder der linken Navigation hinzuzufügen. Der genaue Speicherort dieses Steuerelements hängt von der für das Konto verwendeten Version der New Relic-Benutzeroberfläche ab.

## In diesem Handbuch

- **[Die App verstehen](understanding-the-app.md)** - Was Traffic Insights ist, wie man es mit Filtern steuert, wie die Zahlen gemessen werden und was die Daten Ihnen sagen können und was nicht.
- **[Ermittlungs-Playbook](investigation-playbook.md)** - Empfohlene Ansätze zur Lösung der drei Probleme, für die die App entwickelt wurde: Bandbreitenüberlastung, Crawler-Last und böswilliger Traffic. Jedes dieser Elemente verweist auf das Diagramm, das es bestätigt, und gibt den nativen Eskalationspfad [Erweiterte Sicherheit](../../cdn/advanced-security.md) von Adobe für den Fall an, dass die manuelle Risikominderung nicht ausreicht.