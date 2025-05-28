---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 3%

---
# ECE-Tools

**Version**: 2002.2.5

Diese Referenz enthält 34 Befehle, die über das `ece-tools` Befehlszeilen-Tool verfügbar sind.
Die anfängliche Liste wird automatisch mit dem Befehl `ece-tools list` unter Adobe Commerce in der Cloud-Infrastruktur generiert.

## Allgemein

Dieser Verweis wird aus der Anwendungs-Code-Basis generiert. Um den Inhalt zu ändern, _Sie uns Feedback_ (finden Sie den Link oben rechts). Beitragsrichtlinien finden Sie unter [Code-Beiträge](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Globale Optionen

#### `--help`, `-h`

Zeigt die Hilfe für den angegebenen Befehl an. Wenn kein Befehl angegeben wird, wird die Hilfe für den Listenbefehl angezeigt

- Standard: `false`
- Akzeptiert keinen Wert

#### `--quiet`, `-q`

Keine Nachricht ausgeben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--verbose`, `-v|-vv|-vvv`

Erhöhen Sie die Ausführlichkeit der Meldungen: 1 für die normale Ausgabe, 2 für die ausführlichere Ausgabe und 3 für die Fehlersuche.

- Standard: `false`
- Akzeptiert keinen Wert

#### `--version`, `-V`

Diese Anwendungsversion anzeigen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--ansi`

ANSI-Ausgabe erzwingen (oder deaktivieren —no-ansi)

- Akzeptiert keinen Wert

#### `--no-ansi`

Negieren Sie die Option &quot;—ansi“

- Standard: `false`
- Akzeptiert keinen Wert

#### `--no-interaction`, `-n`

Keine interaktiven Fragen stellen

- Standard: `false`
- Akzeptiert keinen Wert


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Interner Befehl zur Bereitstellung von Shell-Fertigstellungsvorschlägen

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--shell`, `-s`

Der Schalentyp („bash“, „fish“, „zsh„)

- Erfordert einen Wert

#### `--input`, `-i`

Ein Array von Eingabe-Token (z. B. COMP_WORDS oder argv)

- Standard: `[]`
- Erfordert einen Wert

#### `--current`, `-c`

Der Index des „Eingabe“-Arrays, in dem sich der Cursor befindet (z. B. COMP_CWORD)

- Erfordert einen Wert

#### `--api-version`, `-a`

Die API-Version des Abschlussskripts

- Erfordert einen Wert

#### `--symfony`, `-S`

Veraltet

- Erfordert einen Wert


## `build`

```bash
ece-tools build
```

Erstellt eine Anwendung.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Dump des Shell-Fertigstellungsskripts

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Argumente

#### `shell`

Der Shell-Typ (z. B. „bash„), der Wert der &quot;$SHELL“-Env-Var wird verwendet, wenn diese nicht angegeben wird

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--debug`

Verfolgen Sie den Abschluss des Debug-Protokolls

- Standard: `false`
- Akzeptiert keinen Wert


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Erstellt Datenbanksicherungen.

### Argumente

#### `databases`

Datenbanken für die Sicherung. Verfügbare Werte: [Hauptangebotsverkäufe]. Wenn der Argumentwert nicht angegeben ist, werden Datenbanksicherungen mit den in der `MAGENTO_CLOUD_RELATIONSHIP` Umgebungsvariablen oder/und der `stage.deploy.DATABASE_CONFIGURATION` Eigenschaft der Konfigurationsdatei &quot;.magento.env.yaml“ gespeicherten Anmeldeinformationen erstellt.

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--remove-definers`, `-d`

Definierer aus dem Datenbank-Dump entfernen

- Standard: `false`
- Akzeptiert keinen Wert

#### `--dump-directory`, `-a`

Alternatives Verzeichnis zum Speichern des Speicherauszugs verwenden

- Erfordert einen Wert


## `deploy`

```bash
ece-tools deploy
```

Stellt das Programm bereit.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Anzeigen der Hilfe für einen Befehl

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Argumente

#### `command_name`

Der Befehlsname

- Standard: `help`

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--format`

Das Ausgabeformat (txt, xml, json oder md)

- Standard: `txt`
- Erfordert einen Wert

#### `--raw`

So geben Sie die Raw-Befehlshilfe aus

- Standard: `false`
- Akzeptiert keinen Wert


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Befehle auflisten

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Argumente

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

#### `--short`

So überspringen Sie die Beschreibung der Befehlsargumente

- Standard: `false`
- Akzeptiert keinen Wert


## `patch`

```bash
ece-tools patch
```

Wendet benutzerdefinierte Patches an.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Führt nach der Bereitstellung Vorgänge aus.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Szenario(e) ausführen.

### Argumente

#### `scenario`

Szenario(e)

- Standard: `[]`
- Erforderlich

- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Zeigt die Liste der Backup-Dateien an.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Stellen Sie wichtige Konfigurationsdateien wieder her. Führen Sie backup:list aus, um die Liste der Backup-Dateien anzuzeigen.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--force`, `-f`

Vorhandene Dateien beim Wiederherstellen eines Backups überschreiben

- Standard: `false`
- Akzeptiert keinen Wert

#### `--file`

Ein bestimmter Dateiwiederherstellungspfad

- Akzeptiert einen Wert


## `build:generate`

```bash
ece-tools build:generate
```

Generiert alle für die Build-Phase erforderlichen Dateien.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Überträgt generierte Dateien in das Init-Verzeichnis.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Erstellt eine `.magento.env.yaml` mit der angegebenen Konfiguration der Variablen „build“, „deploy“ und „post-deploy“. Überschreibt eine vorhandene `.magento.env.yaml`.

### Argumente

#### `configuration`

Konfiguration im JSON-Format

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Aktualisiert die vorhandene `.magento.env.yaml` mit der angegebenen Konfiguration. Erstellt `.magento.env.yaml` Datei, wenn sie noch nicht vorhanden ist.

### Argumente

#### `configuration`

Konfiguration im JSON-Format

- Erforderlich

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Validiert `.magento.env.yaml` Konfigurationsdatei

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Dump-Konfiguration für die Bereitstellung statischer Inhalte.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Deaktivieren Sie alle Magento Cron-Prozesse und beenden Sie alle laufenden Prozesse.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Aktiviert Magento Cron-Prozesse.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Beendet alle Magento Cron-Prozesse.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Entsperren Sie Cron-Aufträge, die im Status „Wird ausgeführt“ feststecken.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--job-code`

Cron-Auftragscode zum Entsperren.

- Standard: `[]`
- Akzeptiert mehrere Werte


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Generiert die Datei dist/error-codes.md aus der Datei schema.error.yaml.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Aktualisiert Composer für die Bereitstellung von Git.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Anzeigen kodierter Cloud-Konfigurations-Umgebungsvariablen.

### Argumente

#### `variable`

Anzuzeigende Umgebungsvariablen, mögliche Optionen: Services, Routen, Variablen

- Standard: `[]`
- Array

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Zeigt Informationen zu Fehlern nach Fehler-ID oder Informationen zu allen Fehlern der letzten Bereitstellung an.

### Argumente

#### `error-code`

Fehlercode, wenn der Befehl nicht übergeben wird, zeigt Informationen zu allen Fehlern der letzten Bereitstellung an

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).

#### `--json`, `-j`

Wird verwendet, um Ergebnisse im JSON-Format zu erhalten

- Standard: `false`
- Akzeptiert keinen Wert


## `module:refresh`

```bash
ece-tools module:refresh
```

Aktualisiert die Konfiguration, um neu hinzugefügte Module zu aktivieren.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Erzeugt die Datei *.dist des Schemas

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Prüft den idealen Konfigurationszustand.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Überprüft die Master-Slave-Konfiguration.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Prüft SCD bei der Build-Konfiguration.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Überprüfung der SCD-On-Demand-Konfiguration

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Überprüft SCD bei der Bereitstellungskonfiguration.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Prüft, ob DB aufgeteilt werden kann und ob DB bereits aufgeteilt wurde.

### Optionen

Globale Optionen finden Sie unter [Globale Optionen](#global-options).
