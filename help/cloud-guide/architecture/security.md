---
title: Sicherheit der Cloud-Infrastruktur
description: Erfahren Sie, wie Adobe Adobe Commerce in der Cloud-Infrastruktur sicher hält.
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# Sicherheit

Die Adobe Commerce [Pro](pro-architecture.md)Planarchitektur wurde für eine hochsichere Umgebung entwickelt. Jeder Kunde wird in seiner eigenen isolierten Serverumgebung bereitgestellt, getrennt von anderen Kunden. Nachfolgend werden die Sicherheitsdetails der Produktionsumgebung beschrieben.

## Webbrowser

Der Großteil des Traffics, der in die und aus der Cloud-Umgebung fließt, stammt von den Webbrowsern der Verbraucher. Verbraucher-Traffic kann mit HTTPS für alle Seiten auf der Website gesichert werden (entweder mit einer gemeinsamen SSL-Zertifizierung oder dem eigenen SSL-Zertifikat des Kunden gegen eine zusätzliche Gebühr). Checkout- und Kontoseiten werden immer mit HTTPS bereitgestellt. Es empfiehlt sich, alle Seiten unter HTTPS bereitzustellen.

## Content Delivery Network

Fastly bietet ein Content Delivery Network (CDN) und verteilten Denial-of-Service (DDoS)-Schutz. Das Fastly CDN hilft dabei, den direkten Zugriff auf die ursprünglichen Server zu isolieren. Das öffentliche DNS verweist nur auf das Fastly-Netzwerk. Die Fastly DDoS-Lösung schützt vor hochgradig störenden Layer 3- und Layer 4-Angriffen und komplexeren Layer 7-Angriffen. Layer-7-Angriffe können mit benutzerdefinierten Regeln blockiert werden, die auf den gesamten HTTP/HTTPS-Anforderungen und auf Client- und Anfragekriterien basieren, einschließlich Kopfzeilen, Cookies, Anfragepfad und Client-IP oder Indikatoren wie Geolokalisierung.

Siehe [Fastly Services - Übersicht](../cdn/fastly.md).

## Web Application Firewall

Die Fastly Web Application Firewall (WAF) wird verwendet, um zusätzlichen Schutz zu bieten. Die Cloud-basierte WAF von Fastly nutzt Drittanbieterregeln aus kommerziellen und Open-Source-Quellen wie dem OWASP Core RuleSet. Darüber hinaus werden Adobe Commerce-spezifische Regeln verwendet. Kunden werden vor wichtigen Angriffen auf Anwendungsebene geschützt, einschließlich Injection-Angriffen und böswilligen Eingaben, Cross-Site-Scripting, Datenexfiltration, HTTP-Protokollverletzungen und anderen OWASP Top 10-Bedrohungen.

Die WAF-Regeln werden von Adobe Commerce aktualisiert, falls neue Sicherheitslücken entdeckt werden, die es Managed Services ermöglichen, Sicherheitsprobleme vor Software-Patches virtuell zu beheben. Fastly WAF bietet keine Dienste zur Ratenbegrenzung oder Bot-Erkennung an. Bei Bedarf können Kunden einen mit Fastly kompatiblen Bot-Erkennungs-Service eines Drittanbieters lizenzieren.

Siehe [Web Application Firewall (WAF)](../cdn/fastly-waf-service.md).

## Virtuelle private Cloud

Die Produktionsumgebung des Adobe Commerce Pro-Plans ist als Virtual Private Cloud (VPC) konfiguriert, sodass die Produktionsserver isoliert sind und nur über eingeschränkte Möglichkeiten verfügen, eine Verbindung zur und aus der Cloud-Umgebung herzustellen. Nur sichere Verbindungen zu den Cloud-Servern sind zulässig. Sichere Protokolle wie SFTP oder rsync können für Dateiübertragungen verwendet werden.

