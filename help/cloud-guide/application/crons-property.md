---
title: Crons-Eigenschaft
description: Siehe Beispiele zum Konfigurieren der Eigenschaft „crons“ in der Konfigurationsdatei  [!DNL Commerce] application“.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Crons-Eigenschaft

Adobe Commerce verwendet die `crons`-Eigenschaft zum Planen sich wiederholender Aktivitäten. Dies eignet sich ideal für die Planung einer bestimmten Aufgabe, die zu bestimmten Tageszeiten ausgeführt werden soll. Aufgrund der Beschaffenheit schreibgeschützter Umgebungen kann in der Web-Instanz für Adobe Commerce jeweils nur ein Cron-Auftrag für Cloud-Infrastrukturprojekte ausgeführt werden. Es empfiehlt sich, langwierige Aufgaben in kleinere Aufgaben in der Warteschlange aufzuteilen. Alternativ können Sie eine [Worker-Instanz“ ](workers-property.md).

Adobe empfiehlt, `crons` als [Dateisysteminhaber“ ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=de). Führen _nicht_ `crons` als `root` oder als Webserverbenutzer aus.

Diese Konfiguration unterscheidet sich von lokalen Bereitstellungen von Adobe Commerce, die über mehrere standardmäßige Cron-Aufträge verfügen. Siehe [Konfigurieren von Cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=de) im _Konfigurationshandbuch_.

## Einrichten von Cron-Aufträgen

Die `crons` Eigenschaft beschreibt Prozesse, die nach einem Zeitplan ausgelöst werden. Für jeden Auftrag sind ein Name und die folgenden Optionen erforderlich:

- `spec` - Der für die Planung verwendete Cron-Ausdruck.
- `cmd` - Der Befehl, der auf `start` und `stop` ausgeführt werden soll.
- `shutdown_timeout`—(_Optional_) Wenn ein Cron-Auftrag abgebrochen wird, ist dies die Anzahl der Sekunden, nach denen ein SIGKILL-Signal gesendet wird, um den Auftrag oder Prozess zu stoppen. Der Standardwert ist 10 Sekunden.
- `timeout` - (_Optional_) Die maximale Zeit, die ein Cron-Auftrag vor einer Zeitüberschreitung ausgeführt werden kann. Standardmäßig ist der maximal zulässige Wert von 86400 Sekunden (24 Stunden) festgelegt.

