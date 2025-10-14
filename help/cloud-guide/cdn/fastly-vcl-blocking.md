---
title: Benutzerdefinierte VCL zum Blockieren von Anfragen
description: Blockieren eingehender Anfragen nach IP-Adresse mithilfe einer Edge Access Control List (ACL) mit einem benutzerdefinierten VCL-Snippet.
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# Benutzerdefinierte VCL zum Blockieren von Anfragen

Sie können das Fastly CDN-Modul für Magento 2 verwenden, um eine Edge-ACL mit einer Liste von IP-Adressen zu erstellen, die Sie blockieren möchten. Anschließend können Sie diese Liste mit einem VCL-Code-Ausschnitt verwenden, um eingehende Anfragen zu blockieren. Der Code prüft die IP-Adresse der eingehenden Anfrage. Wenn es mit einer in der ACL-Liste enthaltenen IP-Adresse übereinstimmt, blockiert Fastly den Zugriff der Anfrage auf Ihre Site und gibt eine `403 Forbidden error` zurück. Alle anderen Client-IP-Adressen erhalten Zugriff.

**Voraussetzungen:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Liste der zu blockierenden Client-IP-Adressen

## Erstellen einer Edge-ACL zum Blockieren von Client-IP-Adressen

Sie erstellen eine Edge-ACL, um die Liste der zu blockierenden IP-Adressen zu definieren. Nachdem Sie die ACL erstellt haben, können Sie sie in einem benutzerdefinierten VCL-Code-Ausschnitt verwenden, um den Zugriff auf Ihre Staging- oder Produktions-Site zu verwalten.

Verwalten Sie den Zugriff für Staging- und Produktions-Sites, indem Sie die Edge-ACL mit demselben Namen in beiden Umgebungen erstellen. Der VCL-Code-Ausschnitt gilt für beide Umgebungen.

1. Melden Sie sich beim Administrator an.
1. Navigieren Sie zu **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** > **Vollständiger Seitencache** > **Fastly Configuration**.
1. Erweitern Sie den Abschnitt **Edge ACL**.
1. Klicken Sie **ACL hinzufügen**, um eine Liste zu erstellen. Nennen Sie für dieses Beispiel die Liste &quot;Blockierungsliste&quot;.
1. Geben Sie Werte für die IP-Adresse in die Liste ein. Alle zu dieser Liste hinzugefügten Client-IP-Adressen werden blockiert und können nicht auf die Website zugreifen.
1. Aktivieren Sie bei Bedarf optional das Kontrollkästchen **Negiert**.

Sie referenzieren die Edge-ACL anhand des Namens in Ihrem VCL-Code.

## Erstellen Sie die benutzerdefinierte VCL für die -Blockierungsliste

>[!NOTE]
>
>Dieses Beispiel zeigt erfahrenen Benutzern, wie sie ein VCL-Codefragment erstellen, um benutzerdefinierte Blockierungsregeln zu konfigurieren, die in den Fastly-Service hochgeladen werden. Sie können eine Blockierungsliste oder Zulassungsliste vom Adobe Commerce-Administrator basierend auf dem Land konfigurieren, indem Sie die Funktion [Blockierung](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) verwenden, die im Modul Fastly CDN for Magento 2 verfügbar ist.

Nachdem Sie die Edge-ACL definiert haben, können Sie sie verwenden, um das VCL-Snippet zu erstellen und den Zugriff auf die in der ACL angegebenen IP-Adressen zu blockieren. Sie können denselben VCL-Ausschnitt sowohl in der Staging- als auch in der Produktionsumgebung verwenden, müssen den Ausschnitt jedoch separat in jede Umgebung hochladen.

Der folgende benutzerdefinierte VCL-Code-Ausschnitt (JSON-Format) zeigt die Logik zum Blockieren eingehender Anfragen mit einer Client-IP-Adresse, die mit einer Adresse in der ACL der Blockierungsliste übereinstimmt.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Bevor Sie einen Ausschnitt basierend auf diesem Beispiel erstellen, überprüfen Sie die Werte, um festzustellen, ob Sie Änderungen vornehmen müssen:

- `name`: Name für den VCL-Code-Ausschnitt. In diesem Beispiel haben wir den Namen `blocklist` verwendet.

- `priority`: Bestimmt, wann der VCL-Snippet ausgeführt wird. Mit der Priorität `5` sofort ausgeführt und überprüft werden, ob eine Admin-Anfrage von einer zulässigen IP-Adresse stammt. Der Ausschnitt wird vor jedem der standardmäßigen Magento VCL-Ausschnitte (`magentomodule_*`) ausgeführt, denen eine Priorität von 50 zugewiesen wurde. Legen Sie die Priorität für jeden benutzerdefinierten Ausschnitt auf einen Wert von über oder unter 50 fest, je nachdem, wann der Ausschnitt ausgeführt werden soll. Snippets mit Zahlen niedrigerer Priorität werden zuerst ausgeführt.