Kunden können SSH-Tunnel verwenden, um die Kommunikation mit der Anwendung zu sichern. Der Zugang zu AWS PrivateLink kann gegen eine zusätzliche Gebühr gewährt werden. Alle Verbindungen zu diesen Servern werden mithilfe von AWS Security Groups gesteuert, einer virtuellen Firewall, die die Verbindungen zur Umgebung einschränkt. Die technischen Ressourcen der Kunden können mithilfe von SSH auf diese Server zugreifen.

## Verschlüsselung

Amazon Elastic Block Store (EBS) wird für die Speicherung verwendet. Alle EBS-Volumes werden mit dem AES-256-Algorithmus verschlüsselt, was bedeutet, dass die Daten im Ruhezustand verschlüsselt werden. Das System verschlüsselt auch Daten während der Übertragung zwischen dem CDN und dem Ursprungs-Server sowie zwischen den Ursprungs-Servern. Kundenkennwörter werden als Hashes gespeichert. Sensible Anmeldeinformationen, einschließlich Payment Gateway-Anmeldeinformationen, werden mit dem SHA-256-Algorithmus verschlüsselt.

Die Adobe Commerce-Anwendung unterstützt keine Verschlüsselung oder Verschlüsselung auf Spalten- oder Zeilenebene, wenn sich die Daten nicht im Ruhezustand befinden oder nicht zwischen Servern übertragen werden. Der Kunde kann Verschlüsselungsschlüssel von der Anwendung aus verwalten. Vom System verwendete Schlüssel werden im AWS-Schlüsselverwaltungssystem gespeichert und müssen von Managed Services verwaltet werden, um Teile des Dienstes bereitzustellen.

## Endpunkterkennung und -reaktion

[!DNL CrowdStrike Falcon] wird ein leichtgewichtiger Endpoint Detection and Response (EDR)-Agent der nächsten Generation auf allen Endpunkten (einschließlich Servern) innerhalb von Adobe installiert. EDR-Agenten schützen Adobe-Daten und -Systeme durch kontinuierliche Überwachung und Erfassung in Echtzeit, was eine schnelle Identifizierung und Reaktion bei Bedrohungen ermöglicht.

## Penetrationstest

Managed Services führt mit dem vorkonfigurierten Programm regelmäßige Penetrationstests des Adobe Commerce-Cloud-Systems durch. Kunden sind für alle Penetrationstests ihrer benutzerdefinierten Anwendung verantwortlich.

## Zahlungs-Gateway

Adobe Commerce erfordert Integrationen mit Zahlungs-Gateways, bei denen Kreditkartendaten direkt vom Browser des Verbrauchers an das Zahlungs-Gateway übergeben werden. Die Kartendaten sind in keiner der Adobe Commerce Pro Plan Managed Services-Umgebungen verfügbar. Die Transaktionen, die von der eCommerce-Anwendung ausgeführt werden, werden mit einem Verweis auf die Transaktion vom Gateway abgeschlossen.

## Adobe Commerce-Anwendung

Adobe testet den Kern-Anwendungs-Code regelmäßig auf Sicherheitslücken. Patches für Defekte und Sicherheitsprobleme werden Kunden bereitgestellt. Das Produktsicherheits-Team validiert Adobe Commerce-Produkte gemäß den Sicherheitsrichtlinien für OWASP-Anwendungen. Zur Prüfung und Verifizierung der Einhaltung werden verschiedene Tools zur Bewertung von Sicherheitslücken und externe Anbieter eingesetzt. Zu den Sicherheits-Tools gehören:

- Statisches und dynamisches Scannen mit Veracode
- RIPS-Quellcodescan
- Sicherheitslücken-Scan-Services von TrustWave und Alert Logic
- Burp Suite Pro
- OWASPZAP
- undSqlMap

