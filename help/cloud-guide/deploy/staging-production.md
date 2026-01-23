---
title: Bereitstellung für Staging und Produktion
description: Erfahren Sie, wie Sie Ihren Adobe Commerce auf Cloud-Infrastruktur-Code in den Staging- und Produktionsumgebungen bereitstellen, um ihn weiter zu testen.
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
source-git-commit: fe634412c6de8325faa36c07e9769cde0eb76c48
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%

---

# Bereitstellung für Staging und Produktion

Der Prozess für die Bereitstellung und das Go-Live beginnt mit der Entwicklung, setzt die Staging-Phase fort und endet mit der Live-Schaltung in der Produktion. Adobe bietet eine End-to-End-Umgebungslösung, um konsistente Konfigurationen zu gewährleisten. Jede Umgebung unterstützt den direkten URL-Zugriff auf die Storefront sowie den Admin- und SSH-Zugriff für CLI-Befehle.

Wenn Sie bereit sind, Ihren Store bereitzustellen, müssen Sie die Bereitstellung und Tests in der Staging-Umgebung abschließen, bevor Sie sie in der Produktion bereitstellen. Dieser Abschnitt enthält detaillierte Anweisungen und Informationen zum Build- und Bereitstellungsprozess, zum Migrieren von Daten und Inhalten sowie zu Tests.

>[!TIP]
>
>Adobe empfiehlt, vor der Bereitstellung ein [Backup](../storage/snapshots.md) der Umgebung zu erstellen.

