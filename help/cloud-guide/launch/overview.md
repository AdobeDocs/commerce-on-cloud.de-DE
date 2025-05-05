---
title: Site-Launch
description: Erfahren Sie, wie Sie mit der Vorbereitung des Site-Launches beginnen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---

# Site-Launch

Nach Abschluss der Bereitstellung und der Tests in Integrations- und Staging-Umgebungen können Sie mit der Vorbereitung des Site-Starts beginnen. Zunächst sollten alle Entwicklungs- und Testaufgaben abgeschlossen sein, bevor Sie in der Produktionsumgebung arbeiten. Sind Sie bereit zu starten? Überprüfen Sie Checklisten, Best Practices und letzte Schritte, um Ihre Site zu starten.

Wenn Sie diese Informationen vor der Bereitstellung und dem Testen in der Staging-Umgebung geprüft haben, sollten Sie im nächsten Abschnitt die Vorteile von Tests in der Staging-Umgebung zuerst überprüfen. Staging ist eine produktionsnahe Umgebung, die auf ähnlicher Hardware, Konfigurationen, Architektur und Services ausgeführt wird. Dadurch können Sie Ausfallzeiten reduzieren und Ihre Erweiterungs-, Service-, benutzerdefinierten Konfigurationen- und Händlerakzeptanztests zu wichtigen Komponenten für die Freigabe Ihrer Sites und Stores machen.

## Warum sollten Sie die Integration, das Staging und die Produktion vollständig testen?

Wir empfehlen dringend Tests in der Integrations-, Staging- und Produktionsumgebung, da Sie mit Sicherheit sicherstellen müssen, dass Ihr benutzerdefinierter Code, Designs, Erweiterungen und Drittanbieterintegrationen zusammenarbeiten, um Ihre Stores zu betreiben. Die folgenden häufigen Probleme können Sie identifizieren und beheben, wenn Sie das Testen in der Integrations- und Staging-Umgebung abschließen, bevor Sie Ihre Produktionsumgebung aktualisieren:

- Staging unterstützt alle Produktions-Services, Funktionen, Datenbankdaten, Technologie-Stack, Architektur und mehr. Es spiegelt die Produktion wider, d. h. wenn beim Staging Fehler auftreten, erhalten Sie eine Warnung, bevor sie in der Produktion auftreten.

- Code, der in Ihrer lokalen Integrationsumgebung funktioniert, funktioniert möglicherweise nicht in Staging- und Produktionsumgebungen.

- Integrationsumgebungen unterstützen nicht alle Services, die in der Staging- und Produktionsumgebung verfügbar sind, z. B. Fastly und New Relic.

- [Testen Sie ](../test/guidance.md) Site vollständig mit verschiedenen Tools in der Staging-Umgebung auf Auslastung, Belastung, Leistung und Site-Assets.

- Da in Integrationsumgebungen möglicherweise nur Datenbanken mit Testdaten gefüllt sind, die nicht mit einer produktionsähnlichen Umgebung übereinstimmen, können beim Testen in Staging- oder Produktionsumgebungen zusätzliche Fehler oder unerwartetes Verhalten auftreten.

## Voraussetzungen für den Site-Launch

Sie benötigen die folgenden Informationen und Ressourcen, um sich auf den Site-Launch vorzubereiten:

- CNAME-Eintragsinformationen zum Konfigurieren des DNS

- Liste aller Storefront-Domains, die dem Zertifikat hinzugefügt werden sollen

- SSL-/TLS-Zertifikat

Im Rahmen des Adobe Commerce on Cloud Infrastructure-Abonnements stellt Adobe ein Domain-validiertes SSL-/TLS-Zertifikat bereit, das von Let&#39;s Encrypt ausgestellt wird. Jede Pro Production, Staging- und Starter Production (`master`)-Umgebung verfügt über ein eindeutiges Zertifikat, das alle Domains und Subdomains in dieser Umgebung abdeckt. Diese Zertifikate werden automatisch bereitgestellt und auf Ihre Site hochgeladen, nachdem Sie Ihre DNS-Konfiguration für die Entwicklung und Produktion aktualisiert haben. Siehe [Bereitstellen von SSL-/TLS-](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Wenn Sie Ihr eigenes SSL-Zertifikat mit erweiterter Validierung für Ihr Unternehmen bereitstellen möchten, anstatt das Zertifikat „Let&#39;s Encrypt“ zu verwenden, wenden Sie sich an Ihren CTA oder [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket).

## Einrichten des Sicherheits-Scan-Tools

>[!NOTE]
>
>Das Tool zur Sicherheitsprüfung verwendet die folgenden öffentlichen IP-Adressen:
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Fügen Sie diese IP-Adressen zu einer Zulassungsliste in Ihren Netzwerk-Firewall-Regeln hinzu, damit das Tool Ihre Site überprüfen kann. Das Tool sendet Anfragen nur an die Ports 80 und 443.

