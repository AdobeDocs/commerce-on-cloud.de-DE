---
source-git-commit: 9166b44ae53e8cfc6b8022730a6b91406ba696c0
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# Magento-Cloud (Adobe Commerce auf Cloud-Infrastruktur)

<!-- The template to render with above values -->

**Version**: 1.46.1

Diese Referenz enthält 119 Befehle, die über das `magento-cloud` Befehlszeilen-Tool verfügbar sind.
Die anfängliche Liste wird automatisch mit dem Befehl `magento-cloud list` unter Adobe Commerce in der Cloud-Infrastruktur generiert.

## Allgemein

Dieser Verweis wird aus der Anwendungs-Code-Basis generiert. Um den Inhalt zu ändern, können Sie den Quell-Code für die entsprechende Befehlsimplementierung im Repository [Codebase](https://github.com/magento/magento-cloud-cli) aktualisieren und Ihre Änderungen zur Überprüfung übermitteln. Eine andere Möglichkeit ist _Feedback geben_ (finden Sie den Link oben rechts). Beitragsrichtlinien finden Sie unter [Code-Beiträge](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Globale Optionen

#### `--help`, `-h`

Diese Hilfemeldung anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--verbose`, `-v|-vv|-vvv`

Erhöhen der Ausführlichkeit von Nachrichten

- Standard: `false`
- Akzeptiert keinen Wert

#### `--version`, `-V`

Diese Anwendungsversion anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--yes`, `-y`

Bestätigungsfragen mit „Ja“ beantworten; Standardwert für andere Fragen akzeptieren; Interaktion deaktivieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-interaction`

Stellen Sie keine interaktiven Fragen; akzeptieren Sie Standardwerte. Entspricht der Verwendung der Umgebungsvariablen: MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- Standard: `false`
- Akzeptiert keinen Wert


## `clear-cache`

```bash
magento-cloud cc
```

Löschen des CLI-Cache

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Eine codierte Zeichenfolge wie MAGENTO_CLOUD_VARIABLES decodieren

### Argumente

#### `value`

Der zu dekodierende Variablenwert

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die Eigenschaft, die innerhalb der Variablen angezeigt werden soll

- Erfordert einen Wert


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

Öffnen der Onlinedokumentation

### Argumente

#### `search`

Suchbegriff(e)

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--browser`

Der zum Öffnen der URL zu verwendende Browser. Auf 0 für keine festlegen.

- Erfordert einen Wert

#### `--pipe`

Ausgabe der URL an stdout

- Standard: `false`
- Akzeptiert keinen Wert


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

Zeigt Hilfe zu einem Befehl an

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### Argumente

#### `command_name`

Der Befehlsname

- Standard: `help`

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat (txt, json oder md)

- Standard: `txt`
- Erfordert einen Wert

#### `--raw`

So geben Sie die Raw-Befehlshilfe aus

- Standard: `false`
- Akzeptiert keinen Wert


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Führt Befehle auf

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### Argumente

#### `command`

Der auszuführende Befehl

- Erforderlich


#### `namespace`

Der Namespace-Name

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--raw`

Ausgabe der unbearbeiteten Befehlsliste

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat (txt, xml, json oder md)

- Standard: `txt`
- Erfordert einen Wert

#### `--all`

Alle Befehle anzeigen, einschließlich ausgeblendeter Befehle

- Standard: `false`
- Akzeptiert keinen Wert


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

Ausführen eines Befehls für mehrere Projekte

### Argumente

#### `cmd`

Der auszuführende Befehl

- Standard: `[]`
- Erforderlich

- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--projects`, `-p`

Eine Liste der Projekt-IDs, durch Kommas und/oder Leerzeichen getrennt

- Erfordert einen Wert

#### `--continue`

Ausführen von Befehlen auch dann fortsetzen, wenn eine Ausnahme auftritt

- Standard: `false`
- Akzeptiert keinen Wert

#### `--sort`

Eine Eigenschaft, nach der die Liste der Projektoptionen sortiert werden soll

- Standard: `title`
- Erfordert einen Wert

#### `--reverse`

Kehrt die Reihenfolge der Projektoptionen um

- Standard: `false`
- Akzeptiert keinen Wert


## `web`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Öffnen Sie das Projekt in der Web-Benutzeroberfläche

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--browser`

Der zum Öffnen der URL zu verwendende Browser. Auf 0 für keine festlegen.

- Erfordert einen Wert

#### `--pipe`

Ausgabe der URL an stdout

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Abbrechen einer Aktivität

### Argumente

#### `id`

Die Aktivitäts-ID. Die Standardeinstellung ist die letzte kündbare Aktivität.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--type`, `-t`

Nach Typ filtern (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter für den Typ verwendet werden, z. B. &quot;%var%&quot; zur Auswahl variablenbezogener Aktivitäten.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`, `-x`

Nach Typ ausschließen (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter zum Ausschließen von Typen verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--all`, `-a`

Überprüfung der letzten Aktivitäten für alle Umgebungen (bei Auswahl einer Standardaktivität)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

Anzeigen detaillierter Informationen zu einer einzelnen Aktivität

### Argumente

#### `id`

Die Aktivitäts-ID. Standardmäßig wird die letzte Aktivität verwendet.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Eigenschaft

- Erfordert einen Wert

#### `--type`, `-t`

Nach Typ filtern (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter für den Typ verwendet werden, z. B. &quot;%var%&quot; zur Auswahl variablenbezogener Aktivitäten.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`, `-x`

Nach Typ ausschließen (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter zum Ausschließen von Typen verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--state`

Filtern nach Status (bei Auswahl einer Standardaktivität): in_progress, pending, complete oder canceled. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--result`

Filtern nach Ergebnis (bei Auswahl einer Standardaktivität): Erfolg oder Fehler

- Erfordert einen Wert

#### `--incomplete`, `-i`

Nur unvollständige Aktivitäten einbeziehen (bei Auswahl einer Standardaktivität). Dies ist eine Kurzschreibweise für —state=in_progress,pending

- Standard: `false`
- Akzeptiert keinen Wert

#### `--all`, `-a`

Überprüfung der letzten Aktivitäten für alle Umgebungen (bei Auswahl einer Standardaktivität)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Abrufen einer Liste von Aktivitäten für eine Umgebung oder ein Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--type`, `-t`

Filteraktivitäten nach Typ Die Werte können durch Kommas (z. B. „a,b,c„) und/oder Leerzeichen aufgeteilt werden. Der erste Teil des Aktivitätsnamens kann weggelassen werden, z. B. kann „cron“ „environment.cron“-Aktivitäten auswählen. Die Zeichen % oder * können als Platzhalter verwendet werden, z. B. &quot;%var%&quot;, um variablenbezogene Aktivitäten auszuwählen.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`, `-x`

Ausschließen von Aktivitäten nach Typ. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Der erste Teil des Aktivitätsnamens kann weggelassen werden, z. B. kann „cron“ „environment.cron“-Aktivitäten ausschließen. Die Zeichen % oder * können als Platzhalter zum Ausschließen von Typen verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--limit`

Anzahl der angezeigten Ergebnisse begrenzen

- Standard: `10`
- Erfordert einen Wert

#### `--start`

Nur Aktivitäten, die vor diesem Datum erstellt wurden, werden aufgelistet

- Erfordert einen Wert

#### `--state`

Aktivitäten nach Status filtern: in_progress, pending, complete oder canceled. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--result`

Filtern von Aktivitäten nach Ergebnis: Erfolg oder Fehler

- Erfordert einen Wert

#### `--incomplete`, `-i`

Nur unvollständige Aktivitäten auflisten

- Standard: `false`
- Akzeptiert keinen Wert

#### `--all`, `-a`

Aktivitäten in allen Umgebungen auflisten

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: ID*, Erstellt*, Beschreibung*, Fortschritt*, Status*, Ergebnis*, Abgeschlossen, Umgebungen, Typ (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Protokoll einer Aktivität anzeigen

### Argumente

#### `id`

Die Aktivitäts-ID. Standardmäßig wird die letzte Aktivität verwendet.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Intervall für Aktivitätsaktualisierung (Sekunden). Auf 0 gesetzt, um die Aktualisierung zu deaktivieren.

- Standard: `3`
- Erfordert einen Wert

#### `--timestamps`, `-t`

Zeitstempel neben jeder Nachricht anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--type`

Nach Typ filtern (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter für den Typ verwendet werden, z. B. &quot;%var%&quot; zur Auswahl variablenbezogener Aktivitäten.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`, `-x`

Nach Typ ausschließen (bei Auswahl einer Standardaktivität). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter zum Ausschließen von Typen verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--state`

Filtern nach Status (bei Auswahl einer Standardaktivität): in_progress, pending, complete oder canceled. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--result`

Filtern nach Ergebnis (bei Auswahl einer Standardaktivität): Erfolg oder Fehler

- Erfordert einen Wert

#### `--incomplete`, `-i`

Nur unvollständige Aktivitäten einbeziehen (bei Auswahl einer Standardaktivität). Dies ist eine Kurzschreibweise für —state=in_progress,pending

- Standard: `false`
- Akzeptiert keinen Wert

#### `--all`, `-a`

Überprüfung der letzten Aktivitäten für alle Umgebungen (bei Auswahl einer Standardaktivität)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Anzeigen der Konfiguration einer App

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Konfigurationseigenschaft

- Erfordert einen Wert

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--identity-file`, `-i`

[Veraltete Option, nicht mehr verwendet]

- Erfordert einen Wert


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Auflisten von Apps im Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--pipe`

Nur eine Liste der App-Namen ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Name*, Typ*, Festplatte, Pfad, Größe (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

Melden Sie sich mit einem API-Token bei Magento Cloud an

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

Anmelden bei Magento Cloud über einen Browser

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--force`, `-f`

Melden Sie sich erneut an, auch wenn Sie bereits angemeldet sind

- Standard: `false`
- Akzeptiert keinen Wert

#### `--browser`

Der zum Öffnen der URL zu verwendende Browser. Auf 0 für keine festlegen.

- Erfordert einen Wert

#### `--pipe`

Ausgabe der URL an stdout

- Standard: `false`
- Akzeptiert keinen Wert


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

Kontoinformationen anzeigen

### Argumente

#### `property`

Die anzuzeigende Kontoeigenschaft

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--no-auto-login`

Überspringt die automatische Anmeldung. Wenn Sie nicht angemeldet sind, wird nichts ausgegeben und der Exitcode lautet 0, wobei keine anderen Fehler angenommen werden.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--property`, `-P`

Die anzuzeigende Kontoeigenschaft (alternative Syntax)

- Erfordert einen Wert

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Abmelden von Magento Cloud

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--all`, `-a`

Abmelden von allen lokalen Sitzungen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--other`

Von anderen lokalen Sitzungen abmelden

- Standard: `false`
- Akzeptiert keinen Wert


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Einrichten der Blackfire.io-Integration für das Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--server_id`

Die Server-ID

- Erfordert einen Wert

#### `--server_token`

Das Server-Token

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Hinzufügen eines SSL-Zertifikats zum Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--cert`

Der Pfad zur Zertifikatdatei

- Erfordert einen Wert

#### `--key`

Der Pfad zur Datei mit dem privaten Schlüssel des Zertifikats

- Erfordert einen Wert

#### `--chain`

Der Pfad zur Zertifikatkettendatei

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

Löschen eines Zertifikats aus dem Projekt

### Argumente

#### `id`

Die Zertifikat-ID (oder der Beginn)

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

Anzeigen eines Zertifikats

### Argumente

#### `id`

Die Zertifikat-ID (oder der Beginn)

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Zertifikateigenschaft

- Erfordert einen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Auflisten von Projektzertifikaten

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--domain`

Nach Domain-Namen filtern (Suche ohne Berücksichtigung der Groß-/Kleinschreibung)

- Erfordert einen Wert

#### `--exclude-domain`

Zertifikate ausschließen, Übereinstimmung nach Domain-Namen (Suche ohne Berücksichtigung der Groß-/Kleinschreibung)

- Erfordert einen Wert

#### `--issuer`

Nach Aussteller filtern

- Erfordert einen Wert

#### `--only-auto`

Nur automatisch bereitgestellte Zertifikate anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-auto`

Nur manuell hinzugefügte Zertifikate anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--ignore-expiry`

Abgelaufene und nicht abgelaufene Zertifikate anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--only-expired`

Nur abgelaufene Zertifikate anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-expired`

Nur nicht abgelaufene Zertifikate anzeigen (Standard)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--pipe-domains`

Gibt nur eine Liste der Domain-Namen zurück, die von den Zertifikaten abgedeckt werden

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Erstellt, Domains, Läuft ab, ID, Aussteller. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

Commit-Details anzeigen

### Argumente

#### `commit`

Der Commit SHA. Hierbei können auch die Suffixe &quot;HEAD&quot; und „Caret“ (^) oder „Tilde“ (~) für übergeordnete Commits akzeptiert werden.

- Standard: `HEAD`

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die Commit-Eigenschaft zum Anzeigen.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

Commits auflisten

### Argumente

#### `commit`

Der erste Git-Commit SHA. Hierbei können auch die Suffixe &quot;HEAD&quot; und „Caret“ (^) oder „Tilde“ (~) für übergeordnete Commits akzeptiert werden.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--limit`

Die Anzahl der angezeigten Commits.

- Standard: `10`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Autor, Datum, Freigabe, Zusammenfassung. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Erstellen eines lokalen Speicherauszugs der Remote-Datenbank

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--schema`

Das zu entladende Schema. Lassen Sie die Verwendung des Standardschemas (normalerweise „main„) weg.

- Erfordert einen Wert

#### `--file`, `-f`

Ein benutzerdefinierter Dateiname für den Speicherauszug

- Erfordert einen Wert

#### `--directory`, `-d`

Ein benutzerdefiniertes Verzeichnis für den Speicherauszug

- Erfordert einen Wert

#### `--gzip`, `-z`

Komprimieren Sie den Dump mit gzip

- Standard: `false`
- Akzeptiert keinen Wert

#### `--timestamp`, `-t`

Hinzufügen eines Zeitstempels zum Dump-Dateinamen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--stdout`, `-o`

Ausgabe in STDOUT anstelle einer Datei

- Standard: `false`
- Akzeptiert keinen Wert

#### `--table`

Einzuschließende Tabelle(n)

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-table`

Auszuschließende Tabelle(n)

- Standard: `[]`
- Erfordert einen Wert

#### `--schema-only`

Nur-Dump-Schemata, keine Daten

- Standard: `false`
- Akzeptiert keinen Wert

#### `--charset`

Die Codierung des Zeichensatzes für den Speicherauszug

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `db:size`

```bash
magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

Schätzen der Festplattenauslastung einer Datenbank

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--bytes`, `-B`

Größe in Byte anzeigen.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--cleanup`, `-C`

Überprüfen, ob Tabellen bereinigt werden können, und Empfehlungen anzeigen (nur InnoDb).

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: max, percent_used, used. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

SQL auf Remote-Datenbank ausführen

### Argumente

#### `query`

Eine auszuführende SQL-Anweisung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--raw`

Rohe, nicht tabellarische Ausgabe erzeugen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--schema`

Das zu verwendende Schema. Lassen Sie die Verwendung des Standardschemas (normalerweise „main„) weg. Übergeben Sie eine leere Zeichenfolge, um kein Schema zu verwenden.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Hinzufügen einer neuen Domain zum Projekt

### Argumente

#### `name`

Der Domain-Name

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--cert`

Der Pfad zur Zertifikatdatei für diese Domain

- Erfordert einen Wert

#### `--key`

Der Pfad zur Datei mit dem privaten Schlüssel für das angegebene Zertifikat.

- Erfordert einen Wert

#### `--chain`

Der Pfad zur Zertifikatkettendatei bzw. zu den Dateien für das angegebene Zertifikat

- Standard: `[]`
- Erfordert einen Wert

#### `--attach`

Die Produktions-Domain, die diese in den Routen der Umgebung ersetzt. Erforderlich für Nicht-Produktions-Umgebungs-Domains.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Löschen einer Domain aus dem Projekt

### Argumente

#### `name`

Der Domain-Name

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

Anzeigen detaillierter Informationen für eine Domain

### Argumente

#### `name`

Der Domain-Name

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Domain-Eigenschaft

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Abrufen einer Liste aller Domains

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: name*, ssl*, created_at*, registered_name, replace_for, type, updated_at (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Aktualisieren einer Domain

### Argumente

#### `name`

Der Domain-Name

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--cert`

Der Pfad zur Zertifikatdatei für diese Domain

- Erfordert einen Wert

#### `--key`

Der Pfad zur Datei mit dem privaten Schlüssel für das angegebene Zertifikat.

- Erfordert einen Wert

#### `--chain`

Der Pfad zur Zertifikatkettendatei bzw. zu den Dateien für das angegebene Zertifikat

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Aktivieren einer Umgebung

### Argumente

#### `environment`

Die zu aktivierenden Umgebungen

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--parent`

Vor der Aktivierung eine neue übergeordnete Umgebung einrichten

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

Verzweigen einer Umgebung

### Argumente

#### `id`

Die ID (Zweigname) der neuen Umgebung


#### `parent`

Das übergeordnete Element der neuen Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--title`

Der Titel der neuen Umgebung

- Erfordert einen Wert

#### `--type`

Der Typ der neuen Umgebung

- Erfordert einen Wert

#### `--no-clone-parent`

Die Daten der übergeordneten Umgebung nicht klonen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:checkout`

```bash
magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

Auschecken einer Umgebung

### Argumente

#### `id`

Die ID der Umgebung, die ausgecheckt werden soll. Beispiel: „sprint2“

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Löschen einer oder mehrerer Umgebungen

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### Argumente

#### `environment`

Die zu löschenden Umgebungen. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--delete-branch`

Löschen von Git-Verzweigungen für inaktive Umgebungen ohne Bestätigung

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-delete-branch`

Löschen Sie keine Git-Verzweigungen (inaktive Umgebungen)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--type`

Löschen Sie alle Umgebungen eines Typs (fügen Sie beliebige andere ausgewählte Umgebungen hinzu). Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen aufgeteilt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--only-type`, `-t`

Nur Umgebungen eines bestimmten Typs löschen Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen aufgeteilt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude`

Umgebung(en) nicht zu löschen. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`

Umgebungstypen, deren Werte nicht gelöscht werden sollen, können durch Kommas (z. B. „a,b,c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--inactive`

Alle inaktiven Umgebungen löschen (zu allen anderen ausgewählten hinzufügen)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--merged`

Alle zusammengeführten Umgebungen löschen (zu allen anderen ausgewählten hinzufügen)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--allow-delete-parent`

Löschen von Umgebungen mit untergeordneten Elementen zulassen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Aktualisieren der HTTP-Zugriffseinstellungen für eine Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--access`

Zugriffsbeschränkung im Format „permission:address“. 0 verwenden, um alle Adressen zu löschen.

- Standard: `[]`
- Erfordert einen Wert

#### `--auth`

Einfache HTTP-Authentifizierungsdaten im Format „username:password“. Verwenden Sie 0, um alle Anmeldeinformationen zu löschen.

- Standard: `[]`
- Erfordert einen Wert

#### `--enabled`

Ob die Zugriffssteuerung aktiviert werden soll: 1 zum Aktivieren, 0 zum Deaktivieren

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Lesen oder Festlegen von Eigenschaften für eine Umgebung

### Argumente

#### `property`

Der Name der Eigenschaft


#### `value`

Legen Sie einen neuen Wert für die Eigenschaft fest

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

Initialisieren einer Umgebung aus einem öffentlichen Git-Repository

### Argumente

#### `url`

Eine URL zu einem Git-Repository

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--profile`

Der Name des Profils

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Abrufen einer Liste von Umgebungen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--no-inactive`, `-I`

Keine inaktiven Umgebungen anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--pipe`

Geben Sie eine einfache Liste der Umgebungs-IDs aus.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--refresh`

Ob die Liste aktualisiert werden soll.

- Standard: `1`
- Erfordert einen Wert

#### `--sort`

Eine Eigenschaft zum Sortieren nach

- Standard: `title`
- Erfordert einen Wert

#### `--reverse`

In umgekehrter (absteigender) Reihenfolge sortieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--type`

Filtern Sie die Liste nach Umgebungstyp(en). Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: id*, title*, status*, type*, created, machine_name, updated (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

Lesen der Protokolle einer Umgebung

### Argumente

#### `type`

Der Protokolltyp, z. B. „Zugriff“ oder „Fehler“

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--lines`

Die Anzahl der anzuzeigenden Zeilen

- Standard: `100`
- Erfordert einen Wert

#### `--tail`

Protokoll kontinuierlich verfolgen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Zusammenführen einer Umgebung

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### Argumente

#### `environment`

Die zu fusionierende Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Pausieren einer Umgebung

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

Code in eine Umgebung pushen

### Argumente

#### `source`

Die Quellreferenz: ein Zweigname oder Commit-Hash

- Standard: `HEAD`

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--target`

Der Name der Zielverzweigung. Die Standardeinstellung ist die aktuelle Verzweigung.

- Erfordert einen Wert

#### `--force`, `-f`

Nicht-schnelle Vorwärtsaktualisierungen zulassen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--force-with-lease`

Lassen Sie nicht-schnelle Vorwärtsaktualisierungen zu, wenn die Remote-Tracking-Verzweigung auf dem neuesten Stand ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--set-upstream`, `-u`

Legen Sie die Zielumgebung als Upstream für die Quellverzweigung fest. Dadurch wird auch das Zielprojekt als Remote für das lokale Repository festgelegt.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--activate`

Umgebung vor dem Pushen aktivieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--parent`

Die neue übergeordnete Umgebung festlegen (nur verwendet mit —activate)

- Erfordert einen Wert

#### `--type`

Umgebungstyp festlegen (nur verwendet mit —activate )

- Erfordert einen Wert

#### `--no-clone-parent`

Klonen Sie nicht die Daten der übergeordneten Verzweigung (nur verwendet mit —activate)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Erneutes Bereitstellen einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

Beziehungen einer Umgebung anzeigen

### Argumente

#### `environment`

Die Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Beziehungseigenschaft

- Erfordert einen Wert

#### `--refresh`

Ob die Beziehungen aktualisiert werden sollen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Ausgesetzte Umgebung fortsetzen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

Kopieren von Dateien in eine und aus einer Umgebung mithilfe von scp

### Argumente

#### `files`

Zu kopierende Dateien Verwenden Sie das Präfix „remote:“, um Remote-Standorte zu definieren.

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--recursive`, `-r`

Rekursives Kopieren ganzer Ordner

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH zur aktuellen Umgebung

### Argumente

#### `cmd`

Ein Befehl, der in der Umgebung ausgeführt werden soll.

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--pipe`

Ausgabe nur der SSH-URL.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--all`

Ausgabe aller SSH-URLs (für jede Anwendung).

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

Synchronisieren des Codes einer Umgebung und/oder der Daten der übergeordneten Umgebung

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### Argumente

#### `synchronize`

Zu synchronisierende Elemente: „code“, „data“ oder beides

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--rebase`

Code durch Neubasierung anstatt durch Zusammenführen synchronisieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Abrufen der öffentlichen URLs einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--primary`, `-1`

Gibt nur die URL für die primäre Route zurück.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--browser`

Der zum Öffnen der URL zu verwendende Browser. Auf 0 für keine festlegen.

- Erfordert einen Wert

#### `--pipe`

Ausgabe der URL an stdout

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Öffnen eines Tunnels zu Xdebug in der Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--port`

Der lokale Port

- Standard: `9000`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

Anzeigen detaillierter Informationen zu einer einzelnen Integrationsaktivität

### Argumente

#### `integration`

Eine Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.


#### `activity`

Die Aktivitäts-ID. Standardmäßig wird die letzte Integrationsaktivität verwendet.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Eigenschaft

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

[Veraltete Option, nicht verwendet]

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `integration:activity:list`

```bash
magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Abrufen einer Liste von Aktivitäten für eine Integration

### Argumente

#### `id`

Eine Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--type`

Filtern von Aktivitäten nach Typ. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--exclude-type`, `-x`

Ausschließen von Aktivitäten nach Typ. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden. Die Zeichen % oder * können als Platzhalter zum Ausschließen von Typen verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--limit`

Anzahl der angezeigten Ergebnisse begrenzen

- Standard: `10`
- Erfordert einen Wert

#### `--start`

Nur Aktivitäten, die vor diesem Datum erstellt wurden, werden aufgelistet

- Erfordert einen Wert

#### `--state`

Filtern Sie Aktivitäten nach Status. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--result`

Filtern von Aktivitäten nach Ergebnis

- Erfordert einen Wert

#### `--incomplete`, `-i`

Nur unvollständige Aktivitäten auflisten

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: id*, created*, description*, type*, state*, result*, completed (* = Default columns). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

[Veraltete Option, nicht verwendet]

- Erfordert einen Wert


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

Protokoll einer Integrationsaktivität anzeigen

### Argumente

#### `integration`

Eine Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.


#### `activity`

Die Aktivitäts-ID. Standardmäßig wird die letzte Integrationsaktivität verwendet.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--timestamps`, `-t`

Zeitstempel neben jeder Nachricht anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

[Veraltete Option, nicht verwendet]

- Erfordert einen Wert


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Hinzufügen einer Integration zum Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--type`

Der Integrationstyp (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Erfordert einen Wert

#### `--base-url`

Die Basis-URL der Server-Installation

- Erfordert einen Wert

#### `--bitbucket-url`

Die Basis-URL der Bitbucket-Server-Installation

- Erfordert einen Wert

#### `--username`

Der Benutzername des Bitbucket-Servers

- Erfordert einen Wert

#### `--token`

Ein Authentifizierungs- oder Zugriffstoken für die Integration

- Erfordert einen Wert

#### `--key`

Ein Bitbucket-OAuth-Verbraucherschlüssel

- Erfordert einen Wert

#### `--secret`

Ein Bitbucket für OAuth-Verbrauchergeheimnisse

- Erfordert einen Wert

#### `--license-key`

Der Lizenzschlüssel für New Relic-Protokolle

- Erfordert einen Wert

#### `--server-project`

Das Projekt (z. B. „namespace/repo„)

- Erfordert einen Wert

#### `--repository`

Das zu trackende Repository (z. B. „Eigentümer/Repository„)

- Erfordert einen Wert

#### `--build-merge-requests`

GitLab: Erstellen von Zusammenführungsanfragen als Umgebungen

- Standard: `true`
- Erfordert einen Wert

#### `--build-pull-requests`

Jede Pull-Anforderung als Umgebung erstellen

- Standard: `true`
- Erfordert einen Wert

#### `--build-draft-pull-requests`

Erstellen von Pull-Anforderungen

- Standard: `true`
- Erfordert einen Wert

#### `--build-pull-requests-post-merge`

Erstellen von Pull-Anforderungen basierend auf ihrem Post-Merge-Status

- Standard: `false`
- Erfordert einen Wert

#### `--build-wip-merge-requests`

GitLab: WIP-Zusammenführungsanfragen erstellen

- Standard: `true`
- Erfordert einen Wert

#### `--merge-requests-clone-parent-data`

GitLab: Klonen von Daten für Zusammenführungsanfragen

- Standard: `true`
- Erfordert einen Wert

#### `--pull-requests-clone-parent-data`

Klonen der Daten der übergeordneten Umgebung für Pull-Anforderungen

- Standard: `true`
- Erfordert einen Wert

#### `--resync-pull-requests`

Daten der Pull-Anforderung bei jedem Build erneut synchronisieren

- Standard: `false`
- Erfordert einen Wert

#### `--fetch-branches`

Alle Verzweigungen aus der Remote-Instanz abrufen (als inaktive Umgebungen)

- Standard: `true`
- Erfordert einen Wert

#### `--prune-branches`

Löschen von Verzweigungen, die auf der Remote-Instanz nicht vorhanden sind

- Standard: `true`
- Erfordert einen Wert

#### `--resources-init`

Die beim Initialisieren eines neuen Dienstes zu verwendenden Ressourcen („minimum“, „default“, „manual“, „parent„)

- Erfordert einen Wert

#### `--url`

Die URL oder der API-Endpunkt für die Integration

- Erfordert einen Wert

#### `--shared-key`

Webhook: der gemeinsame geheime JWS-Schlüssel

- Erfordert einen Wert

#### `--file`

Der Name einer lokalen Datei, die das hochzuladende Skript enthält

- Erfordert einen Wert

#### `--events`

Eine Liste der Ereignisse, auf die reagiert werden soll, z. B. environment.push

- Standard: `*`
- Erfordert einen Wert

#### `--states`

Eine Liste der Status, auf die Aktionen durchgeführt werden sollen, z. B. „Ausstehend“, „In Bearbeitung“, „Abgeschlossen“

- Standard: `complete`
- Erfordert einen Wert

#### `--environments`

Die einzuschließenden Umgebungs-IDs

- Standard: `*`
- Erfordert einen Wert

#### `--excluded-environments`

Die auszuschließenden Umgebungs-IDs

- Standard: `[]`
- Erfordert einen Wert

#### `--from-address`

[Optional] Benutzerdefinierte Absenderadresse für Warn-E-Mails

- Erfordert einen Wert

#### `--recipients`

Die E-Mail-Adresse(n) des Empfängers

- Standard: `[]`
- Erfordert einen Wert

#### `--channel`

Der Slack-Kanal

- Erfordert einen Wert

#### `--routing-key`

Der PagerDuty-Routing-Schlüssel

- Erfordert einen Wert

#### `--category`

Die Kategorie der Sumo-Logik, die zum Filtern verwendet wird

- Erfordert einen Wert

#### `--index`

Der Splunk-Index

- Erfordert einen Wert

#### `--sourcetype`

Der Splunk-Ereignistyp

- Erfordert einen Wert

#### `--protocol`

Syslog-Transportprotokoll (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Standard: `tls`
- Erfordert einen Wert

#### `--syslog-host`

Syslog-Relais/-Kollektor-Host

- Erfordert einen Wert

#### `--syslog-port`

Syslog-Relais-/Kollektoranschluss

- Erfordert einen Wert

#### `--facility`

Syslog-Einrichtung

- Standard: `1`
- Erfordert einen Wert

#### `--message-format`

Syslog-Nachrichtenformat (&#39;rfc3164&#39; oder &#39;rfc5424&#39;)

- Standard: `rfc5424`
- Erfordert einen Wert

#### `--auth-mode`

Authentifizierungsmodus (&#39;prefix&#39; oder &#39;structured_data&#39;)

- Standard: `prefix`
- Erfordert einen Wert

#### `--auth-token`

Authentifizierungstoken

- Erfordert einen Wert

#### `--verify-tls`

Ob die HTTPS-Zertifikatüberprüfung aktiviert werden soll (empfohlen)

- Standard: `true`
- Erfordert einen Wert

#### `--header`

In POST-Anfragen zu verwendende HTTP-Header Trennen Sie Namen und Werte mit einem Doppelpunkt (:).

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Löschen einer Integration aus einem Projekt

### Argumente

#### `id`

Die Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

Anzeigen von Details einer Integration

### Argumente

#### `id`

Eine Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Integrationseigenschaft

- Akzeptiert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `integration:list`

```bash
magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Liste der Projektintegrationen anzeigen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: ID, Zusammenfassung, Typ. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Aktualisieren einer Integration

### Argumente

#### `id`

Die ID der zu aktualisierenden Integration

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--type`

Der Integrationstyp (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Erfordert einen Wert

#### `--base-url`

Die Basis-URL der Server-Installation

- Erfordert einen Wert

#### `--bitbucket-url`

Die Basis-URL der Bitbucket-Server-Installation

- Erfordert einen Wert

#### `--username`

Der Benutzername des Bitbucket-Servers

- Erfordert einen Wert

#### `--token`

Ein Authentifizierungs- oder Zugriffstoken für die Integration

- Erfordert einen Wert

#### `--key`

Ein Bitbucket-OAuth-Verbraucherschlüssel

- Erfordert einen Wert

#### `--secret`

Ein Bitbucket für OAuth-Verbrauchergeheimnisse

- Erfordert einen Wert

#### `--license-key`

Der Lizenzschlüssel für New Relic-Protokolle

- Erfordert einen Wert

#### `--server-project`

Das Projekt (z. B. „namespace/repo„)

- Erfordert einen Wert

#### `--repository`

Das zu trackende Repository (z. B. „Eigentümer/Repository„)

- Erfordert einen Wert

#### `--build-merge-requests`

GitLab: Erstellen von Zusammenführungsanfragen als Umgebungen

- Standard: `true`
- Erfordert einen Wert

#### `--build-pull-requests`

Jede Pull-Anforderung als Umgebung erstellen

- Standard: `true`
- Erfordert einen Wert

#### `--build-draft-pull-requests`

Erstellen von Pull-Anforderungen

- Standard: `true`
- Erfordert einen Wert

#### `--build-pull-requests-post-merge`

Erstellen von Pull-Anforderungen basierend auf ihrem Post-Merge-Status

- Standard: `false`
- Erfordert einen Wert

#### `--build-wip-merge-requests`

GitLab: WIP-Zusammenführungsanfragen erstellen

- Standard: `true`
- Erfordert einen Wert

#### `--merge-requests-clone-parent-data`

GitLab: Klonen von Daten für Zusammenführungsanfragen

- Standard: `true`
- Erfordert einen Wert

#### `--pull-requests-clone-parent-data`

Klonen der Daten der übergeordneten Umgebung für Pull-Anforderungen

- Standard: `true`
- Erfordert einen Wert

#### `--resync-pull-requests`

Daten der Pull-Anforderung bei jedem Build erneut synchronisieren

- Standard: `false`
- Erfordert einen Wert

#### `--fetch-branches`

Alle Verzweigungen aus der Remote-Instanz abrufen (als inaktive Umgebungen)

- Standard: `true`
- Erfordert einen Wert

#### `--prune-branches`

Löschen von Verzweigungen, die auf der Remote-Instanz nicht vorhanden sind

- Standard: `true`
- Erfordert einen Wert

#### `--resources-init`

Die beim Initialisieren eines neuen Dienstes zu verwendenden Ressourcen („minimum“, „default“, „manual“, „parent„)

- Erfordert einen Wert

#### `--url`

Die URL oder der API-Endpunkt für die Integration

- Erfordert einen Wert

#### `--shared-key`

Webhook: der gemeinsame geheime JWS-Schlüssel

- Erfordert einen Wert

#### `--file`

Der Name einer lokalen Datei, die das hochzuladende Skript enthält

- Erfordert einen Wert

#### `--events`

Eine Liste der Ereignisse, auf die reagiert werden soll, z. B. environment.push

- Standard: `*`
- Erfordert einen Wert

#### `--states`

Eine Liste der Status, auf die Aktionen durchgeführt werden sollen, z. B. „Ausstehend“, „In Bearbeitung“, „Abgeschlossen“

- Standard: `complete`
- Erfordert einen Wert

#### `--environments`

Die einzuschließenden Umgebungs-IDs

- Standard: `*`
- Erfordert einen Wert

#### `--excluded-environments`

Die auszuschließenden Umgebungs-IDs

- Standard: `[]`
- Erfordert einen Wert

#### `--from-address`

[Optional] Benutzerdefinierte Absenderadresse für Warn-E-Mails

- Erfordert einen Wert

#### `--recipients`

Die E-Mail-Adresse(n) des Empfängers

- Standard: `[]`
- Erfordert einen Wert

#### `--channel`

Der Slack-Kanal

- Erfordert einen Wert

#### `--routing-key`

Der PagerDuty-Routing-Schlüssel

- Erfordert einen Wert

#### `--category`

Die Kategorie der Sumo-Logik, die zum Filtern verwendet wird

- Erfordert einen Wert

#### `--index`

Der Splunk-Index

- Erfordert einen Wert

#### `--sourcetype`

Der Splunk-Ereignistyp

- Erfordert einen Wert

#### `--protocol`

Syslog-Transportprotokoll (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Standard: `tls`
- Erfordert einen Wert

#### `--syslog-host`

Syslog-Relais/-Kollektor-Host

- Erfordert einen Wert

#### `--syslog-port`

Syslog-Relais-/Kollektoranschluss

- Erfordert einen Wert

#### `--facility`

Syslog-Einrichtung

- Standard: `1`
- Erfordert einen Wert

#### `--message-format`

Syslog-Nachrichtenformat (&#39;rfc3164&#39; oder &#39;rfc5424&#39;)

- Standard: `rfc5424`
- Erfordert einen Wert

#### `--auth-mode`

Authentifizierungsmodus (&#39;prefix&#39; oder &#39;structured_data&#39;)

- Standard: `prefix`
- Erfordert einen Wert

#### `--auth-token`

Authentifizierungstoken

- Erfordert einen Wert

#### `--verify-tls`

Ob die HTTPS-Zertifikatüberprüfung aktiviert werden soll (empfohlen)

- Standard: `true`
- Erfordert einen Wert

#### `--header`

In POST-Anfragen zu verwendende HTTP-Header Trennen Sie Namen und Werte mit einem Doppelpunkt (:).

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

Bestehende Integration überprüfen

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### Argumente

#### `id`

Eine Integrations-ID. Leer lassen, um aus einer Liste auszuwählen.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

Aktuelles Projekt lokal erstellen

### Argumente

#### `app`

Zu erstellende(s) Programm(e) angeben

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--abslinks`, `-a`

Absolute Links verwenden

- Standard: `false`
- Akzeptiert keinen Wert

#### `--source`, `-s`

Das Quellverzeichnis. Standardmäßig wird der aktuelle Projektstamm verwendet.

- Erfordert einen Wert

#### `--destination`, `-d`

Das Ziel, mit dem der Web-Stamm jeder App verknüpft wird. Standard: _www

- Erfordert einen Wert

#### `--copy`, `-c`

Kopieren Sie in ein Build-Verzeichnis, anstatt eine Verknüpfung von der Quelle aus herzustellen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--clone`

Verwenden Sie Git, um die aktuelle HEAD in das Build-Verzeichnis zu klonen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--run-deploy-hooks`

Ausführen von Bereitstellungs- und/oder Bereitstellungs-Hooks

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-clean`

Alte Builds nicht entfernen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-archive`

Erstellen oder verwenden Sie kein Build-Archiv

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-backup`

Den vorherigen Build nicht sichern

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-cache`

Zwischenspeicherung deaktivieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-build-hooks`

Nach dem Build keine Erweiterungspunkte ausführen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-deps`

Build-Abhängigkeiten nicht lokal installieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--working-copy`

Drush: Verwenden Sie Git, um ein Repository jedes Drupal-Moduls zu klonen, anstatt einfach eine Version herunterzuladen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--concurrency`

Drush: Legen Sie die Anzahl der gleichzeitigen Projekte fest, die gleichzeitig verarbeitet werden

- Standard: `4`
- Erfordert einen Wert

#### `--lock`

Drush: Sperrdatei erstellen oder aktualisieren (nur verfügbar mit Drush Version 7+)

- Standard: `false`
- Akzeptiert keinen Wert


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

Lokalen Projektstamm suchen

### Argumente

#### `subdir`

Der zu suchende Unterordner (&#39;local&#39;, &#39;web&#39; oder &#39;shared&#39;)

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA : Anzeigen von CPU-, Datenträger- und Arbeitsspeichermetriken für eine Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--bytes`, `-B`

Größen in Byte anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--range`, `-r`

Der Zeitraum. Metriken werden für diese Dauer bis zum Endzeitpunkt (-bis) geladen. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 5 m, maximal 8 h oder mehr (je nach Projekt), standardmäßig 10 m.

- Erfordert einen Wert

#### `--interval`, `-i`

Das Zeitintervall. Die Standardeinstellung ist eine Division des Bereichs. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 1m.

- Erfordert einen Wert

#### `--to`

Die Endzeit. Die Standardeinstellung ist jetzt.

- Erfordert einen Wert

#### `--latest`, `-1`

Nur den neuesten einzelnen Datenpunkt anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--service`, `-s`

Nach Service- oder Anwendungsnamen filtern Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--type`

Filtern nach Servicetyp (wenn —service nicht angegeben ist). Die Version ist nicht erforderlich. Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Zeitstempel*, Dienst*, CPU_Prozent*, MEM_Prozent*, Disk_Percent*, TMP_Disk_Percent*, CPU_Limit, CPU_Used, Disk_Limit, Disk_Used, Inodes_Limit, Inodes_Percent, Inodes_Used, MEM_Limit, MEM_Used, TMP_Disk_Limit, TMP_Disk_Used, TMP_Inodes_Limit, TMP_Inodes_Percent, TMP_Inodes_Used, Typ (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA - Anzeige der CPU-Nutzung einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--range`, `-r`

Der Zeitraum. Metriken werden für diese Dauer bis zum Endzeitpunkt (-bis) geladen. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 5 m, maximal 8 h oder mehr (je nach Projekt), standardmäßig 10 m.

- Erfordert einen Wert

#### `--interval`, `-i`

Das Zeitintervall. Die Standardeinstellung ist eine Division des Bereichs. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 1m.

- Erfordert einen Wert

#### `--to`

Die Endzeit. Die Standardeinstellung ist jetzt.

- Erfordert einen Wert

#### `--latest`, `-1`

Nur den neuesten einzelnen Datenpunkt anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--service`, `-s`

Nach Service- oder Anwendungsnamen filtern Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--type`

Filtern nach Servicetyp (wenn —service nicht angegeben ist). Die Version ist nicht erforderlich. Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Zeitstempel*, Dienst*, Verwendet*, Limit*, Prozent*, Typ (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Anzeigen der Festplattenauslastung einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--bytes`, `-B`

Größen in Byte anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--range`, `-r`

Der Zeitraum. Metriken werden für diese Dauer bis zum Endzeitpunkt (-bis) geladen. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 5 m, maximal 8 h oder mehr (je nach Projekt), standardmäßig 10 m.

- Erfordert einen Wert

#### `--interval`, `-i`

Das Zeitintervall. Die Standardeinstellung ist eine Division des Bereichs. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 1m.

- Erfordert einen Wert

#### `--to`

Die Endzeit. Die Standardeinstellung ist jetzt.

- Erfordert einen Wert

#### `--latest`, `-1`

Nur den neuesten einzelnen Datenpunkt anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--service`, `-s`

Nach Service- oder Anwendungsnamen filtern Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--type`

Filtern nach Servicetyp (wenn —service nicht angegeben ist). Die Version ist nicht erforderlich. Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--tmp`

Bericht zur temporären Festplattenauslastung (zeigt die Spalten: Zeitstempel, Dienst, tmp_used, tmp_limit, tmp_percent, tmp_ipercent)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: timestamp*, service*, used*, limit*, percent*, ipercent*, tmp_percent*, limit, iused, tmp_limit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, type (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Anzeigen der Speichernutzung einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--bytes`, `-B`

Größen in Byte anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--range`, `-r`

Der Zeitraum. Metriken werden für diese Dauer bis zum Endzeitpunkt (-bis) geladen. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 5 m, maximal 8 h oder mehr (je nach Projekt), standardmäßig 10 m.

- Erfordert einen Wert

#### `--interval`, `-i`

Das Zeitintervall. Die Standardeinstellung ist eine Division des Bereichs. Sie können Einheiten angeben: Stunden (h), Minuten (m) oder Sekunden (s). Mindestens 1m.

- Erfordert einen Wert

#### `--to`

Die Endzeit. Die Standardeinstellung ist jetzt.

- Erfordert einen Wert

#### `--latest`, `-1`

Nur den neuesten einzelnen Datenpunkt anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--service`, `-s`

Nach Service- oder Anwendungsnamen filtern Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--type`

Filtern nach Servicetyp (wenn —service nicht angegeben ist). Die Version ist nicht erforderlich. Die Zeichen % oder * können als Platzhalter verwendet werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Zeitstempel*, Dienst*, Verwendet*, Limit*, Prozent*, Typ (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Dateien von einem Mount herunterladen, mithilfe von rsync

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--all`, `-a`

Download von allen Mounts

- Standard: `false`
- Akzeptiert keinen Wert

#### `--mount`, `-m`

Die Bereitstellung (als App-relativer Pfad)

- Erfordert einen Wert

#### `--target`

Das Verzeichnis, in das die Dateien heruntergeladen werden. Wenn —all verwendet wird, wird der Einhängepfad angehängt

- Erfordert einen Wert

#### `--source-path`

Verwenden Sie den Quellpfad des Mount (anstelle des Mount-Pfads) als Unterverzeichnis des Ziels, wenn —all verwendet wird

- Standard: `false`
- Akzeptiert keinen Wert

#### `--delete`

Ob irrelevante Dateien im Zielverzeichnis gelöscht werden sollen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--exclude`

Datei(en) vom Download ausschließen (Muster)

- Standard: `[]`
- Erfordert einen Wert

#### `--include`

Nicht auszuschließende Datei(en) (Muster)

- Standard: `[]`
- Erfordert einen Wert

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Erstellt eine Liste der Reittiere

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--paths`

Ausgabe nur der Einhängepfade (einer pro Zeile)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Definition, Pfad. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert


## `mount:size`

```bash
magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Überprüfen der Festplattenauslastung der Bereitstellungen

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--bytes`, `-B`

Größen in Byte anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--refresh`

Aktualisieren des Cache

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Verfügbar, Max, Einhängungen, percent_used, Größen, verwendet. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Dateien mithilfe von rsync auf ein Mount hochladen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--source`

Ein Verzeichnis mit hochzuladenden Dateien

- Erfordert einen Wert

#### `--mount`, `-m`

Die Bereitstellung (als App-relativer Pfad)

- Erfordert einen Wert

#### `--delete`

Gibt an, ob irrelevante Dateien im Mount gelöscht werden sollen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--exclude`

Datei(en) vom Upload ausschließen (Muster)

- Standard: `[]`
- Erfordert einen Wert

#### `--include`

Nicht auszuschließende Datei(en) (Muster)

- Standard: `[]`
- Erfordert einen Wert

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--instance`, `-I`

Eine Instanz-ID

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

BETA-Listenlaufzeitvorgänge für eine Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--full`

Die Länge des anzuzeigenden Befehls nicht begrenzen. Der Standardwert ist 24 Zeilen.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: service*, name*, start*, role, stop (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA - Ausführen eines Vorgangs in der Umgebung

### Argumente

#### `operation`

Der Vorgangsname

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--worker`

Ein Worker-Name

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

Löschen des Build-Cache eines Projekts

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

Lokales Klonen eines Projekts

### Argumente

#### `project`

Die Projekt-ID


#### `directory`

Der Ordner, in den geklont werden soll. Standardmäßig wird der Projekttitel verwendet

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--environment`, `-e`

Die zu klonende Umgebungskennung. Die Standardeinstellung ist der Projektstandard oder die erste verfügbare Umgebung

- Erfordert einen Wert

#### `--depth`

Einen flachen Klon erstellen: Begrenzt die Anzahl der Commits im Verlauf

- Erfordert einen Wert

#### `--build`

Erstellen des Projekts nach dem Klonen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Lesen oder Festlegen von Eigenschaften für ein Projekt

### Argumente

#### `property`

Der Name der Eigenschaft


#### `value`

Legen Sie einen neuen Wert für die Eigenschaft fest

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Abrufen einer Liste aller aktiven Projekte

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--pipe`

Ausgabe einer einfachen Liste von Projekt-IDs. Deaktiviert die Paginierung.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--region`

Nach Region filtern (exakte Übereinstimmung)

- Erfordert einen Wert

#### `--title`

Nach Titel filtern (Suche ohne Unterscheidung zwischen Groß- und Kleinschreibung)

- Erfordert einen Wert

#### `--my`

Nur die Projekte anzeigen, deren Inhaber Sie sind

- Standard: `false`
- Akzeptiert keinen Wert

#### `--refresh`

Ob die Liste aktualisiert werden soll

- Standard: `1`
- Erfordert einen Wert

#### `--sort`

Eine Eigenschaft zum Sortieren nach

- Standard: `title`
- Erfordert einen Wert

#### `--reverse`

In umgekehrter (absteigender) Reihenfolge sortieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--page`

Seitennummer. Dies ermöglicht die Paginierung, trotz Konfiguration oder —count. Ignoriert, wenn —pipe angegeben wurde.

- Erfordert einen Wert

#### `--count`, `-c`

Die Anzahl der pro Seite anzuzeigenden Projekte Verwenden Sie 0, um die Paginierung zu deaktivieren. Ignoriert, wenn —page angegeben wurde.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`

Anzuzeigende Spalten. Verfügbare Spalten: id*, title*, region*, created_at, organization_id, organization_label, organization_name, Status (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

Festlegen des Remote-Projekts für das aktuelle Git-Repository

### Argumente

#### `project`

Die Projekt-ID

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

Lesen einer Datei im Projekt-Repository

### Argumente

#### `path`

Der Pfad zur Datei

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--commit`, `-c`

Der Commit SHA. Hierbei können auch die Suffixe &quot;HEAD&quot; und „Caret“ (^) oder „Tilde“ (~) für übergeordnete Commits akzeptiert werden.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Auflisten der Dateien im Projekt-Repository

### Argumente

#### `path`

Der Pfad zu einem Unterverzeichnis

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--directories`, `-d`

Nur Ordner anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--files`, `-f`

Nur Dateien anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--git-style`

Stilausgabe ähnlich wie „git-tree“

- Standard: `false`
- Akzeptiert keinen Wert

#### `--commit`, `-c`

Der Commit SHA. Hierbei können auch die Suffixe &quot;HEAD&quot; und „Caret“ (^) oder „Tilde“ (~) für übergeordnete Commits akzeptiert werden.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Lesen eines Verzeichnisses oder einer Datei im Projekt-Repository

### Argumente

#### `path`

Der Pfad zum Verzeichnis oder zur Datei

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--commit`, `-c`

Der Commit SHA. Hierbei können auch die Suffixe &quot;HEAD&quot; und „Caret“ (^) oder „Tilde“ (~) für übergeordnete Commits akzeptiert werden.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

Anzeigen detaillierter Informationen zu einer Route

### Argumente

#### `route`

Die ursprüngliche URL der Route

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--id`

Eine auszuwählende Routen-ID

- Erfordert einen Wert

#### `--primary`, `-1`

Primäre Route auswählen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--property`, `-P`

Die anzuzeigende Eigenschaft

- Erfordert einen Wert

#### `--refresh`

Umgehen des Cache-Speichers von Routen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

[Veraltete Option, nicht mehr verwendet]

- Erfordert einen Wert

#### `--identity-file`, `-i`

[Veraltete Option, nicht mehr verwendet]

- Erfordert einen Wert


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

Auflisten aller Routen für eine Umgebung

### Argumente

#### `environment`

Die Umgebungs-ID

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Umgehen des Cache-Speichers von Routen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: route*, type*, to*, url (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

Installieren oder Aktualisieren von CLI-Konfigurationsdateien

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--shell-type`

Der Shell-Typ für die automatische Vervollständigung (bash oder zsh)

- Erfordert einen Wert


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

Aktualisieren der CLI auf die neueste Version

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--no-major`

Nur zwischen Nebenversionen oder Patch-Versionen aktualisieren

- Standard: `false`
- Akzeptiert keinen Wert

#### `--unstable`

Auf eine neue instabile Version aktualisieren, falls verfügbar

- Standard: `false`
- Akzeptiert keinen Wert

#### `--manifest`

Überschreiben des Speicherorts der Manifestdatei

- Erfordert einen Wert

#### `--current-version`

Aktuelle Version überschreiben

- Erfordert einen Wert

#### `--timeout`

Ein Timeout für die Versionsüberprüfung

- Standard: `30`
- Erfordert einen Wert


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Auflisten der Services im Projekt

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--pipe`

Ausgabe nur einer Liste von Service-Namen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Datenträger, Name, Größe, Typ. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Erstellen eines binären Archiv-Dump von Daten aus MongoDB

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--collection`, `-c`

Die Sammlung, die abgelegt werden soll

- Erfordert einen Wert

#### `--gzip`, `-z`

Komprimieren Sie den Dump mit gzip

- Standard: `false`
- Akzeptiert keinen Wert

#### `--stdout`, `-o`

Ausgabe in STDOUT anstelle einer Datei

- Standard: `false`
- Akzeptiert keinen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exportieren von Daten aus MongoDB

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--collection`, `-c`

Die zu exportierende Sammlung

- Erfordert einen Wert

#### `--jsonArray`

Exportieren von Daten als einzelnes JSON-Array

- Standard: `false`
- Akzeptiert keinen Wert

#### `--type`

Der Exporttyp, z. B. „csv“

- Erfordert einen Wert

#### `--fields`, `-f`

Die zu exportierenden Felder

- Standard: `[]`
- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Wiederherstellen eines binären Archiv-Dump von Daten in MongoDB

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--collection`, `-c`

Die wiederherzustellende Sammlung

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Verwenden der MongoDB-Shell

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--eval`

Übergeben eines JavaScript-Fragments an die Shell

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Zugriff auf die Redis-CLI

### Argumente

#### `args`

Dem Redis-Befehl hinzuzufügende Argumente

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Erstellen eines Snapshots einer Umgebung

### Argumente

#### `environment`

Die Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--live`

Live-Backup: Halten Sie die Umgebung nicht an. Wenn festgelegt, bleibt die Umgebung aktiv und steht während des Backups Verbindungen offen. Dadurch werden Ausfallzeiten reduziert, was das Risiko birgt, dass die Daten in einem inkonsistenten Zustand gesichert werden.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

Löschen eines Umgebungs-Snapshots

### Argumente

#### `id`

Die ID des Snapshots. Erforderlich im nicht interaktiven Modus.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

Umgebungsschnappschuss anzeigen

### Argumente

#### `id`

Die ID des Snapshots. Standardmäßig wird die neueste verwendet.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Eigenschaft.

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Auflisten der verfügbaren Momentaufnahmen einer Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

Wiederherstellen eines Umgebungsschnappschusses

### Argumente

#### `snapshot`

Der Name des Snapshots. Standardwert ist die neueste .

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--target`

Die Umgebung, in der wiederhergestellt werden soll. Standardmäßig wird die aktuelle Umgebung des Snapshots verwendet

- Erfordert einen Wert

#### `--branch-from`

Wenn —target noch nicht vorhanden ist, wird hier das übergeordnete Element der neuen Umgebung angegeben

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Auflisten von Quellvorgängen für eine Umgebung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--full`

Die Länge des anzuzeigenden Befehls nicht begrenzen. Der Standardwert ist 24 Zeilen.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: App, Befehl, Vorgang. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

Ausführen eines Quellvorgangs

### Argumente

#### `operation`

Der Vorgangsname

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--variable`

Eine Variable im Format type:name=value, die während des Vorgangs festgelegt werden soll

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

Erstellen eines SSH-Zertifikats

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh-only`

Aktualisieren Sie das Zertifikat nur, falls erforderlich (keine SSH-Konfiguration schreiben)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--new`

Aktualisierung des Zertifikats erzwingen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--new-key`

[Veraltet] Verwenden Sie stattdessen —new

- Standard: `false`
- Akzeptiert keinen Wert


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

Neuen SSH-Schlüssel hinzufügen

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumente

#### `path`

Der Pfad zu einem vorhandenen öffentlichen SSH-Schlüssel

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--name`

Ein Name zur Identifizierung des Schlüssels

- Erfordert einen Wert


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
```

SSH-Schlüssel löschen

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumente

#### `id`

Die ID des zu löschenden SSH-Schlüssels

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Abrufen einer Liste von SSH-Schlüsseln in Ihrem Konto

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: ID*, Titel*, Pfad*, Fingerabdruck (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

Lesen oder Ändern von Abonnementeigenschaften

### Argumente

#### `property`

Der Name der Eigenschaft


#### `value`

Legen Sie einen neuen Wert für die Eigenschaft fest

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--id`, `-s`

Die Abonnement-ID

- Erfordert einen Wert

#### `--date-fmt`

Das Datumsformat (als PHP-Datumsformat-String)

- Standard: `c`
- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH-Tunnel schließen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--all`, `-a`

Alle Tunnel schließen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Beziehungsinformationen für SSH-Tunnel anzeigen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Die anzuzeigende Beziehungseigenschaft

- Erfordert einen Wert

#### `--encode`, `-c`

Ausgabe als base64-kodiertes JSON

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

SSH-Tunnel auflisten

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--all`, `-a`

Alle Tunnel anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Port*, Projekt*, Umgebung*, App*, Beziehung*, URL (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

SSH-Tunnel für die Beziehungen einer App öffnen

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--gateway-ports`, `-g`

Zulassen, dass Remote-Hosts eine Verbindung zu lokalen weitergeleiteten Ports herstellen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Öffnen eines einzelnen SSH-Tunnels zu einer App-Beziehung

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--port`

Der lokale Port

- Erfordert einen Wert

#### `--gateway-ports`, `-g`

Zulassen, dass Remote-Hosts eine Verbindung zu lokalen weitergeleiteten Ports herstellen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--app`, `-A`

Der Name der Remote-Anwendung

- Erfordert einen Wert

#### `--relationship`, `-r`

Die zu verwendende Service-Beziehung

- Erfordert einen Wert

#### `--identity-file`, `-i`

Eine zu verwendende SSH-Identität (privater Schlüssel)

- Erfordert einen Wert


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Hinzufügen eines Benutzers zum Projekt

### Argumente

#### `email`

Die E-Mail-Adresse des Benutzers

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--role`, `-r`

Projektrolle des Benutzers („Admin“ oder „Viewer„) oder Rolle des Umgebungstyps (z. B. „staging:contributor“ oder „production:viewer„). Um einen Benutzer aus einem Umgebungstyp zu entfernen, legen Sie die Rolle als „Keine“ fest. Die Zeichen % oder * können als Platzhalter für den Umgebungstyp verwendet werden, z. B. &quot;%:viewer“, um dem Benutzer die Rolle „viewer“ für alle Typen zu geben. Die Rolle kann abgekürzt werden, z. B. „production:v“.

- Standard: `[]`
- Erfordert einen Wert

#### `--force-invite`

Einladung senden, auch wenn bereits eine gesendet wurde

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

Löschen eines Benutzers aus dem Projekt

### Argumente

#### `email`

Die E-Mail-Adresse des Benutzers

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

Benutzerrolle(n) anzeigen

### Argumente

#### `email`

Die E-Mail-Adresse des Benutzers

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--level`, `-l`

Rollenebene (&#39;Projekt&#39; oder &#39;Umgebung&#39;)

- Erfordert einen Wert

#### `--pipe`

Ausgabe der Rolle an stdout (nach Durchführung von Änderungen)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert

#### `--role`, `-r`

[Veraltet: Verwenden Sie user:update, um die Rolle(n) eines Benutzers zu ändern]

- Erfordert einen Wert


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Auflisten der Projektbenutzer

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: email*, name*, role*, id*, granted_at, updated_at (* = Standardspalten). Das Zeichen &quot;+&quot; kann als Platzhalter für die Standardspalten verwendet werden. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Benutzerrolle(n) in einem Projekt aktualisieren

### Argumente

#### `email`

Die E-Mail-Adresse des Benutzers

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--role`, `-r`

Projektrolle des Benutzers („Admin“ oder „Viewer„) oder Rolle des Umgebungstyps (z. B. „staging:contributor“ oder „production:viewer„). Um einen Benutzer aus einem Umgebungstyp zu entfernen, legen Sie die Rolle als „Keine“ fest. Die Zeichen % oder * können als Platzhalter für den Umgebungstyp verwendet werden, z. B. &quot;%:viewer“, um dem Benutzer die Rolle „viewer“ für alle Typen zu geben. Die Rolle kann abgekürzt werden, z. B. „production:v“.

- Standard: `[]`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

Erstellen einer Variablen

### Argumente

#### `name`

Der Variablenname

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--update`, `-u`

Aktualisieren Sie die Variable, falls sie bereits existiert

- Standard: `false`
- Akzeptiert keinen Wert

#### `--level`, `-l`

Die Ebene, auf der die Variable festgelegt werden soll (&#39;Projekt&#39; oder &#39;Umgebung&#39;)

- Erfordert einen Wert

#### `--name`

Der Variablenname

- Erfordert einen Wert

#### `--value`

Der Wert der Variablen

- Erfordert einen Wert

#### `--json`

Ob der Variablenwert JSON-formatiert ist

- Standard: `false`
- Erfordert einen Wert

#### `--sensitive`

Ob der Variablenwert sensibel ist

- Standard: `false`
- Erfordert einen Wert

#### `--prefix`

Das Präfix des Variablennamens, das dessen Typ bestimmen kann, z. B. „env“. Nur anwendbar, wenn der Name noch kein Präfix enthält. (z. B. „none“ oder „env:„)

- Standard: `none`
- Erfordert einen Wert

#### `--enabled`

Ob die Variable in der Umgebung aktiviert werden soll

- Standard: `true`
- Erfordert einen Wert

#### `--inheritable`

Ob die Variable von untergeordneten Umgebungen vererbt wird

- Standard: `true`
- Erfordert einen Wert

#### `--visible-build`

Ob die Variable zur Erstellungszeit sichtbar sein soll

- Erfordert einen Wert

#### `--visible-runtime`

Ob die Variable zur Laufzeit sichtbar sein soll

- Standard: `true`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Löschen einer Variablen

### Argumente

#### `name`

Der Variablenname

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--level`, `-l`

Variablenebene (&#39;Projekt&#39;, &#39;Umgebung&#39;, &#39;P&#39; oder &#39;E&#39;)

- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

Anzeigen einer Variablen

### Argumente

#### `name`

Der Name der Variablen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--property`, `-P`

Anzeigen einer einzelnen Variableneigenschaft

- Erfordert einen Wert

#### `--level`, `-l`

Variablenebene (&#39;Projekt&#39;, &#39;Umgebung&#39;, &#39;P&#39; oder &#39;E&#39;)

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--pipe`

[Veraltete Option] Nur den Variablenwert ausgeben

- Standard: `false`
- Akzeptiert keinen Wert


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Listenvariablen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--level`, `-l`

Variablenebene (&#39;Projekt&#39;, &#39;Umgebung&#39;, &#39;P&#39; oder &#39;E&#39;)

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: is_enabled, level, name, value. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Aktualisieren einer Variablen

### Argumente

#### `name`

Der Variablenname

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--allow-no-change`

Erfolgreiche Rückgabe (kein Exitcode), wenn keine Änderungen angegeben wurden

- Standard: `false`
- Akzeptiert keinen Wert

#### `--level`, `-l`

Variablenebene (&#39;Projekt&#39;, &#39;Umgebung&#39;, &#39;P&#39; oder &#39;E&#39;)

- Erfordert einen Wert

#### `--value`

Der Wert der Variablen

- Erfordert einen Wert

#### `--json`

Ob der Variablenwert JSON-formatiert ist

- Standard: `false`
- Erfordert einen Wert

#### `--sensitive`

Ob der Variablenwert sensibel ist

- Standard: `false`
- Erfordert einen Wert

#### `--enabled`

Ob die Variable in der Umgebung aktiviert werden soll

- Standard: `true`
- Erfordert einen Wert

#### `--inheritable`

Ob die Variable von untergeordneten Umgebungen vererbt wird

- Standard: `true`
- Erfordert einen Wert

#### `--visible-build`

Ob die Variable zur Erstellungszeit sichtbar sein soll

- Erfordert einen Wert

#### `--visible-runtime`

Ob die Variable zur Laufzeit sichtbar sein soll

- Standard: `true`
- Erfordert einen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--no-wait`, `-W`

Warten Sie nicht, bis der Vorgang abgeschlossen ist

- Standard: `false`
- Akzeptiert keinen Wert

#### `--wait`

Warten Sie, bis der Vorgang abgeschlossen ist (Standard)

- Standard: `false`
- Akzeptiert keinen Wert


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Abrufen einer Liste aller bereitgestellten Worker

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--refresh`

Ob der Cache aktualisiert werden soll

- Standard: `false`
- Akzeptiert keinen Wert

#### `--pipe`

Nur eine Liste der Arbeitskräftenamen ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--project`, `-p`

Die Projekt-ID oder URL

- Erfordert einen Wert

#### `--environment`, `-e`

Die Umgebungskennung. &quot;.“ verwenden , um die Standardumgebung des Projekts auszuwählen.

- Erfordert einen Wert

#### `--format`

Das Ausgabeformat: Tabelle, CSV, TSV oder Nur

- Standard: `table`
- Erfordert einen Wert

#### `--columns`, `-c`

Anzuzeigende Spalten. Verfügbare Spalten: Befehle, Name, Typ. Die Zeichen % oder * können als Platzhalter verwendet werden. Die Werte können durch Kommas (z. B. „a, b, c„) und/oder Leerzeichen getrennt werden.

- Standard: `[]`
- Erfordert einen Wert

#### `--no-header`

Tabellenüberschrift nicht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert
