---
title: Einrichten von PayPal-Zahlungsmethoden
description: Richten Sie PayPal-Zahlungsmethoden für Adobe Commerce in der Cloud-Infrastruktur ein.
feature: Cloud, Checkout, Payments
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Einrichten von PayPal-Zahlungsmethoden

Adobe Commerce on Cloud Infrastructure bietet ein Onboarding-Tool zur Konfiguration von PayPal Express-Checkout-Konten direkt über den Administrator. Dieses Tool ist für ECE 2.1.8 und höher verfügbar. Um die Live-Schaltung und das Testen von PayPal-Zahlungsmethoden besser zu unterstützen, können Sie Ihr PayPal Express-Checkout-Konto für Sandbox- oder Produktionskonten aktivieren und konfigurieren.

Sie können in jeder Umgebung entweder das Sandbox- oder das Produktionskonto konfigurieren:

* Legen Sie für Integrations- und Staging-Umgebungen Sandbox-Anmeldeinformationen fest.
* Legen Sie für Ihre Produktionsumgebung Sandbox-Anmeldeinformationen für erste Tests fest und ersetzen Sie dann durch Live-Produktions-Anmeldeinformationen für einen gestarteten Store.

## PayPal-Konto

Es ist zwar am besten, ein vorgefertigtes und konfiguriertes PayPal-Händlerkonto zu verwenden, Sie können jedoch über den Administrator ein Konto erstellen oder ein persönliches Konto aktualisieren.

[!DNL PayPal onboarding] unterstützt die Verbindung mit den folgenden Konten:

* PayPal-Geschäftskonto
* Persönliches PayPal-Konto, Umwandlung in ein Geschäftskonto. Wenn Sie bereits über ein persönliches PayPal-Konto verfügen, können Sie sich mit diesen Anmeldedaten anmelden und dieses Konto zu einem Geschäftskonto hochstufen, während Sie die Synchronisierung abschließen.

Wenn Sie noch kein PayPal-Konto haben, erstellen Sie eines. Geben Sie eine E-Mail-Adresse für ein neues Konto ein. Wenn kein passendes PayPal-Konto gefunden wird, befolgen Sie die Anweisungen zum Erstellen eines PayPal-Geschäftskontos. Oder Sie können ein Konto direkt über [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection) erstellen.

### PayPal-Beschränkungen

PayPal unterstützt die Verbindung von PayPal Express Checkout für Länder auf der ganzen Welt mit Ausnahme der folgenden Einschränkungen:

* Indien und Japan (zukünftige PayPal-Updates könnten diese Konten unterstützen)
* Israel

Für Brasilien müssen Sie über ein vorhandenes PayPal-Geschäftskonto verfügen, um eine Verbindung herzustellen. Sie können während dieses Vorgangs kein vorhandenes persönliches PayPal-Konto für Brasilien konvertieren. Wenn Sie ein Konto benötigen, erstellen [ein geschäftliches PayPal-Konto](https://www.paypal.com/us/webapps/mpp/account-selection).

## Konfigurieren von PayPal Express Checkout

So konfigurieren Sie PayPal Express Checkout:

1. Rufen Sie den Administrator für die Umgebung auf.
1. Wählen Sie in der linken Navigation die Option **Stores** > **Configuration** und dann **Sales** > **Payment Methods**.
1. Wählen Sie für PayPal **Konfigurieren**. Konfigurationsfelder werden in erweiterbaren Abschnitten für Express-Checkout, Advertise-PayPal-Guthaben und Basic- und Advanced-Einstellungen angezeigt.
1. PayPal-Konto verbinden. Bis das Konto verbunden ist, sind die zu aktivierenden Optionen deaktiviert. Details zu verfügbaren und unterstützten Konten für die Verbindung und Einschränkungen finden Sie unter [PayPal-Konto](#paypal-account).

   * Um Ihr PayPal-Live-Konto zu verbinden, klicken Sie auf Verbinden mit PayPal und folgen Sie den Anweisungen. Jeder Kunde, der mit einem Live-PayPal kauft, schließt ab und stellt Kunden in einem Live-Store aktiv eine Gebühr in Rechnung.
   * Um Ihr Sandbox-Konto zum Testen zu verbinden, klicken Sie auf Sandbox-Anmeldeinformationen und befolgen Sie die Eingabeaufforderungen. Jeder Kunde kauft mit einer Sandbox PayPal vollständig, ohne Kunden aktiv zu belasten.

1. Konfigurieren Sie die Express-Checkout-Einstellungen, um sich zu authentifizieren und die PayPal-API zu verwenden:

   * **E-Mail, die mit dem PayPal-Händlerkonto verknüpft ist** (Optional) Geben Sie die E-Mail-Adresse ein, die mit Ihrem PayPal-Händlerkonto verknüpft ist. Bei dieser E-Mail wird zwischen Groß- und Kleinschreibung unterschieden.
   * **API-Authentifizierungsmethoden** als API-Signatur oder API-Zertifikat.
   * API Benutzername, Passwort und Signatur werden von Ihrem PayPal-Konto erfasst.
   * **Sandbox-Modus** Wählen Sie Ja oder Nein aus, um anzugeben, ob die eingegebenen Anmeldeinformationen für Sandbox verwendet werden. Wenn Sie Produktionsanmeldeinformationen eingegeben haben, klicken Sie auf Nein.
   * **API Uses Proxy** Wählen Sie Ja oder Nein, um festzulegen, ob das System einen Proxy-Server verwendet, um eine Verbindung zwischen Adobe Commerce und dem PayPal-Zahlungssystem herzustellen. Wenn ja, geben Sie den Proxy-Host und Port ein.

1. Ausführliche Informationen und Schritte zur Konfiguration Ihres Kontos finden Sie unter [PayPal Express-Checkout](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout) beginnend mit Schritt 2. Vervollständigen Sie die erforderlichen Einstellungen.

Wenn das Konto konfiguriert und authentifiziert ist, können Sie PayPal-Zahlungsoptionen unter Erforderliche PayPal-Einstellungen aktivieren und deaktivieren:

* **Diese Lösung aktivieren** zeigt Kunden über die Website die PayPal-Zahlungsmethode an.
* **Kontextbezogenes Auscheckerlebnis aktivieren**
* **PayPal-Guthaben aktivieren** ermöglicht Kunden die PayPal-Kreditfinanzierung ohne zusätzliche Kosten. PayPal bezahlt die Bestellung im Voraus und bearbeitet alle Rückzahlungen für den Kredit direkt mit dem Kunden.

## PayPal-Variablen

Fügen Sie bei Verwendung des PayPal-Onboarding-Tools mit Adobe Commerce in der Cloud-Infrastruktur die folgende Variable zum `variables:env` Abschnitt der `magento.app.yaml` hinzu.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Wenn Sie ein Upgrade auf 2.2 von 2.1.8 oder höher durchführen, müssen Sie diese Variable dennoch hinzufügen.
