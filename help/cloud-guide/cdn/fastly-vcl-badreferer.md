---
title: Spam für Verweise blockieren
description: Blockieren Sie Empfehlungs-Spam auf Ihrer Site mit dem Fastly Edge-Wörterbuch und einem benutzerdefinierten VCL-Snippet.
feature: Cloud, Configuration, Security
exl-id: 4ed47a71-7fee-4f37-a7da-3e30052004df
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Spam für Verweise blockieren

Das folgende Beispiel zeigt, wie Sie [Fastly Edge Dictionary](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) mit einem benutzerdefinierten VCL-Snippet konfigurieren, um Empfehlungs-Spam aus Ihrem Adobe Commerce auf der Cloud-Infrastruktur-Site zu blockieren.

>[!NOTE]
>
>Es wird empfohlen, benutzerdefinierte VCL-Konfigurationen zu einer Staging-Umgebung hinzuzufügen, in der Sie sie testen können, bevor Sie sie mit der Produktionsumgebung ausführen.

**Voraussetzungen:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Überprüfen Sie Ihre Site-Protokolle auf gefälschte Verweis-URLs und erstellen Sie eine Liste der zu blockierenden Domains.

## Erstellen einer Referrer-Blockierungsliste

Edge-Wörterbücher erstellen Schlüssel-Wert-Paare, auf die während der Verarbeitung von VCL-Ausschnitten VCL-Funktionen zugreifen können. In diesem Beispiel erstellen Sie ein Edge-Wörterbuch, das die Liste der zu blockierenden Referrer-Websites bereitstellt.

{{admin-login-step}}

1. Klicken Sie auf **Stores** > **Einstellungen** > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Edge-Wörterbücher**.

1. Erstellen Sie den Wörterbuch-Container:

   - Klicken Sie **Container hinzufügen**.

   - Geben Sie auf *Seite* Container“ einen **Wörterbuchnamen**-`referrer_blocklist` ein.

   - Wählen Sie **Aktivieren nach der Änderung** aus, um Ihre Änderungen für die Version der Fastly-Service-Konfiguration bereitzustellen, die Sie bearbeiten.

   - Klicken Sie **Hochladen**, um das Wörterbuch an Ihre Fastly-Service-Konfiguration anzuhängen.

1. Fügen Sie die Liste der Domain-Namen hinzu, die dem `referrer_blocklist` blockiert werden sollen:

   - Klicken Sie auf das Symbol Einstellungen für das `referrer_blocklist`.

   - Hinzufügen und Speichern von Schlüssel-Wert-Paaren im neuen Wörterbuch. In diesem Beispiel ist jeder **Schlüssel** der Domain-Name einer zu blockierenden Referrer-URL und **Wert** wird `true`.

     ![Ungültige Referrer-Wörterbuchelemente hinzufügen](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Klicken Sie **Abbrechen**, um zur Systemkonfigurationsseite zurückzukehren.

1. Klicken Sie **Konfiguration speichern**.

1. Aktualisieren Sie den Cache entsprechend der Benachrichtigung oben auf der Seite.

Weitere Informationen zu Edge-Wörterbüchern finden Sie unter [Erstellen und Verwenden von Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) und [benutzerdefinierte VCL-Snippets](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) in der Fastly-Dokumentation.

## Erstellen eines benutzerdefinierten VCL-Snippets zum Blockieren von Referrer-Spam

Der folgende benutzerdefinierte VCL-Code-Ausschnitt (JSON-Format) zeigt die Logik zum Überprüfen und Blockieren von Anfragen. Das VCL-Snippet erfasst den Host einer Referrer-Website in einer Kopfzeile und vergleicht dann den Host-Namen mit der Liste der URLs im `referrer_blocklist`. Wenn der Host-Name übereinstimmt, wird die Anfrage mit einem `403 Forbidden` Fehler blockiert.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

Bevor Sie einen Ausschnitt basierend auf diesem Beispiel erstellen, überprüfen Sie die Werte, um festzustellen, ob Sie Änderungen vornehmen müssen:

- `name` — Name des VCL-Snippets. Für dieses Beispiel haben wir `block_bad_referrer` verwendet.

- `dynamic` - Wert 0 zeigt einen [regulären Ausschnitt](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) an, der für die Fastly-Konfiguration in die versionierte VCL hochgeladen werden soll.

- `priority` - Bestimmt, wann der VCL-Snippet ausgeführt wird. Die Priorität `5` darin, diesen Code-Ausschnitt auszuführen, bevor einem der standardmäßigen Magento VCL-Ausschnitte (`magentomodule_*`) eine Priorität von 50 zugewiesen wird. Legen Sie die Priorität für jeden benutzerdefinierten Ausschnitt auf einen Wert von über oder unter 50 fest, je nachdem, wann der Ausschnitt ausgeführt werden soll. Snippets mit Zahlen niedrigerer Priorität werden zuerst ausgeführt.

- `type` - Gibt einen Speicherort an, an dem der Ausschnitt in die VCL-Version eingefügt werden soll. In diesem Beispiel ist der VCL-Ausschnitt ein `recv`. Wenn der Ausschnitt in die VCL-Version eingefügt wird, wird er der `vcl_recv`-Unterroutine, unterhalb des standardmäßigen Fastly-VCL-Codes und über allen Objekten hinzugefügt.

- `content` - Der Ausschnitt des VCL-Codes, der in einer Zeile ohne Zeilenumbrüche ausgeführt werden soll.

Nachdem Sie den Code für Ihre Umgebung überprüft und aktualisiert haben, verwenden Sie eine der folgenden Methoden, um das benutzerdefinierte VCL-Snippet zu Ihrer Fastly-Service-Konfiguration hinzuzufügen:

- [Fügen Sie das benutzerdefinierte VCL-Snippet von der Admin &#x200B;](#add-the-custom-vcl-snippet). Diese Methode wird empfohlen, wenn Sie auf Admin zugreifen können. (Erfordert [Fastly Version 1.2.58](fastly-configuration.md#upgrade) oder höher.)

- Speichern Sie das JSON-Code-Beispiel in einer Datei (z. B. `allowlist.json`) und [&#x200B; Sie es mithilfe der Fastly-API &#x200B;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Verwenden Sie diese Methode, wenn Sie nicht auf Admin zugreifen können.

## Hinzufügen des benutzerdefinierten VCL-Snippets

{{admin-login-step}}

1. Klicken Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

1. Klicken Sie **Benutzerdefiniertes Snippet erstellen**.

1. Fügen Sie die VCL-Snippet-Werte hinzu:

   - **Name** — `block_bad_referrer`

   - **Typ** — `recv`

   - **Priorität** — `5`

   - **VCL** Snippet-Inhalt —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Klicken Sie **Erstellen**.

   ![Erstellen eines benutzerdefinierten Referrer-Block-VCL-Snippets](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. Nachdem die Seite neu geladen wurde, klicken Sie im Abschnitt **Fastly-Konfiguration** auf *VCL zu Fastly*.

1. Aktualisieren Sie nach Abschluss des Uploads den Cache entsprechend der Benachrichtigung oben auf der Seite.

Validiert die aktualisierte VCL-Version während des Upload-Prozesses schnell. Wenn die Validierung fehlschlägt, bearbeiten Sie Ihr benutzerdefiniertes VCL-Snippet, um alle Probleme zu beheben. Laden Sie dann die VCL erneut hoch.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