Mit dem Sicherheits-Scan-Tool können Sie Ihre Store-Websites regelmäßig überwachen und Updates für bekannte Sicherheitsrisiken, Malware und veraltete Software erhalten. Dieses Tool ist ein kostenloser Service, der für alle Implementierungen und Versionen von Adobe Commerce in der Cloud-Infrastruktur verfügbar ist. Sie greifen über Ihr [Commerce Marketplace-Konto auf das Tool zu](https://account.magento.com/customer/account/login).

- Überwachen des Sicherheitsstatus Ihrer Sites und angewendete Sicherheitsaktualisierungen

- Empfangen von Sicherheitsaktualisierungen und Site-spezifischen Benachrichtigungen

Informationen [ Einrichten und Verwenden des Sicherheits](https://experienceleague.adobe.com/de/docs/commerce-admin/systems/security/security-scan)Scan-Tools finden Sie im Benutzerhandbuch. Normalerweise verwenden Sie dieses Tool, wenn Sie mit dem Benutzerakzeptanztest (UAT) beginnen.

Jede Site, die Sie durchsuchen, muss über die Registerkarte Sicherheitsprüfung registriert werden. Während des Registrierungsprozesses müssen Sie den Haftungsausschluss akzeptieren, bevor Sie mit dem Scannen beginnen können. Sie steuern sowohl den Zeitplan als auch die Autorisierung des Benutzers, nach Abschluss jeder Überprüfung Benachrichtigungen zu erhalten. Sie können die Suche für ein bestimmtes, wiederkehrendes Datum und eine bestimmte Uhrzeit planen oder bei Bedarf eine Suche ausführen.

Das Security Scan Tool verwendet mehrere Benutzeragenten-Zeichenfolgen, um Malware-Aktivitäten in Echtzeit zu simulieren. Möglicherweise werden die folgenden Benutzeragenten in Ihren Analytics- oder Zugriffsprotokollen angezeigt:

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Site scannen

1. Zugriff auf Ihr [Commerce Marketplace-Konto](https://account.magento.com/customer/account/login).

1. Klicken Sie auf die Registerkarte Sicherheitsprüfung und wählen Sie **Zu Sicherheitsprüfung wechseln**.

1. Wählen Sie in der _Aktionen_ für die Website **Suche ausführen** aus. Ein Benachrichtigungsstatus zeigt die zeitgesteuerte Suche an.

### So überprüfen Sie den Bericht:

1. Wenn der Bericht abgeschlossen ist, wird eine Benachrichtigung angezeigt.

1. Wählen Sie in der Zeile Site den Bericht, den Sie anzeigen möchten, aus der Spalte **Berichte** aus. Die Reihenfolge ist von der neuesten zur ältesten.

Der Bericht listet Probleme auf, einschließlich fehlgeschlagener Scans, nicht identifizierter Ergebnisse und erfolgreicher Scans. Jeder Eintrag enthält detaillierte Informationen für die Überprüfung, eine Liste der zu untersuchenden Probleme und zu ergreifende Maßnahmen. Einige dieser Aktionen erfordern möglicherweise das Herunterladen und Installieren von Sicherheits-Patches. Fügen Sie alle erforderlichen Patches zu einer Entwicklungsverzweigung auf Ihrer lokalen Workstation hinzu, bevor Sie sie zur Produktionsverzweigung hinzufügen.

Die Scan-Ergebnisse enthalten eine Bezeichnung, die den Status „Scan bestanden“ oder „Fehlgeschlagen“ mit detaillierten Informationen zu den durchgeführten Prüfungen beschreibt:

- „Fehlgeschlagen“ zeigt an, dass die Website eine schwerwiegende Sicherheitslücke enthält.

- „Nicht identifiziert“ bedeutet, dass Ihr Team oder Ihr Hosting-Anbieter eine eingehendere Überprüfung benötigen, um festzustellen, ob weitere Maßnahmen erforderlich sind.

Die Suchergebnisse enthalten auch empfohlene Schritte zur Behebung jedes fehlgeschlagenen Sicherheitstests. Ergebnisse der Sicherheitsüberprüfung sind geschützt und können nur von registrierten Benutzern angezeigt werden. Nur Benutzer, die im Site-Registrierungsprozess angegeben sind, erhalten Benachrichtigungen über den Abschluss der Überprüfung.

## Bereit zum Starten Ihrer Site

Wenn Sie bereit sind, den Site-Launch-Prozess zu starten, lesen Sie Folgendes:

- [Checkliste starten](checklist.md)

- [Launch-Schritte](steps.md)
