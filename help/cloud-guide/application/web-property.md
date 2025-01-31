---
title: Web-Eigenschaft
description: Siehe Beispiele zum Konfigurieren der Web-Eigenschaft in der Konfigurationsdatei  [!DNL Commerce] .application.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Web-Eigenschaft

Die `web`-Eigenschaft definiert, wie Ihre Anwendung im Web (in HTTP) verfügbar gemacht wird, bestimmt, wie die Webanwendung Inhalte bereitstellt, und steuert, wie der Anwendungs-Container auf eingehende Anfragen reagiert, indem Regeln für jeden Speicherort (_)_. Ein Block stellt einen absoluten Pfad dar, der mit einem Schrägstrich (`/`) beginnt.

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

Sie können Ihre `locations` mithilfe der folgenden Schlüsselwerte für jeden `locations` anpassen:

| Attribut | Beschreibung |
| :--- | :--- |
| `allow` | Bereitstellung von Dateien, die nicht den „Regeln“ entsprechen. Standardwert = `true` |
| `expires` | Legen Sie die Anzahl der Sekunden fest, die Inhalte im Browser zwischengespeichert werden. Dieser Schlüssel aktiviert die `cache-control`- und `expires`-Kopfzeilen für statische Inhalte. Wenn dieser Wert nicht festgelegt ist, werden die `expires`-Anweisung und die resultierenden Kopfzeilen bei der Bereitstellung statischer Inhaltsdateien nicht einbezogen. Ein negativer 1 (`-1`)-Wert führt zu keiner Zwischenspeicherung und ist der Standardwert. Sie können Zeitwerte mit den folgenden Einheiten ausdrücken: `ms` (Millisekunden), `s` (Sekunden), `m` (Minuten), `h` (Stunden), `d` (Tage), `w` (Wochen), `M` (Monate, 30d) oder `y` (Jahre, 365d) |
| `headers` | Legen Sie benutzerdefinierte Kopfzeilen wie `X-Frame-Options` für statische Inhalte fest, die von diesem Speicherort bereitgestellt werden. |
| `index` | Listen Sie die statischen Dateien auf, die für Ihr Programm bereitgestellt werden sollen, z. B. die `index.html`. Dieser Schlüssel erwartet eine Sammlung. Dies funktioniert nur, wenn der Zugriff auf die Datei(en) durch den `allow` oder `rules` Schlüssel für diesen Speicherort „erlaubt“ wird. |
| `rules` | Überschreibungen für einen Speicherort angeben. Verwenden Sie einen regulären Ausdruck, um eine Anfrage abzugleichen. Wenn eine eingehende Anfrage mit der Regel übereinstimmt, wird die reguläre Verarbeitung der Anfrage durch die in der Regel verwendeten Schlüssel überschrieben. |
| `passthru` | Legen Sie die URL fest, die verwendet wird, falls eine statische Datei oder PHP-Datei nicht gefunden werden kann. Normalerweise ist diese URL der Frontleiter für Ihre Anwendungen, z. B. `/index.php` oder `/app.php`. |
| `root` | Legen Sie den Pfad relativ zum Stamm der Anwendung fest, die im Web verfügbar gemacht wird. Das öffentliche Verzeichnis (Speicherort &quot;/„) für ein Cloud-Projekt ist standardmäßig auf „pub“ festgelegt. |
| `scripts` | Laden von Skripten an diesem Speicherort zulassen. Legen Sie den Wert auf `true` fest, um Skripte zuzulassen. |

Die Standardkonfiguration ermöglicht Folgendes:

- Vom Stammpfad (`/`) aus kann nur auf Web und Medien zugegriffen werden
- Über die `~/pub/static`- und `~/pub/media` kann auf jede Datei zugegriffen werden

Das folgende Beispiel zeigt die Standardkonfiguration in der Datei `.magento.app.yaml` für eine Reihe von Speicherorten, auf die über das Web zugegriffen werden kann und die mit einem Eintrag in der Eigenschaft [`mounts` verknüpft sind](properties.md#mounts):

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>Dieses Beispiel zeigt die standardmäßige Web-Konfiguration für ein Cloud-Projekt, das für die Unterstützung einer einzelnen Domain konfiguriert ist. Bei einem Projekt, das die Unterstützung mehrerer Websites oder Stores erfordert, muss die `web` so eingerichtet sein, dass freigegebene Domains unterstützt werden. Siehe [Konfigurieren von Speicherorten für freigegebene Domains](../store/multiple-sites.md#configure-locations-for-shared-domains).