Die vollständige Code-Basis wird mit diesen Tools alle zwei Wochen gescannt. Kunden werden über Sicherheits-Patches per E-Mail, Benachrichtigungen in der Anwendung und im [Sicherheitscenter](https://helpx.adobe.com/de/security.html) benachrichtigt.

Kunden müssen sicherstellen, dass diese Patches innerhalb von 30 Tagen nach der Veröffentlichung gemäß den PCI-Richtlinien auf ihre benutzerdefinierte Anwendung angewendet werden. Adobe bietet außerdem ein [Security Scan Tool](https://experienceleague.adobe.com/de/docs/commerce-admin/systems/security/security-scan) mit dem Händler ihre Websites regelmäßig überwachen und Updates zu bekannten Sicherheitsrisiken, Malware und unbefugtem Zugriff erhalten können. Das Security Scan Tool ist ein kostenloser Dienst, der auf jeder beliebigen Adobe Commerce-Version ausgeführt werden kann.

Um Sicherheitsforscher dazu zu ermutigen, Sicherheitslücken zu identifizieren und zu melden, [&#x200B; Adobe Commerce zusätzlich zu internen Tests ein &#x200B;](https://hackerone.com/magento)Bug-Bounty-Programm“. Des Weiteren wird dem Kunden der vollständige Quellcode des Programms zur eigenen Überprüfung, falls gewünscht, bereitgestellt.

## Schreibgeschütztes Dateisystem

Der gesamte ausführbare Code wird in einem schreibgeschützten Dateisystemabbild bereitgestellt, wodurch die für Angriffe verfügbaren Oberflächen erheblich reduziert werden. Der Bereitstellungsprozess erstellt ein Squash-FS-Image, um die Möglichkeiten für das Einfügen von PHP- oder JavaScript-Code in das System oder das Ändern von Adobe Commerce-Anwendungsdateien zu reduzieren.

## Remote-Bereitstellung

Die einzige Möglichkeit, ausführbaren Code in die Managed Services-Produktionsumgebung zu bringen, besteht darin, ihn durch einen Bereitstellungsprozess auszuführen. Bei der Bereitstellung wird Quell-Code aus Ihrem Quell-Repository in ein Remote-Repository gepusht, das einen Bereitstellungsprozess initiiert. Der Zugriff auf dieses Bereitstellungsziel wird gesteuert, sodass Sie die vollständige Kontrolle darüber haben, wer auf das Bereitstellungsziel zugreifen kann. Alle Bereitstellungen von Anwendungs-Code in der Nicht-Produktionsumgebung werden vom Kunden gesteuert.

## Protokollierung

Alle AWS-Aktivitäten werden in AWS CloudTrail protokolliert. Betriebssystem-, Anwendungs- und Datenbankprotokolle werden auf den Produktions-Servern gespeichert und in Backups gespeichert. Alle Quellcodeänderungen werden in einem Git-Repository aufgezeichnet. Der Bereitstellungsverlauf ist in der Adobe Commerce [Project Web Interface) &#x200B;](../project/overview.md). Alle Support-Zugriffe werden protokolliert und Support-Sitzungen aufgezeichnet.

Siehe [Protokolle anzeigen und verwalten](../test/log-locations.md).

## Sensible Daten

Sensible Daten können entweder personenbezogene Daten von Verbrauchern oder vertrauliche Daten von Managed Services-Kunden umfassen. Der Schutz sensibler Kunden- und Verbraucherdaten ist für Adobe Commerce Managed Services eine wichtige Aufgabe. Sowohl Managed Services- als auch Adobe-Kunden haben rechtliche Verpflichtungen in Bezug auf persönlich identifizierbare Informationen. Zusätzlich zu den Sicherheitsfunktionen der Architektur gibt es weitere Steuerelemente, um die Verteilung und den Zugriff auf vertrauliche Daten einzuschränken.

Kunden besitzen eigene Daten und haben die Kontrolle darüber, wo sich diese Daten befinden. Der Kunde gibt den Speicherort an, an dem sich seine Produktions- und Entwicklungsinstanzen befinden. Sie geben auch an, welcher Speicherort für die Adobe Commerce-Reporting-Umgebung mit Commerce verwendet wird und ob diese Adobe Commerce-Reporting-Anwendung Zugriff auf persönlich identifizierbare Informationen hat oder nicht. Produktionsinstanzen finden sich in den meisten AWS-Regionen, während Entwicklungs- und Adobe Commerce-Reporting-Umgebungen derzeit entweder in den Vereinigten Staaten oder in der Europäischen Union zu finden sind.

Sensible Daten können das Fastly CDN-Server-Netzwerk durchlaufen, werden jedoch nicht im Fastly-Netzwerk gespeichert. Alle am Managed Services-Angebot beteiligten Partner haben vertragliche Verpflichtungen, den Schutz sensibler Daten sicherzustellen. Managed Services verschiebt keine sensiblen Kunden- oder Verbraucherdaten von den vom Kunden angegebenen Speicherorten.

Im Rahmen der Bereitstellung der im Managed Services-Angebot enthaltenen Services benötigen Managed Services-Mitarbeiter Zugriff auf Produktionssysteme, die sensible Daten enthalten. Diese Mitarbeiter werden in ihren Pflichten zum Schutz sensibler Kunden- und Verbraucherdaten geschult. Der Zugriff auf diese Systeme ist auf Mitarbeiter beschränkt, die Zugriff benötigen. Diese Mitarbeiter haben nur für die Zeit Zugriff, die für die Bereitstellung dieser Dienste benötigt wird.

## Datenschutz-Grundverordnung

Die Datenschutz-Grundverordnung (DSGVO) ist ein Rechtsrahmen, der Richtlinien für die Erhebung und Verarbeitung personenbezogener Daten für Einzelpersonen in der Europäischen Union (EU) festlegt. Diese Vorschriften gelten unabhängig davon, wo sich die Website befindet, und EU-Besucher haben Zugriff darauf.

Die Besucher müssen über die Daten informiert werden, die eine Website von ihnen erfasst, und müssen der Informationserfassung ausdrücklich zustimmen. Websites müssen Besucher benachrichtigen, wenn personenbezogene Daten der Website verletzt werden.

Der Händler oder das Unternehmen, der bzw. das die Website betreibt, muss über einen dedizierten Datenschutzbeauftragten verfügen, der die Datensicherheit der Website überwacht. Der Datenschutzbeauftragte (oder das Website-Management-Team) sollte kontaktiert werden können, wenn ein Besucher die Löschung seiner Daten verlangt.

Die DSGVO fordert, dass alle personenbezogenen Daten (wie Namen, Rasse und Geburtsdatum) entweder anonymisiert oder pseudonymisiert erfasst werden.

>[!NOTE]
>
>Diese Seite bietet einen allgemeinen Überblick darüber, was im Rahmen der DSGVO zu beachten ist. Einzelheiten zur Speicherung _[personenbezogenen Daten durch Adobe Commerce finden Sie &#x200B;](https://experienceleague.adobe.com/de/docs/commerce-operations/security-and-compliance/overview)_ „Sicherheits- und Compliance-Handbuch“. Um zu bestimmen, wie Ihr Unternehmen rechtliche Verpflichtungen einhalten sollte, wenden Sie sich an Ihren Rechtsbeistand oder lesen Sie den [offiziellen Text](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

## Backups

Backups werden stündlich für die letzten 24 Betriebsstunden durchgeführt. Nach dem 24-Stunden-Zeitraum werden Backups mithilfe des AWS EBS Snapshot-Service planmäßig aufbewahrt. Siehe [Aufbewahrungsrichtlinie](pro-architecture.md#retention-policy).

Der Service erstellt ein unabhängiges Backup auf redundantem Speicher. Da die EBS-Volumes verschlüsselt sind, werden auch die Backups verschlüsselt. Außerdem führt Managed Services bedarfsgesteuerte Backups auf Kundenwunsch durch.

Ihr Managed Services-Sicherungs- und Wiederherstellungsansatz verwendet eine Hochverfügbarkeitsarchitektur in Kombination mit vollständigen Systemsicherungen. Jedes Projekt wird - alle Daten, Code und Assets - über drei separate AWS-Verfügbarkeitszonen hinweg repliziert, wobei jede Zone über ein separates Rechenzentrum verfügt.

Siehe [Snapshots und Backup-Verwaltung](../storage/snapshots.md).
