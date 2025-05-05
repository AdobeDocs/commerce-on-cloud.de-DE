---
title: Cloud-spezifische Variablen
description: Anzeigen einer Liste der für Adobe Commerce spezifischen Umgebungsvariablen in der Cloud-Infrastruktur.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Cloud-spezifische Variablen

Umgebungsvariablen, die für Adobe Commerce in der Cloud-Infrastruktur spezifisch sind, verwenden das `MAGENTO_CLOUD_*`:

| Variable | Beschreibung |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | Der absolute Pfad zum Programmverzeichnis. |
| `MAGENTO_CLOUD_APPLICATION` | Ein base64-kodiertes JSON-Objekt, das die Anwendung beschreibt. Er ist dem Inhalt der `.magento.app.yaml`-Datei zugeordnet und enthält Unterschlüssel. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | Der Name des Programms, das in der `.magento.app.yaml` konfiguriert wurde. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Der absolute Pfad zum Web-Dokument-Stamm, falls zutreffend. |
| `MAGENTO_CLOUD_ENVIRONMENT` | Der Name der Umgebungsverzweigung. |
| `MAGENTO_CLOUD_PROJECT` | Die Projekt-ID. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | Ein Base64-kodiertes JSON-Objekt, das die Definition des Schlüssels (Beziehungsname) und des Werts (Arrays von Beziehungspaaren) des Endpunkts darstellt. Jede Beziehungsendpunktdefinition ist eine zerlegte Form einer URL. Es enthält eine `scheme`, eine `host`, eine `port` und _optional_ eine `username`, `password`, `path` und einige zusätzliche Informationen in `query`. |
| `MAGENTO_CLOUD_ROUTES` | Beschreiben Sie die in der Datei `.magento/routes.yaml` definierten Routen. |
| `MAGENTO_CLOUD_TREE_ID` | Die Baumstruktur-ID für die Anwendung, die der SHA der Baumstruktur in Git entspricht. |
| `MAGENTO_CLOUD_VARIABLES` | Ein base64-kodiertes JSON-Objekt mit Schlüssel-Wert-Paaren, z. B. `"key":"value"`. |
| `MAGENTO_CLOUD_LOCKS_DIR` | Stellt den Pfad zum Einhängepunkt für den Anbieter der Sperre in der Cloud-Infrastruktur bereit. Der Sperranbieter verhindert den Start doppelter Cron-Aufträge und Cron-Gruppen. |

>[!WARNING]
>
>Um Umgebungsvariablen mithilfe der [[!DNL Cloud Console]](../project/overview.md) zu [Konfigurationseinstellungen überschreiben](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=de) hinzuzufügen, müssen Sie dem Variablennamen `env:` voranstellen, wie im folgenden Beispiel:
>
>![Beispiel für eine Umgebungsvariable](../../assets/set-env-variable-ui.png)

Da sich Werte im Laufe der Zeit ändern können, ist es am besten, die Variable zur Laufzeit zu überprüfen und sie zum Konfigurieren Ihrer Anwendung zu verwenden. Verwenden Sie zum Beispiel die Variable `MAGENTO_CLOUD_RELATIONSHIPS` , um umgebungsbezogene Beziehungen wie folgt abzurufen:

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## Anzeigen von Umgebungsvariablen

Sie können den `env:config:show`-Befehl aus [dem `ece-tools` Package](../dev-tools/package-overview.md) verwenden, um eine Liste von Variablen für die aktuelle Umgebung anzuzeigen.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

Beispielausgabe für die `variables`:

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
