---
title: Hinzufügen von Sitemap- und Suchmaschinenrobotern
description: Erfahren Sie, wie Sie in der Cloud-Infrastruktur Sitemap- und Suchmaschinenroboter zu Adobe Commerce hinzufügen.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Hinzufügen von Sitemap- und Suchmaschinenrobotern

Der Versuch, die `sitemap.xml`-Datei zu generieren und in das Stammverzeichnis zu schreiben, führt zum folgenden Fehler:

```
Please make sure that "/" is writable by the web-server.
```

Mit Adobe Commerce in der Cloud-Infrastruktur können Sie nur in bestimmte Verzeichnisse schreiben, z. B. `var`, `pub/media`, `pub/static` oder `app/etc`. Wenn Sie die `sitemap.xml` Datei mithilfe des Admin-Bedienfelds generieren, müssen Sie den `/media/` angeben.

Sie müssen keine `robots.txt`-Datei generieren, da sie den `robots.txt` Inhalt nach Bedarf generiert und in der Datenbank speichert. Sie können den Inhalt in Ihrem Browser mit dem `<domain.your.project>/robots.txt` oder `<domain.your.project>/robots` Link anzeigen.

Dies erfordert die ECE-Tools-Version 2002.0.12 und höher mit einer aktualisierten `.magento.app.yaml`. Ein Beispiel für diese Regeln finden Sie im [Magento-Cloud-Repository](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**So generieren Sie eine `sitemap.xml` in Version 2.2 oder höher**:

1. Zugriff auf Admin.
1. Klicken Sie im Menü _Marketing_ auf **Siteübersicht** im Abschnitt _SEO und Suche_.
1. Klicken Sie in _Ansicht_ Sitemap“ auf **Sitemap hinzufügen**.
1. Geben _in der Ansicht_ Neue Sitemap“ die folgenden Werte ein:

   - **Dateiname**:`sitemap.xml`
   - **path**:`/media/`

1. Klicken Sie **Speichern und generieren**. Die neue Sitemap wird im Raster _Sitemap_ verfügbar.
1. Klicken Sie auf den Pfad in der Spalte _Link für Google_.

**So fügen Sie der `robots.txt` Inhalt hinzu**:

1. Zugriff auf Admin.
1. Klicken Sie _Menü_ Inhalt **im Abschnitt** Design _auf_ Konfiguration“.
1. Klicken Sie in _Ansicht_ Design-Konfiguration **für** Website in der Spalte _Aktion_ auf Bearbeiten.
1. Klicken Sie in der Ansicht _Hauptwebsite_ auf **Suchmaschinenroboter**.
1. Aktualisieren Sie das Feld **Benutzerdefinierte Anweisung von robots.txt bearbeiten**.
1. Klicken Sie **Konfiguration speichern**.
1. Überprüfen Sie die `<domain.your.project>/robots.txt` oder `<domain.your.project>/robots` URL in Ihrem Browser.

>[!NOTE]
>
>Wenn die `<domain.your.project>/robots.txt` eine `404 error` generiert, [ Sie &quot;Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um die Umleitung von `/robots.txt` zu `/media/robots.txt` zu entfernen.

## Umschreiben mit Fastly VCL-Snippet

Wenn Sie über verschiedene Domains verfügen und separate Sitemaps benötigen, können Sie eine VCL erstellen, um zur richtigen Sitemap zu routen. Generieren Sie die `sitemap.xml`-Datei im Admin-Bedienfeld wie oben beschrieben und erstellen Sie dann ein benutzerdefiniertes Fastly-VCL-Snippet, um die Umleitung zu verwalten. Siehe [Benutzerdefinierte Fastly-VCL-Snippets](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Sie können benutzerdefinierte VCL-Snippets über die Admin-Benutzeroberfläche oder die Fastly-API hochladen. Siehe [Beispiele und Tutorials für benutzerdefinierte VCL-Snippets](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Verwenden eines Fastly VCL-Snippets für die Umleitung

Erstellen Sie ein benutzerdefiniertes VCL-Snippet , um den Pfad für `sitemap.xml` zu `/media/sitemap.xml` mithilfe der Schlüssel-Wert-Paare `type` und `content` neu zu schreiben.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

Das folgende Beispiel zeigt, wie Sie den Pfad für `robots.txt` und `sitemap.xml` zu `/media/robots.txt` und `/media/sitemap.xml` umschreiben

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**So verwenden Sie ein Fastly-VCL-Snippet für eine bestimmte Domain-Umleitung**:

Erstellen Sie eine `pub/media/domain_robots.txt`-Datei, in der die Domain `domain.com` ist, und verwenden Sie das nächste VCL-Snippet:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

Der VCL-Ausschnitt leitet `http://domain.com/robots.txt` und zeigt die `pub/media/domain_robots.txt` an.

Um eine Umleitung für `robots.txt` und `sitemap.xml` in einem einzigen Snippet zu konfigurieren, erstellen Sie `pub/media/domain_robots.txt`- und `pub/media/domain_sitemap.xml`-Dateien, wobei die Domain `domain.com` ist, und verwenden Sie das nächste VCL-Snippet:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

In der `sitemap`-Admin-Konfiguration müssen Sie den Speicherort der Datei mithilfe von `pub/media/` anstelle von `/` angeben.

### Konfigurieren der Indizierung nach Suchmaschine

Um `robots.txt` Anpassungen in der Produktionsumgebung zu aktivieren, aktivieren Sie die Indizierung durch Suchmaschinen für die Option `<environment-name>` in den Projekteinstellungen in der Cloud-Konsole:

- Legacy Cloud Console - Die URL folgt dem Muster `https://<region-id>.magento.cloud/projects/<project_id>`

  Stellen Sie die Einstellung [!UICONTROL Indexing by search engines] (Legacy-Konsole) [!UICONTROL Hide from search engines] (Adobe-Konsole) auf **Ein**.

  ![Verwenden der [!DNL Cloud Console] zum Verwalten von Umgebungen](../../assets/robots-indexing-by-search-engine.png)

- Adobe Cloud Console: Die URL folgt dem Muster ``https://console.adobecommerce.com/<username>/<project_id>``

  Deaktivieren Sie die [!UICONTROL Hide from search engines].

- Sie können auch die Magento-Cloud-CLI verwenden, um diese Einstellung zu aktualisieren:

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- Die Indizierung durch Suchmaschinen kann nur in der Produktion aktiviert werden, nicht aber in einer der niedrigeren Umgebungen.
>
>- Wenn Sie PWA Studio Auf die Zulassungsliste setzen verwenden und nicht auf Ihre konfigurierte `robots.txt` zugreifen können, fügen Sie `robots.txt` zur [Frontname](https://github.com/magento/magento2-upward-connector#front-name-allowlist) unter **Stores** > Configuration > **General** > **Web** > UPWARD PWA Configuration hinzu.

