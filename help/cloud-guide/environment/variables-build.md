---
title: Erstellen von Variablen
description: Siehe die Liste der Umgebungsvariablen, die Aktionen in der Build-Phase der Adobe Commerce auf Cloud-Infrastruktur steuern.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Erstellen von Variablen

Die folgenden _Build_-Variablen steuern Aktionen in der Build-Phase und können Werte von den [globalen Variablen](variables-global.md) erben und überschreiben. Fügen Sie diese Variablen in den `build` Schritt der `.magento.env.yaml` ein:

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Weitere Informationen zum Anpassen des Build- und Bereitstellungsprozesses finden Sie unter:

- [Bereitstellungskonfiguration](configure-env-yaml.md)
- [Bereitstellungsprozess](../deploy/process.md)

Die folgenden Variablen wurden in Version 2.2 entfernt:

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **default**—`1`
- **Version**—Adobe Commerce 2.1.4 und höher

Legen Sie die Ebene der Verzeichnisverschachtelung für das Speichern von Fehlerberichtsdateien fest, um zu vermeiden, dass das Berichtsverzeichnis mit Zehntausenden von Dateien gefüllt wird, was die Verwaltung und Überprüfung der Daten erschweren kann. Die Standardeinstellung ist `1`. In der Regel müssen Sie den Standardwert nur ändern, wenn Sie Probleme bei der Verwaltung von Fehlerberichtsdateien im `<magento_root>/var/report/` haben.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Liste mit Adobe Commerce-Qualitäts-Patches angeben, die während der Bereitstellung angewendet werden sollen.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

