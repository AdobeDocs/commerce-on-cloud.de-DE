---
title: Einrichten des Elasticsearch-Service
description: Erfahren Sie, wie Sie den Elasticsearch-Service für Adobe Commerce in der Cloud-Infrastruktur aktivieren.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Einrichten des Elasticsearch-Service

[Elasticsearch](https://www.elastic.co) ist ein Open-Source-Produkt, mit dem Sie Daten aus beliebigen Quellen und Formaten in Echtzeit suchen und visualisieren können.

{{elasticsearch-support}}

Informationen zu Adobe Commerce Version 2.4.4 und höher finden Sie unter [Einrichten des OpenSearch-Service](opensearch.md).

- Elasticsearch führt schnelle und erweiterte Suchen nach Produkten im Produktkatalog durch
- Elasticsearch Analyzer unterstützen mehrere Sprachen
- Unterstützt Stoppwörter und Synonyme
- Die Indizierung wirkt sich erst dann auf Kunden aus, wenn der Neuindizierungsvorgang abgeschlossen ist

>[!TIP]
>
>Adobe empfiehlt, immer Elasticsearch für Ihr Adobe Commerce on Cloud Infrastructure-Projekt einzurichten, auch wenn Sie ein Drittanbieter-Suchwerkzeug für Ihr Adobe Commerce-Programm konfigurieren möchten. Das Einrichten von Elasticsearch bietet eine Fallback-Option für den Fall, dass das Such-Tool eines Drittanbieters fehlschlägt.

{{service-instruction}}

**So aktivieren Sie Elasticsearch**:

1. Fügen Sie für Einstiegsprojekte den `elasticsearch`-Service zur `.magento/services.yaml` hinzu, wobei die Elasticsearch-Version und der zugewiesene Speicherplatz in MB angegeben werden.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Bei Pro-Projekten müssen Sie ein Adobe Commerce-Support-Ticket einreichen, um die Elasticsearch-Version in der Staging- und Produktionsumgebung zu ändern.

1. Legen Sie die `relationships`-Eigenschaft in der `.magento.app.yaml` fest.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Informationen darüber, wie sich diese Änderungen auf Ihre Umgebungen auswirken, finden Sie unter [Services](services-yaml.md).

1. Verwenden Sie nach Abschluss des Bereitstellungsprozesses SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Indizieren Sie den Katalogsuchindex neu.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Reinigen Sie den Cache.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Kompatibilität der Elasticsearch-Software

Wenn Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt installieren oder aktualisieren, überprüfen Sie immer die Kompatibilität zwischen der Elasticsearch-Service-Version und dem [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php)-Client für Adobe Commerce.

- **Erstmaliges Setup**-Vergewissern Sie sich, dass die in der `services.yaml` angegebene Elasticsearch-Version mit dem für Adobe Commerce konfigurierten Elasticsearch PHP-Client kompatibel ist.

- **Projekt-Upgrade**-Überprüfen Sie, ob der Elasticsearch-PHP-Client in der neuen Anwendungsversion mit der Elasticsearch-Service-Version kompatibel ist, die in der Cloud-Infrastruktur installiert ist.

Die Unterstützung der Service-Version und der Kompatibilität für Adobe Commerce in der Cloud-Infrastruktur wird von den Versionen bestimmt, die in der Cloud-Infrastruktur bereitgestellt werden, und unterscheidet sich manchmal von den Versionen, die von Adobe Commerce On-Premise-Bereitstellungen unterstützt werden. Siehe [Service-Versionen](services-yaml.md#service-versions).

**So überprüfen Sie die Kompatibilität der Elasticsearch-Software**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Elasticsearch-Details für die aktive Umgebung anzeigen.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. Alternativ können Sie SSH verwenden, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Überprüfen Sie die Version des Composer-Pakets auf `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   Überprüfen Sie in der Antwort die installierte Version in der Eigenschaft `versions` .

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   Außerdem finden Sie die Elasticsearch-PHP-Client-Version in der `composer.lock`-Datei im Umgebungsstammverzeichnis.

1. Rufen Sie in der Befehlszeile die Verbindungsdetails des Elasticsearch-Services ab.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Suchen Sie in der Antwort die IP-Adresse für den Endpunkt des Elasticsearch-Services:

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Rufen Sie die installierte Elasticsearch-Service-`version:number` vom Service-Endpunkt ab.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Überprüfen Sie die Versionskompatibilität zwischen dem Elasticsearch-Service und dem PHP-Client.

   Wenn die Versionen nicht kompatibel sind, führen Sie eine der folgenden Aktualisierungen an Ihrer Umgebungskonfiguration durch:

   - Ändern Sie den Elasticsearch-PHP-Client auf eine Version, die mit der Elasticsearch-Service-Version kompatibel ist.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Ändern Sie die Version des Elasticsearch-Dienstes in der Datei `services.yaml` in eine Version, die mit dem PHP-Client von Elasticsearch kompatibel ist.

     {{pro-update-service}}

## Elasticsearch-Service neu starten

Wenn Sie den [Elasticsearch ](https://www.elastic.co)-Service neu starten müssen, wenden Sie sich an den Adobe Commerce-Support.

## Zusätzliche Suchkonfiguration

- Standardmäßig wird die Suchkonfiguration für Cloud-Umgebungen bei jeder Bereitstellung neu generiert. Sie können die Variable `SEARCH_CONFIGURATION`-Bereitstellung verwenden, um benutzerdefinierte Sucheinstellungen zwischen Bereitstellungen beizubehalten. Siehe [Bereitstellen von Variablen](../environment/variables-deploy.md#search_configuration).

- Nachdem Sie den Elasticsearch-Service für Ihr Projekt eingerichtet haben, verwenden Sie die Admin-Benutzeroberfläche, um die Elasticsearch-Verbindung zu testen und Elasticsearch-Einstellungen für Adobe Commerce anzupassen.

### Hinzufügen von Plug-ins für Elasticsearch

Optional können Sie Plug-ins für Elasticsearch hinzufügen, indem Sie den `configuration:plugins` Abschnitt zum Elasticsearch-Service in der `.magento/services.yaml` hinzufügen. Beispielsweise aktiviert der folgende Code die Plug-ins für die ICU-Analyse und die phonetische Analyse .

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Wenn Sie das Elastic Suite-Drittanbieter-Plug-in verwenden, müssen Sie [das `ece-tools`-Paket aktualisieren](../dev-tools/update-package.md) auf Version 2002.0.19 oder höher.
Fügen Sie beim Einrichten von Elastic Suite die Konfigurationseinstellungen zur `ELASTICSUITE_CONFIGURATION` Bereitstellungsvariablen hinzu. Diese Konfiguration speichert die Einstellungen in allen Bereitstellungen.

### Plug-ins für Elasticsearch entfernen

Wenn Sie die Plug-in-Einträge aus `elasticsearch:` in `.magento/services.yaml` entfernen, werden sie nicht wie erwartet deinstalliert oder deaktiviert. Sie müssen Ihre Elasticsearch-Daten neu indizieren. Dieses Verhalten ist beabsichtigt, einen möglichen Verlust oder eine Beschädigung von Daten zu verhindern, die von diesen Plug-ins abhängen.

**So entfernen Sie Elasticsearch-Plug-ins**:

1. Entfernen Sie die Elasticsearch-Plug-in-Einträge aus Ihrer `.magento/services.yaml`.
1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Übertragen Sie die `.magento/services.yaml` Änderungen in Ihr Cloud-Repository.
1. Indizieren Sie den Katalogsuchindex neu.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Reinigen Sie den Cache.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Weitere Informationen zur Verwendung oder Fehlerbehebung beim Elastic Suite-Plug-in mit Adobe Commerce finden Sie unter [Elastic Suite-Dokumentation](https://github.com/Smile-SA/elasticsuite).

## Fehlerbehebung

In den folgenden Adobe Commerce-Support-Artikeln finden Sie Hilfe bei der Fehlerbehebung bei Elasticsearch-Problemen:

- [Elasticsearch 5 ist konfiguriert, aber die Suchseite wird nicht mit dem Fehler „Felddaten sind deaktiviert…“ geladen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html?lang=de)
- [Elasticsearch in Adobe Commerce-Fehlerbehebung](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Elasticsearch-Indexstatus ist `yellow` oder `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html?lang=de)
