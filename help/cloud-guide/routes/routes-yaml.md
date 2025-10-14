---
title: Konfigurieren von Routen
description: Erfahren Sie, wie Sie die Routen für eingehende HTTPS-Anfragen für die Adobe Commerce in Cloud-Infrastrukturumgebungen definieren.
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Konfigurieren von Routen

Die `routes.yaml` im `.magento/routes.yaml` definiert Routen für Ihre Adobe Commerce in Cloud-Infrastrukturintegrations-, Staging- und Produktionsumgebungen. Routen bestimmen, wie die Anwendung eingehende HTTP- und HTTPS-Anfragen verarbeitet.

Die standardmäßige `routes.yaml` gibt die Routenvorlagen für die Verarbeitung von HTTP-Anfragen als HTTPS für Projekte an, die eine einzelne Standarddomäne haben, und für Projekte, die für mehrere Domains konfiguriert sind:

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Verwenden Sie die `magento-cloud` CLI, um eine Liste der konfigurierten Routen anzuzeigen:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Konfigurationsaktualisierungen für Pro-Umgebungen

{{pro-self-service-warning}}

## Routenvorlagen

Die `routes.yaml`-Datei ist eine Liste von Routenvorlagen und deren Konfigurationen. Sie können die folgenden Platzhalter in Routenvorlagen verwenden:

- Der `{default}` Platzhalter stellt den qualifizierten Domain-Namen dar, der als Standard für das Projekt konfiguriert ist.

  Beispiel: ein Projekt mit dem Standard-Domain-`example.com` und den folgenden Routenvorlagen:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Diese Vorlagen werden in einer Produktionsumgebung in die folgenden URLs aufgelöst:

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- Der `{all}` Platzhalter stellt alle für das Projekt konfigurierten Domain-Namen dar.

  Beispiel: ein Projekt mit `example.com` und `example1.com` Domains mit den folgenden Routenvorlagen:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Diese Vorlagen werden für alle Domains im Projekt in die folgenden Routen aufgelöst:

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  Der `{all}` Platzhalter ist für Projekte nützlich, die für mehrere Domains konfiguriert sind. In einer produktionsfremden Verzweigung wird `{all}` für jede Domain durch die Projekt-ID und die Umgebungs-ID ersetzt.

  Wenn für ein Projekt keine Domains konfiguriert sind, was während der Entwicklung häufig vorkommt, verhält sich der `{all}` Platzhalter wie der `{default}` Platzhalter.

Adobe Commerce generiert auch Routen für jede aktive Integrationsumgebung. Bei Integrationsumgebungen wird der Platzhalter `{default}` durch den folgenden Domain-Namen ersetzt:

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

Beispielsweise hat die `refactorcss` Verzweigung für das im `us` Cluster gehostete `mswy7hzcuhcjw`-Projekt die folgende Domain:

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Wenn Ihr Cloud-Projekt mehrere Stores unterstützt, befolgen Sie die Anweisungen zur Routenkonfiguration für [mehrere Websites oder Stores](../store/multiple-sites.md).

### Schrägstrich

Routendefinitionen enthalten einen Schrägstrich, der auf einen Ordner oder ein Verzeichnis hinweist. Derselbe Inhalt kann jedoch mit oder ohne Schrägstrich bereitgestellt werden. Die folgenden URLs lösen dasselbe auf, können jedoch als _zwei verschiedene_ URLs interpretiert werden:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>Es empfiehlt sich, für Ordner einen Schrägstrich als Platzhalter zu verwenden. Unabhängig von der gewählten Methode ist es jedoch wichtig, &quot;**&quot;** generieren, um zwei Speicherorte zu vermeiden.

## Routenprotokolle

Alle Umgebungen unterstützen automatisch sowohl HTTP als auch HTTPS.

- Wenn die Konfiguration nur die HTTP-Route angibt, werden HTTPS-Routen automatisch erstellt, sodass die Site sowohl von HTTP als auch von HTTPS bedient werden kann, ohne dass Umleitungen erforderlich sind.

  Beispiel: ein Projekt mit der Standard-Domain `example.com` und der folgenden Routenvorlage:

  ```text
  http://{default}/
  ```

  Diese Vorlage wird in die folgenden URLs aufgelöst:

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Wenn die Konfiguration nur die HTTPS-Route angibt, werden alle HTTP-Anfragen an HTTPS umgeleitet.

  Beispiel: Ein Projekt mit der Standarddomäne `example.com` mit der folgenden Routenvorlage:

  ```text
  https://{default}/
  ```

  Diese Vorlage wird in die folgende URL aufgelöst:

  ```text
  https://example.com/
  ```

  Außerdem wird die folgende Umleitung verarbeitet:

  `http://example.com/` ==> `https://example.com/`

