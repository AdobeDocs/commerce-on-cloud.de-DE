---
title: Ausgehende E-Mails konfigurieren
description: Erfahren Sie, wie Sie ausgehende E-Mails für Adobe Commerce in der Cloud-Infrastruktur aktivieren.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Ausgehende E-Mails konfigurieren

Sie können ausgehende E-Mails für Integrations- (und nur für Starter-Umgebungen) über die [!DNL Cloud Console] oder die Befehlszeile aktivieren und deaktivieren. Aktivieren Sie ausgehende E-Mails, um E-Mails zur Zwei-Faktor-Authentifizierung oder zum Zurücksetzen des Passworts für Benutzer von Cloud-Projekten zu senden.

Ausgehende E-Mails sind standardmäßig in Produktions- und Staging-Umgebungen (nur Pro) aktiviert. Die **[!UICONTROL Enable outgoing emails]** kann jedoch unabhängig vom Status in den Umgebungseinstellungen deaktiviert erscheinen, bis Sie die `enable_smtp`-Eigenschaft über die [Befehlszeile) &#x200B;](#enable-emails-in-the-cli) die [Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console).

Durch die Aktualisierung des `enable_smtp`-Eigenschaftswerts [Befehlszeile](#enable-emails-in-the-cli) ändert sich auch der [!UICONTROL Enable outgoing emails] für diese Umgebung in der Cloud-Konsole.

>[!NOTE]
>
>Durch Aktivieren/Deaktivieren der **[!UICONTROL Enable outgoing emails]** werden E-Mails in der Pro Staging- oder Produktionsumgebung nicht aktiviert/deaktiviert.

{{redeploy-warning}}

## Aktivieren von E-Mails in der Cloud-Konsole

Verwenden Sie den Umschalter **[!UICONTROL Outgoing emails]** in der Ansicht _Umgebung konfigurieren_, um die E-Mail-Unterstützung zu aktivieren oder zu deaktivieren.

Wenn ausgehende E-Mails in Pro-Produktions- oder Staging-Umgebungen deaktiviert oder wieder aktiviert werden müssen, können Sie ein [Adobe Commerce-Support-Ticket senden](https://experienceleague.adobe.com/de/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Der Status der ausgehenden E-Mails wird für Pro-Staging- oder Produktionsumgebungen in der Cloud-Konsole möglicherweise nicht angezeigt.

**So verwalten Sie den E-Mail-Support über die[!DNL Cloud Console]**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.
1. Wählen Sie ein Projekt in der Liste _Alle Projekte_ aus.
1. Klicken Sie im Projekt-Dashboard oben rechts auf das Konfigurationssymbol.
1. Klicken Sie auf **[!UICONTROL Environments]** und wählen Sie eine bestimmte Umgebung aus der Liste aus (mit Ausnahme von Staging und Produktion für Pro).
1. Um ausgehende E-Mails zu aktivieren oder zu deaktivieren, _Sie „Ausgehende E-Mails aktivieren_ **Ein** oder **Aus**.

   ![Konfiguration ausgehender E-Mails aktivieren](../../assets/outgoing-emails.png)

Nachdem Sie die Einstellung geändert haben, erstellt die Umgebung die neue Konfiguration und stellt sie bereit.

## Aktivieren von E-Mails in der CLI

Sie können die E-Mail-Konfiguration für eine aktive Umgebung mithilfe des Befehls `magento-cloud` CLI `environment:info` ändern, um die Eigenschaft `enable_smtp` festzulegen. Durch die Aktivierung von SMTP wird die `MAGENTO_CLOUD_SMTP_HOST` Umgebungsvariable mit der IP-Adresse des SMTP-Hosts für das Senden von E-Mails aktualisiert.

**So verwalten Sie den E-Mail-Support über die Befehlszeile**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Überprüfen Sie die Einstellung für ausgehende E-Mails für die Umgebung.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Ändern Sie die Konfiguration des E-Mail-Supports, indem Sie die Umgebungsvariable `enable_smtp` auf `true` oder `false` festlegen.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Warten Sie, bis die Umgebung erstellt und bereitgestellt wurde.

1. Verwenden Sie ein SSH, um sich bei der Remote-Umgebung anzumelden.

1. Überprüfen Sie, ob die E-Mail funktioniert, und senden Sie eine Test-E-Mail an eine Adresse, die Sie überprüfen können.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Überprüfen Sie, ob die E-Mail von SendGrid aufgenommen wird.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
