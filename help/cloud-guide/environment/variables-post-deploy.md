---
title: Variablen nach der Bereitstellung
description: Siehe die Liste der Umgebungsvariablen, die Aktionen in der Phase nach der Bereitstellung in der Adobe Commerce in der Cloud-Infrastruktur steuern.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variablen nach der Bereitstellung

Die folgenden _nach der Bereitstellung_ Variablen steuern Aktionen in der Phase nach der Bereitstellung und können Werte von den „globalen Variablen[&#x200B; erben und &#x200B;](variables-global.md). Fügen Sie diese Variablen in den `post-deploy` Schritt der `.magento.env.yaml` ein:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Weitere Informationen zum Anpassen des Build- und Bereitstellungsprozesses finden Sie unter:

- [Bereitstellungskonfiguration](configure-env-yaml.md)
- [Bereitstellungsprozess](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Default**— `[]` (ein leeres Array)
- **Version**—Adobe Commerce 2.1.4 und höher

Konfigurieren Sie _Time To First Byte_ (TTFB)-Tests für bestimmte Seiten, um die Leistung Ihrer Site zu testen. Geben Sie für jede Seite, für die der Test erforderlich ist, einen absoluten Pfadverweis oder eine URL mit Protokoll und Host an.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Nachdem Sie die Seiten zum Testen und Übertragen Ihrer Änderungen angegeben haben, wird der _Time To First Byte_-Test während der Phase nach der Bereitstellung ausgeführt und sendet die Ergebnisse für jeden Pfad in das Cloud-Protokoll:

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Bei umgeleiteten Pfaden wird im Protokoll der Pfad des Umleitungsziels anstelle des in der Umgebungsvariablen konfigurierten Pfads angegeben. Wenn Sie einen ungültigen Pfad angeben, zeigt das Protokoll eine Warnmeldung an.

## `WARM_UP_CONCURRENCY`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Geben Sie das Limit der gleichzeitigen Anforderungen an, die während Cache-Aufwärmvorgängen gesendet werden sollen, um die Server-Last zu reduzieren. Dieser Wert begrenzt die Anzahl paralleler Verbindungen und ist für Umgebungskonfigurationen nützlich, bei denen die Variable `WARM_UP_PAGES` nach der Bereitstellung mehrere Seiten für das Vorausfüllen des Cache angibt.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Standard**— `index.php`
- **Version**—Adobe Commerce 2.1.4 und höher

Passen Sie die Liste der Seiten an, die zum Vorausfüllen des Cache im `post_deploy` verwendet werden. Sie müssen den Hook nach der Bereitstellung konfigurieren. Siehe den [Hooks](../application/hooks-property.md) der `.magento.app.yaml`.

- **Einzelseiten** - Geben Sie eine einzelne Seite an, die dem Cache hinzugefügt werden soll. Sie müssen die Standard-Basis-URL nicht angeben. Im folgenden Beispiel wird die `BASE_URL/index.php` zwischengespeichert:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **mehrere Domains** - Listen Sie mehrere URLs auf. Im folgenden Beispiel werden Seiten aus zwei Domains zwischengespeichert:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **Mehrere Seiten** - Verwenden Sie das folgende Format, um mehrere Seiten gemäß einem bestimmten Muster für reguläre Ausdrücke zwischenzuspeichern:

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: Mögliche Varianten `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: Filtern Sie die URLs mit einem `regexp` oder einem `url` mit exakter Übereinstimmung oder verwenden Sie ein Sternchen (\*) für alle Seiten. Produkt-SKU für den Entitätstyp des `product` verwenden
   - `store_id|store_code`: Verwenden Sie die ID oder den Code des Stores oder ein Sternchen (\*) für alle Stores, können Sie mehrere Store-IDs oder Codes getrennt mit `|` übergeben

  Im folgenden Beispiel werden basierend auf diesen Kriterien `category` und `cms-page` Entitätstypen zwischengespeichert:
   - Alle Kategorieseiten für Store mit ID `1`
   - Alle Kategorieseiten für Stores mit Code `store1` und `store2`
   - Kategorieseiten-`cars` für Store mit Code-`store_en`
   - CMS-`contact` für alle Stores
   - cms-`contact` für Stores mit der ID `1` und `2`
   - Jede Kategorieseite, die `car_` enthält und mit `html` für den Store mit der ID 2 endet
   - Jede Kategorieseite, die `tires_` für das Speichern mit Code `store_gb` enthält

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  Im folgenden Beispiel wird für den Entitätstyp &quot;`product`&quot; basierend auf diesen Kriterien zwischengespeichert:
   - Alle Produkte für alle Stores (programmgesteuert auf 100 pro Store begrenzt, um Leistungsprobleme zu vermeiden)
   - Alle Produkte für Store `store1`
   - Produkte mit `sku1` für alle Geschäfte
   - Produkte mit `sku1` für Geschäfte mit Code `store1` und `store2`
   - Produkte mit `sku1`, `sku2` und `sku3` für Geschäfte mit Code `store1` und `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  Im folgenden Beispiel wird für den Entitätstyp &quot;`store-page`&quot; basierend auf diesen Kriterien zwischengespeichert:
   - `/contact-us` für alle Stores
   - `/contact-us` für Store mit ID `1`
   - `/contact-us` für Stores mit Code `code1` und `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
