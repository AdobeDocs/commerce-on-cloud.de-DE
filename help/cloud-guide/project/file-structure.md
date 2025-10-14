---
title: Projektstruktur
description: Erfahren Sie mehr über die Dateistruktur und Projektvorlagen für Adobe Commerce in Cloud-Infrastrukturen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Projektstruktur

Ein Adobe Commerce on Cloud Infrastructure-Projekt enthält wichtige Dateien für die Anmeldeinformationen und die Anwendungskonfiguration. Diese Dateien sind in als Vorlage entsprechend der Adobe Commerce-Version verfügbar. Weitere Informationen finden Sie unter Cloud-Vorlagen basierend auf der Adobe Commerce-Version im [`magento/magento-cloud` GitHub-Repository](https://github.com/magento/magento-cloud).

In der folgenden Tabelle werden die in einem Cloud-Projekt enthaltenen Dateien beschrieben:

| Datei | Beschreibung |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Konfigurationsdatei, die `www` zur Apex-Domain und `php` Anwendung umleitet, um HTTP bereitzustellen. Siehe [Konfigurieren von Routen](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Eine Konfigurationsdatei, die eine MySQL-Instanz (MariaDB), Redis und OpenSearch oder ein Elasticsearch definiert. Siehe [Konfigurieren von Services](../services/services-yaml.md). |
| `/app` | Der `code` Ordner wird für benutzerdefinierte Module verwendet. Der Ordner `design` wird für [benutzerdefinierte Designs“ &#x200B;](../store/custom-theme.md). Der Ordner `etc` enthält Konfigurationsdateien für das Programm. |
| `/m2-hotfixes` | Wird für benutzerdefinierte Patches verwendet. |
| `/update` | Ein vom Support-Modul verwendeter Service-Ordner. |
| `.gitignore` | Geben Sie an, welche Dateien und Verzeichnisse ignoriert werden sollen. Siehe [`.gitignore` Referenz](#ignoring-files). |
| `.magento.app.yaml` | Eine Konfigurationsdatei, die die Eigenschaften zum Erstellen der Anwendung definiert. Siehe [Konfigurieren einer Anwendung](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Konfigurationsdatei für die Build-, Bereitstellungs- und Nachbereitstellungsphasen. Das `ece-tools`-Paket enthält ein Beispiel für diese Datei. Siehe [Konfigurieren von &#x200B;](../environment/configure-env-yaml.md). |
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

Es gibt eine `.gitignore`-Basisdatei mit dem Projekt-Repository von Adobe Commerce in der Cloud-Infrastruktur. Sehen Sie sich die neueste [.gitignore-Datei im Magento-Cloud-Repository &#x200B;](https://github.com/magento/magento-cloud/blob/master/.gitignore). Um eine Datei hinzuzufügen, die sich in der `.gitignore` befindet, können Sie die Option `-f` (force) beim Staging eines Commits verwenden:

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

1. Fügen Sie die für die Basisvorlage entworfene `.gitignore` hinzu. Wenn Sie beispielsweise die `.gitignore` für die Vorlage Version 2.2.6 benötigen, verwenden Sie die Datei [.gitignore für &#x200B;](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore).2.6. als Referenz.

1. Löschen Sie den Git-Cache.

   ```bash
   git rm -r --cached .
   ```

1. Änderungen hinzufügen und bestätigen.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
