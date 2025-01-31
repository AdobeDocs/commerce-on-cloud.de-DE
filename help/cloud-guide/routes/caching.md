---
title: Caching
description: Erfahren Sie, wie Sie das Caching für Ihre Adobe Commerce in Cloud-Infrastrukturumgebungen aktivieren.
feature: Cloud, Cache, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '378'
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

Im vorherigen Beispiel werden die folgenden Routen zwischengespeichert:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

Die folgenden Routen werden **zwischengespeichert**:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Reguläre Ausdrücke in Routen werden **nicht** unterstützt.

## Aufbewahrungsfrist im Cache

Die Aufbewahrungsfrist im Cache wird durch den Wert der `Cache-Control`-Antwort-Kopfzeile bestimmt. Wenn in der Antwort keine `Cache-Control`-Kopfzeile enthalten ist, wird der `default_ttl` verwendet.

## Cache-Schlüssel

Um zu entscheiden, wie eine Antwort zwischengespeichert werden soll, erstellt Adobe Commerce einen Cache-Schlüssel, der von mehreren Faktoren abhängt, und speichert die mit diesem Schlüssel verknüpfte Antwort. Wenn eine Anfrage denselben Cache-Schlüssel enthält, wird die Antwort wiederverwendet. Sein Zweck ähnelt dem des HTTP-[`Vary`-Headers](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

Mit den Parametern `headers` und `cookies` können Sie diesen Cache-Schlüssel ändern.

Der Standardwert für diese Schlüssel lautet:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Cache-Attribute

### `enabled`

Wenn auf `true` gesetzt, aktivieren Sie den Cache für diese Route. Wenn auf `false` gesetzt, deaktivieren Sie den Cache für diese Route.

### `headers`

Definiert, von welchen Werten der Cache-Schlüssel abhängen muss.

Wenn der `headers` beispielsweise der folgende ist:

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
>Sie können keine Platzhalter im Cookie-Namen verwenden. Verwenden Sie entweder einen präzisen Cookie-Namen oder stimmen Sie alle Cookies mit einem Sternchen (`*`) überein. Beispielsweise sind `SESS*` oder `~SESS` derzeit **ungültige** Werte.

Cookies haben die folgenden Einschränkungen:

- Sie können maximal **50 Cookies** im System setzen. Andernfalls löst die Anwendung eine `Unable to send the cookie. Maximum number of cookies would be exceeded` Ausnahme aus.
- Die maximale Cookie-Größe beträgt **4096 Byte**. Andernfalls löst die Anwendung eine `Unable to send the cookie. Size of '%name' is %size bytes` Ausnahme aus.

### `default_ttl`

Wenn die Antwort keine `Cache-Control`-Kopfzeile hat, wird der `default_ttl` verwendet, um die Aufbewahrungsfrist im Cache in Sekunden zu definieren. Der Standardwert ist `0`, was bedeutet, dass nichts zwischengespeichert wird.
