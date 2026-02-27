---
title: Checkliste starten
description: Überprüfen Sie die Checklisten-Elemente für den Site-Launch.
exl-id: efc97d4a-a9f3-49fa-b977-061282765e90
source-git-commit: ca2d94364787695398b2b8af559733fe52ec2949
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# Checkliste starten

Laden Sie vor der Bereitstellung in der Produktionsumgebung die [Launch-Checkliste](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) herunter und verwenden Sie sie mit diesen Anweisungen, um zu bestätigen, dass Sie alle erforderlichen Konfigurationen und Tests abgeschlossen haben. Eine Übersicht über den gesamten Bereitstellungsprozess für Starter und Pro finden Sie unter [Bereitstellen Ihres Stores](../deploy/staging-production.md).

## Vollständiger Test in der Produktion

Unter [Testen &#x200B;](../test/staging-and-production.md) Bereitstellung“ können Sie alle Aspekte Ihrer Sites, Stores und Umgebungen testen. Zu diesen Tests gehören die schnelle Verifizierung, Benutzerakzeptanztests (UAT) und Leistungstests.

## TLS und Fastly

Adobe stellt für jede Umgebung ein SSL-/TLS-Zertifikat zum Verschlüsseln bereit. Dieses Zertifikat ist für Fastly erforderlich, um sicheren Traffic über HTTPS bereitzustellen.

Um dieses Zertifikat verwenden zu können, müssen Sie Ihre DNS-Konfiguration aktualisieren, damit Adobe die Domain-Validierung abschließen und das Zertifikat auf Ihre Umgebung anwenden kann. Jede Umgebung verfügt über ein eindeutiges Zertifikat, das die Domains für die Adobe Commerce auf Cloud-Infrastruktur-Sites abdeckt, die in dieser Umgebung bereitgestellt werden. Es wird empfohlen, die Konfigurationsaktualisierungen während des [Schnell einrichten“-Prozesses abzuschließen und &#x200B;](../cdn/fastly-configuration.md).

## DNS-Konfiguration mit Produktionseinstellungen aktualisieren

Wenn Sie bereit sind, Ihre Site zu starten, müssen Sie die DNS-Konfiguration aktualisieren, um den Traffic von Ihrer Produktionsumgebung durch den Fastly-Service zu leiten.

**Voraussetzungen:**

- [Schnelles Einrichten und Testen in Ihrer Entwicklungsumgebung](../cdn/fastly-configuration.md#)

- Die Konfiguration der Produktionsumgebung wurde mit allen erforderlichen Domains aktualisiert

  In der Regel fügen Sie gemeinsam mit Ihrem technischen Kundenberater alle Domains und Subdomains der obersten Ebene hinzu, die für Ihre Stores erforderlich sind. Um die Domains für Ihre Produktionsumgebung hinzuzufügen oder zu ändern, [&#x200B; Sie ein Adobe Commerce-Support-Ticket &#x200B;](https://support.magento.com/hc/en-us/articles/360019088251). Warten Sie auf die Bestätigung, dass Ihre Projektkonfiguration aktualisiert wurde.

  Bei Ausgangsprojekten müssen Sie die Domains zu Ihrem Projekt hinzufügen. Siehe [Verwalten von Domains](../cdn/fastly-custom-cache-configuration.md#manage-domains).

- Für Ihre Produktionsumgebungen bereitgestelltes SSL-/TLS-Zertifikat

  Wenn Sie die ACME-Challenge-Datensätze für Ihre Produktions-Domains während des Fastly-Einrichtungsprozesses hinzugefügt haben, lädt Adobe das SSL-/TLS-Zertifikat automatisch in Ihre Produktionsumgebung hoch, wenn Sie die DNS-Konfiguration aktualisieren, um Traffic an den Fastly-Service zu leiten. Wenn Sie das Zertifikat nicht vorab bereitgestellt haben oder Ihre Domains aktualisiert haben, muss Adobe die Domain-Validierung abschließen und das Zertifikat bereitstellen. Dies kann bis zu 12 Stunden dauern.

### So aktualisieren Sie die DNS-Konfiguration für den Site-Launch:

1. Aktualisieren Sie die folgende DNS-Konfiguration für Ihre Produktions-Site:

   - Legen Sie alle erforderlichen Umleitungen fest, insbesondere wenn Sie von einer vorhandenen Website migrieren.

   - Den Root-Ressourceneintrag der Zone so einstellen, dass er den Hostnamen adressiert

   - Verringern Sie den Wert für die Time-to-Live (TTL), um DNS-Informationen zu aktualisieren und Kunden auf den richtigen Produktionsspeicher zu verweisen

     Wir empfehlen beim Wechsel des DNS-Eintrags einen deutlich niedrigeren TTL-Wert. Dieser Wert teilt dem DNS mit, wie lange der DNS-Eintrag zwischengespeichert werden soll. Durch die Kürzung wird das DNS schneller aktualisiert. Sie können beispielsweise den TTL-Wert von drei Tagen auf 10 Minuten ändern, wenn Sie Ihre Site aktualisieren. Beachten Sie, dass die Verkürzung des TTL-Werts die DNS-Infrastruktur zusätzlich belastet. Stellen Sie den vorherigen, höheren Wert nach dem Start der Site wieder her.


1. Fügen Sie CNAME-Einträge hinzu, um die Subdomains für Ihre Produktionsumgebung auf den Fastly-Service `prod.magentocloud.map.fastly.net` verweisen, z. B.:

   | Domain oder Subdomain | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. Fügen Sie bei Bedarf A- und AAAA-Einträge hinzu, um die Apex-Domain (`<domain-name>.com`) den folgenden Fastly-IP-Adressen zuzuordnen:

   | Apex-Domain | NAME | AAAANAME |
   | --------------- | ----------------- | -------- |
   | `<domain-name>.com` | `151.101.1.124` | 2a04:4e42:200::380 |
   | `<domain-name>.com` | `151.101.65.124` | 2a04:4e42:400::380 |
   | `<domain-name>.com` | `151.101.129.124` | 2a04:4e42:600::380 |
   | `<domain-name>.com` | `151.101.193.124` | 2a04:4e42::380 |

>[!IMPORTANT]
>
>Die DNS-Anweisungen in [RFC1034](https://www.rfc-editor.org/rfc/rfc1912) (**Abschnitt 2.4**) geben Folgendes an:
>_Ein CNAME-Eintrag darf nicht gleichzeitig mit anderen Daten vorhanden sein. Mit anderen Worten: Wenn suzy.podunk.xx ein Alias für sue.podunk.xx ist, können Sie nicht auch einen MX-Eintrag für suzy.podunk.edu, einen A-Eintrag oder sogar einen TXT-Eintrag haben._
>
>Aus diesem Grund sollten DNS-Einträge für Subdomains vom Typ `CNAME` und für Apex-Domains (Root-Domains) vom Typ `A` sein. Wenn Sie diese Regel verwerfen, kann es zu Störungen des E-Mail-Service oder der DNS-Verbreitung kommen, da Sie die Möglichkeit verlieren, andere Datensätze wie MX oder NS hinzuzufügen. Einige DNS-Anbieter umgehen dies möglicherweise, indem sie interne Anpassungen verwenden, aber die Befolgung des Standards gewährleistet Stabilität und Flexibilität (z. B. den Wechsel des DNS-Anbieters).

1. Aktualisieren Sie die Basis-URL.

   - Verwenden Sie SSH, um sich bei der Produktionsumgebung anzumelden.

     ```bash
     magento-cloud ssh -e production
     ```

   - Verwenden Sie die CLI, um die Basis-URL für Ihren Store zu ändern.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **HINWEIS**: Sie können die Basis-URL auch über den Administrator aktualisieren. Siehe [Store-URLs](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=de) im _Handbuch zu Adobe Commerce Stores und Kauferlebnissen_.

1. Warten Sie einige Minuten, bis die Site aktualisiert wird.

1. Testen Sie Ihre Site.

## Produktionskonfiguration überprüfen

Führen Sie einen endgültigen Durchlauf durch, um die Produktionskonfiguration für einen oder mehrere Stores zu validieren. Sie können die Konfiguration in der Produktionsumgebung aktualisieren. Wenn die Einstellungen schreibgeschützt sind, müssen Sie möglicherweise eine SSH-Verbindung öffnen und CLI-Befehle verwenden, um die Konfiguration zu ändern oder Konfigurationsänderungen in Ihrer lokalen Umgebung vorzunehmen. Nach Abschluss der Aktualisierungen können Sie die Änderungen in Staging- und Produktionsumgebungen bereitstellen.

Im Folgenden werden Änderungen und Prüfungen empfohlen:

- [Ausgehende E-Mail-Tests abgeschlossen](../project/outgoing-emails.md)

- [Sichere Konfiguration für Admin-Anmeldedaten und Basis-Admin-URL](https://experienceleague.adobe.com/de/docs/commerce-admin/systems/security/security-admin)

- [Alle Bilder für das Web optimieren](../cdn/fastly-image-optimization.md)

- [Überprüfen der Minimierungseinstellungen für HTML, JavaScript und CSS](../deploy/static-content.md)

## Schnelles Caching überprüfen

- Testen und überprüfen Sie, ob das Fastly-Caching auf der Produktions-Site ordnungsgemäß funktioniert. Detaillierte Tests und Prüfungen finden Sie unter [Fastly Testing](../test/staging-and-production.md#check-fastly-caching).

- [Stellen Sie sicher, dass die neueste Version des Fastly CDN-Moduls für Commerce in Ihrer Produktionsumgebung installiert ist](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Stellen Sie sicher, dass die aktuelle Version des Fastly-VCL-Codes hochgeladen wurde](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## Leistungstests

Wir empfehlen Ihnen, die [Leistungs-Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)-Optionen im Rahmen des Bereitschaftsprozesses vor der Markteinführung zu überprüfen.

Sie können auch mit den folgenden Drittanbieteroptionen testen:

- [Siege](https://www.joedog.org/siege-home/): Traffic-Formungs- und Test-Software, die Ihren Store an die Grenze bringt. Treten Sie mit einer konfigurierbaren Anzahl simulierter Clients auf Ihre Website. Siege unterstützt einfache Authentifizierung, Cookies, HTTP-, HTTPS- und FTP-Protokolle.

- [Jmeter](https://jmeter.apache.org/): Ausgezeichnete Belastungstests, um die Leistung bei Traffic-Spitzen wie bei Flash-Verkäufen zu messen. Erstellen Sie benutzerdefinierte Tests, die für Ihre Site ausgeführt werden.

- [New Relic](https://support.newrelic.com/s/) (bereitgestellt): Hilft beim Auffinden von Prozessen und Bereichen der Site, was zu einer langsamen Leistung führt, wobei die pro Aktion aufgezeichnete Zeit, wie die Übertragung von Daten, Abfragen, Redis und mehr, genutzt wird.

- [WebPageTest](https://www.webpagetest.org/) und [Pingdom](https://www.pingdom.com/): Echtzeit-Analyse der Ladezeit Ihrer Site-Seiten mit verschiedenen Ursprungsorten. Pingdom kostet eine Gebühr. WebPageTest ist ein kostenloses Tool.

## Sicherheitskonfiguration

- [Einrichten der Sicherheitsprüfung](overview.md#set-up-the-security-scan-tool)

- [Sichere Konfiguration für Admin-Benutzer](https://experienceleague.adobe.com/de/docs/commerce-admin/systems/security/security-admin)

- [Sichere Konfiguration für Admin-URL](https://experienceleague.adobe.com/de/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Entfernen Sie alle Benutzer, die nicht mehr im Adobe Commerce on Cloud-Infrastrukturprojekt vorhanden sind.](../project/user-access.md)

- [Konfigurieren der Zwei-Faktor-Authentifizierung](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## Leistungsüberwachung

Sie können New Relic-Services zur Leistungsüberwachung in Pro- und Starter-Umgebungen verwenden. Bei Pro-Plan-Konten stellen wir die Warnhinweisrichtlinie Managed Warnhinweise für Adobe Commerce zur Verfügung, um die Anwendungs- und Infrastrukturleistung mithilfe von New Relic APM- und Infrastrukturagenten zu überwachen. Weitere Informationen zur Verwendung dieser Dienste finden Sie unter [Überwachen der Leistung mit verwalteten Warnhinweisen](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

### Nächster Schritt

[Launch-Schritte](steps.md)