Standardmäßig weist jedes Commerce-Cloud-Projekt die folgende `crons` in der `.magento.app.yaml` auf:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Wenn für Ihr Projekt benutzerdefinierte Cron-Aufträge erforderlich sind, können Sie sie zur standardmäßigen `crons` hinzufügen. Siehe [Erstellen eines Cron-Auftrags](#build-a-cron-job).

### `crontab`

Adobe Commerce hat Pro-Projekten eine Konfigurationsoption „auto-crons“ hinzugefügt, um die Self-Service-`crons` in den Staging- und Produktionsumgebungen zu unterstützen. Wenn diese Option aktiviert ist, können Sie `crontab` verwenden, um die Cron-Konfiguration zu überprüfen. Dies ist _nicht_ bei Starter-Projekten verfügbar.

Obwohl Sie `crontab` verwenden können, um die Konfiguration in Pro-Projekten zu überprüfen, verwendet Adobe Commerce keine `crontab`, um Cron-Aufträge für Sites auszuführen, die in der Cloud-Infrastruktur bereitgestellt sind.

**So überprüfen Sie die Cron-Konfiguration in Pro-Umgebungen**:

1. Verwenden Sie [SSH](../development/secure-connections.md#use-an-ssh-command), um sich bei der Remote-Umgebung anzumelden.

1. Auflisten der geplanten Cron-Prozesse.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Wenn der Befehl `crontab -l` einen `Command not found` zurückgibt (nur in Pro-Staging- und Produktionsumgebungen), müssen Sie [ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um die Konfigurationsoption „Self-Service-Crons“ für Ihr Projekt zu aktivieren.

Das folgende Beispiel zeigt die `crontab` Ausgabe für eine Umgebung, die nur die standardmäßige `crons`-Konfiguration hat:

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Erstellen eines Cron-Vorgangs

Ein Cron-Auftrag enthält den Zeitplan und die Zeitvorgabe sowie den Befehl zur Ausführung zur geplanten Zeit. Bei Starter- und Pro `integration`-Umgebungen beträgt das Mindestintervall einmal pro fünf Minuten. Bei Pro-Staging- und Produktionsumgebungen beträgt das Mindestintervall einmal pro Minute. In Adobe Commerce in der Cloud-Infrastruktur fügen Sie der `.magento.app.yaml` im Abschnitt `crons` benutzerdefinierte Cron-Aufträge hinzu. Das allgemeine Format ist für die Planung `spec` und `cmd`, um den auszuführenden Befehl oder das auszuführende benutzerdefinierte Skript anzugeben.

### Technische Daten

Adobe Commerce verwendet einen Ausdruck mit fünf Werten für eine `crons` (Spezifikation): `* * * * *`

1. Minute (0 bis 59) Für alle Starter- und Pro-Umgebungen beträgt die für Cron-Aufträge unterstützte Mindestfrequenz fünf Minuten. Möglicherweise müssen Sie die Einstellungen in Ihrem Administrator konfigurieren.
2. Stunde (0 bis 23)
3. Tag des Monats (1 bis 31)
4. Monat (1 bis 12)
5. Wochentag (0 bis 6) (Sonntag bis Samstag; 7 ist auf einigen Systemen auch Sonntag)

Einige Beispiele:

- `00 */3 * * *` wird alle drei Stunden in der ersten Minute ausgeführt (12:00 Uhr, 3:00 Uhr, 6:00 Uhr)
- `20 */8 * * *` wird alle 8 Stunden um Minute 20 ausgeführt (12:20 Uhr, 8:20 Uhr, 16:20 Uhr)
- `00 00 * * *` läuft einmal täglich um Mitternacht
- `00 * * * 1` läuft einmal pro Woche am Montag um Mitternacht.

>[!NOTE]
>
>Die in der `.magento.app.yaml` angegebene `crons` basiert auf der Zeitzone des Servers, nicht auf der Zeitzone, die in den Speicherkonfigurationswerten in der Datenbank angegeben ist.

Berücksichtigen Sie bei der Festlegung der Zeitplanung die Zeit, die zum Abschließen der Aufgabe benötigt wird. Wenn Sie beispielsweise einen Auftrag alle drei Stunden ausführen und die Aufgabe 40 Minuten in Anspruch nimmt, können Sie den geplanten Zeitpunkt ändern.

### Befehl

Die `cmd` gibt den auszuführenden Befehl oder das auszuführende benutzerdefinierte Skript an. Das Befehlsskriptformat kann Folgendes enthalten:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Beispiel:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

In diesem Beispiel ist `<path-to-php-binary>` `/usr/bin/php`. Das Installationsverzeichnis, das die Projekt-ID enthält, wird `/app/abc123edf890/bin/magento`, und die Skriptaktion wird `export:start catalog_category_product`.

### Hinzufügen benutzerdefinierter Cron-Aufträge zu Ihrem Projekt

Auf der Adobe Commerce auf der Cloud-Infrastrukturplattform können Sie Anpassungen zum `crons` Abschnitt der [`.magento.app.yaml`](../application/configure-app-yaml.md) hinzufügen.

>[!NOTE]
>
>Bei Starter- und Pro `integration`-Umgebungen beträgt das Mindestintervall einmal pro fünf Minuten. Bei Pro-Staging- und Produktionsumgebungen beträgt das Mindestintervall einmal pro Minute. Es können keine häufigeren Intervalle als die standardmäßigen Mindestwerte konfiguriert werden.

In Adobe Commerce Pro-Projekten muss die [Funktion für automatisches ](#set-up-cron-jobs)&quot; in Ihrem Projekt aktiviert sein, bevor Sie benutzerdefinierte Cron-Aufträge mithilfe der `.magento.app.yaml`-Datei zu Staging- und Produktionsumgebungen hinzufügen können. Wenn diese Funktion nicht aktiviert ist, [ Sie &quot;Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket)&quot;, um automatische Klicks zu aktivieren.

**So fügen Sie benutzerdefinierte Cron-Aufträge**:

1. Bearbeiten Sie in Ihrer lokalen Entwicklungsumgebung die `.magento.app.yaml` im `/app` von Adobe Commerce.

1. Fügen Sie im Abschnitt `crons` Ihre Anpassung im folgenden Format hinzu:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   Im folgenden Beispiel exportiert der `productcatalog` den Produktkatalog alle acht Stunden und 20 Minuten nach der vollen Stunde.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Cron-Aufträge aktualisieren

Um einen benutzerdefinierten Auftrag hinzuzufügen, zu entfernen oder zu aktualisieren, ändern Sie die Konfiguration im Abschnitt `crons` der `.magento.app.yaml`. Testen Sie dann die Aktualisierungen in der Remote-`integration`-Umgebung, bevor Sie die Änderungen in die Staging- und Produktionsumgebungen pushen.

## Cron-Aufträge deaktivieren

Sie können Cron-Aufträge manuell deaktivieren, bevor Sie Wartungsaufgaben wie die Neuindizierung oder die Cache-Bereinigung durchführen, um Leistungsprobleme zu vermeiden. Sie können die `ece-tools` CLI-`cron:disable` verwenden, um alle Cron-Aufträge zu deaktivieren und alle aktiven Cron-Prozesse zu stoppen.

**So deaktivieren Sie Cron-Aufträge**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Deaktivieren Sie Cron-Aufträge und stoppen Sie aktive Cron-Prozesse.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Nachdem Sie alle erforderlichen Wartungsaufgaben abgeschlossen haben, stellen Sie sicher, dass Sie die Cron-Aufträge erneut aktivieren.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Fehlerbehebung bei Cron-Aufträgen

Adobe hat das Adobe Commerce on Cloud Infrastructure-Paket aktualisiert, um die Cron-Verarbeitung auf der Adobe Commerce on Cloud Infrastructure-Plattform zu optimieren und Probleme mit Cron zu beheben. Wenn bei der Cron-Verarbeitung Probleme auftreten, stellen Sie sicher, dass Ihr Projekt die aktuelle Version des `ece-tools` verwendet. Siehe [Aktualisieren der ECE-Tools](../dev-tools/update-package.md).

Sie können Informationen zur Cron-Verarbeitung für jede Umgebung in den Protokolldateien auf Anwendungsebene überprüfen. Siehe [Anwendungsprotokolle](../test/log-locations.md#application-logs).

In den folgenden Adobe Commerce-Support-Artikeln finden Sie Hilfe bei der Fehlerbehebung bei Problemen im Zusammenhang mit Cron:

- [Cron-Aufgaben sperren Aufgaben von anderen Gruppen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html?lang=de)

- [Setzt hängengebliebene Cron-Aufträge manuell in der Cloud zurück](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html?lang=de)
