---
title: Einrichten des OpenSearch-Service
description: Erfahren Sie, wie Sie den OpenSearch-Service für Adobe Commerce in der Cloud-Infrastruktur aktivieren.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# Einrichten des OpenSearch-Service

Der [OpenSearch](https://www.opensearch.org)-Service ist eine Open-Source-Version von Elasticsearch 7.10.2, die auf die Lizenzänderungen für Elasticsearch folgt. Siehe [OpenSource-Projekt](https://github.com/opensearch-project) in GitHub.

{{elasticsearch-support}}

OpenSearch ermöglicht es Ihnen, Daten aus beliebigen Quellen und Formaten zu suchen und in Echtzeit zu visualisieren.

- Schnelle und erweiterte Suche nach Produkten im Produktkatalog
- OpenSearch Analyzer unterstützt mehrere Sprachen
- Unterstützt Stoppwörter und Synonyme
- Die Indizierung wirkt sich erst dann auf Kunden aus, wenn der Neuindizierungsvorgang abgeschlossen ist

{{service-instruction}}

>[!TIP]
>
>Adobe empfiehlt, OpenSearch für Ihr Adobe Commerce on Cloud-Infrastrukturprojekt immer einzurichten, auch wenn Sie ein Drittanbieter-Suchwerkzeug für Ihr Adobe Commerce-Programm konfigurieren möchten. Das Einrichten von OpenSearch bietet eine Fallback-Option, wenn das Drittanbieter-Such-Tool fehlschlägt.

**So aktivieren Sie OpenSearch**:

1. Fügen Sie für Starter- und Pro-Integrationsumgebungen den `opensearch`-Service zur `.magento/services.yaml`-Datei mit der entsprechenden Version und zugewiesenem Speicherplatz in MB hinzu. In diesem Fall ist Version 2 geeignet. Die Nebenversion ist nicht erforderlich, da die Cloud-Infrastruktur die neueste Version von OpenSearch verwendet.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Bei Pro-Projekten müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um die OpenSearch-Version in der Staging- und Produktionsumgebung zu ändern.

1. Legen Sie die `relationships`-Eigenschaft in der `.magento.app.yaml` fest oder überprüfen Sie sie.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Informationen darüber, wie sich diese Änderungen auf Ihre Umgebungen auswirken, finden Sie unter [Konfigurieren von Services](services-yaml.md).

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

## OpenSearch-Software-Kompatibilität

Wenn Sie Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt installieren oder aktualisieren, überprüfen Sie immer die Kompatibilität zwischen der Version des OpenSearch-Service und dem [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php)-Client für Adobe Commerce.

- **Erstmaliges Setup**-Bestätigen Sie, dass die in der `services.yaml` angegebene OpenSearch-Version mit dem für Adobe Commerce konfigurierten OpenSearch PHP-Client kompatibel ist.

- **Projekt-Upgrade**-Überprüfen Sie, ob der OpenSearch PHP-Client in der neuen Anwendungsversion mit der in der Cloud-Infrastruktur installierten OpenSearch-Service-Version kompatibel ist.

Die Unterstützung der Service-Version und -Kompatibilität hängt von den Versionen ab, die in der Cloud-Infrastruktur getestet und bereitgestellt werden, und unterscheidet sich manchmal von den Versionen, die von Adobe Commerce On-Premise-Bereitstellungen unterstützt werden. Siehe [Systemanforderungen](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=de) im _Installationshandbuch_ für eine Liste der unterstützten Versionen.

**So überprüfen Sie die Kompatibilität der OpenSearch-Software**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. OpenSearch-Details für die aktive Umgebung anzeigen.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. Alternativ können Sie SSH verwenden, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Rufen Sie die Verbindungsdetails des OpenSearch-Service ab.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Suchen Sie in der Antwort die IP-Adresse und den Port für den OpenSearch-Service-Endpunkt:

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Rufen Sie die installierte OpenSearch-Service-`version:number` vom Service-Endpunkt ab.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Starten Sie den OpenSearch-Service neu.

Wenn Sie den OpenSearch-Service neu starten müssen, wenden Sie sich an den Adobe Commerce-Support.

## Zusätzliche Suchkonfiguration

- Standardmäßig wird die Suchkonfiguration für Cloud-Umgebungen bei jeder Bereitstellung neu generiert. Sie können die Variable `SEARCH_CONFIGURATION`-Bereitstellung verwenden, um benutzerdefinierte Sucheinstellungen zwischen Bereitstellungen beizubehalten. Siehe [Bereitstellen von Variablen](../environment/variables-deploy.md#search_configuration).

- Nachdem Sie den OpenSearch-Service für Ihr Projekt eingerichtet haben, verwenden Sie die Admin-Benutzeroberfläche, um die OpenSearch-Verbindung zu testen und die OpenSearch-Einstellungen für Adobe Commerce anzupassen.

### Plug-ins für OpenSearch hinzufügen

Optional können Sie Plug-ins für OpenSearch hinzufügen, indem Sie den `configuration:plugins` Abschnitt zum OpenSearch-Service in der `.magento/services.yaml`-Datei hinzufügen. Beispielsweise aktiviert der folgende Code die Plug-ins für die ICU-Analyse und die phonetische Analyse .

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Siehe das [OpenSearch-Projekt](https://github.com/opensearch-project) für weitere Informationen zu Plug-ins.

### Entfernen von Plug-ins für OpenSearch

Wenn Sie die Plug-in-Einträge aus dem Abschnitt `opensearch:` der `.magento/services.yaml`-Datei entfernen **wird der Service nicht** oder deaktiviert. Um den Dienst vollständig zu deaktivieren, müssen Sie Ihre OpenSearch-Daten neu indizieren, nachdem Sie die Plug-ins aus Ihrer `.magento/services.yaml`-Datei entfernt haben. Dieses Design verhindert den möglichen Verlust oder die Beschädigung von Daten, die von diesen Plug-ins abhängt.

**So entfernen Sie OpenSearch-Plug-ins**:

1. Entfernen Sie die OpenSearch-Plug-in-Einträge aus Ihrer `.magento/services.yaml`.
1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
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
