---
title: Redirects
description: Erfahren Sie, wie Sie Umleitungsregeln für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt verwalten.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Redirects

Die Verwaltung von Weiterleitungsregeln ist eine gängige Anforderung für Web-Anwendungen, insbesondere in Fällen, in denen Sie eingehende Links, die sich geändert oder im Laufe der Zeit entfernt haben, nicht verlieren möchten.

Im Folgenden wird gezeigt, wie Sie mit der `routes.yaml`-Konfigurationsdatei Umleitungsregeln für Ihre Adobe Commerce in Cloud-Infrastrukturprojekten verwalten. Wenn die in diesem Thema beschriebenen Weiterleitungsmethoden bei Ihnen nicht funktionieren, können Sie Caching-Kopfzeilen verwenden, um dasselbe zu tun.

{{route-placeholder}}

## Updates für Pro-Umgebungen

{{pro-self-service-warning}}

>[!WARNING]
>
>Bei Adobe Commerce in Cloud-Infrastrukturprojekten kann die Konfiguration zahlreicher Nicht-Regex-Umleitungen und -Umschreibungen in der `routes.yaml` Leistungsprobleme verursachen. Wenn Ihre `routes.yaml`-Datei 32 KB oder größer ist, leiten Sie Ihre Nicht-Regex-Dateien weiter und schreiben sie auf Fastly um. Siehe [Nicht-Regex-Umleitungen an Fastly statt Nginx (Routen) &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html?lang=de) _Adobe Commerce-Hilfezentrum_.

## Umleitungen für ganze Routen

Mithilfe von Umleitungen für ganze Routen können Sie einfache Routen mithilfe der `routes.yaml` definieren. Sie können beispielsweise wie folgt von einer Apex-Domain zu einer `www`-Subdomain umleiten:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Teilweise weiterzuleitende Routen

In der `.magento/routes.yaml`-Datei können Sie basierend auf Mustervergleich partielle Umleitungsregeln zu vorhandenen Routen hinzufügen:

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

Teilweise Weiterleitungen funktionieren auf allen Routen, einschließlich Routen, die direkt von der Anwendung bedient werden.

Zwei Schlüssel sind unter `redirects` verfügbar:

- **expires** - Optional, gibt die Zeitdauer an, für die die Umleitung im Browser zwischengespeichert wird. Beispiele für gültige Werte sind `3600s`, `1d`, `2w` und `3m`.

- **Pfade** - Ein oder mehrere Schlüssel-Wert-Paare, die die Konfiguration für Teilumleitungsregeln angeben.

  Für jede Umleitungsregel ist der Schlüssel ein Ausdruck zum Filtern von Anfragepfaden für die Umleitung. Der Wert ist ein -Objekt, das das Zielziel für die Umleitung und Optionen für die Verarbeitung der Umleitung angibt.

  Das value-Objekt hat die folgenden Eigenschaften:

  | Eigenschaft | Beschreibung |
  | ---------- | ----------- |
  | `to` | Erforderlich, ein partieller absoluter Pfad, eine URL mit Protokoll und Host oder ein Muster, das das Zielziel für die Umleitungsregel angibt. |
  | `regexp` | Optional, standardmäßig `false`. Gibt an, ob der Pfadschlüssel als regulärer PCRE-Ausdruck interpretiert werden soll. |
  | `prefix` | Gibt an, ob die Umleitung sowohl für den Pfad als auch für alle untergeordneten Elemente oder nur für den Pfad selbst gilt. Die Standardeinstellung ist `true`. Dieser Wert wird nicht unterstützt, wenn `regexp` `true` ist. |
  | `append_suffix` | Bestimmt, ob das Suffix mit der Umleitung übernommen wird. Die Standardeinstellung ist `true`. Dieser Wert wird nicht unterstützt, wenn der `regexp` Schlüssel `true` oder* ist, wenn der `prefix` Schlüssel `false` ist. |
  | `code` | Gibt den HTTP-Status-Code an. Gültige Status-Codes sind [`301` (dauerhaft verschoben](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8) und [`308`](https://www.rfc-editor.org/rfc/rfc7238). Die Standardeinstellung ist `302`. |
  | `expires` | Optional, gibt die Zeitdauer an, während der die Umleitung im Browser zwischengespeichert wird. Standardmäßig wird der `expires` Wert verwendet, der direkt unter dem `redirects` definiert wird. Auf dieser Ebene können Sie jedoch die Gültigkeitsdauer des Caches für einzelne partielle Weiterleitungen optimieren. |

## Beispiele für partielle Routen-Umleitungen

Die folgenden Beispiele zeigen, wie Sie mithilfe verschiedener `paths`-Konfigurationsoptionen partielle Weiterleitungen in der `routes.yaml`-Datei angeben.

### Mustervergleich für reguläre Ausdrücke

Verwenden Sie das folgende Format, um Umleitungsanfragen basierend auf einem regulären Ausdruck zu konfigurieren.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Diese Konfiguration filtert Anfragepfade anhand eines regulären Ausdrucks und leitet übereinstimmende Anfragen an `https://example.com` weiter. Beispielsweise wird eine Anfrage an `https://example.com/regexp/a/b/c/match` an `https://example.com/a/b/c` umgeleitet.

### Präfixmusterübereinstimmung

Verwenden Sie das folgende Format, um Umleitungsanfragen für Pfade zu konfigurieren, die mit einem angegebenen Präfixmuster beginnen.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Diese Konfiguration funktioniert wie folgt:

- Leitet Anfragen, die dem `/from` Muster entsprechen, an den `http://{default}/to` weiter.

- Leitet Anfragen, die dem `/from/another/path` Muster entsprechen, an `https://{default}/to/another/path` weiter.

- Wenn Sie die `prefix`-Eigenschaft in `false` ändern, werden Anfragen, die mit dem `/from` Muster-Trigger übereinstimmen, umgeleitet, Anfragen, die mit dem `/from/another/path`-Muster übereinstimmen, jedoch nicht.

### Suffix-Mustervergleich

Verwenden Sie das folgende Format, um Umleitungsanfragen zu konfigurieren, die das Pfadsuffix von der Anfrage an das Ziel anhängen:

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Diese Konfiguration funktioniert wie folgt:

- Leitet Anfragen, die dem `/from/path/suffix` Muster entsprechen, an den `https://{default}/to` weiter.

- Wenn Sie die `append_suffix` Eigenschaft in `true` ändern, werden Anfragen, die mit übereinstimmen, `/from/path/suffix` an den Pfad `https://{default}/to/path/suffix`.

### Pfadspezifische Cache-Konfiguration

Verwenden Sie das folgende Format, um die Zeit zum Zwischenspeichern einer Umleitung von einem bestimmten Pfad aus anzupassen:

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Diese Konfiguration funktioniert wie folgt:

- Umleitungen vom ersten Pfad (`/from`) werden einen Tag lang zwischengespeichert.

- Umleitungen vom zweiten Pfad (`/here`) werden zwei Wochen lang zwischengespeichert.