Außerdem können Sie &quot;[&#x200B; mit New Relic verfolgen“ aktivieren](../monitor/track-deployments.md) um Bereitstellungsereignisse zu überwachen und bei der Leistungsanalyse zwischen Bereitstellungen zu helfen.

## Starter-Bereitstellungsfluss

Adobe empfiehlt die Erstellung einer `staging` Verzweigung aus der `master` Verzweigung, um die Entwicklung und Bereitstellung Ihres Starterplans optimal zu unterstützen. Dann sind zwei Ihrer vier aktiven Umgebungen bereit: `master` für die Produktion und `staging` für das Staging.

Ausführliche Informationen zum Prozess finden Sie unter [Workflow für die Entwicklung und Bereitstellung](../architecture/starter-develop-deploy-workflow.md).

## Pro-Bereitstellungsfluss

Pro verfügt über eine große Integrationsumgebung mit zwei aktiven Verzweigungen, einer globalen `master`-, Staging- und Produktionsverzweigung. Wenn Sie Ihr Projekt erstellen, kann der Code verzweigen, entwickeln und für das Erstellen und Bereitstellen Ihrer Site pushen. Obwohl die Integrationsumgebung viele Verzweigungen haben kann, verfügen Staging und Produktion für jede Umgebung nur über eine Verzweigung.

Ausführliche Informationen zum Prozess finden Sie unter [Workflow für Entwicklung und Bereitstellung von Pro](../architecture/pro-develop-deploy-workflow.md).

## Bereitstellen von Code für das Staging

Die Staging-Umgebung bietet eine produktionsnahe Umgebung mit einer Datenbank, einem Webserver und allen Services, einschließlich Fastly und New Relic. Sie können vollständig über die [[!DNL Cloud Console]](../project/overview.md) oder (Cloud-CLI[Befehle) &#x200B;](../dev-tools/cloud-cli-overview.md) eine Terminal-Anwendung übertragen, zusammenführen und bereitstellen.

### Bereitstellen von Code mit dem [!DNL Cloud Console]

Die [!DNL Cloud Console] bietet Funktionen zum Erstellen, Verwalten und Bereitstellen von Code in Integrations-, Staging- und Produktionsumgebungen für Starter- und Pro-Pläne.

**Stellen Sie bei Pro-Projekten die Integrationsverzweigung für das Staging bereit**:

1. [Anmelden](https://accounts.magento.cloud) bei Ihrem Projekt.
1. Wählen Sie die `integration` Verzweigung aus.
1. Wählen Sie die **Zusammenführen**-Option aus, um sie für das Staging bereitzustellen.

   ![Zusammenführen](../../assets/button-merge.png){width="150"}

1. Schließen Sie alle [Tests](../test/staging-and-production.md) in der Staging-Umgebung ab.
1. Wenn die Staging-Umgebung fertig ist, wählen Sie die Option **Zusammenführen**, um sie für die Produktion bereitzustellen.

**Stellen Sie zunächst die Entwicklungsverzweigung für das Staging bereit**:

1. [Anmelden](https://accounts.magento.cloud) bei Ihrem Projekt.
1. Wählen Sie die vorbereitete Code-Verzweigung aus.
1. Wählen Sie die **Zusammenführen**-Option aus, um sie für das Staging bereitzustellen.

   ![Zusammenführen](../../assets/button-merge.png){width="150"}

1. Schließen Sie alle [Tests](../test/staging-and-production.md) in der Staging-Umgebung ab.
1. Wenn die Staging-Umgebung fertig ist, wählen Sie die Option **Zusammenführen**, um sie für die Produktion (`master`) bereitzustellen.

### Bereitstellen von Code über die Befehlszeile

Die Cloud-CLI stellt Befehle zum Bereitstellen von Code bereit. Sie benötigen SSH- und Git-Zugriff auf Ihr Projekt.

#### Schritt 1: Bereitstellen und Testen der Integrationsumgebung

1. Sehen Sie sich nach der Anmeldung beim Projekt die Integrationsumgebung an:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synchronisieren Sie Ihre lokale Integrationsumgebung mit der Remote-Umgebung:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Erstellen Sie einen Schnappschuss der Umgebung als Backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Aktualisieren Sie den Code in Ihrer lokalen Verzweigung nach Bedarf.

1. Hinzufügen, Übertragen und Übertragen von Änderungen in die Umgebung.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Vollständige Site-Tests.

#### Schritt 2: Zusammenführen von Änderungen in der Staging-Umgebung und Bereitstellung

1. Checken Sie die Staging-Umgebung aus:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synchronisieren Sie Ihre lokale Staging-Umgebung mit der Remote-Umgebung:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Erstellen Sie einen Schnappschuss der Umgebung als Backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Zusammenführen der Integrationsumgebung mit der Staging-Umgebung zur Bereitstellung:

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Vollständige Site-Tests.

#### Schritt 3: Bereitstellung für Produktion

1. Auschecken, synchronisieren und einen Schnappschuss der lokalen Produktionsumgebung erstellen.

1. Zusammenführen der Staging-Umgebung mit der Produktionsumgebung zur Bereitstellung:

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Vollständige Site-Tests.

## Statische Dateien migrieren

[Statische Dateien](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) werden in `mounts` gespeichert. Es gibt zwei Methoden zum Migrieren von Dateien von einem Quell-Bereitstellungs-Speicherort, wie z. B. Ihrer lokalen Umgebung, zu einem Ziel-Bereitstellungs-Speicherort. Bei beiden Methoden wird das Dienstprogramm `rsync` verwendet, Adobe empfiehlt jedoch die Verwendung der `magento-cloud` CLI zum Verschieben von Dateien zwischen der lokalen und der Remote-Umgebung. Außerdem empfiehlt Adobe die Verwendung der `rsync`-Methode beim Verschieben von Dateien von einer Remote-Quelle an einen anderen Remote-Speicherort.

### Migrieren von Dateien mithilfe der CLI

Mit den Befehlen `mount:upload` und `mount:download` CLI können Sie Dateien zwischen der lokalen und der Remote-Umgebung migrieren. Beide Befehle verwenden das `rsync`, aber die CLI-Befehle bieten Optionen und Eingabeaufforderungen, die auf die Adobe Commerce in der Cloud-Infrastrukturumgebung zugeschnitten sind. Wenn Sie beispielsweise den einfachen Befehl ohne Optionen verwenden, werden Sie von der CLI aufgefordert, die Bereitstellung bzw. Bereitstellungen auszuwählen, die hochgeladen oder heruntergeladen werden sollen.

```bash
magento-cloud mount:download
```

Beispielantwort:

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**So laden Sie Dateien aus einem lokalen `pub/media/` in den Remote-`pub/media/` für die aktuelle Umgebung hoch**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Beispielantwort:

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Verwenden Sie die Option `--help` für die Befehle `mount:upload` und `mount:download` , um weitere Optionen anzuzeigen. Beispielsweise gibt es eine `--delete` Option, um irrelevante Dateien während der Migration zu entfernen.

### Migrieren von Dateien mithilfe von rsync

Alternativ können Sie das `rsync`-Dienstprogramm zum Migrieren von Dateien verwenden.

```bash
rsync -azvP <source> <destination>
```

Dieser Befehl verwendet die folgenden Optionen:

- `a`
- Dateien während der Migration `z` komprimieren
- `v`-verbose
- `P` Fortschritt

Siehe [rsync](https://linux.die.net/man/1/rsync)-Hilfe.

>[!NOTE]
>
>Um Medien direkt von Remote-zu-Remote-Umgebungen zu übertragen, müssen Sie die SSH-Agent-Weiterleitung aktivieren (siehe GitHub[Anleitung](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Direktes Migrieren statischer Dateien von Remote- zu Remote-Umgebungen (schneller Ansatz)**:

1. Verwenden Sie SSH, um sich bei der Quellumgebung anzumelden. Verwenden Sie nicht die `magento-cloud` CLI. Die Verwendung der Option `-A` ist wichtig, da sie die Weiterleitung der Authentifizierungsagenten-Verbindung ermöglicht.

   >[!TIP]
   >
   >Um den Link **SSH-Zugriff** in Ihrer [!DNL Cloud Console] zu finden, wählen Sie die Umgebung aus und klicken Sie auf **Site-Zugriff**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Verwenden Sie den Befehl `rsync` , um den `pub/media`-Ordner aus Ihrer Quellumgebung in eine andere Remote-Umgebung zu kopieren.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Melden Sie sich bei der anderen Remote-Umgebung an, um zu überprüfen, ob die Dateien erfolgreich migriert wurden.

## Migrieren der Datenbank

>[!WARNING]
>
>Der Import und Export von Datenbanken kann lange dauern, was sich auf die Leistung und Verfügbarkeit der Site auswirken kann. Planen Sie Import- und Exportvorgänge außerhalb der Spitzenzeiten, um eine langsame Leistung oder Ausfälle an den Produktions-Sites zu verhindern.

>[!BEGINSHADEBOX]

**Voraussetzung:** Ein Datenbank-Dump (siehe Schritt 3) sollte Datenbank-Trigger enthalten. Um sie zu entladen, bestätigen Sie, dass Sie die Berechtigung [TRIGGER haben](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>Die Datenbank der Integrationsumgebung dient ausschließlich Entwicklungstests und kann Daten enthalten, die nicht in die Staging- und Produktionsumgebung migriert werden sollen.

Bei kontinuierlichen Integrationsbereitstellungen wird von Adobe **nicht empfohlen** Daten von Integration zu Staging und Produktion zu migrieren. Sie können Testdaten bestehen oder wichtige Daten überschreiben. Alle wichtigen Konfigurationen werden mit der [Konfigurationsdatei](../store/store-settings.md) und `setup:upgrade` Befehl während der Erstellung und Bereitstellung übergeben.

>[!ENDSHADEBOX]

Adobe **empfiehlt** Daten aus der Produktion in die Staging-Umgebung zu migrieren, um Ihre Site vollständig zu testen und in einer produktionsnahen Umgebung mit allen Services und Einstellungen zu speichern.

>[!NOTE]
>
>Um Medien direkt von Remote-zu-Remote-Umgebungen zu übertragen, müssen Sie die SSH-Agent-Weiterleitung aktivieren (siehe [-Anleitung](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Datenbank sichern

Es empfiehlt sich, eine Sicherungskopie der Datenbank zu erstellen. Das folgende Verfahren basiert auf den Anweisungen von [Sichern der Datenbank](../storage/database-dump.md).

**So entladen Sie die Datenbank**:

1. [Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden](../development/secure-connections.md#use-an-ssh-command) die die zu kopierende Datenbank enthält.

1. Listen Sie die Umgebungsbeziehungen auf und beachten Sie die Anmeldeinformationen der Datenbank.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Bei Pro Staging und Produktion befindet sich der Name der Datenbank in der Variablen `MAGENTO_CLOUD_RELATIONSHIPS` (in der Regel identisch mit dem Anwendungsnamen und dem Benutzernamen).

1. Erstellen Sie eine Sicherungskopie der Datenbank. Um einen Zielordner für den DB-Dump auszuwählen, verwenden Sie die Option `--dump-directory` .

   Verwenden Sie für Starter-Umgebungen und Pro-Integrationsumgebungen `main` als Namen der Datenbank:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Dump-Optionen:
   - `--dump-directory=<dir>` - Wählen Sie einen Zielordner für den Datenbank-Dump aus.
   - `--remove-definers` - Entfernen von DEFINER-Anweisungen aus dem Datenbank-Dump

1. Obwohl die ECE-Tools-Methode bevorzugt wird, besteht eine andere Methode darin, eine Datenbank-Dump-Datei mit nativem MySQL im GZIP-Format zu erstellen.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Wenn Sie die Zwei-Faktor-Authentifizierung für die Zielumgebung konfiguriert haben, ist es besser, verwandte 2FA-Tabellen auszuschließen, um zu vermeiden, dass sie nach der Datenbankmigration neu konfiguriert wird:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Geben Sie `logout` ein, um die SSH-Verbindung zu beenden.

### Datenbank ablegen und neu erstellen

Beim Importieren von Daten müssen Sie eine Datenbank ablegen und erstellen.

**So legen Sie die Datenbank ab und erstellen sie neu**:

1. Richten Sie einen [SSH-](../development/secure-connections.md#ssh-tunneling)) in die Remote-Umgebung ein.

1. Verbindung mit dem Datenbank-Service herstellen.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. Ziehen Sie an der `MariaDB [main]>` die Datenbank ab.

   Für die Integration von Starter und Pro:

   ```shell
   drop database main;
   ```

   Für Produktions- und Staging-Umgebungen:

   ```shell
   drop database <database_name>;
   ```

1. Datenbank neu erstellen.

   Für die Integration von Starter und Pro:

   ```shell
   create database main;
   ```

1. Importieren Sie die Datenbank.

   Zur Produktion importieren:

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Zum Staging importieren:

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Mit diesen Befehlen wird die Datenbank-Dump-Datei dekomprimiert, die `DEFINER`-Anweisungen entfernt und die Datenbank mit den angegebenen Anmeldeinformationen importiert.
