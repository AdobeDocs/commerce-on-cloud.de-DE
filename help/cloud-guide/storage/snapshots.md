---
title: Backup-Verwaltung
description: Erfahren Sie, wie Sie manuell eine Sicherung für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt erstellen und wiederherstellen.
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: 3efc5478428c4ede9e2106e1cbef8362c525ccd8
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 0%

---

# Backup-Verwaltung

Sie können eine manuelle Sicherung aktiver Starter-Umgebungen jederzeit mithilfe der Schaltfläche **[!UICONTROL Backup]** im [!DNL Cloud Console] oder mithilfe des Befehls `magento-cloud snapshot:create` durchführen.

Ein Backup oder _Snapshot_ ist ein vollständiges Backup von Umgebungsdaten, das alle persistenten Daten aus laufenden Services (MySQL-Datenbank) und alle Dateien umfasst, die auf den bereitgestellten Volumes (var, pub/media, app/etc) gespeichert sind. Der Schnappschuss enthält _nicht_ Code, da der Code bereits im Git-basierten Repository gespeichert ist. Sie können keine Kopie eines Schnappschusses herunterladen.

>[!WARNING]
>
>Sicherungen enthalten normalerweise den Inhalt von bereitgestellten Verzeichnissen, einschließlich öffentlicher Webverzeichnisse wie `pub/media`, verschieben jedoch keine Backup-Ausgabedateien in öffentliche Webverzeichnisse wie `pub/media` oder `pub/static`.

Die Backup-/Snapshot-Funktion **nicht** für die Pro Staging- und Produktionsumgebungen, die standardmäßig regelmäßige Backups für die Notfallwiederherstellung erhalten. Weitere Informationen finden Sie unter [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery). Im Gegensatz zu den automatischen Live-Backups in den Pro-Staging- und Produktionsumgebungen werden die Backups **nicht** automatisch erstellt. Es liegt _Ihrer_ Verantwortung, manuell ein Backup zu erstellen oder einen Cron-Auftrag einzurichten, um regelmäßig ein Backup Ihrer Starter- oder Pro-Integrationsumgebungen zu erstellen.

## Manuelles Backup erstellen

Sie können ein manuelles Backup einer beliebigen aktiven Starter-Umgebung und Integration Pro-Umgebung aus der [!DNL Cloud Console] erstellen oder einen Snapshot aus der Cloud-CLI erstellen. Sie müssen über eine [Administratorrolle](../project/user-access.md) für die Umgebung verfügen.

**So erstellen Sie eine Sicherung einer beliebigen Starter-Umgebung mit dem[!DNL Cloud Console]**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.
1. Wählen Sie in der Projekt-Navigationsleiste eine Umgebung aus. Die Umgebung muss aktiv sein.
1. Klicken Sie in _Ansicht_ Sicherungen“ auf **[!UICONTROL Backup]**. Diese Option ist für eine Pro-Umgebung nicht verfügbar.

   ![Sicherung](../../assets/button-backup.png){width="150"}

**So erstellen Sie eine Sicherungskopie einer Integrationsumgebung mit dem[!DNL Cloud Console]**:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.
1. Wählen Sie in der Projekt-Navigationsleiste eine Integrations-/Entwicklungsumgebung aus. Die Umgebung muss aktiv sein.
1. Wählen Sie oben rechts die Option **[!UICONTROL Backup]** aus. Diese Option ist sowohl für Starter- als auch für Pro-Umgebungen verfügbar.
1. Klicken Sie auf die Schaltfläche **[!UICONTROL Yes]** .

**So erstellen Sie einen Schnappschuss mithilfe der `magento-cloud` CLI**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.
1. Checken Sie die Umgebungsverzweigung für den Snapshot aus.
1. Erstellen Sie den Snapshot.

   ```bash
   magento-cloud snapshot:create --live
   ```

   Alternativ können Sie den `magento-cloud backup` short-Befehl verwenden. Mit der Option `--live` wird die Umgebung ausgeführt, um Ausfallzeiten zu vermeiden. Um eine vollständige Liste der Optionen anzuzeigen, geben Sie `magento-cloud snapshot:create --help` ein.

   Beispielantwort:

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Überprüfen Sie die neuesten Momentaufnahmen.

   ```bash
   magento-cloud snapshot:list
   ```

   Die Liste gibt Informationen zum Snapshot-Status zurück:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