Im folgenden Beispiel werden drei Patches angegeben, die während der Bereitstellung angewendet werden sollen.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Siehe [Patches anwenden](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **default**—`6`
- **Version**—Adobe Commerce 2.1.4 und höher

Gibt an[&#x200B; welche GZIP](https://www.gnu.org/software/gzip)-Komprimierungsstufe (`0` zu `9`) beim Komprimieren statischer Inhalte verwendet werden soll; `0` deaktiviert die Komprimierung.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **default**—`600`
- **Version**—Adobe Commerce 2.1.4 und höher

Wenn die Zeit, die zum Komprimieren der statischen Assets benötigt wird, das Komprimierungs-Timeout überschreitet, wird der Bereitstellungsprozess unterbrochen. Legen Sie die maximale Ausführungszeit in Sekunden für den Befehl zur Komprimierung statischer Inhalte fest.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **default**—`false`
- **Version**—Adobe Commerce 2.4.2 und höher

Wenn auf `true` gesetzt, wird verhindert, dass während der Erstellungsphase statische Inhalte für übergeordnete Designs generiert werden.

Legen Sie während der Build-Phase `SCD_NO_PARENT: false` fest, damit das Generieren statischer Inhalte für die übergeordneten Designs keine Auswirkungen auf die Site-Bereitstellung hat oder zu unnötigen Site-Ausfallzeiten führt. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Sie können mehrere Gebietsschemata pro Design konfigurieren. Diese Anpassung beschleunigt den Build-Prozess, indem die Anzahl der unnötigen Design-Dateien reduziert wird. Sie können beispielsweise das Design _magento/backend_ auf Englisch und ein benutzerdefiniertes Design in anderen Sprachen erstellen.

Im folgenden Beispiel wird das `Magento/backend`-Design mit drei Gebietsschemata erstellt:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Im folgenden Beispiel werden drei Designs mit drei Gebietsschemata erstellt:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Sie können auch wählen, ob _Design bereitgestellt_ soll:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.2.0 und höher

Ermöglicht die Verlängerung der maximal erwarteten Ausführungszeit für die Bereitstellung statischer Inhalte.

Standardmäßig legt Adobe Commerce in der Cloud-Infrastruktur die maximal erwartete Ausführung auf 900 Sekunden fest. In einigen Szenarien benötigen Sie jedoch möglicherweise mehr Zeit, um die Bereitstellung statischer Inhalte für ein Cloud-Projekt abzuschließen.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **default**—`quick`
- **Version**—Adobe Commerce 2.2.0 und höher

Passen Sie die [Bereitstellungsstrategie](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=de) für statische Inhalte an. Siehe [Bereitstellen von statischen Ansichtsdateien](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=de).

Verwenden Sie diese Optionen _nur_ wenn Sie mehr als ein Gebietsschema haben:

- `standard`: Stellt alle statischen Ansichtsdateien für alle Pakete bereit.
- `quick` - (_Standard_) minimiert die Bereitstellungszeit.
- `compact` - Spart Speicherplatz auf dem Server. In Adobe Commerce Version 2.2.4 und früher überschreibt diese Einstellung den Wert für `scd_threads` mit dem Wert `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **default**—automatic
- **Version**—Adobe Commerce 2.1.4 und höher

Legt die Anzahl der Threads für die Bereitstellung statischer Inhalte fest. Der Standardwert wird anhand der erkannten CPU-Thread-Anzahl festgelegt und überschreitet den Wert 4 nicht. Eine Erhöhung der Thread-Anzahl beschleunigt die Bereitstellung statischer Inhalte; eine Verringerung der Thread-Anzahl verlangsamt die Bereitstellung. Sie können den Thread-Wert festlegen, z. B.:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Um die Bereitstellungszeit weiter zu verkürzen, verwenden [Konfigurationsverwaltung](../store/store-settings.md) mit dem Befehl `scd-dump` , um die statische Bereitstellung in die Build-Phase zu verschieben.

## `SCD_USE_BALER`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.3.0 und höher

[Baler](https://github.com/magento/baler) scannt Ihren generierten JavaScript-Code und erstellt ein optimiertes JavaScript-Bundle. Durch die Bereitstellung des optimierten Bundles auf Ihrer Site kann die Anzahl der Netzwerkanfragen beim Laden Ihrer Site reduziert und die Seitenladezeiten verbessert werden.

Legen Sie diese Option auf `true` fest, um die Ballenpresse nach der Bereitstellung statischer Inhalte auszuführen.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Da sich die Ballenpresse in der Alpha-Version befindet, ist es nicht ratsam, sie in Produktionsumgebungen zu verwenden.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Standard**— _Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Legen Sie `true` fest, um den `composer dump-autoload`-Befehl während einer Cloud Docker-Installation zu überspringen. Diese Variable ist nur für Cloud Docker-Container mit beschreibbaren Dateisystemen relevant. In solchen Fällen verhindert das Überspringen des Befehls, dass andere Befehle auf Code aus dem gelöschten `generated` zugreifen.

Wenn Adobe Commerce `composer dump-autoload` ausgeführt wird, werden automatisch geladene Dateien mit Links zu generierten Klassen im `generated` erstellt, was in Produktionsumgebungen mit schreibgeschützten Dateisystemen kein Problem darstellt. Bei Cloud Docker-Installationen mit beschreibbaren Dateisystemen (die nur für Tests und die Entwicklung mit `./vendor/bin/ece-docker build:compose --with-test` erstellt wurden) können Sie den `bin/magento -n setup:upgrade`-Befehl jedoch ohne die Option `--keep-generated` ausführen, wodurch das `generated` gelöscht wird. Wenn das Verzeichnis gelöscht wird, schlägt der `composer dump-autoload`-Befehl fehl, da der Autoload Links zu Dateien im gelöschten Verzeichnis enthält.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Standard**— _Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Wenn auf `true` gesetzt, wird die statische Inhaltsbereitstellung während der Erstellungsphase übersprungen.

Wenn Sie bereits während der Build-Phase mit [Konfigurationsverwaltung](../store/store-settings.md) statische Inhalte bereitstellen, können Sie die Bereitstellung statischer Inhalte für einen schnellen Build-Test überspringen.

Legen Sie in der Build-Phase `SKIP_SCD: false` so fest, dass der statische Inhaltserstellungsprozess während der Build-Phase stattfindet, in der der Prozess keine Auswirkungen auf die Site-Bereitstellung hat oder unnötige Site-Ausfallzeiten verursacht. Siehe [Statische Inhaltsbereitstellung](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Aktivieren oder deaktivieren Sie die [Symfony](https://symfony.com/doc/current/console/verbosity.html) Debug-Ausführlichkeitsstufe für `bin/magento` CLI-Befehle, die während der Bereitstellungsphase ausgeführt werden.

>[!NOTE]
>
>Um VERBOSE_COMMANDS zur Steuerung der Details in der Befehlsausgabe für erfolgreiche und fehlgeschlagene `bin/magento` CLI-Befehle zu verwenden, müssen Sie [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug` festlegen.

Wählen Sie den Detaillierungsgrad der Protokolle aus:

- `-v`= Normalausgabe
- `-vv`= Ausführlichere Ausgabe
- `-vvv` = ausführliche Ausgabe Ideal für die Fehlersuche

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
