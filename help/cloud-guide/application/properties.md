---
title: Eigenschaften
description: Verwenden Sie die Eigenschaftenliste als Verweis, wenn Sie die  [!DNL Commerce]  für den Build und die Bereitstellung in der Cloud-Infrastruktur konfigurieren.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 32bd1f64-43d6-48a3-84b7-bea22f125bb0
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Eigenschaften für die Anwendungskonfiguration

Die `.magento.app.yaml`-Datei verwendet Eigenschaften, um die Umgebungsunterstützung für das [!DNL Commerce]-Programm zu verwalten.

| -Name | Beschreibung | Standard | Erforderlich |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Benutzerrollen anpassen | — | Nein |
| [`crons`](crons-property.md) | Spezifikationen aktualisieren und Cron-Aufträge planen | — | Nein |
| [`dependencies`](#dependencies) | Aktivieren zusätzlicher Abhängigkeiten | `php:composer/composer: '2.2.4'` | Nein |
| [`disk`](#disk) | Definieren der Größe der persistenten Festplatte | `5120` | Ja |
| [`firewall`](firewall-property.md) | (Nur Starter) Steuern des ausgehenden Traffics | — | Nein |
| [`hooks`](hooks-property.md) | Anpassen von Shell-Befehlen für die Build-, Bereitstellungs- und Post-Bereitstellungsphase | — | Nein |
| [`mounts`](#mounts) | Pfade festlegen | Pfade:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | Nein |
| [`name`](#name) | Programmnamen definieren | `mymagento` | Ja |
| [`relationships`](#relationships) | Zuordnen von Diensten | Dienste:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | Nein |
| [`runtime`](#runtime) | Die Laufzeiteigenschaft enthält Erweiterungen, die für die [!DNL Commerce] erforderlich sind. | Erweiterungen:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Ja |
| [`type`](#type-and-build) | Festlegen des Bild-Basis-Containers | `php:8.3` | Ja |
| [`variables`](variables-property.md) | Anwenden einer Umgebungsvariablen auf eine bestimmte Commerce-Version | — | Nein |
| [`web`](web-property.md) | Externe Anfragen verarbeiten | — | Ja |
| [`workers`](workers-property.md) | Externe Anfragen verarbeiten | — | Ja, wenn die Web-Eigenschaft nicht verwendet wird |

{style="table-layout:auto"}

## `name`

Die `name`-Eigenschaft stellt den Anwendungsnamen bereit, der in der [`routes.yaml`](../routes/routes-yaml.md)-Datei zum Definieren des Upstreams des HTTP-Codes verwendet wird (standardmäßig `mymagento:http`). Wenn beispielsweise der Wert von `name` `app` ist, müssen Sie `app:http` im Upstream-Feld verwenden.

>[!WARNING]
>
>Ändern Sie den Namen der Anwendung nicht, nachdem sie bereitgestellt wurde. Dies führt zu Datenverlust.

## `type` und `build`

Die `type`- und `build`-Eigenschaften stellen Informationen zum Basis-Container-Image bereit, das zum Erstellen und Ausführen des Projekts erforderlich ist.

Die unterstützte `type` ist PHP. Geben Sie die PHP-Version wie folgt an:

```yaml
type: php:<version>
```

Die `build`-Eigenschaft bestimmt standardmäßig, was beim Erstellen des Projekts geschieht. Die `flavor` gibt einen Standardsatz von auszuführenden Build-Aufgaben an. Das folgende Beispiel zeigt die Standardkonfiguration für `type` und `build` aus `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.4
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Installieren und Verwenden von Composer 2

Die `build: flavor:`-Eigenschaft wird nicht für Composer 2.x verwendet. Daher müssen Sie Composer während der Build-Phase manuell installieren. Um Composer 2.x in Ihren Starter- und Pro-Projekten zu installieren und zu verwenden, müssen Sie drei Änderungen an Ihrer `.magento.app.yaml`-Konfiguration vornehmen:

1. `composer` als `build: flavor:` entfernen und `none` hinzufügen. Diese Änderung verhindert, dass Cloud die standardmäßige Version 1.x von Composer zum Ausführen von Build-Aufgaben verwendet.
1. Fügen Sie `composer/composer: '^2.0'` als `php` Abhängigkeit für die Installation von Composer 2.x hinzu.
1. Fügen Sie die `composer` Build-Aufgaben einem `build`-Hook hinzu, um die Build-Aufgaben mit Composer 2.x auszuführen.

Verwenden Sie die folgenden Konfigurationsfragmente in Ihrer eigenen `.magento.app.yaml`:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Weitere Informationen [ Composer finden Sie ](../development/overview.md#required-packages)Erforderliche Pakete“.

## `dependencies`

Geben Sie Abhängigkeiten an, die Ihre Anwendung möglicherweise während des Build-Prozesses benötigt.

Adobe Commerce unterstützt Abhängigkeiten von den folgenden Sprachen:

- PHP
- Rubin
- Node.js

Diese Abhängigkeiten sind unabhängig von den etwaigen Abhängigkeiten Ihrer Anwendung und in der `PATH`, während des Build-Prozesses und in der Laufzeitumgebung Ihrer Anwendung verfügbar.

Sie können diese Abhängigkeiten wie folgt angeben:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Verwenden Sie , um die PHP-Konfiguration zur Laufzeit zu ändern, z. B. um Erweiterungen zu aktivieren. Die folgenden Erweiterungen sind erforderlich:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Siehe [PHP-Einstellungen](php-settings.md) für Details zum Aktivieren von Erweiterungen.

## `disk`

Definiert die persistente Festplattengröße der Anwendung in MB.

```yaml
disk: 5120
```

Die empfohlene Mindestgröße beträgt 256 MB. Wenn der Fehler `UserError: Error building the project: Disk size may not be smaller than 128MB`angezeigt wird, erhöhen Sie die Größe auf 256 MB.

>[!NOTE]
>
>Für Pro-Staging- und Produktionsumgebungen müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um die `mounts`- und `disk` für Ihr Programm zu aktualisieren. Geben Sie beim Senden des Tickets die erforderlichen Konfigurationsänderungen an und fügen Sie eine aktualisierte Version Ihrer `.magento.app.yaml`-Datei hinzu.
>
>Es ist nicht möglich, den Festplattenspeicher in der Staging- oder Produktionsumgebung vorübergehend zu erhöhen. Dieser Vorgang kann nicht rückgängig gemacht werden.

## `relationships`

Definiert die Dienstzuordnung in der Anwendung.

Die `name` ist für die Anwendung in der `MAGENTO_CLOUD_RELATIONSHIPS` Umgebungsvariablen verfügbar. Die `<service-name>:<endpoint-name>` Beziehung ist den in der `.magento/services.yaml` definierten Werten für Name und Typ zugeordnet.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

Im Folgenden finden Sie ein Beispiel für die Standardbeziehungen:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

Unter [Services](../services/services-yaml.md) finden Sie eine vollständige Liste der derzeit unterstützten Service-Typen und Endpunkte.

## `mounts`

Ein Objekt, dessen Schlüssel Pfade relativ zum Stamm der Anwendung sind. Die Einhängung ist ein beschreibbarer Bereich auf der Festplatte für Dateien. Im Folgenden finden Sie eine standardmäßige Liste der Bereitstellungen, die in der `magento.app.yaml`-Datei unter Verwendung der `volume_id[/subpath]`-Syntax konfiguriert wurden:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

Das Format für das Hinzufügen Ihres Mount zu dieser Liste ist wie folgt:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared` - Gibt ein Volume zwischen Ihren Programmen innerhalb einer Umgebung frei.
- `disk` () - Definiert die für das freigegebene Volume verfügbare Größe.

>[!NOTE]
>
>Für Pro-Staging- und Produktionsumgebungen müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um die `mounts`- und `disk` für Ihr Programm zu aktualisieren. Geben Sie beim Senden des Tickets die erforderlichen Konfigurationsänderungen an und fügen Sie eine aktualisierte Version Ihrer `.magento.app.yaml`-Datei hinzu.

Sie können die Web-Bereitstellung zugänglich machen, indem Sie es zu dem [`web`](web-property.md) Block von Standorten hinzufügen.

>[!WARNING]
>
>Sobald Ihre Site Daten hat, ändern Sie nicht den `subpath` Teil des Bereitstellungsnamens. Dieser Wert ist die eindeutige Kennung für den `files`. Wenn Sie diesen Namen ändern, gehen alle am alten Speicherort gespeicherten Site-Daten verloren.

## `access`

Die Eigenschaft &quot;`access`&quot; gibt eine minimale Benutzerrollenebene an, die SSH-Zugriff auf die Umgebungen erlaubt. Die verfügbaren Benutzerrollen sind:

- `admin` - Kann Einstellungen ändern und Aktionen in der Umgebung ausführen; hat _Mitwirkende_ und _Betrachter_-Rechte.
- `contributor` - Kann Code in diese Umgebung pushen und von der Umgebung aus eine Verzweigung erstellen; verfügt über _Viewer_-Rechte.
- `viewer` - Kann nur die Umgebung anzeigen.

Die Standardbenutzerrolle ist `contributor`, wodurch der SSH-Zugriff von Benutzern mit ausschließlich &quot;_&quot;-_ eingeschränkt wird. Sie können die Benutzerrolle in `viewer` ändern, um SSH-Zugriff nur für Benutzer mit &quot;_&quot;-_ zuzulassen:

```yaml
access:
    ssh: viewer
```
