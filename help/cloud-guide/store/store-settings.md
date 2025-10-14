---
title: Store-Konfigurationsverwaltung
description: Erfahren Sie, wie Sie Store-Konfigurationseinstellungen in allen Adobe Commerce in Cloud-Infrastrukturumgebungen verwalten und synchronisieren.
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Store-Konfigurationsverwaltung

Die Standardkonfigurationen für Ihren Store werden in einem `config.xml` für das entsprechende Modul gespeichert. Wenn Sie Einstellungen im Commerce Admin- oder CLI-`bin/magento config:set` ändern, werden die Änderungen in der Hauptdatenbank übernommen, insbesondere in der `core_config_data`. Diese Einstellungen überschreiben die Standardkonfigurationen, die in der `config.xml`-Datei gespeichert sind.

Store-Einstellungen, die auf die Konfigurationen im Abschnitt Admin **Stores** > **Settings** > **Configuration** verweisen, werden je nach Konfigurationstyp in den Bereitstellungskonfigurationsdateien gespeichert:

- `app/etc/config.php` - Konfigurationseinstellungen für Stores, Websites, Module oder Erweiterungen, statische Dateioptimierung und Systemwerte im Zusammenhang mit der Bereitstellung statischer Inhalte. Siehe die [config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html?lang=de) im _Konfigurationshandbuch_.
- `app/etc/env.php` - Werte für systemspezifische Überschreibungen und sensible Einstellungen, die _NOT_ in der Quell-Code-Verwaltung gespeichert werden sollen. Siehe [env.php-Referenz](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=de) im _Konfigurationshandbuch_.

>[!NOTE]
>
>Da Adobe Commerce in der Cloud-Infrastruktur nur die Produktions- und Wartungsmodi unterstützt **kann der Abschnitt** Erweitert **>** nicht über Admin aufgerufen werden. Sie müssen über [Umgebungs-Administratorrechte](../project/user-access.md) verfügen, um Konfigurationsverwaltungsaufgaben abzuschließen. Sie können zusätzliche Einstellungen mithilfe von [Umgebungsvariablen](../environment/configure-env-yaml.md) konfigurieren.

Die Konfigurationsverwaltung bietet eine Möglichkeit, mithilfe der Pipeline-Bereitstellung konsistente Speichereinstellungen in Ihren Umgebungen mit minimalen Ausfallzeiten bereitzustellen. Das Adobe Commerce on Cloud Infrastructure-Projekt umfasst den Build-Server, Build- und Bereitstellungsskripte sowie Bereitstellungsumgebungen, die mit Blick auf die [Pipeline-Bereitstellungsstrategie](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=de) entwickelt wurden.

## Außerkraftsetzungsschema der Konfiguration

Alle Systemkonfigurationen werden während der Build- und Bereitstellungsphasen gemäß dem folgenden Überschreibungsschema festgelegt:

1. Wenn eine Umgebungsvariable vorhanden ist, verwenden Sie die benutzerdefinierte Konfiguration und ignorieren Sie die Standardkonfiguration.
1. Wenn keine Umgebungsvariable vorhanden ist, verwenden Sie die Konfiguration aus einem `MAGENTO_CLOUD_RELATIONSHIPS` Name-Wert-Paar in der [`.magento.app.yaml`](../application/configure-app-yaml.md). Ignorieren Sie die Standardkonfiguration.
1. Wenn eine Umgebungsvariable nicht vorhanden ist und `MAGENTO_CLOUD_RELATIONSHIPS` kein Name-Wert-Paar enthält, entfernen Sie alle benutzerdefinierten Konfigurationen und verwenden Sie die Werte aus der Standardkonfiguration.

Zusammenfassend lässt sich sagen, dass Umgebungsvariablen alle anderen Werte überschreiben.

