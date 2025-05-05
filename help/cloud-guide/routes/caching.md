---
title: Caching
description: Learn how to enable caching for your Adobe Commerce on cloud infrastructure environments.
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
source-git-commit: a1ed2818cbaf5adf8b673df0ee9b9218e6f700a2
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Caching

Sie können das Caching in Ihrer Cloud-Infrastruktur-Projektumgebung aktivieren. Wenn Sie das Caching deaktivieren, stellt Adobe Commerce die Dateien direkt bereit.

{{route-placeholder}}

## Einrichten der Zwischenspeicherung

Aktivieren Sie das Caching für Ihre Anwendung, indem Sie die Cache-Regeln in der `.magento/routes.yaml` wie folgt konfigurieren:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Route-basiertes Caching

Aktivieren Sie die feinkörnige Zwischenspeicherung, indem Sie Zwischenspeicherungsregeln für mehrere Routen separat einrichten, wie im folgenden Beispiel gezeigt:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

The preceding example caches the following routes:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

And the following routes are **not** cached:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Regular expressions in routes are **not** supported.

## Cache duration

Die Aufbewahrungsfrist im Cache wird durch den Wert der `Cache-Control`-Antwort-Kopfzeile bestimmt. Wenn in der Antwort keine `Cache-Control`-Kopfzeile enthalten ist, wird der `default_ttl` verwendet.

## Cache-Schlüssel

Um zu entscheiden, wie eine Antwort zwischengespeichert werden soll, erstellt Adobe Commerce einen Cache-Schlüssel, der von mehreren Faktoren abhängt, und speichert die mit diesem Schlüssel verknüpfte Antwort. Wenn eine Anfrage denselben Cache-Schlüssel enthält, wird die Antwort wiederverwendet. Sein Zweck ähnelt dem des HTTP-[`Vary`-Headers](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

Mit den Parametern `headers` und `cookies` können Sie diesen Cache-Schlüssel ändern.

The default value for these keys follows:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Cache attributes

### `enabled`

When set to `true`, enable the cache for this route. When set to `false`, disable the cache for this route.

### `headers`

Defines on which values the cache key must depend.

For example, if the `headers` key is the following:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Dann speichert Adobe Commerce für jeden Wert der `Accept`-HTTP-Kopfzeile eine andere Antwort zwischen.

### `cookies`

Der `cookies` Schlüssel definiert, von welchen Werten der Cache-Schlüssel abhängen muss.

Beispiel:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

Der Zwischenspeicherschlüssel hängt vom Wert des `value`-Cookies in der Anfrage ab.

Ein Sonderfall liegt vor, wenn der `cookies` den Wert `["*"]` hat. Dieser Wert bedeutet, dass jede Anfrage mit einem Cookie den Cache umgeht. Dies ist der Standardwert.

>[!NOTE]
>
>Sie können keine Platzhalter im Cookie-Namen verwenden. Use either a precise cookie name or match all cookies with an asterisk (`*`). For example, `SESS*` or `~SESS` are currently **not** valid values.

Cookies have the following restrictions:

- There is a set maximum of **50 cookies** in the system. Andernfalls löst die Anwendung eine `Unable to send the cookie. Maximum number of cookies would be exceeded` Ausnahme aus. To increase the number of cookies to 200, apply the [MDVA-12304 patch](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html?lang=de) using using the [Quality Patches Tool](https://experienceleague.adobe.com/de/docs/commerce-learn/tutorials/tools/quality-patch-tool).
- Die maximale Cookie-Größe beträgt **4096 Byte**. Andernfalls löst die Anwendung eine `Unable to send the cookie. Size of '%name' is %size bytes` Ausnahme aus.

### `default_ttl`

Wenn die Antwort keine `Cache-Control`-Kopfzeile hat, wird der `default_ttl` verwendet, um die Aufbewahrungsfrist im Cache in Sekunden zu definieren. Der Standardwert ist `0`, was bedeutet, dass nichts zwischengespeichert wird.