- `type`: Gibt den Typ des VCL-Ausschnitts an, der die Position des Ausschnitts im generierten VCL-Code bestimmt. In diesem Beispiel verwenden wir `recv`, das den VCL-Code in die `vcl_recv` Unterroutine, unter dem Textbaustein VCL und über allen Objekten einfügt. Eine Liste der Snippet[Typen finden Sie &#x200B;](https://docs.fastly.com/api/config#api-section-snippet) der Snippet-Referenz Fastly VCL .

- `content`: Der auszuführende VCL-Code-Ausschnitt, der die Client-IP-Adresse prüft. Wenn sich die IP in der Edge-ACL befindet, wird der Zugriff für die gesamte Website mit einem `403 Forbidden` blockiert. Alle anderen Client-IP-Adressen erhalten Zugriff.

Nachdem Sie den Code für Ihre Umgebung überprüft und aktualisiert haben, verwenden Sie eine der folgenden Methoden, um das benutzerdefinierte VCL-Snippet zu Ihrer Fastly-Service-Konfiguration hinzuzufügen:

- [Fügen Sie das benutzerdefinierte VCL-Snippet von der Admin &#x200B;](#add-the-custom-vcl-snippet). Diese Methode wird empfohlen, wenn Sie auf Admin zugreifen können. (Erfordert [Fastly Version 1.2.58](fastly-configuration.md#upgrade-fastly-module) oder höher.)

- Speichern Sie das JSON-Code-Beispiel in einer Datei (z. B. `blocklist.json`) und [&#x200B; Sie es mithilfe der Fastly-API &#x200B;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Verwenden Sie diese Methode, wenn Sie nicht auf Admin zugreifen können.

## Hinzufügen des benutzerdefinierten VCL-Snippets

{{admin-login-step}}

1. Klicken Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

1. Klicken Sie **Benutzerdefiniertes Snippet erstellen**.

1. Fügen Sie die VCL-Snippet-Werte hinzu:

   - **Name** — `blocklist`

   - **Typ** — `recv`

   - **Priorität** — `5`

   - Fügen Sie den **VCL**-Ausschnittinhalt hinzu:

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Klicken Sie **Erstellen**, um die VCL-Snippet-Datei mit dem Namensmuster `type_priority_name.vcl` zu generieren, z. B. `recv_5_blocklist.vcl`

1. Nachdem die Seite neu geladen wurde, klicken **im Abschnitt „Fastly-Konfiguration** auf *VCL zu Fastly hochladen*, um die Datei zur Fastly-Service-Konfiguration hinzuzufügen.

1. Aktualisieren Sie den Cache nach den Uploads gemäß der Benachrichtigung oben auf der Seite.

Validiert die aktualisierte Version des VCL-Codes während des Upload-Prozesses schnell. Wenn die Validierung fehlschlägt, bearbeiten Sie das benutzerdefinierte VCL-Snippet, um das Problem zu beheben. Laden Sie dann die VCL erneut hoch.

## Weitere VCL-Beispiele für das Blockieren von Anfragen

Die folgenden Beispiele zeigen, wie Anfragen mit Inline-Bedingungsanweisungen anstelle einer ACL-Liste blockiert werden können.

>[!WARNING]
>
>In diesen Beispielen wird der VCL-Code als JSON-Payload formatiert, die in einer Datei gespeichert und in einer Fastly-API-Anfrage gesendet werden kann. Sie können das [VCL-Snippet über die Admin](#add-the-custom-vcl-snippet) oder als JSON-Zeichenfolge mit der Fastly-API übermitteln. Um Validierungsfehler bei Verwendung der Fastly-API mit einer JSON-Zeichenfolge zu vermeiden, müssen Sie einen Backslash verwenden, um Sonderzeichen zu umgehen.

>[!NOTE]
>Wenn Sie den VCL-Code vom Administrator senden, extrahieren Sie die einzelnen Werte aus dem Beispiel-VCL-Code und geben Sie sie in die entsprechenden Felder ein. Beispiel:
>- Name: `<name of the VCL>`
>- Dynamisch: `<0/1>`
>- Typ: `<type>`
>- Priorität: `<priority>`
>- Inhalt: `<content>`

Siehe [Verwenden dynamischer VCL-Snippets](https://docs.fastly.com/vcl/vcl-snippets/) in der Fastly-VCL-Dokumentation.

### VCL-Code-Beispiel: Block nach Ländercode

In diesem Beispiel wird der zweistellige ISO 3166-1-Ländercode für das Land verwendet, das mit der IP-Adresse verknüpft ist.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>Anstatt ein benutzerdefiniertes VCL-Fragment zu verwenden, können Sie die Funktion Fastly [Blocking](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) in Adobe Commerce in Cloud Infrastructure Admin verwenden, um die Blockierung nach Länder-Code oder einer Liste von Länder-Codes zu konfigurieren.

### VCL-Code-Beispiel: Durch HTTP-Benutzeragenten-Anfrage-Header blockieren

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
