---
title: Projektstruktur
description: Erfahren Sie mehr über die Dateistruktur und Projektvorlagen für Adobe Commerce in Cloud-Infrastrukturen.
exl-id: 364e40e4-a5b3-4d23-b86d-74fc0696ac19
TQID: https://experienceleague.adobe.com/B6fTvmHLFa5THSgLKsjl1smPC8ekPdXB9A-vyqFVwG8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 0%

---

# Projektstruktur

Ein Adobe Commerce on Cloud Infrastructure-Projekt enthält wichtige Dateien für die Anmeldeinformationen und die Anwendungskonfiguration. Diese Dateien sind in als Vorlage entsprechend der Adobe Commerce-Version verfügbar. Weitere Informationen finden Sie unter Cloud-Vorlagen basierend auf der Adobe Commerce-Version im [`magento/magento-cloud` GitHub-Repository](https://github.com/magento/magento-cloud).

In der folgenden Tabelle werden die in einem Cloud-Projekt enthaltenen Dateien beschrieben:

| Datei | Beschreibung |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Konfigurationsdatei, die `www` zur Apex-Domain und `php` Anwendung umleitet, um HTTP bereitzustellen. Siehe [Konfigurieren von Routen](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Eine Konfigurationsdatei, die eine MySQL-Instanz (MariaDB), Redis und OpenSearch oder Elasticsearch definiert. Siehe [Konfigurieren von Services](../services/services-yaml.md). |
| `/app` | Der `code` Ordner wird für benutzerdefinierte Module verwendet. Der Ordner `design` wird für [benutzerdefinierte Designs“ ](../store/custom-theme.md). Der Ordner `etc` enthält Konfigurationsdateien für das Programm. |
| `/m2-hotfixes` | Wird für benutzerdefinierte Patches verwendet. |
| `/update` | Ein vom Support-Modul verwendeter Service-Ordner. |
| `.gitignore` | Geben Sie an, welche Dateien und Verzeichnisse ignoriert werden sollen. Siehe [`.gitignore` Referenz](#ignoring-files). |
| `.magento.app.yaml` | Eine Konfigurationsdatei, die die Eigenschaften zum Erstellen der Anwendung definiert. Siehe [Konfigurieren einer Anwendung](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Konfigurationsdatei für die Build-, Bereitstellungs- und Nachbereitstellungsphasen. Das `ece-tools`-Paket enthält ein Beispiel für diese Datei. Siehe [Konfigurieren von ](../environment/configure-env-yaml.md). |
| `composer.json` | Ruft Adobe Commerce und die Konfigurationsskripte zur Vorbereitung der Anwendung ab. Siehe [Erforderliche Pakete](../development/overview.md#required-packages). |
| `composer.lock` | Speichert Versionsabhängigkeiten für jedes Paket. Siehe [Erforderliche Pakete](../development/overview.md#required-packages). |
| `magento-vars.php` | Wird zum Definieren von [mehreren Stores](../store/multiple-sites.md) und Sites mithilfe von Variablen verwendet. |

{style="table-layout:auto"}

>[!NOTE]
>
>Wenn Sie Ihre lokalen Änderungen an den Remoteserver pushen, verwendet das Bereitstellungsskript die von den Konfigurationsdateien im Verzeichnis `.magento` definierten Werte. Anschließend löscht das Skript das Verzeichnis und seinen Inhalt. Ihre lokale Entwicklungsumgebung ist davon nicht betroffen.

## Anwendungsstammverzeichnis

Der Speicherort des Anwendungsstammverzeichnisses hängt von der Umgebung ab.

- **Starter- und Pro-Integration**: `/app`
- **Starterproduktion**: `/<project-ID>`
- **Pro Staging**: `/<project-ID>_stg`
- **Pro Production**: `/<project-ID>`

### Beschreibbare Verzeichnisse

Die Remote-Integrations-, Staging- und Produktionsumgebungen sind schreibgeschützt. Die folgenden Verzeichnisse sind aus *die* Ordner:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>In Produktions- und Staging-Umgebungen verfügt jeder Knoten im Cluster mit drei Knoten über ein `/tmp`, das nicht mit den anderen Knoten geteilt wird.

## Dateien ignorieren

Es gibt eine `.gitignore`-Basisdatei mit dem Projekt-Repository von Adobe Commerce in der Cloud-Infrastruktur. Sehen Sie sich die neueste [.gitignore-Datei im Magento-Cloud-Repository ](https://github.com/magento/magento-cloud/blob/master/.gitignore). Um eine Datei hinzuzufügen, die sich in der `.gitignore` befindet, können Sie die Option `-f` (force) beim Staging eines Commits verwenden:

```bash
git add <path/filename> -f
```

## Basisvorlage ändern

Sie können die folgenden Schritte verwenden, um die Struktur eines vorhandenen Projekts zu ändern und so die neueste Basisvorlage für Adobe Commerce in der Cloud-Infrastruktur zu verwenden.

1. Klonen Sie das Projekt auf einer lokalen Workstation.

1. Aktualisieren Sie die `composer.json` Datei mit den folgenden Werten für den Abschnitt `extra` .

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Fügen Sie die für die Basisvorlage entworfene `.gitignore` hinzu. Wenn Sie beispielsweise die `.gitignore` für die Vorlage Version 2.2.6 benötigen, verwenden Sie die Datei [.gitignore für ](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore).2.6. als Referenz.

1. Löschen Sie den Git-Cache.

   ```bash
   git rm -r --cached .
   ```

1. Änderungen hinzufügen und bestätigen.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