Bereitstellen aller Seiten über TLS. Für diese Konfiguration müssen Sie mithilfe einer der folgenden Methoden Umleitungen für alle unverschlüsselten Anfragen an das TLS-Äquivalent konfigurieren:

- Ändern Sie in der `routes.yaml`-Datei das Protokoll in HTTPS .

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- Aktivieren Sie für Staging- und Produktionsumgebungen die Option [TLS bei Fastly erzwingen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html?lang=de) über die Admin-Benutzeroberfläche. Wenn Sie diese Option verwenden, übernimmt Fastly die Umleitung zu HTTPS, sodass Sie die `routes.yaml` nicht aktualisieren müssen.

## Routenoptionen

Konfigurieren Sie jede Route separat anhand der folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| ---------------- | ----------- |
| `type: upstream` | Bedient eine Anwendung. Außerdem verfügt es über eine `upstream`-Eigenschaft, die den Namen des Programms (wie in `.magento.app.yaml` definiert) gefolgt vom `:http`-Endpunkt angibt. |
| `type: redirect` | Leitet zu einer anderen Route um. Darauf folgt die `to`-Eigenschaft, die eine HTTP-Umleitung zu einer anderen Route darstellt, die durch ihre Vorlage identifiziert wird. |
| `cache:` | Steuert [Zwischenspeicherung für die Route](caching.md). |
| `redirects:` | Steuerelemente [Umleitungsregeln](redirects.md). |
| `ssi:` | Steuerelemente zum Aktivieren von [Server Side Includes](server-side-includes.md). |

## Einfache Routen

In den folgenden Beispielen leitet die Routenkonfiguration die Apex-Domain und die `www` Subdomain an die `mymagento` App weiter. Diese Route leitet keine HTTPS-Anfragen um.

**Beispiel 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

In diesem Beispiel folgt das Anfrage-Routing diesen Regeln:

- Der Server antwortet direkt auf Anfragen mit dem folgenden URL-Muster:

  ```text
  http://example.com/path
  ```

- Der Server gibt eine _301-Umleitung_ für Anfragen mit dem folgenden URL-Muster aus:

  ```text
  http://www.example.com/mypath
  ```

  Diese Anfragen werden an die Apex-Domain weitergeleitet, z. B.:

  ```text
  http://example.com/mypath
  ```

Im folgenden Beispiel leitet die Routenkonfiguration URLs von der Domain www nicht zur Apex-Domain um. Stattdessen werden Anfragen sowohl von der www- als auch von der apex-Domain gesendet.

**Beispiel 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Platzhalterrouten

Adobe Commerce in der Cloud-Infrastruktur unterstützt Platzhalterrouten, sodass Sie mehrere Subdomains derselben Anwendung zuordnen können. Dies funktioniert für Umleitungs- und Upstream-Routen. Der Route wird ein Sternchen (\*) vorangestellt. Beispielsweise werden die folgenden Elemente an dasselbe Programm weitergeleitet:

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Dies fungiert als Sammeldomäne in einer Live-Umgebung.

### Weiterleiten einer nicht zugeordneten Domain

Sie können ein System, das keiner Domain zugeordnet ist, über einen Punkt (`.`) weiterleiten, um die Subdomain zu trennen.

**Beispiel:**

Ein Projekt mit einer `add-theme` Verzweigung wird an die folgende URL weitergeleitet:

```text
http://add-theme-projectID.us.magento.com/
```

Wenn Sie die folgende Routenvorlage definieren:

```text
http://www.{default}/
```

Die Route wird zur folgenden URL aufgelöst:

```text
http://www.add-theme-projectID.us.magento.com/
```

Sie können eine beliebige Subdomain einfügen, bevor der Punkt und die Route aufgelöst werden.

**Beispiel:**

Definieren Sie die folgende Routenvorlage:

```text
http://*.{default}/
```

Diese Vorlage löst die beiden folgenden URLs auf:

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

Sie können das Routenmuster für nicht zugeordnete Domains anzeigen, indem Sie eine SSH-Verbindung zur -Umgebung herstellen und die `magento-cloud`-CLI zum Auflisten der Routen verwenden:

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Umleitungen und Caching

Wie unter [Umleitungen](redirects.md) ausführlicher erläutert, können Sie komplexe Umleitungsregeln wie _partielle Umleitungen_ verwalten und Regeln für routenbasierte [Caching) &#x200B;](caching.md):

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
