---
title: SendGrid-E-Mail-Service
description: Erfahren Sie mehr über den SendGrid-E-Mail-Service für Adobe Commerce in der Cloud-Infrastruktur und wie Sie Ihre DNS-Konfiguration testen können.
exl-id: 06236068-df32-468f-99ec-c379984be136
source-git-commit: 0cb86dd5e4fe627b198ac3c1a6b14607f377a9a3
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 0%

---

# SendGrid-E-Mail-Service

Der SMTP-Proxydienst (Simple Mail Transfer Protocol) von SendGrid bietet Dienste zur Authentifizierung ausgehender E-Mails und zur Überwachung der Reputation, einschließlich Unterstützung für:

* Alle ausgehenden Transaktions-E-Mails
* Dedizierte IP-Adressen
* Domain-Registrierung, DomainKeys Identified Mail-Signaturen (DKIM) für die Validierung von E-Mail-Domains (nur für Pro-Staging- und Produktionsumgebungen)
* Benutzerdefinierte Domain-Registrierung (nur für Pro)
* Automatisierte Integration für Starter- und Pro-Integrationsumgebungen. Pro-Produktions- und Staging-Umgebungen erfordern eine manuelle Bereitstellung und Konfiguration während des Hardware-Bereitstellungsprozesses von Infrastructure as a Service (IaaS)

Der SendGrid SMTP-Proxy ist nicht für die Verwendung als Allzweck-E-Mail-Server zum Empfang eingehender E-Mails oder zur Verwendung mit E-Mail-Marketing-Kampagnen vorgesehen. Wenn Sie Willkommens-E-Mails importieren und an eine große Anzahl von Kunden senden möchten (z. B. 10.000 oder mehr), teilen Sie den Import und den E-Mail-Versand über mehrere Tage auf. Dies hilft, die täglichen Sendebeschränkungen einzuhalten und die Reputation des Absenders zu schützen.

>[!TIP]
>
>Stellen Sie sicher, dass Sie die entsprechenden Store-E-Mail-Adressen in der Admin konfiguriert haben, indem Sie zu Stores > Konfiguration > Allgemein navigieren, um Probleme mit der Zustellbarkeit und Domain-Überprüfung zu vermeiden. Sie müssen die Option **[!UICONTROL Use Default]** deaktivieren und die Standardwerte durch eine Domain ersetzen, deren Inhaber Sie sind. E-Mail-Dienste mit öffentlicher/freigegebener Domain, wie gmail.com und outlook.com, sollten beim Senden von E-Mails über Sendgrid nicht als Absender-E-Mail-Adresse konfiguriert werden.

## E-Mail aktivieren oder deaktivieren

Sie können ausgehende E-Mails für jede Umgebung über die Cloud-Konsole oder die Befehlszeile aktivieren oder deaktivieren.

