---
title: Server-seitige Includes
description: Erfahren Sie, wie Sie Server-seitige Includes mit Adobe Commerce in der Cloud-Infrastruktur verwenden.
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# Server-seitige Includes

[Server-seitige Includes](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) sind Anweisungen in HTML-Seiten, die auf dem Server ausgewertet werden, während die Seiten gerendert werden. Mit SSI können Sie dynamisch generierte Inhalte zu einer bestehenden HTML-Seite hinzufügen, ohne die gesamte Seite zu bedienen.

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

Mit SSI können Sie in Ihre HTML-Antwort-Anweisungen einfügen, die bewirken, dass der Server Teile der HTML ausfüllt und dabei alle vorhandenen (Caching[Konfigurationen ](caching.md).

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
