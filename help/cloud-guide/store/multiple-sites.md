---
title: Einrichten mehrerer Websites oder Stores
description: Erfahren Sie, wie Sie mehrere Websites oder Stores für Adobe Commerce in der Cloud-Infrastruktur konfigurieren.
feature: Cloud, Configuration, Routes, Site Navigation
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Einrichten mehrerer Websites oder Stores

Sie können Adobe Commerce so konfigurieren, dass es über mehrere Websites oder Stores verfügt, z. B. über einen englischen Store, einen französischen Store und einen deutschen Store. Siehe [Grundlegendes zu Websites, Stores und Store-Ansichten](best-practices.md#store-views).

>[!WARNING]
>
>Die Anzahl der Katalogdaten steigt mit der Anzahl der Websites und Stores. Je nach Projektarchitektur können die zusätzlichen Stores zu einem längeren Indizierungsprozess und langsameren Antwortzeiten für nicht zwischengespeicherte Katalogseiten führen. Adobe empfiehlt, die Leistung der Site genau zu überwachen.

Der Prozess zum Einrichten mehrerer Stores hängt davon ab, ob Sie eindeutige oder freigegebene Domains verwenden.

Mehrere Stores mit eindeutigen Domains:

```
https://first.store.com/
https://second.store.com/
```

Mehrere Stores mit derselben Domain:

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Um eine Store-Ansicht zur Site-Basis-URL hinzuzufügen, müssen Sie nicht mehrere Verzeichnisse erstellen. Siehe [Hinzufügen des Store-Codes zur Basis-URL](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) im _Konfigurationshandbuch_.

## Hinzufügen von Domains

Benutzerdefinierte Domains können zu Pro Staging und einer beliebigen Produktionsumgebung hinzugefügt werden. Sie können nicht zu Integrationsumgebungen hinzugefügt werden.

Der Prozess zum Hinzufügen einer Domain hängt vom Typ des Cloud-Kontos ab:

- Für Pro-Staging und Produktion

  Fügen Sie die neue Domain zu Fastly hinzu, siehe [Domains verwalten](../cdn/fastly-custom-cache-configuration.md#manage-domains) oder öffnen Sie ein Support-Ticket, um Hilfe anzufordern. Darüber hinaus müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um anzufordern, dass einem Cluster neue Domains hinzugefügt werden.

- Nur für die Erstproduktion

  Fügen Sie die neue Domain zu Fastly hinzu, siehe [Verwalten von Domains](../cdn/fastly-custom-cache-configuration.md#manage-domains) oder [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) um Hilfe zu erhalten. Darüber hinaus müssen Sie die neue Domain zur Registerkarte **Domains** im [!DNL Cloud Console] hinzufügen: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Konfigurieren der lokalen Installation

Informationen zum Konfigurieren Ihrer lokalen Installation für die Verwendung mehrerer Stores finden Sie [Mehrere Websites oder Stores][config-multiweb] im _Konfigurationshandbuch_.

Nachdem Sie die lokale Installation erfolgreich erstellt und getestet haben, um mehrere Stores zu verwenden, müssen Sie Ihre Integrationsumgebung vorbereiten:

1. **Routen oder Standorte konfigurieren** - Geben Sie an, wie eingehende URLs von Adobe Commerce verarbeitet werden

   - [Routen für separate Domains](#configure-routes-for-separate-domains)
   - [Speicherorte für freigegebene Domains](#configure-locations-for-shared-domains)

1. **Einrichten von Websites, Stores und Store-Ansichten** - Konfigurieren mithilfe der Admin-Benutzeroberfläche von Adobe Commerce
1. **Variablen ändern** - Geben Sie die Werte der `MAGE_RUN_TYPE` und `MAGE_RUN_CODE` Variablen in der `magento-vars.php` an
1. **Bereitstellungs- und Testumgebungen**: Bereitstellen und Testen der `integration`

>[!TIP]
>
>Sie können eine lokale Umgebung verwenden, um mehrere Websites oder Stores einzurichten. Weitere Informationen finden Sie in der Anleitung zum Cloud [-Docker unter „Einrichten mehrerer Websites oder Stores](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/).

### Konfigurationsaktualisierungen für Pro-Umgebungen

{{pro-self-service-warning}}

### Konfigurieren von Routen für separate Domains

Routen definieren, wie eingehende URLs verarbeitet werden. Für mehrere Stores mit eindeutigen Domains müssen Sie jede Domain in der `routes.yaml` definieren. Die Art und Weise, wie Sie Routen konfigurieren, hängt davon ab, wie Ihre Site funktionieren soll.

**So konfigurieren Sie Routen in einer Integrationsumgebung**:

1. Öffnen Sie auf Ihrer lokalen Workstation die `.magento/routes.yaml` in einem Texteditor.

1. Definieren Sie die Domain und Subdomains. Der `mymagento` Upstream-Wert ist derselbe Wert wie die name-Eigenschaft in der `.magento.app.yaml`.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Speichern Sie Ihre Änderungen in der `routes.yaml`.

1. Fahren Sie fort [Einrichten von Websites, Stores und Store-Ansichten](#set-up-websites-stores-and-store-views).

### Konfigurieren von Speicherorten für freigegebene Domains

Wo die Routen-Konfiguration definiert, wie die URLs verarbeitet werden, definiert die `web`-Eigenschaft in der `.magento.app.yaml`-Datei, wie Ihre Anwendung für das Web verfügbar gemacht wird. Web _Speicherorte_ ermöglichen eine höhere Granularität für eingehende Anfragen. Wenn Ihre Domain beispielsweise `store.com` ist, können Sie `/first` (Standardwebsite) und `/second` für Anfragen an zwei verschiedene Stores verwenden, die eine Domain gemeinsam nutzen.

**So konfigurieren Sie einen neuen Web-Speicherort**:

1. Erstellen Sie einen Alias für den Stamm (`/`). In diesem Beispiel wird der Alias in Zeile 3 `&app`.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Erstellen Sie einen Pass-Through für die Website (`/website`) und verweisen Sie mit dem Alias aus dem vorherigen Schritt auf den Stamm.

   Der Alias ermöglicht `website` den Zugriff auf Werte aus dem Stammverzeichnis. In diesem Beispiel befindet sich die Website `passthru` in Zeile 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**So konfigurieren Sie einen Speicherort mit einem anderen Verzeichnis**:

1. Erstellen Sie einen Alias für den Stamm (`/`) und die statischen (`/static`) Speicherorte.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Erstellen Sie ein Unterverzeichnis für die Website unter dem `pub`: `pub/<website>`

1. Kopieren Sie die `pub/index.php` in das `pub/<website>` Verzeichnis und aktualisieren Sie den `bootstrap` (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Erstellen Sie einen Pass-Through für die `index.php`.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Übertragen Sie die geänderten Dateien und übertragen Sie sie.

   - `pub/<website>/index.php` (Wenn diese Datei `.gitignore` ist, erfordert die Push-Benachrichtigung möglicherweise die Option „Erzwingen“.)
   - `.magento.app.yaml`

### Einrichten von Websites, Stores und Store-Ansichten

Richten Sie in der _Admin_-Benutzeroberfläche Ihre Adobe Commerce-**Websites**, **Stores** und **Store-Ansichten** ein. Siehe [Einrichten mehrerer Websites, Stores und Store-Ansichten in der Admin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) im _Konfigurationshandbuch_.

Es ist wichtig, denselben Namen und Code Ihrer Websites, Stores und Store-Ansichten von Ihrem Administrator zu verwenden, wenn Sie Ihre lokale Installation einrichten. Sie benötigen diese Werte, wenn Sie die `magento-vars.php` aktualisieren.

### Variablen ändern

Anstatt einen virtuellen NGINX-Host zu konfigurieren, übergeben Sie die Variablen `MAGE_RUN_CODE` und `MAGE_RUN_TYPE` mit der `magento-vars.php`-Datei in Ihrem Projektstammverzeichnis.

**So übergeben Sie Variablen mithilfe der `magento-vars.php`-Datei**:

1. Öffnen Sie die `magento-vars.php` in einem Texteditor.

   Die [standardmäßige `magento-vars.php`-](https://github.com/magento/magento-cloud/blob/master/magento-vars.php)) sollte wie folgt aussehen:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Verschieben Sie den kommentierten `if` Block so, dass er _hinter_ dem `function` Block steht und nicht mehr kommentiert ist.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Ersetzen Sie die folgenden Werte im `if (isHttpHost("example.com"))`:
   - `example.com` - mit der Basis-URL Ihrer _Website_
   - `default` - mit dem eindeutigen CODE für Ihre _Website_ oder _Store-Ansicht_
   - `store` - mit einem der folgenden Werte:
      - `website` - Laden Sie die _Website_ in die Storefront
      - `store` - Laden einer _Store-Ansicht_ in die Storefront

   Für mehrere Sites mit eindeutigen Domains:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Bei mehreren Sites mit derselben Domain müssen Sie den _Host_ und den _URI_ überprüfen:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Speichern Sie Ihre Änderungen in der `magento-vars.php`.

### Bereitstellen und Testen auf dem Integrations-Server

Übertragen Sie Ihre Änderungen in Ihre Adobe Commerce in die Cloud-Infrastrukturintegrationsumgebung und testen Sie Ihre Site.

1. Code-Änderungen zur Remote-Verzweigung hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Warten Sie, bis die Bereitstellung abgeschlossen ist.

1. Öffnen Sie nach der Bereitstellung Ihre Store-URL in einem Webbrowser.

   Bei einer eindeutigen Domain verwenden Sie das Format: `http://<magento-run-code>.<site-URL>`

   Beispiel: `http://french.master-name-projectID.us.magentosite.cloud/`

   Verwenden Sie bei einer freigegebenen Domain das Format `http://<site-URL>/<magento-run-code>`

   Beispiel: `http://master-name-projectID.us.magentosite.cloud/french/`

1. Testen Sie Ihre Site gründlich und führen Sie den Code zur weiteren Bereitstellung mit der `integration` Verzweigung zusammen.

## Bereitstellung für Staging und Produktion

Befolgen Sie den Bereitstellungsprozess für [Bereitstellung in Staging und Produktion](../deploy/staging-production.md). Für Starter- und Pro-Umgebungen verwenden Sie die [!DNL Cloud Console], um Code über Umgebungen hinweg zu pushen.

Adobe empfiehlt, alles in der Staging-Umgebung zu testen, bevor es in die Produktionsumgebung verschoben wird. Nehmen Sie Code-Änderungen in der Integrationsumgebung vor und starten Sie den Prozess zur Bereitstellung in allen Umgebungen erneut.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
