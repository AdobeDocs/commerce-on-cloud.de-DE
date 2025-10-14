---
title: Benutzerdefinierte VCL zum Zulassen von Anfragen
description: Filtern Sie eingehende Anfragen und erlauben Sie den Zugriff nach IP-Adresse für Adobe Commerce-Sites durch mit einer Fastly Edge ACL-Liste und einem benutzerdefinierten VCL-Snippet.
feature: Cloud, Configuration, Security
exl-id: 836779b5-5029-4a21-ad77-0c82ebbbcdd5
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# Benutzerdefinierte VCL zum Zulassen von Anfragen

Sie können eine Fastly Edge ACL-Liste mit einem benutzerdefinierten VCL-Code-Fragment verwenden, um eingehende Anfragen zu filtern und den Zugriff nach IP-Adresse zuzulassen. Die ACL-Liste gibt die zuzulassen IP-Adressen an.

Erstellen Sie eine Zulassungsliste, um den Zugriff auf Ihre Staging-Umgebung zu beschränken, sodass nur Anfragen von angegebenen IP-Adressen für interne Entwickler und genehmigte externe Services zulässig sind. Sie können auch eine Zulassungsliste erstellen, um den Zugriff auf den Administrator in Staging- und Produktionsumgebungen zu sichern.

Das folgende Beispiel zeigt, wie Sie ein benutzerdefiniertes VCL-Snippet mit einer [Fastly Access Control List (ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls) verwenden, um den Zugriff auf den Admin für eine Adobe Commerce in einer Cloud-Infrastrukturprojektumgebung zu sichern. Wenn Sie das benutzerdefinierte VCL-Snippet zur Cloud-Umgebung hinzufügen, lässt Fastly nur Anfragen von IP-Adressen zu, die in der ACL enthalten sind.

>[!TIP]
>
>Verwenden Sie für Staging- und Integrationsumgebungen, die nicht öffentlich zugänglich sein sollten, die in der [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) verfügbare HTTP-Zugriffssteuerungsoption, um den Zugriff auf die gesamte Site nach IP-Adresse zu verwalten.

**Voraussetzungen:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Liste der Client-IP-Adressen, die auf der Zulassungsliste enthalten sein sollen

## Erstellen einer Edge-ACL zum Zulassen von Client-IP-Adressen

Edge-ACLs erstellen IP-Adresslisten für die Verwaltung des Zugriffs auf Ihre Site. In diesem Beispiel erstellen Sie eine Edge-ACL und fügen die Liste der Client-IP-Adressen hinzu, die auf den Admin Ihrer Projektumgebung zugreifen dürfen.

{{admin-login-step}}

1. Klicken Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Edge ACL**.

1. Erstellen Sie den ACL-Container:

   - Klicken Sie **ACL hinzufügen**.

   - Geben Sie auf der *ACL* Container-Seite einen **ACL-Namen**-`allowlist` ein.

   - Wählen Sie **Aktivieren nach der Änderung** aus, um Ihre Änderungen für die Version der Fastly-Service-Konfiguration bereitzustellen, die Sie bearbeiten.

   - Klicken Sie **Hochladen**, um die ACL an Ihre Fastly-Service-Konfiguration anzuhängen.

1. Fügen Sie die Liste der IP-Adressen hinzu, die auf den Admin zugreifen dürfen:

   - Klicken Sie auf das Symbol Einstellungen für die `allowlist` ACL.

   - Fügen Sie für jede Client *IP-Adresse den* IP-Wert“ hinzu und speichern Sie ihn.

   - Klicken Sie **Abbrechen**, um zur Systemkonfigurationsseite zurückzukehren.

1. Klicken Sie **Konfiguration speichern**.

1. Aktualisieren Sie den Cache entsprechend der Benachrichtigung oben auf der Seite.

## Erstellen Sie das benutzerdefinierte VCL-Snippet, um den Admin-Zugriff zu sichern

Der folgende benutzerdefinierte VCL-Code-Ausschnitt (JSON-Format) zeigt die Logik zum Filtern von Anfragen an den Administrator und zum Zulassen des Zugriffs, wenn die Client-IP-Adresse mit einer Adresse in der `allowlist`-ACL übereinstimmt.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Bevor [ein benutzerdefiniertes Snippet erstellen](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html?lang=de#add-the-custom-vcl-snippet) überprüfen Sie in diesem Beispiel die Werte, um festzustellen, ob Sie Änderungen vornehmen müssen. Geben Sie dann jeden Wert in die entsprechenden Felder ein, z. B. `type` in das Feld Typ `content` in das Feld Inhalt .

- `name` — Name des VCL-Snippets. In diesem Beispiel `allowlist`.

- `priority` - Bestimmt, wann der VCL-Snippet ausgeführt wird. Die Priorität besteht darin, sofort `5` auszuführen und zu überprüfen, ob eine Admin-Anfrage von einer zulässigen IP-Adresse kommt. Der Ausschnitt wird vor jedem der standardmäßigen Magento VCL-Ausschnitte (`magentomodule_*`) ausgeführt, denen eine Priorität von 50 zugewiesen wurde. Legen Sie die Priorität für jeden benutzerdefinierten Ausschnitt auf einen Wert von über oder unter 50 fest, je nachdem, wann der Ausschnitt ausgeführt werden soll. Snippets mit Zahlen niedrigerer Priorität werden zuerst ausgeführt.

- `type` - Gibt einen Speicherort an, an dem der Ausschnitt in den versionierten VCL-Code eingefügt werden soll. Dieser VCL ist ein `recv` Snippet-Typ, der den Snippet-Code unterhalb des standardmäßigen Fastly-VCL-Codes und über allen Objekten zur `vcl_recv`-Unterroutine hinzufügt.

- `content` - Der auszuführende VCL-Code-Ausschnitt. In diesem Beispiel filtert der Code Anfragen an den Administrator und ermöglicht den Zugriff, wenn die Client-IP-Adresse mit einer Adresse in der `allowlist`-ACL übereinstimmt. Wenn die Adresse nicht übereinstimmt, wird die Anfrage mit einem `403 Forbidden` Fehler blockiert.

  Wenn die URL für Ihren Administrator geändert wurde, ersetzen Sie den Beispielwert `/admin` durch die URL für Ihre Umgebung. Beispiel: `/company-admin`.

Im Code-Beispiel ist die Bedingung `!req.http.Fastly-FF` bei der Verwendung von &quot;[&quot; &#x200B;](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). Diesen Code nicht entfernen oder bearbeiten.

Nachdem Sie den Code für Ihre Umgebung überprüft und aktualisiert haben, verwenden Sie eine der folgenden Methoden, um das benutzerdefinierte VCL-Snippet zu Ihrer Fastly-Service-Konfiguration hinzuzufügen:

- [Fügen Sie das benutzerdefinierte VCL-Snippet von der Admin &#x200B;](#add-the-custom-vcl-snippet). Diese Methode wird empfohlen, wenn Sie auf Admin zugreifen können. (Erfordert [Fastly CDN-Modul für Magento 2 Version 1.2.58](fastly-configuration.md#upgrade) oder höher.)

- Speichern Sie das JSON-Code-Beispiel in einer Datei (z. B. `allowlist.json`) und [&#x200B; Sie es mithilfe der Fastly-API &#x200B;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Verwenden Sie diese Methode, wenn Sie nicht auf Admin zugreifen können.

## Hinzufügen des benutzerdefinierten VCL-Snippets

{{admin-login-step}}

1. Klicken Sie **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System**.

1. Erweitern Sie **Vollständiger Seitencache** > **Fastly-Konfiguration** > **Benutzerdefinierte VCL-Snippets**.

1. Klicken Sie **Benutzerdefiniertes Snippet erstellen**.

1. Fügen Sie die VCL-Snippet-Werte hinzu:

   - **Name** — `allowlist`

   - **Typ** — `recv`

   - **Priorität** — `5`

   - Fügen Sie den **VCL**-Ausschnittinhalt hinzu:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Klicken Sie **Erstellen**, um die VCL-Snippet-Datei mit dem Namensmuster `type_priority_name.vcl` zu generieren, z. B. `recv_5_allowlist.vcl`

1. Nachdem die Seite neu geladen wurde, klicken **im Abschnitt „Fastly-Konfiguration** auf *VCL zu Fastly hochladen*, um die Datei zur Fastly-Service-Konfiguration hinzuzufügen.

1. Aktualisieren Sie nach Abschluss des Uploads den Cache entsprechend der Benachrichtigung oben auf der Seite.

Validiert die aktualisierte Version des VCL-Codes während des Upload-Prozesses schnell. Wenn die Validierung fehlschlägt, bearbeiten Sie das benutzerdefinierte VCL-Snippet, um das Problem zu beheben. Laden Sie dann die VCL erneut hoch.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
