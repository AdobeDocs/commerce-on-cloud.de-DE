---
title: Best Practices für die Store-Konfiguration
description: Lesen Sie die Best Practices für die Konfiguration Ihres Stores in Adobe Commerce in der Cloud-Infrastruktur.
feature: Cloud, Best Practices
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1087'
ht-degree: 0%

---

# Best Practices für die Store-Konfiguration

Detaillierte Informationen zum Konfigurieren Ihres Shops, Ihrer Websites und Websites finden Sie im [Adobe Commerce-Benutzerhandbuch](https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html). Diese Seite bietet Best Practices, hilfreiche Informationen und Richtlinien für die Konfiguration Ihrer Stores, Sites und mehr mit zusätzlichen Inhalten, die im Laufe der Zeit und über Versionen hinweg gepostet werden können.

## Marketing-Kampagnen und -Promotions

Diese Informationen sind für Adobe Commerce unter Cloud-Infrastrukturen 2.1.x und 2.2.x hilfreich.

Um Kampagnen und Promotions zu erstellen, erstellen Sie die Optionen und Einstellungen in [Inhalts-Staging](https://experienceleague.adobe.com/docs/commerce-admin/content-design/staging/content-staging.html). Mit dieser Funktion können Sie Ihre Kampagnen erstellen und in der Vorschau anzeigen, bevor sie für den Kundenverkauf veröffentlicht werden. Im Folgenden finden Sie hilfreiche Informationen. Genaue Anweisungen finden Sie im Inhalt des verknüpften Adobe Commerce-Benutzerhandbuchs.

_Kampagnen_ sind Marketing-Events für saisonale Verkäufe, neue Produktlinien und mehr. Jede Kampagne kann benutzerdefinierte Designs, Inhaltsbausteine, Widgets zur Steuerung und Anzeige von Inhalten und zugehörige Promotions mit Preisregeln enthalten. Da eine Kampagne sehr umfangreich ist, erstellen Sie sie im Content-Staging mit einem Start- und Enddatum.

_Promotions_ bieten Rabatte, einmalige Angebote, Coupons, erstmalige Kaufanreize und mehr. Sie erstellen diese Aktionen als _Preisregeln_ die die Bedingungen, Rabatte und Optionen festlegen, um Kunden zum Kauf zu ermutigen. Sie können Preisregeln für den [Warenkorb](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart.html) oder [Katalog](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog.html) mit zusätzlichen Optionen für Banner, Belohnungspunkte und mehr erstellen. Sie können Kampagnen für Ihre Werbeaktionen planen und Preisregeln für große Veranstaltungen wie eine neue Produktlinie oder saisonale Verkäufe anwenden.

Im Folgenden finden Sie Tipps zum Erstellen, Aktualisieren und Verwalten von Promotions und Kampagnen:

* Eine Promotion kann Teil einer Kampagne sein. Eine Kampagne kann nicht Teil einer Promotion sein. Sie können Listen von Promotions als Preisregeln haben, die mehrmals mit mehreren Kampagnen verwendet werden können.
* Wenn Sie eine Promotion erstellen, wird immer eine erste Kampagne erstellt, die inaktiv ist. Es hat ein Startdatum, aber kein Enddatum. Sie können diese ursprüngliche Kampagne ignorieren. Sie können ein neues Update mit dem richtigen Kampagnenzeitplan planen und aktivieren.
* Eine Kampagne hat ein Start- und Enddatum, keine Promotion. Der Planer, der beim Erstellen einer Promotion angezeigt wird, konfiguriert nicht die Start- und Enddaten für die Promotion. Damit können Sie eine Kampagne für diese Promotion planen, während Sie sich auf der Konfigurationsseite der Promotion befinden.
* Staging-Inhalte können nicht direkt bearbeitet werden. Wenn Sie die Einstellungen und Optionen in der Kampagne bearbeiten müssen, bearbeiten Sie das Original oder eine Replikation und drücken Sie, um im Staging-Inhalt zu überschreiben. Wenn Sie beispielsweise kein Enddatum für eine Kampagne angeben, müssen Sie das Original bearbeiten und zur Aktualisierung pushen.

## Erweiterte Preise und Staging-Inhalte

Diese Informationen sind für Adobe Commerce unter Cloud-Infrastrukturen 2.1.x und 2.2.x hilfreich.

In der Regel können Sie [Erweiterte Preise](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/pricing/pricing-advanced.html) für Produkte im Bereich **Produkte** > **Kataloge** des Administrators festlegen. Führen Sie mit Staging-Inhalten einige zusätzliche Schritte aus, um die Preise zu einer Promotion und Kampagne hinzuzufügen.

So bearbeiten Sie erweiterte Preise und aktualisieren das Staging von Inhalten:

1. Melden Sie sich beim Administrator an.
1. Navigieren Sie zu **Produkte** > **Katalog** und wählen Sie ein Produkt aus und bearbeiten Sie es.
1. Wählen Sie auf der Registerkarte „Preise“ die Option **Erweiterte Preise**. Bearbeiten Sie den Preis und speichern Sie Änderungen.
1. Klicken Sie oben auf der Seite auf **Neues Update planen**.
1. Erstellen Sie eine Promotion für das Produkt.
1. Füllen Sie die Informationen zur Promotion aus. Geben Sie für die Planung ein Anfangs- und Enddatum sowie eine Uhrzeit ein.
1. Speichern Sie die Promotion. Eine inaktive erste Kampagne wird erstellt.
1. Sie können eine Vorschau anzeigen, um den Sonderpreis, den Namen der Promotion, den regulären Preis und den geplanten Datumsbereich für die Kampagne zu überprüfen.

Weitere Schritte können Sie mit den Anweisungen unter &quot;[ für Katalogpreisregeln planen“ ](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rule-catalog-scheduled-changes.html). Klicken Sie **Weiter**, um die Schritte zu durchlaufen.

## Preisregeln

Preisregeln können Logik und Bedingungen enthalten, die so unbegrenzt sind wie Ihre Marketing-Fantasie. Einige beliebte Beispiele sind Buy One Get One Free, Buy One Get One Get One 50% Rabatt, ein $25 Rabatt auf Bestellungen über $100 Dollar und mehr.

Informationen zum Erstellen einer Preisregel finden Sie im [Adobe Commerce-Benutzerhandbuch](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog-create.html).

Im Folgenden finden Sie ein Beispiel für die Erstellung einer Preisregel für einen Rabatt „Nur Erstbestellung“. Für diesen Rabatt empfiehlt sich Folgendes:

* Erstellen Sie eine Preisregel mit einem [Kundensegment](https://experienceleague.adobe.com/en/docs/commerce-admin/customers/segments/customer-segment-price-rule) mit einer Bedingung: Gesamtzahl der Bestellungen weniger als 1
* Fügen Sie dieses Kundensegment als Bedingung zur Warenkorbregel hinzu
* Optional - Fügen Sie Bedingungen und Regeln hinzu, um die Rabatte auf bestimmte SKUs oder Produktkategorien für gezielte Käufe anzuwenden.

Dadurch wird sichergestellt, dass neue Kunden oder Bestandskunden, die keinen Kauf getätigt haben, den Rabatt nur bei ihrer ersten Bestellung erhalten. Sie können Banner erstellen und E-Mail-Werbeaktionen für den Erstkauf-Rabatt senden.

## Ansichten speichern

Mit einer einzigen Implementierung von Adobe Commerce in der Cloud-Infrastruktur können Sie mehrere Stores einrichten und ausführen. Siehe [Einrichten mehrerer Websites oder Stores](multiple-sites.md).

Für Stores, die nicht miteinander interagieren, können Sie mehrere (_)_. Jede Website verfügt über bestimmte Artikel, Kundendaten, Checkout-Artikel und Warenkorb, die nicht mit anderen Websites in Adobe Commerce geteilt werden.

Jede Website kann einen oder mehrere _Stores_ mit verschiedenen Kategorien und Artikeln, freigegebenen Kundendaten, Checkout und Warenkorb enthalten. Für diese Geschäfte kann sich ein Kunde einmal anmelden und mit einem einzigen Checkout über verschiedene Produktkataloge hinweg einkaufen.

Außerdem können Sie _Store-Ansichten_ für verschiedene Sprachen, Layouts und Designs erstellen. Jede Ansicht kann beim Freigeben von Artikeln, Kundendaten, Checkout und Warenkorb über eine separate Domain, ein eigenes Branding und eine eigene Sprache verfügen.

Im Folgenden finden Sie Beispiele für eine bessere Erklärung:

* Einzelne Website mit einem Geschäft und zwei Ansichten für das englische und spanische Gebietsschema. Alle Artikeldaten, Kunden, der Checkout und der Warenkorb werden freigegeben.

  ![Beispiel 1 speichern](../../assets/example-store1.png)

* Eine einzelne Website mit einem Geschäft für Damenbekleidung enthält zwei Ansichten: eine für Englisch und eine für Spanisch. Das Geschäft für Kinderbekleidung enthält eine einzige Shop-Ansicht in englischer Sprache. Alle Artikeldaten, Kunden, der Checkout und der Warenkorb werden freigegeben. Die Stores können unterschiedliche Domains und Themen haben.

  ![Beispiel 2 speichern](../../assets/example-store2.png)

* Zwei Websites für Kleidung und eine andere für Wohnkultur mit verschiedenen Katalogen und separaten Artikeln, Kundendaten und Warenkorb. Jede Website könnte mehrere Stores und Ansichten haben, die Artikel, Kundendaten, Checkout und Warenkorb nur innerhalb dieser Website teilen.

  ![Beispiel 3 speichern](../../assets/example-store3.png)

>[!WARNING]
>
>Die Anzahl der Katalogdaten steigt mit der Anzahl der Websites und Stores. Je nach Projektarchitektur können die zusätzlichen Stores zu einem längeren Indizierungsprozess und langsameren Antwortzeiten für nicht zwischengespeicherte Katalogseiten führen. Adobe empfiehlt, die Leistung der Site genau zu überwachen.
