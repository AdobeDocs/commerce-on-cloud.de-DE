---
title: Cache-Konfiguration anpassen
description: Erfahren Sie, wie Sie die Cache-Konfigurationseinstellungen überprüfen und anpassen können, nachdem die Fastly-Service-Einrichtung abgeschlossen ist.
feature: Cloud, Configuration, Iaas, Cache
exl-id: f6901931-7b3f-40a8-9514-168c6243cc43
source-git-commit: 551a00932165dd1c0a876b8151ba14752ceac802
workflow-type: tm+mt
source-wordcount: '1953'
ht-degree: 0%

---

# Cache-Konfiguration anpassen

Nachdem Sie den Fastly-Service in Ihren Staging- und Produktionsumgebungen eingerichtet und getestet haben, überprüfen und passen Sie die Cache-Konfigurationseinstellungen an. Sie können beispielsweise die Einstellungen aktualisieren, um zu ermöglichen, dass TLS HTTP-Anfragen an Fastly umleitet, die Bereinigungseinstellungen aktualisiert und die Standardauthentifizierung aktiviert, um Ihre Site während der Entwicklung mit einem Passwort zu schützen.

Die folgenden Abschnitte enthalten eine Übersicht und Anweisungen zum Konfigurieren einiger Cache-Einstellungen. Weitere Informationen zu den verfügbaren Konfigurationsoptionen finden Sie in der Dokumentation [Fastly CDN Module for Magento 2](https://github.com/fastly/fastly-magento2/tree/master/Documentation) .

## TLS erzwingen

Fastly bietet die _Force TLS_-Option zur Weiterleitung unverschlüsselter Anfragen (HTTP) an Fastly. Nachdem Ihre Staging- oder Produktionsumgebung mit einem [gültigen SSL-/TLS-Zertifikat](fastly-configuration.md#provision-ssltls-certificates) bereitgestellt wurde, können Sie die Fastly-Konfiguration für Ihren Store aktualisieren, um die Option TLS erzwingen zu aktivieren. Siehe das Fastly [TLS-Handbuch erzwingen](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md) in der Dokumentation _Fastly CDN Module for Magento 2_.

>[!NOTE]
>
>Die Aktivierung der Option TLS erzwingen ist eine empfohlene Best Practice für Adobe Commerce in Cloud-Infrastrukturspeichern.

## Fastly-Zeitüberschreitung verlängern

Die Fastly-Service-Konfiguration legt einen standardmäßigen Timeout-Zeitraum von 180 Sekunden für HTTPS-Anfragen an den Administrator fest. Jede Anforderungsverarbeitung, die die Zeitüberschreitung überschreitet, gibt einen 503-Fehler zurück. Daher können bei Anfragen, die eine langwierige Verarbeitung erfordern, oder beim Versuch, Massenvorgänge durchzuführen, 503 Fehler auftreten.

Um Massenaktionen abzuschließen, die länger als 3 Minuten dauern, ändern Sie den Wert _Admin Path Timeout_, um 503 Fehler zu vermeiden.

>[!NOTE]
>
>Wenn Sie im Feld **Benutzerdefinierter Admin-Pfad** unter **Stores** > **Configuration** > **Advanced** > **Admin** > **Admin-Basis-URL** einen benutzerdefinierten Admin-Pfad-Endpunkt angegeben haben, müssen Sie in dieser Umgebung auch die [ADMIN_URL](../environment/variables-admin.md#change-the-admin-url) auf denselben Wert setzen. Wenn die Einstellungen unterschiedlich sind, funktioniert die Zeitüberschreitung nicht.
>
>Informationen zum Erweitern von Fastly-Timeout-Parametern für andere als Admin in der Fastly-Benutzeroberfläche finden Sie unter [Erhöhen von Timeouts für lange Aufträge](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md).

**So verlängern Sie die Fastly-Zeitüberschreitung für den Administrator**:

{{admin-login-step}}

1. Klicken Sie auf **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** und erweitern Sie **Vollständiger Seitencache**.

1. Erweitern Sie im Abschnitt _Fastly_ den Eintrag **Erweiterte Konfiguration**.

1. Legen Sie den Wert **Zeitüberschreitung für Administratorpfad** in Sekunden fest. Dieser Wert darf nicht länger als 10 Minuten (600 Sekunden) sein.

>[!NOTE]
>
>Die Konfigurationseinstellung **_Admin-Pfad_** Zeitüberschreitung) kontrolliert keine Zeitüberschreitungswerte außerhalb von Adobe Commerce, z. B. die Zeitüberschreitung bei Fastly WAF. Um den Wert für die Fastly WAF-Zeitüberschreitung anzupassen, müssen Sie ein Adobe-Support-Ticket öffnen, um ihn im Fastly-Service zu aktualisieren.

1. Klicken **oben auf** Seite auf „Konfiguration speichern“.

1. Wählen Sie nach dem Neuladen der Seite **&#x200B;**&#x200B;Abschnitt _Fastly-Konfiguration_ die Option VCL in Fastly hochladen aus.

Ruft schnell den Admin-Pfad für die Generierung der VCL-Datei aus der `app/etc/env.php`-Konfigurationsdatei ab.

## Bereinigungsoptionen konfigurieren

Fastly bietet mehrere Arten von Bereinigungsoptionen auf Ihrer Magento-Cache-Verwaltungsseite, einschließlich Optionen zum Bereinigen von Produktkategorien, Produkt-Assets und Inhalten. Wenn diese Option aktiviert ist, sucht Fastly automatisch nach Ereignissen, um diese Caches zu bereinigen. Wenn Sie eine Bereinigungsoption deaktivieren, können Sie Fastly-Caches manuell bereinigen, nachdem Sie Aktualisierungen über die Seite Cache-Verwaltung abgeschlossen haben.

Zu den Bereinigungsoptionen gehören:

- **Kategorie bereinigen** - Bereinigt den Inhalt einer Produktkategorie (nicht den Produktinhalt), wenn Sie ein einzelnes Produkt hinzufügen und aktualisieren. Sie können diese Option deaktivieren und die Option Produkt bereinigen aktivieren, wodurch Produkte und Produktkategorien bereinigt werden.
- **Produkt bereinigen** - Löscht alle Inhalte aus Produkt- und Produktkategorien, wenn eine einzelne Änderung an einem Produkt gespeichert wird. Die Aktivierung der Produktbereinigung kann hilfreich sein, um Kunden sofort Aktualisierungen zu erhalten, wenn ein Preis geändert, eine Produktoption hinzugefügt oder der Produktbestand nicht vorrätig ist.
- **CMS-Seite bereinigen** - Löscht Seiteninhalte beim Aktualisieren und Hinzufügen von Seiten zur Adobe Commerce CMS. Sie können beispielsweise löschen, wenn Sie Ihre Nutzungsbedingungen oder die Rückgaberichtlinie aktualisieren. Wenn Sie diese Änderungen nur selten vornehmen, können Sie die automatische Bereinigung deaktivieren.
- **Soft purge** - Legt geänderten Inhalt auf veraltet fest und löscht ihn entsprechend dem veralteten Zeitpunkt. Zusätzlich zu den veralteten Zeiten werden Kunden veraltete Inhalte bereitgestellt, während Fastly den Inhalt im Hintergrund aktualisiert.

![Bereinigungsoptionen konfigurieren](../../assets/cdn/fastly-purge-options.png)

**So konfigurieren Sie die Optionen für die Schnellbereinigung**:

1. Erweitern Sie im Abschnitt _Fastly_ den Eintrag **Erweiterte Konfiguration**, um die Bereinigungsoptionen anzuzeigen.

1. Wählen Sie für jede Bereinigungsoption **Ja**, um die automatische Bereinigung zu aktivieren, oder **Nein**, um die automatische Bereinigung zu deaktivieren.

   Wenn Sie eine Bereinigungsoption deaktivieren, müssen Sie den Cache für diese Kategorie auf der Seite _Cache-Verwaltung_ manuell bereinigen.

1. Klicken **oben auf** Seite auf „Konfiguration speichern“.

1. Wählen Sie nach dem Neuladen der Seite **&#x200B;**&#x200B;Abschnitt _Fastly-Konfiguration_ die Option VCL in Fastly hochladen aus.

Weitere Informationen finden Sie [den Fastly-Konfigurationsoptionen](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options).

## Geo-IP-Handhabung konfigurieren

Das Fastly-Modul beinhaltet GeoIP-Handhabung, um Besucher automatisch umzuleiten oder eine Liste von Geschäften bereitzustellen, die ihrem jeweiligen Länder-Code entsprechen. Wenn Sie bereits eine Erweiterung für die GeoIP-Verarbeitung verwenden, müssen Sie die Funktionen möglicherweise mit den Fastly-Optionen überprüfen.

**So richten Sie die GeoIP-Verarbeitung ein**:

{{admin-login-step}}

1. Klicken Sie auf **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** und erweitern Sie **Vollständiger Seitencache**.

1. Erweitern Sie im Abschnitt _Fastly_ den Eintrag **Erweiterte Konfiguration**.

1. Scrollen Sie nach unten und wählen Sie **Ja** aus, um **GeoIP aktivieren**. Es werden zusätzliche Konfigurationsoptionen angezeigt.

1. Wählen Sie für GeoIP-Aktion aus, ob der Besucher automatisch mit **Umleiten** umgeleitet wird oder ob eine Liste von Stores bereitgestellt wird, aus denen er mit **Dialog** auswählen kann.

1. Wählen **bei der** „Länderzuordnung“ die Option **Hinzufügen** aus, um einen aus zwei Buchstaben bestehenden Ländercode einzugeben, der einem bestimmten Adobe Commerce-Store aus einer Liste zugeordnet werden soll.

   ![GeoIP-Länderkarten hinzufügen](/help/assets/cdn/fastly-geo-code.png)

1. Klicken **oben auf** Seite auf „Konfiguration speichern“.

1. Wählen Sie nach dem Neuladen der Seite **&#x200B;**&#x200B;Abschnitt _Fastly-Konfiguration_ die Option VCL in Fastly hochladen aus.

>[!NOTE]
>
>Die aktuelle Implementierung des Adobe Commerce Fastly GeoIP-Moduls unterstützt keine Umleitungen zwischen mehreren Websites.

Fastly bietet auch eine Reihe von [geolocation-bezogenen VCL-Funktionen](https://developer.fastly.com/reference/vcl/variables/geolocation/) für eine maßgeschneiderte Geolocation-Codierung.

## Fastly Edge-Module aktivieren

Fastly Edge Modules ist ein flexibles Framework, das die Definition von UI-Komponenten und zugehörigem VCL-Code über eine Vorlage ermöglicht. Diese Module erleichtern die Anpassung und Erweiterung der Fastly-Service-Konfiguration über die Benutzeroberfläche, anstatt benutzerdefinierte VCL-Snippets zu verwenden.

Edge-Module ermöglichen es Ihnen, bestimmte Funktionen wie CORS-Kopfzeilen und Cloud-Sitemap-Neuschreibungen zu aktivieren und die Integration zwischen Ihrem Adobe Commerce-Store und anderen CMS oder Back-Ends zu konfigurieren.

Um auf das Edge-Modulmenü zuzugreifen, um die verfügbaren Module anzuzeigen, zu konfigurieren und zu verwalten, aktivieren Sie die Option _Fastly Edge-Module aktivieren_. Siehe [Fastly Edge-Module](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md) in der Dokumentation zu Fastly CDN-Modulen.

## Konfigurieren der Backends und der Ursprungsabschirmung

Backend-Einstellungen ermöglichen eine Feinabstimmung der Fastly-Leistung mit Origin-Abschirmung und Zeitüberschreitungen. Ein _Back-End_ ist ein bestimmter Speicherort (IP oder Domain) mit konfigurierten Ursprungs-Shield- und Zeitüberschreitungseinstellungen zum Überprüfen und Bereitstellen von zwischengespeicherten Inhalten.

_Ursprungsabschirmung_ leitet alle Anfragen für Ihren Store an einen bestimmten Point of Presence (POP) weiter. Wenn eine Anfrage empfangen wird, prüft POP auf zwischengespeicherte Inhalte und stellt diese bereit. Wenn er nicht zwischengespeichert wird, wird er zum Shield-POP weitergeleitet und dann zum Ursprungs-Server, auf dem der Inhalt zwischengespeichert wird. Die Schilde reduzieren den Verkehr direkt zum Ursprung.

Der standardmäßige Fastly-VCL-Code legt Standardwerte für die Ursprungsabschirmung und Zeitüberschreitungen für Ihre Adobe Commerce auf Cloud-Infrastruktur-Sites fest. In einigen Fällen müssen Sie möglicherweise die Standardwerte ändern. Wenn Sie z. B. TTFB-Fehler (Time to First Byte) erhalten, müssen Sie möglicherweise den Wert _Timeout für das erste Byte_ anpassen.

>[!NOTE]
>
>Wenn Ihre Site funktionell über eine Backend-Integration wie [Wordpress](fastly-vcl-wordpress.md) bereitgestellt werden muss, passen Sie Ihre Fastly-Service-Konfiguration an, um das Backend hinzuzufügen und Weiterleitungen von Ihrem Adobe Commerce-Store zu Wordpress zu verwalten. Weitere Informationen finden Sie unter [Fastly Edge Modules - Other CMS/Backend Integration](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) in der Fastly-Moduldokumentation.

**Überprüfen der Konfiguration der Backend-Einstellungen**:

{{admin-login-step}}

1. Klicken Sie auf **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** und erweitern Sie **Vollständiger Seitencache**.

1. Erweitern Sie den Abschnitt **Fastly-**.

1. Erweitern Sie **Backend-Einstellungen** und wählen Sie das Zahnrad aus, um das standardmäßige Backend zu überprüfen. Ein Modal wird geöffnet, in dem die aktuellen Einstellungen mit Optionen zum Ändern angezeigt werden.

   ![Ändern des Back-End](../../assets/cdn/fastly-backend.png)

1. Wählen Sie den **Shield**-Standort (oder das Rechenzentrum) aus.

   Die standardmäßige Fastly-Konfiguration für Ihr Projekt legt den Speicherort fest, der Ihrer Cloud Service-Region am nächsten ist. Wenn Sie ihn ändern müssen, wählen Sie einen Speicherort in der Nähe des Standardspeicherorts aus.

1. Ändern Sie die Zeitüberschreitungswerte (in Mikrosekunden) für die Verbindung mit dem Shield, die Zeit zwischen den Bytes und die Zeit für das erste Byte. Es wird empfohlen, die standardmäßigen Zeitüberschreitungseinstellungen beizubehalten.

1. Wählen Sie optional die Option **Aktivieren des Backends und von Shield nach dem Bearbeiten oder Speichern**.

1. Klicken Sie **Hochladen**, um Ihre Änderungen zu speichern und sie auf die Fastly-Server hochzuladen.

1. Wählen Sie in Admin die Option **Konfiguration speichern**.

Weitere Informationen finden Sie im [Handbuch zu Backend-Einstellungen](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md) in der Fastly-Moduldokumentation.

## Einfache Authentifizierung

Die einfache Authentifizierung ist eine Funktion, um jede Seite und jedes Asset auf Ihrer Site mit einem Benutzernamen und einem Kennwort zu schützen.

Adobe **empfiehlt** die Standardauthentifizierung in Ihrer Produktionsumgebung nicht zu aktivieren. Sie können ihn im Staging konfigurieren, um Ihre Site während des Entwicklungsprozesses zu schützen. Siehe [Standardauthentifizierungshandbuch“ in ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) Dokumentation zum Fastly CDN-Modul.

Wenn Sie Benutzerzugriff hinzufügen und die einfache Authentifizierung in der Staging-Umgebung aktivieren, können Sie weiterhin auf den Administrator zugreifen, ohne zusätzliche Anmeldeinformationen zu benötigen.

>[!NOTE]
>
>Überprüfen **nicht** ob [!UICONTROL Enable HTTP access control] in der Cloud-Konsole für eine Umgebung aktiviert ist, in der Fastly aktiviert ist (z. B. Staging- oder Nicht-Live-Produktionsumgebungen). Wenn die Zugriffssteuerung auf diese Weise konfiguriert ist, können Benutzer, die zuvor Zugriff auf die Website hatten, möglicherweise weiterhin auf die Website zugreifen, wenn ihre Anmeldeinformationen von Fastly zwischengespeichert bleiben, auch nachdem ihr Zugriff widerrufen wurde.

## Erstellen benutzerdefinierter VCL-Snippets

Fastly unterstützt eine angepasste Version der Varnish Configuration Language (VCL), um die Fastly-Service-Konfiguration anzupassen. Sie können beispielsweise den Zugriff für bestimmte Benutzer oder IP-Adressen mithilfe von VCL-Codeblöcken mit Edge- und Access Control List (ACL)-Wörterbüchern zulassen, blockieren oder umleiten.

Anweisungen zum Erstellen benutzerdefinierter VCL-Snippets, Edge-Wörterbücher und ACLs finden Sie unter [Benutzerdefinierte Fastly-VCL-Snippets](fastly-vcl-custom-snippets.md).

>[!NOTE]
>
>Bevor Sie benutzerdefinierten VCL-Code, Edge-Wörterbücher und ACLs zu Ihrer Fastly-Modulkonfiguration hinzufügen, überprüfen Sie, ob der Fastly-Caching-Service mit der Standardkonfiguration funktioniert. Siehe [Schnelles Setup](fastly-configuration.md).

## Verwalten von Domains

Sowohl für Starter- als auch für Pro-Projekte können Sie die Option [!UICONTROL Domains] verwenden, um die Fastly-Domain-Konfiguration für Ihren Store hinzuzufügen und zu verwalten.

- Rufen Sie für Startprojekte die Projekt-URL auf der Registerkarte [!UICONTROL Domains] im [!DNL Cloud Console] auf, um Ihre Projekt-URL hinzuzufügen.

- Senden Sie für Pro-Projekte ein [Adobe Commerce-Support-Ticket](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um die Domain zu Ihrer Cloud-Projektkonfiguration hinzuzufügen. Das Support-Team aktualisiert auch die Adobe Commerce Fastly-Kontokonfiguration, um die Domain hinzuzufügen.

**Verwaltung der Fastly-Domain-Konfiguration über den Administrator**:

{{admin-login-step}}

1. Wählen Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** und erweitern Sie **Vollständiger Seitencache**.

1. Wählen Sie im Admin _Fastly Configuration_-Bereich **Domains**.

1. Klicken Sie auf **Domains verwalten**, um die Seite „Domains“ zu öffnen.

1. Fügen Sie die Top-Level- und Subdomain-Namen für die Stores in der Cloud-Umgebung hinzu.

   Sie können nur Domains angeben, die bereits zu Ihrer Cloud-Infrastrukturkonfiguration hinzugefügt wurden.

   ![Fastly-Domain-Konfiguration für Starter hinzufügen](../../assets/cdn/fastly-starter-activate-domain.png)

1. Klicken Sie auf **Aktivieren**, um die Fastly-Domain-Konfiguration zu aktualisieren.

>[!NOTE]
>
>Wenn dieselbe Domain für ein anderes Fastly-Konto konfiguriert wurde, müssen Sie ein Adobe Commerce-Support-Ticket einreichen, um die Domain-Delegierung anzufordern, bevor Sie die Domain zu Adobe Commerce hinzufügen können. Siehe [Mehrere Fastly-Konten und zugeordnete Domains](fastly.md#multiple-fastly-accounts-and-assigned-domains).

## Wartungsmodus aktivieren

Verwenden Sie die Option _Wartungsmodus_, um den administrativen Zugriff von bestimmten IP-Adressen auf Ihre Site zuzulassen und gleichzeitig eine Fehlerseite für alle anderen Anfragen zurückzugeben.

**So aktivieren Sie den Wartungsmodus mit Administratorzugriff**:

1. Öffnen Sie den Abschnitt _Fastly-_&quot; in der Admin-Liste.

1. Aktualisieren Sie im Abschnitt _Edge ACL_ die `maint_allow` Zugriffssteuerungsliste (ACL) mit den administrativen IP-Adressen, die auf Ihren Store zugreifen können, während er sich im Wartungsmodus befindet.

   ![IP-Wartungsmodus-Zulassungsliste aktualisieren](../../assets/cdn/fastly-maint-allowlist.png)

1. Wählen Sie im _Wartungsmodus_ die Option **Wartungsmodus aktivieren** aus.

   Nach der Aktivierung des Wartungsmodus wird der gesamte Traffic blockiert, mit Ausnahme von Anfragen von den IP-Adressen in der `maint_allowlist`-ACL. Sie können die `maint_allowlist` aktualisieren, um die IP-Adressen in der ACL zu ändern.

   Detaillierte Konfigurationsanweisungen finden Sie im [Handbuch für den Wartungsmodus](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md) in der Dokumentation zum Fastly CDN für Magento 2-Modul.