>[!TIP]
>
>Weitere Informationen [&#x200B; Überschreibungsschema für &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=de) Pipeline-Bereitstellung finden Sie _Konfigurationsverwaltung_ im Konfigurationshandbuch.

Wenn dieselbe Einstellung an mehreren Stellen konfiguriert ist, verwendet die Anwendung die folgende Konfigurationshierarchie, um zu bestimmen, welcher Wert auf die Umgebung angewendet werden soll:

| Priorität | configuration<br>method | Beschreibung |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>Umgebungsvariablen | Auf der Registerkarte &quot;_&quot;_ Umgebungskonfiguration im [!DNL Cloud Console] hinzugefügte Werte. Geben Sie hier Werte für vertrauliche oder umgebungsspezifische Konfigurationen an. Die hier angegebenen Einstellungen können nicht von der Administratorin bzw. vom Administrator bearbeitet werden. Siehe [Umgebungskonfigurationsvariablen](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Im Abschnitt `variables` der `.magento.app.yaml` Datei hinzugefügte Werte. Geben Sie hier Werte an, um eine konsistente Konfiguration über alle Umgebungen hinweg sicherzustellen. **Geben Sie in der `.magento.app.yaml` keine sensiblen Werte an.** Siehe [Anwendungseinstellungen](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Hier gespeicherte umgebungsspezifische Konfigurationswerte werden mithilfe des Befehls `app:config:dump` hinzugefügt. Legen Sie die systemspezifischen und sensiblen Werte mithilfe von Umgebungsvariablen oder der CLI fest. Siehe [Sensible Daten](#sensitive-data). Die `env.php` ist **nicht** in der Quell-Code-Verwaltung enthalten. |
| 4 | `app/etc/config.php` | Hier gespeicherte Werte werden mithilfe des Befehls `app:config:dump` hinzugefügt. Freigegebene Konfigurationswerte werden `config.php` hinzugefügt. Legen Sie die freigegebene Konfiguration über den Administrator oder mithilfe der CLI fest. Die `config.php` ist in der Quell-Code-Verwaltung enthalten. |
| 5 | Datenbank | Hier gespeicherte Werte werden hinzugefügt, indem Konfigurationen im Admin festgelegt werden. Konfigurationen, die mit einer der oben genannten Methoden festgelegt wurden, sind gesperrt (ausgegraut) und können nicht von der Administratorin bzw. dem Administrator bearbeitet werden. |
| 6 | `config.xml` | Bei vielen Konfigurationen sind die Standardwerte in der `config.xml`-Datei für ein Modul festgelegt. Wenn Adobe Commerce einen von einer der oben genannten Methoden festgelegten Wert nicht finden kann, wird er auf den Standardwert zurückgesetzt, falls dieser festgelegt ist. |

{style="table-layout:auto"}

## Konfigurations-Dump

Sie können den folgenden `ece-tools`-Befehl verwenden, um eine `config.php`-Datei zu generieren, die alle aktuellen Store-Konfigurationen enthält:

```bash
./vendor/bin/ece-tools config:dump
```

Die Daten, die in die `app/etc/config.php`-Datei „geschrieben _werden_ gesperrt, was bedeutet, dass das entsprechende Feld in Commerce Admin **schreibgeschützt** wird. Die `config.php` enthält nur die von Ihnen konfigurierten Einstellungen. Die Standardwerte werden nicht gesperrt. Durch das Sperren nur der von Ihnen aktualisierten Werte wird auch sichergestellt, dass alle in der Staging- und Produktionsumgebung verwendeten Erweiterungen nicht aufgrund schreibgeschützter Konfigurationen beschädigt werden, insbesondere nicht durch Fastly.

>[!WARNING]
>
>Der Befehl `ece-tools config:dump` ruft keine detaillierten Konfigurationen für Module wie B2B ab. Wenn Sie einen umfassenden Konfigurations-Dump benötigen, verwenden Sie den Befehl `app:config:dump` , aber dieser Befehl sperrt Konfigurationswerte in einem schreibgeschützten Status.

### Sensible Daten

Alle sensiblen Konfigurationen werden in die `app/etc/env.php`-Datei exportiert, wenn Sie den Befehl `bin/magento app:config:dump` verwenden. Sie können vertrauliche Werte mithilfe des CLI-Befehls festlegen: `bin/magento config:sensitive:set`. Siehe [Sensitive und umgebungsspezifische Einstellungen](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) im Handbuch _Commerce PHP Extensions_, um zu erfahren, wie Sie Konfigurationseinstellungen als sensibel oder systemspezifisch festlegen.

Eine Liste der [sensiblen oder systemspezifischen Einstellungen](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html?lang=de) finden Sie im _Konfigurationshandbuch_.

### SCD-Leistung

Abhängig von der Größe Ihres Stores können Sie eine große Anzahl von statischen Inhaltsdateien bereitstellen. Normalerweise werden statische Inhalte während der Bereitstellungsphase bereitgestellt, wenn sich die Anwendung im Wartungsmodus befindet. Die optimale Konfiguration besteht darin, während der Build-Phase statische Inhalte zu generieren. Siehe [Auswählen einer Bereitstellungsstrategie](../deploy/static-content.md).

Wenn Sie die Konfigurationsverwaltung nach dem Speichern der Konfigurationen aktiviert haben, sollten Sie die SCD_*-Variablen von der Bereitstellungsphase in die Erstellungsphase verschieben, um die Generierung statischer Inhalte während der Erstellungsphase ordnungsgemäß zu aktivieren. Siehe [Umgebungsvariablen](../environment/configure-env-yaml.md#environment-variables).

**Vor der Konfigurationsverwaltung**:

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

**Nach der Aktivierung der Konfigurationsverwaltung**:

Verschieben Sie die SCD_*-Variablen in den Build-Schritt:

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

>[!NOTE]
>
>Vor der Bereitstellung statischer Dateien komprimieren die Build- und Bereitstellungsphasen statische Inhalte mithilfe von GZIP. Das Komprimieren statischer Dateien reduziert die Serverauslastung und erhöht die Site-Leistung. Unter [Build-Optionen](../environment/variables-build.md) erfahren Sie mehr über das Anpassen oder Deaktivieren der Dateikomprimierung.

## Verfahren zur Verwaltung Ihrer Einstellungen

Die folgende Abbildung zeigt einen allgemeinen Überblick über diesen Prozess:

![Überblick über die Verwaltung der Starter-Konfiguration](../../assets/starter/configuration-management-flow.png)

**So konfigurieren Sie Ihren Store und generieren eine Konfigurationsdatei**:

1. Schließen Sie alle Konfigurationen für Ihre Stores in Admin für eine der Umgebungen ab:

   - Starter: Ein aktiver Entwicklungszweig
   - Pro: Eine aktive Verzweigung in der Integrationsumgebung

   Diese Konfigurationen enthalten nicht die eigentlichen Produkte, es sei denn, Sie planen, die Datenbank aus dieser Umgebung in Staging- und Produktionsumgebungen zu entladen. In der Regel enthalten Entwicklungsdatenbanken keine vollständigen Speicherdaten.

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Erstellen Sie einen lokalen Dump der Remote-Datenbank.

   ```bash
   magento-cloud db:dump
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen, um eine Remote-Umgebung zu aktualisieren.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Melden Sie sich nach Abschluss der Bereitstellung bei der Administratorin bzw. dem Administrator der aktualisierten Umgebung an, um die Einstellungen zu überprüfen. Fahren Sie bei Bedarf mit dem Zusammenführen aller zusätzlichen Konfigurationen mit der Staging- und Produktionsumgebung fort.

### Aktualisieren von Konfigurationen

Wenn Sie Ihre Umgebung über die Admin ändern und den Befehl erneut ausführen, werden neue Konfigurationen an den Code in der `config.php` angehängt.

>[!WARNING]
>
>Sie können die `config.php` zwar manuell in der Staging- und Produktionsumgebung bearbeiten, dies wird jedoch **empfohlen**. Die Datei hilft, alle Konfigurationen über alle Umgebungen hinweg konsistent zu halten. Löschen Sie niemals die `config.php` Datei, um sie neu zu erstellen. Durch das Löschen der Datei können bestimmte Konfigurationen und Einstellungen entfernt werden, die für Build- und Bereitstellungsprozesse erforderlich sind.

### Konfigurationsdateien wiederherstellen

Kopien der ursprünglichen `app/etc/env.php`- und `app/etc/config.php` wurden während des Bereitstellungsprozesses erstellt und im selben Ordner gespeichert. Im Folgenden werden die BAK (Backup-Dateien) und PHP (Original-Dateien) im selben `app/etc` Ordner dargestellt:

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

In älteren Konfigurationen wurde die `app/etc/config.local.php`-Datei verwendet. Siehe [Migrieren älterer Konfigurationen](#migrate-older-configurations).

**So stellen Sie Konfigurationsdateien wieder**:

1. Verwenden Sie auf Ihrer lokalen Workstation SSH, um sich beim Remote-Projekt und in der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Überprüfen Sie den Speicherort und die Verfügbarkeit der Sicherungsdateien.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Beispielantwort:

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Wiederherstellen von Sicherungsdateien.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migrieren älterer Konfigurationen

Wenn Sie auf Adobe Commerce auf Cloud-Infrastruktur 2.2 oder höher aktualisieren, empfiehlt es sich möglicherweise, die Einstellungen aus der `config.local.php` in die neue `config.php` zu migrieren. Wenn die Konfigurationseinstellungen in Ihrem Admin mit dem Inhalt der Datei übereinstimmen, befolgen Sie die Anweisungen zum Generieren und Hinzufügen der `config.php`.

Wenn sie sich unterscheiden, können Sie Inhalte aus der `config.local.php` an Ihre neue `config.php` anhängen:

1. Befolgen Sie die Anweisungen zum Generieren der `config.php`.

1. Öffnen Sie die `config.php` und löschen Sie die letzte Zeile.

1. Öffnen Sie die `config.local.php` und kopieren Sie den Inhalt.

1. Fügen Sie den Inhalt in die `config.php` ein, speichern Sie ihn und fügen Sie ihn vollständig zu Git hinzu.

1. Bereitstellen in Ihren Umgebungen.

Sie schließen diese Migration nur einmal ab. Verwenden Sie nach der Migration die `config.php`.

### Gebietsschema ändern

Sie können Ihre Store-Gebietsschemata ändern, ohne einen komplexen Konfigurationsimport- und -exportprozess zu befolgen, _wenn_ Sie [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) aktiviert haben. Sie können die Gebietsschemata mithilfe von „Admin“ aktualisieren.

Sie können der Staging- oder Produktionsumgebung ein weiteres Gebietsschema hinzufügen, indem Sie `SCD_ON_DEMAND` in einer Integrationsverzweigung aktivieren, eine aktualisierte `config.php` mit den neuen Gebietsschema-Informationen generieren und die Konfigurationsdatei in die Zielumgebung kopieren.

>[!WARNING]
>
>Dieser Prozess **überschreibt** die Store-Konfiguration. Führen Sie nur dann die folgenden Schritte aus, wenn die Umgebungen dieselben Stores enthalten.

1. Aktivieren Sie in der Integrationsumgebung die `SCD_ON_DEMAND`-Variable mithilfe der [`.magento.env.yaml`-Datei](../environment/configure-env-yaml.md).

1. Fügen Sie die erforderlichen Gebietsschemata mithilfe Ihres Administrators hinzu.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden und die `/app/etc/config.php`-Datei zu generieren, die alle Gebietsschemata enthält.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Kopieren Sie die neue Konfigurationsdatei aus der Remote-Integrationsumgebung in Ihr lokales Umgebungsverzeichnis.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen, um eine Remote-Umgebung zu aktualisieren.
