---
title: Web Application Firewall (WAF)
description: Erfahren Sie, wie der Fastly WAF-Service den Traffic von bösartigen Anfragen erkennt, protokolliert und blockiert, bevor er das Adobe Commerce-Netzwerk oder Sites beschädigen kann.
feature: Cloud, Configuration, Security
exl-id: f00e35f2-9800-4e24-a4d0-d36fde59a003
TQID: https://experienceleague.adobe.com/GhpLOxZbJMYhBTmj8W4a90wfmFYq-8h2bvrKQWj6ZWk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
subfeature_v2:
  - id: f2261633-201d-46c5-8a66-999e70527a83
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 987
ht-degree: 0%

---

# Web Application Firewall (WAF)

Der auf Fastly basierende WAF-Service (Web Application Firewall) für Adobe Commerce auf Cloud-Infrastrukturen erkennt, protokolliert und blockiert den Traffic bösartiger Anfragen, bevor er Ihre Websites oder Ihr Netzwerk beschädigen kann. Der WAF-Service ist nur für Produktionsumgebungen verfügbar.

Der WAF-Service bietet die folgenden Vorteile:

- **PCI-Compliance** - Die Aktivierung von WAF stellt sicher, dass die Storefronts von Adobe Commerce in Produktionsumgebungen die Sicherheitsanforderungen von PCI DSS 6.6 erfüllen.
- **Standard-WAF-Richtlinie**: Die standardmäßige WAF-Richtlinie, die von Fastly konfiguriert und gepflegt wird, bietet eine Sammlung von Sicherheitsregeln, die darauf zugeschnitten sind, Ihre Adobe Commerce-Web-Anwendungen vor einer Vielzahl von Angriffen zu schützen, einschließlich Injektions-Angriffen, böswilligen Eingaben, Cross-Site-Scripting, Datenexfiltration, HTTP-Protokollverletzungen und anderen [OWASP Top Ten](https://owasp.org/www-project-top-ten/)-Sicherheitsbedrohungen.
- **WAF-Onboarding und -Aktivierung** - Adobe stellt die standardmäßige WAF-Richtlinie innerhalb von 2 bis 3 Wochen nach der endgültigen Bereitstellung in Ihrer Produktionsumgebung bereit und aktiviert sie.
- **Support für Betrieb und Wartung**—
   - Adobe und Schnelle Einrichtung und Verwaltung Ihrer Protokolle, Regeln und Warnhinweise für den WAF-Service.
   - Adobe löst Support-Tickets für Kunden im Zusammenhang mit Problemen mit dem WAF-Service aus, die legitimen Traffic als Probleme der Priorität 1 blockieren.
   - Automatisierte Upgrades der WAF Service-Version gewährleisten eine sofortige Abdeckung neuer oder sich entwickelnder Exploits. Siehe [Wartung und Upgrades für WAF](#waf-maintenance-and-updates).

>[!TIP]
>
>Weitere Informationen zur Aufrechterhaltung der PCI-Compliance für Ihre Adobe Commerce in Cloud-Infrastrukturspeichern finden Sie unter [PCI-Compliance](https://business.adobe.com/products/magento/pci-compliance.html).

## Aktivieren der WAF

Adobe aktiviert den WAF-Service für neue Konten innerhalb von 2 bis 3 Wochen nach der endgültigen Bereitstellung. Die WAF wird über den Fastly CDN-Service implementiert. Sie müssen weder Hardware noch Software installieren oder warten.

>[!NOTE]
>
>Bevor Sie den WAF-Service verwenden können, müssen Sie den gesamten externen Traffic zu Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt über den Fastly-Service leiten. Siehe [Schnelles Setup](fastly-configuration.md).

## Funktionsweise

Der WAF-Service integriert sich mit Fastly und verwendet die Cache-Logik innerhalb des Fastly CDN-Service, um Traffic auf den globalen Fastly-Knoten zu filtern. Wir aktivieren den WAF-Service in Ihrer Produktionsumgebung mit einer standardmäßigen WAF-Richtlinie, die auf [ModSecurity-Regeln von Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) und den OWASP Top 10-Sicherheitsbedrohungen basiert.

Der WAF-Service überprüft HTTP- und HTTPS-Traffic (GET- und POST-Anfragen) anhand des WAF-Regelsatzes und blockiert bösartigen Traffic, der bestimmten Regeln nicht entspricht. Der Service untersucht nur den ursprünglichen Traffic, der versucht, den Cache zu aktualisieren. Daher stoppen wir den Großteil des Angriffsverkehrs im Fastly-Cache und schützen Ihren Ursprungs-Traffic vor bösartigen Angriffen. Durch die Verarbeitung des Ursprungs-Traffics behält der WAF-Service die Cache-Leistung bei und führt für jede nicht zwischengespeicherte Anfrage eine Latenz von schätzungsweise 1,5 Millisekunden bis 20 Millisekunden ein.

## Fehlerbehebung bei blockierten Anfragen

Wenn der WAF-Service aktiviert ist, prüft er den gesamten Web- und Admin-Traffic anhand der WAF-Regeln und blockiert jede Web-Anfrage, die eine Regel Trigger. Wenn eine Anfrage blockiert wird, wird dem Anfragenden eine standardmäßige `403 Forbidden` angezeigt, die eine Referenz-ID für das Blockierungsereignis enthält.

![Fehlerseite für WAF](../../assets/cdn/fastly-waf-403-error.png)

Sie können diese Fehlerantwortseite über den Administrator anpassen. Siehe [Anpassen der WAF-](fastly-custom-response.md#customize-the-waf-error-page).

Wenn Ihre Adobe Commerce-Adminseite oder Storefront als Antwort auf eine rechtmäßige URL-Anfrage eine `403 Forbidden` Fehlerseite zurückgibt, senden Sie ein [Adobe Commerce-Support-Ticket](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case). Kopieren Sie die Referenz-ID aus der Fehlerantwortseite und fügen Sie sie in die Ticketbeschreibung ein.

Informationen zur Identifizierung der WAF-Antwort für eine bestimmte Anfrage mithilfe von New Relic finden Sie unter:

- `Agent_response` - Gibt den WAF-Antwort-Code an (`200` bedeutet gut und `406` bedeutet blockiert)
- `sigsci`-Tags - Kennzeichnet die Anfrage anhand der Art der Anfrage mit einem bestimmten signalwissenschaftlichen Tag

## Wartung und Updates für WAF

Fastly aktualisiert und implementiert Patches für neue CVEs/Vorlagenregeln basierend auf Regelaktualisierungen von kommerziellen Drittanbietern, Fastly Research und Open Sources. Aktualisiert die veröffentlichten Regeln schnell nach Bedarf oder wenn Änderungen an den Regeln aus ihren jeweiligen Quellen verfügbar sind. Außerdem kann Fastly Regeln, die mit den veröffentlichten Regelklassen übereinstimmen, zur WAF-Instanz eines beliebigen Services hinzufügen, nachdem der WAF-Service aktiviert wurde. Diese Updates gewährleisten eine sofortige Abdeckung neuer oder sich entwickelnder Exploits.

Adobe und Fastly verwalten den Aktualisierungsprozess, um sicherzustellen, dass neue oder modifizierte WAF-Regeln effektiv in Ihrer Produktionsumgebung funktionieren, bevor die Aktualisierungen im Blockierungsmodus bereitgestellt werden.

## Probleme

Wenn Sie feststellen, dass der WAF legitime Anfragen blockiert, sind diese häufig falsch-positiv und müssen übersprungen werden, oder es muss eine Problemumgehung über den WAF-Service implementiert werden. Senden Sie ein Support-Ticket und geben Sie die betroffene URL, die genauen Schritte zur Reproduktion des Fehlers und die Fehlerreferenz im Textformular (im Gegensatz zu einem Screenshot) an, um Transkriptionsfehler zu vermeiden.

## Einschränkungen

Der standardmäßige WAF-Service, der von Fastly unterstützt wird, unterstützt die folgenden Funktionen nicht:

- Schutz vor Malware oder Bot-Schutz - Erwägen Sie [&#x200B; Verwendung von &#x200B;](./fastly-vcl-allowlist.md) oder Drittanbieterdiensten.
- Ratenbegrenzung - Siehe [Ratenbegrenzung](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) in der Fastly-Dokumentation oder [Ratenbegrenzung](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) im Sicherheitsabschnitt _Commerce Web API_.
- Konfigurieren eines Protokollierungsendpunkts für den Kunden - Siehe [PrivateLink-](../development/privatelink-service.md)) als Alternative.

Mit dem WAF-Service können Sie Traffic basierend auf IP-Adressen blockieren oder zulassen. Sie können Ihrem Fastly-Service Zugriffssteuerungslisten (ACL) und benutzerdefinierte VCL-Snippets hinzufügen, um die IP-Adressen und die VCL-Logik zum Blockieren oder Zulassen von Traffic anzugeben. Siehe [Benutzerdefinierte Fastly-VCL-Snippets](fastly-vcl-custom-snippets.md).

Das Filtern nach TCP-, UDP- oder ICMP-Anfragen wird vom WAF-Service nicht unterstützt. Diese Funktionalität wird jedoch durch den integrierten DDoS-Schutz bereitgestellt, der im Fastly CDN-Service enthalten ist. Siehe [DoS-Schutz](fastly.md#ddos-protection).

