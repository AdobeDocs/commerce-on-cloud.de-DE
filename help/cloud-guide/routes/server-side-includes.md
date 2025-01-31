---
title: Server-seitige Includes
description: Erfahren Sie, wie Sie Server-seitige Includes mit Adobe Commerce in der Cloud-Infrastruktur verwenden.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Server-seitige Includes

[Server-seitige Includes](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) sind Anweisungen zum HTML von Seiten, die auf dem Server ausgewertet werden, während die Seiten gerendert werden. Mit SSI können Sie dynamisch generierte Inhalte zu einer bestehenden HTML-Seite hinzufügen, ohne die gesamte Seite zu bedienen.

Sie können SSI für jede Route in Ihrem `.magento/routes.yaml` aktivieren oder deaktivieren; Beispiel:

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI ermöglicht es Ihnen, in Ihre HTML-Antwort-Anweisungen einzuschließen, die bewirken, dass der Server Teile der HTML ausfüllt und dabei alle vorhandenen [Caching-Konfiguration](caching.md) berücksichtigt.

Das folgende Beispiel zeigt, wie Sie am Anfang einer Seite ein dynamisches Datums-Steuerelement und am Ende ein weiteres Datums-Steuerelement einfügen, das alle 600 Sekunden aktualisiert wird:

Fügen Sie einer Seite wie `/index.php` Folgendes hinzu:

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

Fügen Sie Folgendes zu `time.php` hinzu:

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

Navigieren Sie zu der Seite, der Sie das Steuerelement hinzugefügt haben. Aktualisieren Sie die Seite mehrmals. Beachten Sie, dass sich die Zeit oben auf der Seite ändert, die Zeit unten jedoch nur alle 600 Sekunden.
