---
title: Playbook für Ermittlungen
description: Erfahren Sie, wie Sie die CDN-Bandbreitenüberdeckung, die Bot- und Crawler-Belastung sowie bösartigen Traffic mithilfe von Adobe Commerce Traffic Insights untersuchen und wann eskaliert werden sollte.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# Playbook für Ermittlungen

Die [!DNL Adobe Commerce Traffic Insights] App soll Ihnen bei der Untersuchung der folgenden Probleme helfen:

- Bandbreitendeckung
- Crawler laden
- böswilliger Datenverkehr

Alternativ können Sie auch [Erweiterte Sicherheit: Native Bot-Verwaltung, Layer 7-DDoS und Ratenbegrenzung](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting), den nativen Eskalationspfad von Adobe, anfordern, wenn die manuelle Risikominderung nicht ausreicht. Jeder Schritt bezieht sich auf das Widget, das das Symptom zeigt, sodass Sie von einer Metrik zu einer konkreten Aktion wechseln können.

>[!WARNING]
>
>Die Vorschläge auf dieser Seite sind nur Richtlinien. Validieren Sie eine Blockierungsregel immer anhand Ihres eigenen Traffics, bevor Sie sie bereitstellen.

## CDN-Bandbreitendeckung

Bevor Sie eine Bandbreitenüberlastung in Betracht ziehen, sollten Sie wissen, wie die Bandbreite in Rechnung gestellt wird. Der Traffic für **alle** Fastly-Services, die mit dem [!DNL Adobe Commerce on Cloud Infrastructure]-Konto gebündelt sind, einschließlich aller Produktions **und** Staging-Umgebungen, zählt im Vergleich zum Jahresbonus in Ihrem Vertrag für die allgemeine Nutzung. Beginnen Sie mit **Bandbreite > Gesamtbandbreite** und weisen Sie dann das Volume mit **Bandbreite nach Inhaltstyp** und **Bandbreite nach Domain-Details** zu.

### Medieninhalte

Einige Geschäfte dienen aufgrund ihres Katalogs legitimerweise einem großen Teil der Bandbreite als Medien. Wenn **Bandbreite nach Inhaltstyp** eine erhebliche Medienbandbreite anzeigt, sollten Sie die folgenden Maßnahmen in Betracht ziehen:

- Experimentieren Sie mit [Schnell verlustbehaftete Konvertierung](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion) um kleinere Bilder in geringerer Qualität bereitzustellen.
- Untersuchen Sie [Fastly Deep Image Optimization](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization), um skalierte Bilder auf der Seite des Content Delivery Network (CDN) zu generieren.

### Große Dateien

Einige Websites enthalten große Dateien oder bestimmte, umfangreiche Antworten, z. B. Enterprise Resource Planning (ERP)-Integrationen oder -Exporte. Verwenden Sie **URLs nach Bandbreite** um die **BW** und **Avg Size** zu überprüfen, um diese großen Dateien zu finden. Sie können **Pfadsegment-Ebene 1 nach Bandbreite** für eine übergeordnete Ansicht verwenden.

### Stark 404 s

Eine Adobe Commerce **404-Seite nicht gefunden** ist normalerweise eine umfangreiche, Design-stilisierte Seite (~1,5 MB) und **nicht zwischenspeicherbar** sodass wiederholte 404-Seiten anormalen Traffic generieren können. Sogar eine kleine fehlende Ressource wie `favicon.ico` kann zu einer umfangreichen `404` anstelle einer kleinen Datei werden. Verwenden Sie die Spalten **404** und **404 BW** in **Bandwidth By Domain Details**, **URLs By Bandwidth**, **Top IPs By Bandwidth** und **Stats By IP Subnets**, um Clients, IPs und URLs zu finden, die konsistent 404 Volumes generieren. Verringern oder beschränken Sie dann diesen Zugriff, geben Sie beispielsweise stattdessen eine einfache `403` zurück.

