---
title: Wiederherstellen einer Umgebung
description: Erfahren Sie, wie Sie die Adobe Commerce-Anwendung aus einem Cloud-Infrastrukturprojekt deinstallieren und eine Umgebung wieder in einen stabilen Zustand versetzen.
role: Developer
topic: Development
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Wiederherstellen einer Umgebung

Wenn in der Integrationsumgebung Probleme auftreten und Sie keine [gültige Sicherung](../storage/snapshots.md) haben oder die Umgebung auf ein leeres Blatt zurücksetzen möchten, können Sie Ihre Umgebung mit einer der folgenden Methoden wiederherstellen/zurücksetzen:

- Zurücksetzen oder Zurücksetzen des Codes in der Git-Verzweigung
- [!DNL Commerce] deinstallieren
- Erzwingen einer erneuten Bereitstellung
- Manuelles Zurücksetzen der Datenbank

{{stuck-deployment-tip}}

## Zurücksetzen der Git-Verzweigung

Durch Zurücksetzen der Git-Verzweigung wird der Code in der Vergangenheit in einen stabilen Status zurückgesetzt.

**So setzen Sie Ihre Verzweigung zurück**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Überprüfen Sie den Verlauf des Git-Commits. Verwenden Sie `--oneline`, um abgekürzte Commits in einer Zeile anzuzeigen:

   ```bash
   git log --oneline
   ```

   Beispielantwort:

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. Wählen Sie einen Commit-Hash, der den letzten bekannten stabilen Status Ihres Codes darstellt.

   Um Ihre Verzweigung auf den ursprünglichen initialisierten Status zurückzusetzen, suchen Sie nach dem ersten Commit, mit dem Ihre Verzweigung erstellt wurde. Sie können `--reverse` verwenden, um den Verlauf in umgekehrter chronologischer Reihenfolge anzuzeigen.

1. Verwenden Sie die Option zum Zurücksetzen der Verzweigung. Seien Sie vorsichtig mit diesem Befehl, da er alle Änderungen seit dem ausgewählten Commit verwirft.

   ```bash
   git reset --hard <commit>
   ```

1. Übertragen Sie Ihre Änderungen in eine Trigger-A-Bereitstellung, wodurch Adobe Commerce neu installiert wird.

   ```bash
   git push --force <origin> <branch>
   ```

## Commerce deinstallieren

Durch die Deinstallation der [!DNL Commerce]-Anwendung kehrt Ihre Umgebung in den Originalzustand zurück, indem Sie die Datenbank wiederherstellen, die Bereitstellungskonfiguration entfernen und die `var/` Unterverzeichnisse löschen. In dieser Anleitung wird auch die Git-Verzweigung auf einen früheren stabilen Status zurückgesetzt. Wenn Sie nicht über eine aktuelle Sicherung verfügen, aber auf die Remote-Umgebung über SSH zugreifen können, führen Sie die folgenden Schritte aus, um Ihre Umgebung wiederherzustellen:

- Deaktivieren der Konfigurationsverwaltung
- Adobe Commerce deinstallieren
- Zurücksetzen der Git-Verzweigung

Durch die Deinstallation der Adobe Commerce-Software wird die Datenbank gelöscht, die Bereitstellungskonfiguration entfernt und die `var/`-Unterverzeichnisse gelöscht. Es ist wichtig, die [Konfigurationsverwaltung](../store/store-settings.md) zu deaktivieren, damit die vorherigen Konfigurationseinstellungen bei der nächsten Bereitstellung nicht automatisch angewendet werden. Stellen Sie sicher, dass das `app/etc/`-Verzeichnis nicht die `config.php`-Datei enthält.

**Deinstallieren der Adobe Commerce-Software**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Entfernen Sie die Konfigurationsdatei.
   - Für Adobe Commerce 2.2 und höher:

     ```bash
     rm app/etc/config.php
     ```

   - Für Adobe Commerce 2.1:

     ```bash
     rm app/etc/config.local.php
     ```

1. Deinstallieren Sie die Adobe Commerce-Anwendung.

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. Bestätigen Sie, dass Adobe Commerce erfolgreich deinstalliert wurde.

   Die folgende Meldung wird angezeigt, um eine erfolgreiche Deinstallation zu bestätigen:

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. Löschen Sie die `var/` Unterverzeichnisse.

   ```bash
   rm -rf var/*
   ```

1. Abmelden.

>[!TIP]
>
>Optional ist es eine gute Praxis, Caches zu bereinigen.
>
>```bash
>magento-cloud project:clear-build-cache
>```

## Erzwingen einer erneuten Bereitstellung

Wenn Sie versucht haben, Adobe Commerce zu deinstallieren und Ihre Bereitstellung weiterhin fehlschlägt, können Sie versuchen, manuell eine erneute Bereitstellung zu erzwingen.

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## Datenbank zurücksetzen

Wenn Sie versucht haben, Adobe Commerce zu deinstallieren, und der Befehl fehlgeschlagen ist oder nicht abgeschlossen werden konnte, können Sie die Datenbank manuell zurücksetzen.

**Zurücksetzen der Datenbank**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.

1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Stellen Sie eine Verbindung zur Datenbank her.

   ```bash
   mysql -h database.internal
   ```

1. `main` ablegen.

   ```shell
   drop database main;
   ```

1. Erstellen Sie eine leere `main`.

   ```shell
   create database main;
   ```

1. Löschen Sie die folgenden Konfigurationsdateien.

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. Trigger Melden Sie sich ab und führen Sie eine erneute Bereitstellung durch.

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
