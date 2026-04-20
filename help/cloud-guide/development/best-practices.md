---
title: Best Practices für die Aktualisierung Ihres Projekts
description: Hier finden Sie eine Liste mit Best Practices für die Aktualisierung Ihrer Projektdateien.
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
source-git-commit: 31494a956babaf15320d0ffa86fcba9e845d53a1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Best Practices für die Aktualisierung Ihres Projekts

Befolgen Sie die Best Practices für Builds und die Bereitstellung und verwenden Sie den [Upgrades und Patches](../development/commerce-version.md), um Ihre Anwendung zu aktualisieren. Befolgen Sie die folgenden Richtlinien, um Ihre Upgrade- und Post-Upgrade-Arbeiten zu planen:

- **Projekt sichern**-Sichern Sie die Datenbank in Integrations-, Staging- und Produktionsumgebungen, bevor Sie die Adobe Commerce und alle Drittanbieter- oder benutzerdefinierten Erweiterungen aktualisieren. Siehe [Datenbank sichern](../development/commerce-version.md#project-backup).

- **Prüfung auf Kompatibilitätsprobleme**-

   - Sicherstellen, dass alle benutzerdefinierten Designs mit der neuen Adobe Commerce-Version kompatibel sind

   - Verwenden Sie nach dem Upgrade von Drittanbieter- und benutzerdefinierten Erweiterungen den Befehl `magento-cloud local:build` , um die Abhängigkeiten von Composer vor der Bereitstellung zu überprüfen, und führen Sie das Tool [Upgrade-Kompatibilität](#use-the-upgrade-compatibility-tool) aus, um Inkompatibilitäten auf Code-Ebene zwischen Ihrer aktuellen Version und der Zielversion zu identifizieren. Verwenden Sie dann das [Upgrade-Kompatibilitätstool](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool) um Inkompatibilitäten auf Code-Ebene zu identifizieren und zu priorisieren, bevor Sie sie in der Integration, Staging oder Produktion bereitstellen.

   - Lesen Sie die Versionshinweise und Erweiterungsdokumentation zu Adobe Commerce , um sicherzustellen, dass Sie alle Abhilfemaßnahmen oder Konfigurationsänderungen implementiert haben, die erforderlich sind, um bekannte funktionale Probleme und Fehler im Zusammenhang mit der Aktualisierung von Adobe Commerce und seinen Erweiterungen zu beheben.

   - Stellen Sie sicher, dass die installierten Service-Versionen mit der neuen Adobe Commerce-Version kompatibel sind, und aktualisieren Sie die Services nach Bedarf. Siehe [Services](../services/services-yaml.md).

   - Testen Sie Ihre Datenbank, um alle Probleme zu beheben, die durch Aktualisierungen der Adobe Commerce-Version und -Erweiterungen entstehen.

   - Nehmen Sie alle erforderlichen Aktualisierungen an umgebungsspezifischen Einstellungen vor, bevor Sie sie in der Remote-Umgebung bereitstellen.

   - Stellen Sie sicher, dass die Version des Suchdienstes mit der PHP-Client-Version kompatibel ist. Siehe [Elasticsearch einrichten](../services/elasticsearch.md) oder [OpenSearch einrichten](../services/opensearch.md).

- **Überprüfen der Datenbankkonnektivität und des verfügbaren Speichers in Remote-Umgebungen**-

   - Verwenden Sie SSH, um sich beim Remote-Server anzumelden und die Verbindung zur MySQL-Datenbank zu überprüfen. Siehe [Verbindung zur Datenbank herstellen](../services/mysql.md#connect-to-the-database).

   - Überprüfen des verfügbaren Speichers in der Remote-Umgebung - Verwenden Sie den Befehl `disk free`, um den verfügbaren Speicherplatz in Ihren Cloud-Umgebungen anzuzeigen und zu verwalten. Siehe [Verwalten von Festplattenspeicher](../storage/manage-disk-space.md).

      - Überprüfen Sie die Größe der upgegradeten Datenbank und stellen Sie sicher, dass der `services.yaml` genügend Speicherplatz zugewiesen ist.

      - Freier Speicherplatz - Löschen Sie den Cache und bereinigen Sie die `/log`- und `/tmp` vor der Bereitstellung.

- **Planung und Durchführung eines erfolgreichen Upgrades für lokale und Integrationsumgebungen vor der Bereitstellung im Staging**-Nach dem Upgrade sollten Sie Ihre Bereitstellung testen und alle Probleme beheben.

- **Führen Sie Code zu Staging und dann zu Produktion zusammen** Testen Sie alle Probleme in der Staging-Umgebung und lösen Sie sie, bevor Sie Änderungen in die Produktionsumgebung übertragen.

- **Aufgaben nach dem Upgrade**-

   - Verwenden Sie SSH, um sich beim Remote-Server anzumelden und Folgendes zu überprüfen:

      - Überprüfen Sie den Indexerstatus und indizieren Sie ihn nach Bedarf neu. Siehe [Indexer verwalten](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html) im _Konfigurationshandbuch_.

      - Überprüfen Sie die `cron` und die `cron_schedule` in der Adobe Commerce-Datenbank, um den Cron-Status zu überprüfen, und führen Sie Cron-Aufträge bei Bedarf erneut aus.
Siehe [Protokollierung](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging) im _Konfigurationshandbuch_.

   - Führen Sie nach dem Upgrade Benutzerakzeptanztests für Staging- und Produktionsumgebungen durch und beheben Sie alle Probleme im Zusammenhang mit Upgrades von Drittanbietern und benutzerdefinierten Erweiterungen.

## Verwenden des Upgrade-Kompatibilitäts-Tools

Führen Sie das Upgrade-Kompatibilitäts-Tool (UCT) im Rahmen Ihrer Analyse vor dem Upgrade aus, um den Umfang und die Auswirkungen eines Upgrades zu verstehen.

- UCT vergleicht Ihre aktuelle Instanz mit einer Adobe Commerce-Zielversion und gibt eine Liste kritischer Probleme, Fehler und Warnungen zurück, die vor einem Upgrade behoben werden müssen.
- Verwenden Sie `--coming-version (-c)`, um sie mit Ihrer geplanten Zielversion zu vergleichen, und `--ignore-current-version-compatibility-issues` Sie sich nur auf neue Probleme konzentrieren, die durch das Upgrade eingeführt werden.
- Behandeln Sie den UCT HTML-Bericht als Eingabe für Ihre Upgrade-Checkliste neben der Erweiterungskompatibilität, den Service-Versionen und den Datenbankprüfungen.

Details zur Einrichtung und Verwendung finden Sie unter:

- [Überblick über das Upgrade-Kompatibilitäts-Tool](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [Ausführen des Kompatibilitäts-Tools für Aktualisierungen](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

Für Cloud-Händler, die das Site-Wide Analysis Tool verwenden, können Sie auch UCT über das Dashboard Trigger und den HTML-Bericht direkt aus dem Widget herunterladen. Siehe Integrieren des [Site-Wide Analysis Tool](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool).
