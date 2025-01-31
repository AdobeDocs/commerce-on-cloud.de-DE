---
title: Umgebung konfigurieren
description: Erfahren Sie, wie Sie mithilfe von Umgebungsvariablen Aktionen für alle Commerce in Cloud-Infrastrukturumgebungen konfigurieren, erstellen und bereitstellen, einschließlich Pro Staging und Produktion.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Konfigurieren von Umgebungsvariablen für die Bereitstellung

Die `.magento.env.yaml`-Datei verwendet Umgebungsvariablen, um die Verwaltung von Build- und Bereitstellungsaktionen in allen Ihren Umgebungen zu zentralisieren, einschließlich Pro-Staging und Produktion. Um eindeutige Aktionen in jeder Umgebung zu konfigurieren, müssen Sie diese Datei in jeder Umgebung ändern.

>[!TIP]
>
>Bei YAML-Dateien wird zwischen Groß- und Kleinschreibung unterschieden und es werden keine Registerkarten zugelassen. Achten Sie darauf, in der gesamten `.magento.env.yaml`-Datei konsistente Einzüge zu verwenden. Andernfalls funktioniert Ihre Konfiguration möglicherweise nicht wie erwartet. Die Beispiele in der Dokumentation und in der Beispieldatei verwenden _Einzug mit_ Leerzeichen. Verwenden Sie den Befehl [ece-tools validate](#validate-configuration-file), um Ihre Konfiguration zu überprüfen.

## Dateistruktur

Die `.magento.env.yaml`-Datei enthält zwei Abschnitte: `stage` und `log`. Im Abschnitt `stage` werden Aktionen gesteuert, die während der Phasen des [Cloud-Bereitstellungsprozesses](../deploy/process.md) auftreten.

- `stage` - Verwenden Sie den Abschnitt Phase , um bestimmte Aktionen für die folgenden Bereitstellungsphasen zu definieren:
   - `global` - Steuert Aktionen sowohl in der Erstellungs-, der Bereitstellungs- als auch in der Nachbereitstellungsphase. Sie können diese Einstellungen in den Abschnitten Erstellen, Bereitstellen und Nach der Bereitstellung überschreiben.
   - `build` - Steuert Aktionen nur in der Erstellungsphase. Wenn Sie in diesem Abschnitt keine Einstellungen angeben, verwendet die Build-Phase Einstellungen aus dem Abschnitt Global .
   - `deploy` - Steuert Aktionen nur in der Bereitstellungsphase. Wenn Sie in diesem Abschnitt keine Einstellungen angeben, verwendet die Bereitstellungsphase Einstellungen aus dem Abschnitt „Global“.
   - `post-deploy` - Steuert Aktionen _nach_ Bereitstellung der Anwendung und _danach_ beginnt der Container Verbindungen zu akzeptieren.
- `log` - Im Abschnitt „Protokoll“ können Sie [Benachrichtigungen](set-up-notifications.md) einschließlich Benachrichtigungstypen und Detaillierungsgrad konfigurieren.
   - `slack` - Konfigurieren Sie eine Nachricht, die an einen Slack-Bot gesendet werden soll.
   - `email` - Konfigurieren Sie eine E-Mail, die an einen oder mehrere E-Mail-Empfänger gesendet werden soll.
   - [Protokollhandler](log-handlers.md): Konfigurieren Sie Meldungen zu Hardware- und Softwareanwendungen, die an einen Remote-Protokollierungsserver gesendet werden.

### Umgebungsvariablen

Das `ece-tools` legt Werte in der `env.php` anhand von Werten aus [Cloud-Variablen](variables-cloud.md), in der [!DNL Cloud Console] festgelegten Variablen und der `.magento.env.yaml`-Konfigurationsdatei fest. Die Umgebungsvariablen in der `.magento.env.yaml`-Datei passen die Cloud-Umgebung an, indem sie Ihre bestehende Commerce-Konfiguration überschreiben. Wenn ein Standardwert `Not Set` ist, ergreift das `ece-tools`-Paket die Aktion **NO** und verwendet den [!DNL Commerce] Standardwert oder den Wert aus der Konfiguration MAGENTO_CLOUD_RELATIONSHIPS . Wenn der Standardwert festgelegt ist, setzt das `ece-tools`-Paket diesen Standardwert.

Die folgenden Themen enthalten detaillierte Definitionen aller Variablen, die Sie in der `.magento.env.yaml`-Datei verwenden können, z. B. ob ein Standardwert festgelegt ist oder nicht:

- [Global](variables-global.md) - Variablen steuern Aktionen in jeder Phase: Erstellen, Bereitstellen und Nachbereitstellen
- [Build](variables-build.md) - Variablen steuern Buildaktionen
- [Bereitstellen](variables-deploy.md) - Variablen steuern Bereitstellungsaktionen
- [Nach der Bereitstellung](variables-post-deploy.md) - Variablen steuern Aktionen nach der Bereitstellung

### Erstellen einer Konfigurationsdatei über CLI

Sie können eine `.magento.env.yaml` Konfigurationsdatei für eine Cloud-Umgebung mithilfe der folgenden `ece-tools`-Befehle generieren.

>Erstellt eine Konfigurationsdatei

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Aktualisieren von Werten in der Konfigurationsdatei

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Beide Befehle erfordern ein einziges Argument: ein JSON-formatiertes Array, das einen Wert für mindestens eine Build-, Bereitstellungs- oder Post-Bereitstellungsvariable angibt. Der folgende Befehl legt beispielsweise Werte für die Variablen `SCD_THREADS` und `CLEAN_STATIC_FILES` fest:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

und erstellt eine `.magento.env.yaml`-Datei mit den folgenden Einstellungen:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Sie können den Befehl `cloud:config:update` verwenden, um die neue Datei zu aktualisieren. Beispielsweise ändert der folgende Befehl den `SCD_THREADS` und fügt die `SCD_COMPRESSION_TIMEOUT` hinzu:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

Die aktualisierte Datei enthält die folgende Konfiguration:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Konfigurationsdatei validieren

Verwenden Sie den folgenden `ece-tools`-Befehl, um die `.magento.env.yaml`-Konfigurationsdatei zu validieren, bevor Sie Änderungen an die Remote-Cloud-Umgebung pushen.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

Die folgende Beispielantwort enthält eine Liste der zu korrigierenden Elemente:

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP-Konstanten

Sie können PHP-Konstanten in `.magento.env.yaml` Dateidefinitionen anstelle von hartcodierten Werten verwenden. Das folgende Beispiel definiert die `driver_options` mithilfe einer PHP-Konstante:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>Das konstante Parsen funktioniert nicht, wenn eine `symfony/yaml` Paketversion vor 3.2 verwendet wird.

## Fehlerbehandlung

Wenn ein Fehler aufgrund eines unerwarteten Werts in der `.magento.env.yaml`-Konfigurationsdatei auftritt, erhalten Sie eine Fehlermeldung. Beispielsweise enthält die folgende Fehlermeldung eine Liste empfohlener Änderungen an jedem Element mit einem unerwarteten Wert, wobei manchmal gültige Optionen bereitgestellt werden:

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Nehmen Sie Korrekturen vor, übertragen Sie die Änderungen und übertragen Sie sie. Wenn Sie keine Fehlermeldung erhalten, übergeben die Änderungen an Ihrer Konfigurationsdatei die Validierung.

## Optimierung der Konfigurationsverwaltung

Wenn Sie die Konfigurationsverwaltung nach dem Speichern der Konfigurationen aktiviert haben, sollten Sie die SCD_*-Variablen von der Bereitstellung in die Build-Phase verschieben. Siehe [Strategien zur Bereitstellung statischer Inhalte](../deploy/static-content.md).

>Vor der Konfigurationsverwaltung:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>Verschieben Sie nach der Aktivierung der Konfigurationsverwaltung die SCD_*-Variablen in den Build-Schritt:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