Informationen zum Erstellen eines Datenbank-Dump für eine beliebige Umgebung, einschließlich Staging und Produktion, finden Sie [ Knowledgebase-Artikel ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud) Erstellen eines Datenbank-Dump .

## Manuelles Backup wiederherstellen

Sie müssen [Administratorzugriff](../project/user-access.md) auf die Umgebung haben. Sie haben bis zu **sieben Tage**, um _manuelle Sicherung_. Beim Wiederherstellen eines Backups ändert sich der Code der aktuellen Git-Verzweigung nicht. Die Wiederherstellung eines Backups auf diese Weise gilt nicht für Pro-Staging- und Produktionsumgebungen. Siehe [Pro-Backup und Notfall-Wiederherstellung](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Die Wiederherstellungszeiten hängen von der Größe der Datenbank ab:

- Eine große Datenbank (200 GB oder mehr) kann 5 Stunden dauern.
- Mittlere Datenbank (150 GB) kann 2 1/2 Stunden dauern
- Kleine Datenbank (60 GB) kann 1 Stunde dauern

>[!TIP]
>
>Wiederherstellen ohne Sicherung:
>
>- Informationen zum Zurücksetzen auf den vorherigen Code oder zum Entfernen hinzugefügter Erweiterungen in einer Umgebung finden Sie unter [Code zurücksetzen](#roll-back-code).
>- Informationen zum Wiederherstellen einer instabilen Umgebung, die _keine)_ hat, finden Sie unter [Wiederherstellen einer Umgebung](../development/restore-environment.md).

**So stellen Sie eine Sicherung mithilfe der[!DNL Cloud Console]** wieder her:

1. Melden Sie sich beim [[!DNL Cloud Console]](https://console.adobecommerce.com) an.
1. Wählen Sie in der Projekt-Navigationsleiste eine Umgebung aus.
1. Wählen _Ansicht_ Sicherungen“ ein Backup aus der Liste _Gespeichert_ aus. Die Sicherungsfunktion gilt **nicht** für die Pro-Umgebungen.
1. Klicken Sie im Menü ![Mehr](../../assets/icon-more.png){width="32"} (_mehr_) auf **Wiederherstellen**.
1. Überprüfen Sie die Informationen Wiederherstellen aus Sicherung und klicken Sie auf **Ja, Wiederherstellen**.

**So stellen Sie einen Snapshot mithilfe der Cloud-CLI wieder her**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.
1. Checken Sie die Umgebungsverzweigung aus, um sie wiederherzustellen.
1. Listet alle verfügbaren Momentaufnahmen auf.

   ```bash
   magento-cloud snapshot:list
   ```

   Die Liste gibt Informationen zu den verfügbaren Momentaufnahmen zurück:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Wiederherstellen eines Snapshots mit der Snapshot-ID aus der Liste.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Wiederherstellen eines Snapshots zur Notfallwiederherstellung

Um den Snapshot für die Notfallwiederherstellung in Pro-Staging- und Produktionsumgebungen wiederherzustellen, [ Sie den Datenbank-Dump direkt vom Server ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Rollback-Code

Backups und Momentaufnahmen enthalten _keine_ Kopie Ihres Codes. Ihr Code ist bereits im Git-basierten Repository gespeichert, sodass Sie Git-basierte Befehle verwenden können, um Code zurückzusetzen (oder zurückzusetzen). Verwenden Sie beispielsweise `git log --oneline`, um durch frühere Commits zu scrollen; verwenden Sie dann [`git revert`](https://git-scm.com/docs/git-revert), um Code aus einem bestimmten Commit wiederherzustellen.

Außerdem können Sie Code in einer (inaktiven _Verzweigung_. Verwenden Sie Git-Befehle, um eine Verzweigung zu erstellen, anstatt `magento-cloud` Befehle zu verwenden. Siehe Informationen zu [Git-](../dev-tools/cloud-cli-overview.md#git-commands)) im Thema Cloud-CLI .
