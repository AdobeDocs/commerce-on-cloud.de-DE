---
title: Benutzerdefinierte VCL zur Umgehung des Fastly-Cache
description: Fehlerbehebung beim Anfrage-Traffic an den Ursprungs-Server durch Erstellen eines benutzerdefinierten VCL-Ausschnitts zur Umgehung des Fastly-Cache.
feature: Cloud, Configuration, Cache
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Benutzerdefinierte VCL zur Umgehung des Fastly-Cache

Sie können ein benutzerdefiniertes VCL-Snippet erstellen, um den Fastly-Cache zu umgehen, damit Sie den Anfragedatenverkehr an den Ursprungsserver beheben können. Sie können beispielsweise einen Ausschnitt erstellen, um zu ermitteln, ob Site-Probleme durch das Caching verursacht werden, oder um Kopfzeilen zu beheben.

Sie können das Snippet so konfigurieren, dass das Fastly-Caching für Anfragen von einer bestimmten IP-Adresse oder URL umgangen wird.

>[!NOTE]
>
>Stellen Sie vor dem Zusammenführen einer benutzerdefinierten VCL-Konfiguration in einer Produktionsumgebung sicher, dass Sie den Code in der Staging-Umgebung testen.

**Voraussetzungen:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Um den Fastly-Cache basierend auf der IP-Adresse oder URL zu umgehen**:

{{admin-login-step}}

1. Klicken Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

1. Klicken Sie **Benutzerdefiniertes Snippet erstellen**.

1. Fügen Sie die VCL-Snippet-Werte hinzu:

   - **Name** — `bypass_fastly`

   - **Typ** — `recv`

   - **Priorität** — `5`

   - **VCL** Snippet-Inhalt —

     Im folgenden Beispiel wird Fastly für eine bestimmte IP-Adresse umgangen:

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     Im folgenden Beispiel wird Fastly für ein bestimmtes URL-Muster umgangen:

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     Verwenden Sie für eine exakte URL-Übereinstimmung den `==`-Operator anstelle des `~`-Operators. Einzelheiten finden Sie in [Fastly VCL]Referenz.

1. Klicken Sie **Erstellen**.

   ![Erstellen eines VCL-Snippets unter Umgehung von Fastly](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. Nachdem die Seite neu geladen wurde, klicken Sie im Abschnitt **Fastly-Konfiguration** auf *VCL zu Fastly*.

1. Aktualisieren Sie nach Abschluss des Uploads den Cache entsprechend der Benachrichtigung oben auf der Seite.

   Validiert die aktualisierte VCL-Version während des Upload-Prozesses schnell. Wenn die Validierung fehlschlägt, bearbeiten Sie Ihr benutzerdefiniertes VCL-Snippet, um alle Probleme zu beheben. Laden Sie dann die VCL erneut hoch.

Nachdem Sie den VCL-Ausschnitt hinzugefügt haben, können Sie cURL-Befehle verwenden, um von der angegebenen IP-Adresse oder URL aus Anfragen an den Ursprungs-Server zu senden, wie im folgenden Beispiel gezeigt:

```bash
curl -svo /dev/null www.example.com/index.html
```

Überprüfen Sie dann die Antwort, um Probleme mit nicht zwischengespeicherten Inhalten zu beheben.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL-Referenz]: https://docs.fastly.com/vcl/