Ausgehende E-Mails sind in Pro-Produktions- und Staging-Umgebungen standardmäßig aktiviert. [!UICONTROL Outgoing emails] können jedoch in den Umgebungseinstellungen deaktiviert erscheinen, bis Sie die `enable_smtp`-Eigenschaft über die [Befehlszeile“ ](outgoing-emails.md#enable-emails-in-the-cli) die [Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console). Sie können ausgehende E-Mails für Integrations- und Staging-Umgebungen aktivieren, um E-Mails zur Zwei-Faktor-Authentifizierung oder zum Zurücksetzen des Passworts für Benutzer von Cloud-Projekten zu senden. Siehe [Konfigurieren von E-Mails zum ](outgoing-emails.md).

Wenn ausgehende E-Mails in Pro-Produktions- oder Staging-Umgebungen deaktiviert oder wieder aktiviert werden müssen, können Sie ein [Adobe Commerce-Support-Ticket senden](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Wenn Sie den Wert der [!UICONTROL enable_smtp]-Eigenschaft [Befehlszeile](outgoing-emails.md#enable-emails-in-the-cli) aktualisieren, wird auch der [!UICONTROL Enable outgoing emails] für diese Umgebung in der [Cloud-Konsole](outgoing-emails.md#enable-emails-in-the-cloud-console) geändert.

## Raster-Dashboard

Alle Cloud-Projekte werden unter einem zentralen Konto verwaltet, sodass nur der Support Zugriff auf das SendGrid-Dashboard hat. SendGrid stellt keine Funktionen zur Einschränkung von Unterkonten bereit.

Um in den Aktivitätsprotokollen den Versandstatus oder eine Liste der E-Mail-Adressen, an die keine Zustellung, Ablehnung oder Blockierung erfolgt, einzusehen, [ Sie ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). Das Support-Team **kann** Aktivitätsprotokolle abrufen, die älter als 30 Tage sind.

Fügen Sie Ihrer Anfrage nach Möglichkeit die folgenden Informationen hinzu:

* Die betroffene E-Mail-Adresse oder die betroffenen Adressen
* den betreffenden Zeitraum (nur innerhalb der letzten 30 Tage)
* Betreff der E-Mail

## DomainKeys Identified Mail (DKIM)

DKIM ist eine E-Mail-Authentifizierungstechnologie, mit der Internet Service Provider (ISPs) sowohl legitime als auch gefälschte Absenderadressen identifizieren können. Diese Technik wird häufig bei Phishing- und E-Mail-Betrügereien verwendet. DKIM ist auf einen Domain-Eigentümer angewiesen, der die DNS-Einträge verwaltet. Bei Verwendung von DKIM verwendet der Absenderserver einen privaten Schlüssel zum Signieren der Nachrichten. Außerdem fügt der Domain-Inhaber den DNS-Einträgen der Absender-Domain einen DKIM-Eintrag hinzu, bei dem es sich um einen geänderten `TXT`-Eintrag handelt. Dieser `TXT` enthält einen öffentlichen Schlüssel, den die Empfänger-E-Mail-Server zum Überprüfen der Signatur einer Nachricht verwenden. Mit dem Verschlüsselungsverfahren für öffentliche Schlüssel von DKIM können Empfängerinnen und Empfänger die Authentizität eines Absenders überprüfen. Siehe Erklärung zu [DKIM-Einträgen](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Die Unterstützung der SendGrid-DKIM-Signaturen und der Domain-Authentifizierung ist nur in der Produktions- und Staging-Umgebung für Pro-Projekte verfügbar, nicht aber in allen Starter-Umgebungen. Daher werden ausgehende Transaktions-E-Mails wahrscheinlich durch Spam-Filter gekennzeichnet. Die Verwendung von DKIM verbessert die Versandrate als authentifizierter E-Mail-Absender. Um die Nachrichtenversand-Rate zu verbessern, können Sie von Starter auf Pro aktualisieren oder Ihren eigenen SMTP-Server oder E-Mail-Versand-Dienstleister verwenden. Siehe [Konfigurieren von E-Mail](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications)-Verbindungen im _Handbuch für Admin-Systeme_.

### Absender- und Domain-Authentifizierung

Damit SendGrid in Ihrem Namen Transaktions-E-Mails aus Pro-Produktions- oder Staging-Umgebungen senden kann, müssen Sie Ihre DNS-Einstellungen so konfigurieren, dass sie die drei SendGrid-Subdomain-DNS-Einträge enthalten. Jedem SendGrid-Konto ist ein eindeutiger `TXT`-Eintrag zugewiesen, der zur Authentifizierung ausgehender E-Mails verwendet wird.

>[!TIP]
>
>Stellen Sie sicher, dass Sie die **[!UICONTROLSStore-E-Mail]** Adressen) mit der richtigen Domain in **[!UICONTROL Stores > Configuration > General > Store Email Addresses]** konfigurieren. Die Domain-Authentifizierung wird für die E-Mail-Adresse des Absenders durchgeführt. Wenn die Standardeinstellung (`example.com`) konfiguriert ist, werden die E-Mails von `example.com` durch Sendgrid blockiert.

**So aktivieren Sie die Domain-**:

1. Senden Sie ein [Support-Ticket](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket), um die Aktivierung von DKIM für eine bestimmte Domain anzufordern (**nur Pro Staging- und Produktionsumgebungen**).
1. Aktualisieren Sie Ihre DNS-Konfiguration mit den `TXT`- und `CNAME`-Einträgen, die Sie im Support-Ticket erhalten haben.

**Beispiel `TXT` Datensatz mit Konto-ID**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Beispiel `CNAME` Datensätze**:

| Domain | verweist auf | Datensatztyp |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| S1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| S2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM-Signaturen und automatisierte Sicherheit

Beim Einrichten einer authentifizierten Domain können Sie zwischen automatisierter und manueller Sicherheit wählen. Wenn Sie sich für automatisierte Sicherheit entscheiden, verwaltet SendGrid Ihre DKIM- und SPF-Einträge automatisch. Wenn Sie Ihrem Konto eine neue dedizierte Versand-IP-Adresse hinzufügen, aktualisiert SendGrid Ihre DNS-Einstellungen und die DKIM-Signatur sofort. Wenn Sie die automatisierte Sicherheit deaktivieren, müssen Sie Ihre DKIM-Signatur bei jeder Änderung der Versand-Domain aktualisieren.

**Beispiel für aktivierte automatisierte Sicherheit**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Beispiel: Automatische Sicherheit deaktiviert**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Nach der Einrichtung der Domain-Authentifizierung verarbeitet SendGrid automatisch Sicherheitsrichtlinien-Framework (Security Policy Framework, SPF)- und DKIM-Einträge. Nachdem SendGrid die `CNAME` Einträge bereitgestellt hat, die Sie Ihren DNS-Einträgen hinzufügen können, können Sie dedizierte IP-Adressen hinzufügen und andere Kontoaktualisierungen durchführen, ohne Ihre SPF-Einträge manuell verwalten zu müssen. Siehe [Automatisierte Sicherheit und Ihre DKIM-Signatur](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

So testen Sie Ihre DNS-Konfiguration:

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Schwellenwert für Transaktions-E-Mails

Der Schwellenwert für Transaktions-E-Mails bezieht sich auf die Anzahl der Transaktions-E-Mail-Nachrichten, die Sie innerhalb eines bestimmten Zeitraums von Pro-Umgebungen senden können, z. B. 12.000 E-Mails pro Monat aus Nicht-Produktionsumgebungen. Dieser Schwellenwert dient dem Schutz vor Spam und einer möglichen Beschädigung der E-Mail-Reputation.

Es gibt keine strengen Beschränkungen für die Anzahl der E-Mails, die in der Produktionsumgebung gesendet werden können, solange der Sender Reputation Score über 95 % liegt. Die Reputation wird von der Anzahl der unzustellbaren oder abgelehnten E-Mails und davon beeinflusst, ob DNS-basierte Spam-Verzeichnisse Ihre Domain als potenzielle Spam-Quelle gekennzeichnet haben. Siehe [E-Mails werden nicht gesendet, wenn die SendGrid-Punktzahl für Adobe Commerce überschritten wird](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) in der _Wissensdatenbank für den Commerce-Support_.

**So überprüfen Sie, ob die maximale Punktzahl überschritten wird**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Überprüfen Sie die `/var/log/mail.log` auf `authentication failed : Maxium credits exceeded` Einträge.

   Wenn `authentication failed` Protokolleinträge angezeigt werden und die **E-Mail-**) mindestens 95 beträgt, können Sie [Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) um eine Erhöhung der Kreditzuteilung anzufordern.

>[!NOTE]
>
>Die `var/log/mail.log`-Datei ist ein *Laufprotokoll*. Wenn neue Einträge hinzugefügt werden, werden ältere Einträge im Laufe der Zeit aus der Datei entfernt. Im Protokoll ist nur die letzte Protokollaktivität verfügbar. Die älteren Protokolleinträge werden nach dem Entfernen aus „mail.log“ nicht archiviert oder aufbewahrt.

### Reputation des E-Mail-Versands

Eine Reputation beim E-Mail-Versand ist eine Bewertung, die einem Unternehmen, das E-Mail-Nachrichten sendet, von einem Internet Service Provider (ISP) zugewiesen wurde. Je höher die Punktzahl ist, desto wahrscheinlicher ist es, dass ein ISP Nachrichten an den Posteingang einer Empfängerin oder eines Empfängers sendet. Wenn die Punktzahl unter eine bestimmte Stufe fällt, kann der ISP Nachrichten an den Spam-Ordner der Empfänger weiterleiten oder sogar Nachrichten vollständig ablehnen. Die Reputationsbewertung wird durch mehrere Faktoren bestimmt, z. B. durch einen 30-Tage-Durchschnitt Ihrer IP-Adressen im Vergleich zu anderen IP-Adressen und die Spam-Beschwerderate. Siehe [8 Möglichkeiten, Ihre Reputation beim E-Mail-Versand zu überprüfen](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).

### E-Mail-Unterdrückungslisten

Eine E-Mail-Unterdrückungsliste ist eine Liste von Empfängern, an die keine E-Mails gesendet werden sollten, wenn dies Ihren Ruf als Versender und Ihre Versandraten beeinträchtigen würde. Sie ist gemäß CAN-SPAM Act erforderlich, um sicherzustellen, dass E-Mail-Absender eine Methode zum Opt-out von Empfängern haben, die sich abgemeldet oder E-Mails als Spam gekennzeichnet haben. Die Unterdrückungsliste erfasst auch E-Mails, die unzustellbar, blockiert oder ungültig sind.

Um zu verhindern, dass E-Mails überhaupt an den Spam-Ordner gesendet werden, folgen Sie dem Artikel zu Best Practices von [Why Are My Emails Going to Spam?](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder).

Wenn einige Empfänger Ihre E-Mails nicht erhalten, können [ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket), um eine Überprüfung der Unterdrückungslisten anzufordern und die Empfänger bei Bedarf zu entfernen.

Weitere Informationen finden Sie unter [Was ist eine Unterdrückungsliste?](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
