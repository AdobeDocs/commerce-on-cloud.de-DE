---
title: Datenbank sichern
description: Erfahren Sie, wie Sie mit ECE-Tools eine Sicherung der Datenbank für ein Adobe Commerce in einem Cloud-Infrastrukturprojekt erstellen.
feature: Cloud, Iaas, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Datenbank sichern

Mit dem Befehl `ece-tools db-dump` können Sie eine Kopie Ihrer Datenbank erstellen, ohne alle Umgebungsdaten von Services und Bereitstellungen zu erfassen. Standardmäßig erstellt dieser Befehl Sicherungen im `/app/var/dump-main` für alle Datenbankverbindungen, die in der Umgebungskonfiguration angegeben sind. Der DB-Dump-Vorgang schaltet die Anwendung in den Wartungsmodus, stoppt Verbraucherwarteschlangenprozesse und deaktiviert Cron-Aufträge, bevor der Dump beginnt.

Beachten Sie die folgenden Richtlinien für DB-Dump:

- Für Produktionsumgebungen empfiehlt Adobe, Datenbank-Dump-Vorgänge außerhalb der Spitzenzeiten abzuschließen, um Service-Unterbrechungen zu minimieren, die auftreten, wenn sich die Website im Wartungsmodus befindet.
- Wenn beim Dump-Vorgang ein Fehler auftritt, löscht der Befehl die Dump-Datei, um Speicherplatz zu sparen. Überprüfen Sie die Protokolle auf Details (`var/log/cloud.log`).
- Bei Pro-Produktionsumgebungen werden mit diesem Befehl nur von _einem_ der drei Knoten mit hoher Verfügbarkeit ausgegeben, sodass Produktionsdaten, die während des Speicherauszugs auf einen anderen Knoten geschrieben werden, möglicherweise nicht kopiert werden. Der Befehl generiert eine `var/dbdump.lock`-Datei, um zu verhindern, dass der Befehl auf mehr als einem Knoten ausgeführt wird.
- Für ein Backup aller Umgebungs-Services empfiehlt Adobe die Erstellung eines [Backup](snapshots.md).

Sie können mehrere Datenbanken sichern, indem Sie die Datenbanknamen an den Befehl anhängen. Im folgenden Beispiel werden zwei Datenbanken gesichert: `main` und `sales`:

```bash
php vendor/bin/ece-tools db-dump main sales
```

Verwenden Sie den Befehl `php vendor/bin/ece-tools db-dump --help` für weitere Optionen:

- `--dump-directory=<dir>` - Wählen Sie einen Zielordner für den Datenbank-Dump aus.
- `--remove-definers` - Entfernen von DEFINER-Anweisungen aus dem Datenbank-Dump

**Erstellen eines Datenbank-Dump in der Staging- oder Produktionsumgebung**:

1. [Verwenden Sie SSH, um sich anzumelden, oder erstellen Sie einen Tunnel, um eine Verbindung zur Remote-Umgebung herzustellen](../development/secure-connections.md) die die zu kopierende Datenbank enthält.

1. Listen Sie die Umgebungsbeziehungen auf und beachten Sie die Anmeldeinformationen der Datenbank.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   oder

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. Erstellen Sie eine Sicherungskopie der Datenbank. Um einen Zielordner für den DB-Dump auszuwählen, verwenden Sie die Option `--dump-directory` .

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   Beispielantwort:

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /tmp/qxmtlseakof6y/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. Der Befehl `db-dump` erstellt eine `dump-<timestamp>.sql.gz`-Archivdatei im Remote-Projektverzeichnis.

>[!TIP]
>
>Wenn Sie diese Daten in eine bestimmte Umgebung übertragen möchten, finden Sie weitere Informationen unter [Migrieren von Daten und statischen Dateien](../deploy/staging-production.md#migrate-static-files).