### Geringe FPC-Trefferquote

[!DNL Adobe] empfiehlt die Aktivierung von Fastly [Shielding](https://www.fastly.com/documentation/guides/concepts/shielding/), sodass ein Haupt-CDN-Cache-Aggregator den Ursprung bedient, sodass weniger Anfragen von lokalen Points of Presence ([POPs](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)) am nächsten zum Client gelangen. Siehe [Überprüfen der Konfiguration](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding).

Der POP-zu-Client- und der Shield-zu-POP-Traffic werden separat gezählt. Während die Client-Antwort komprimiert wird, wird der Shield-zu-POP-Traffic [nicht komprimiert](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) um die Unterstützung für Edge Side Includes ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/)) beizubehalten. Dies bedeutet, dass eine niedrige Vollseiten-Cache (FPC)-Trefferquote eine viel höhere Bandbreite auf dynamischen Seiten bewirkt. Bestätigen Sie das Symptom mit **FPC Hit Ratio**, **FPC Stats By Domain** und **CDN Network Segment Bandwidth**.

Eine niedrige Trefferrate wird oft durch eine große Anzahl von Crawler-Suchmaschinen verursacht (siehe [Suchbots und Crawler](#search-bots-and-crawlers)). Eine weitere Abmilderung besteht darin, [Crawler einen veralteten Cache bereitzustellen](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/) sofern verfügbar. Wenn umfassende, häufige Cache-Invalidierungen die Ursache sind, verwenden Sie **Cache-Invalidierung nach Tags** und **FPC Age By Top URLs**, um die abgewanderten Tags/URLs zu finden.

## Bots und Crawler suchen

Um die Wirkung von Crawler zu messen, beginnen Sie mit **Bekannte Bots nach** und **Bekannte Bots - Wirkungsdetails** um zu sehen, welche Bots am aktivsten sind, und [&#x200B; Sie dann &#x200B;](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use) einem bestimmten Bot, um nur seine Anfragen zu untersuchen.

### Zu viele Anfragen

Der häufigste Grund dafür, dass ein Suchbot zu viele Anfragen sendet, liegt darin, dass Seiten analysiert werden, die `<meta name="robots" content="index,follow">` enthalten. Bots können Links für die obere Navigation und die mehrschichtige Navigation in einer nahezu endlosen Schleife folgen. Zur Behebung dieses Problems sollten folgende Optionen in Betracht gezogen werden:

>[!WARNING]
>
> Konsultieren Sie einen Experten für Suchmaschinenoptimierung (SEO), bevor Sie die Crawler-Aktivität einschränken. Umschulung kann sich negativ auf Ihre SEO auswirken.

- Fügen Sie `nofollow` zu Links für die obere Navigation und die mehrschichtige Navigation hinzu, z. B. `<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`.
- Ändern Sie das Seiten-Meta-Tag in `index,nofollow` - entweder als allgemeine [Design-Konfigurationseinstellung](https://experienceleague.adobe.com/de/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt) oder pro Seitentyp mit benutzerdefinierten Erweiterungen. Halten Sie `sitemap.xml` genau, sodass Bots immer über eine aktuelle Liste von zu indizierenden Seiten verfügen.
- Aktualisieren Sie `robots.txt`, um Pfade zu blockieren, und Ressourcen, auf die Bots keinen Zugriff haben sollten.
- Beachten Sie, dass die `crawl-delay`-Direktive nicht Teil des offiziellen Robots-Ausschlussprotokolls ist, aber es funktioniert für einige Bots, wie Bingbot, Slurp, SEMrushBot und einige andere. Der Googlebot ignoriert diese Anweisung.
- Fügen Sie Regeln für das Ratenlimit hinzu. Im Fastly[Modul gibt es &#x200B;](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection) (missbräuchlichen Crawler-Schutz). Für eine feinere Steuerung kann ein [benutzerdefinierter VCL-Code (Varnish Configuration Language](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets)-Ausschnitt) `429` (zu viele Anfragen) oder `405` (Methode nicht zulässig) für einen Benutzeragenten-Regex mit einem individuellen Ratenlimit zurückgeben. Die bevorzugte Methode und den bevorzugten Antwort-Code finden Sie in der Dokumentation zum Crawler-Verzeichnis . Siehe Fastlys [VCL-Leitlinien zur Ratenbegrenzung](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/).
- KI und Crawler mit großen Sprachmodellen (LLM) sind ein immer größer werdender Sonderfall. Sie identifizieren sich nicht immer konsistent, sodass VCL-Benutzeragentenregeln hinterherhinken können. Das Add[on „Erweiterte Sicherheit](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/advanced-security) von Adobe verfügt über [natives Bot-Management](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting) das verifizierte von verdächtigen KI-Crawlers und -Abrufern am Edge unterscheiden kann, was VCL alleine nicht kann.

### Blockieren unerwünschter Crawler

Wenn bestimmte Suchmaschinen erheblichen Traffic generieren und für das Unternehmen nicht wichtig sind, können sie vollständig blockiert werden:

- Einige Bots folgen `robots.txt` Änderungen 1-2 Tage später, nachdem sie ihre Parsing-Regeln erneut gelesen und aktualisiert haben.
- Wenn eine Crawler `robots.txt` ignoriert, blockieren Sie sie mit einem benutzerdefinierten VCL-Code-Ausschnitt ([Beispiel](https://experienceleague.adobe.com/de/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent)). Einige Crawler dokumentieren dies explizit als die bevorzugte oder einzige Methode zur Frequenzkontrolle.

## Bösartige Scripts &amp; Scraper

Verwenden Sie die Traffic Insights-App, um die allgemeinen Angriffsrichtungen zu identifizieren, und filtern Sie nach Bedarf nach Fokusbereichen. Wenn Red-Flag-Anfragen überwiegend von bestimmten IPs, Subnetzen oder geografischen Standorten stammen (**Top-IPs nach Anzahl der Anfragen**, **Stats nach IP-Subnetzen**, **Stats nach Land**), sollten Sie sie mit benutzerdefiniertem Fastly VCL blockieren.

Jedes Cloud-Infrastrukturprojekt verfügt bereits über einen Grundbestand an automatischem Schutz, unabhängig von der Konfiguration, die Sie durchführen. Die enthaltene Web Application Firewall (WAF) blockiert sofort SQL-Injections und bekannte bösartige IP-Signale (Backdoor, Attack Tooling, CMDEXE, Log4J-JNDI, Traversal, XSS) und begrenzt die Anzahl anderer nicht bösartiger IPs, sobald sie 50 Anfragen/Minute, 350 Anfragen/10 Minuten oder 1.800 Anfragen/Stunde überschreiten. Diese Baseline wird durch **Anforderungen nach WAF-Antwort** und die WAF-Signalspalten in den Tabellen dieser App angezeigt. Eine Spitze in diesen Spalten bedeutet nicht unbedingt, dass Sie nicht geschützt werden.

- Achten Sie auf das Füllen von Anmeldeinformationen, die Übernahme von Konten, die Erstellung gefälschter Konten, Kartentests, das Scraping von Inhalten und das Horten von Inventar/Warenkorb. Diese von Bots gesteuerten Missbrauchsmuster werden auf der Registerkarte **Bots-Aktivität und Anforderungsanalyse** angezeigt. Die Signatur, nach der in den Endpunkten „Top-IPs nach Anzahl der Anfragen“ und „Details zur Auswirkung auf bekannte Bots“ **Traffic mit hohem Volumen, geringer Vielfalt,** Anmelde-, Konto-, Checkout- oder **gesucht werden sollte**.
- Schützen Sie Checkout- und Checkout-API-Endpunkte mit [Google reCAPTCHA](https://experienceleague.adobe.com/de/docs/commerce-admin/systems/security/captcha/security-google-recaptcha) vor Bot-Angriffen.
- Verwenden Sie das native Ratenlimit des Fastly-Moduls [Pfadschutz](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection).
- Markieren Sie [WAF-Signale der nächsten Generation](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/) im kommagetrennten `Sigsci_Tags` und kombinieren Sie relevante Signalübereinstimmungen zu einer zielgerichteten Blockierungsregel. Der Wert einer verdächtigen Anfrage kann wie `BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT` aussehen. Der WAF kennzeichnet eine IP mit `SITE-FLAGGED-IP` bis zu einem Schwellenwert, bevor sie automatisch zu blockieren beginnt. Die Widgets **WAF-Angriff und -Anomalie**, **WAF-Bots** und **Anfragen nach WAF-Antwort** und die WAF-Spalten in den IP-, Subnetz- und Ländertabellen zeigen diese an.
- Gängige Ansätze finden Sie im Adobe[Artikel zum Blockieren von bösartigem Traffic für Adobe Commerce &#x200B;](https://experienceleague.adobe.com/de/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level) Fastly-Ebene .
- Bei komplexen Szenarien, in denen eine manuelle Blockierung keine praktikable Option ist, z. B. bei nachhaltigen Bot-Kampagnen, Angriffen, die sich über viele IPs/APIs erstrecken, oder bei Layer 7 Distributed Denial of Service (DDoS), sollten Sie zunächst das Add-on [Advanced Security](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/advanced-security) von Adobe in Betracht ziehen (siehe [Natives Bot-Management](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)). Es läuft auf der gleichen Fastly-Edge, die Ihre Storefront bedient. Wenn Sie Funktionen außerhalb seines Bereichs benötigen, ist ein verwalteter Bot-Minderungs-Service eines Drittanbieters mit nativer Fastly-Integration wie [Datadome](https://docs.datadome.co/docs/module-fastly) oder [HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) (früher PerimeterX) die empfohlene Alternative. All diese Optionen verursachen zusätzliche Kosten.

## Erweiterte Sicherheit: native Bot-Verwaltung, Layer 7-DDoS und Ratenbegrenzung

In den vorherigen Abschnitten wird beschrieben, was mit den Daten der Traffic Insights-App und dem Handbuch Fastly VCL getan werden kann. Für Szenarien, in denen dies nicht ausreicht, z. B. nachhaltige oder sich weiterentwickelnde Bot-Kampagnen, DDoS auf Ebene 7 (Anwendungsebene) oder Missbrauch, der sich dünn über viele IPs und API-Endpunkte verteilt, bietet Adobe [Erweiterte Sicherheit](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/advanced-security).

Advanced Security ist ein kostenpflichtiges Add-on für [!DNL Adobe Commerce on Cloud Infrastructure], das die Edge-Bot-Verwaltung (einschließlich KI-Crawler und Abruferkennung), den Layer 7-DDoS-Schutz und die erweiterte Ratenbegrenzung auf derselben Fastly-Plattform hinzufügt, die bereits die Storefront bedient. Unter [Erweiterte Sicherheit](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/cdn/advanced-security) finden Sie alle Funktionen, aktuellen Einschränkungen und Informationen zur Anfrage.

Verwenden Sie nach dem Kauf und der Aktivierung die Traffic Insights-App, um zu überprüfen, ob die erweiterte Sicherheit funktioniert. Die Entscheidungen werden über dieselben `Sigsci_Tags` und `Agent_response` Felder hinter **WAF-Angriff- und Anomaliesignalen**, **WAF-Bots-Signalen** und **Anfragen nach WAF-Antwort** gemeldet. Vergleichen Sie diese Widgets vor und nach der Aktivierung, um zu bestätigen, dass sie aktiv auf Ihren Traffic reagieren.
