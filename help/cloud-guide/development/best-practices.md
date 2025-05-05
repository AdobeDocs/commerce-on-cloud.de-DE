---
title: Best Practices für die Aktualisierung Ihres Projekts
description: Hier finden Sie eine Liste mit Best Practices für die Aktualisierung Ihrer Projektdateien.
feature: Cloud, Best Practices, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Best Practices für die Aktualisierung Ihres Projekts

Befolgen Sie die Best Practices für Builds und die Bereitstellung und verwenden Sie den [Upgrades und Patches](../development/commerce-version.md), um Ihre Anwendung zu aktualisieren. Befolgen Sie die folgenden Richtlinien, um Ihre Upgrade- und Post-Upgrade-Arbeiten zu planen:

- **Projekt sichern**-Sichern Sie die Datenbank in Integrations-, Staging- und Produktionsumgebungen, bevor Sie die Adobe Commerce und alle Drittanbieter- oder benutzerdefinierten Erweiterungen aktualisieren. Siehe [Datenbank sichern](../development/commerce-version.md#project-backup).

- **Prüfung auf Kompatibilitätsprobleme**-

   - Sicherstellen, dass alle benutzerdefinierten Designs mit der neuen Adobe Commerce-Version kompatibel sind

   - Verwenden Sie nach dem Upgrade von Drittanbieter- und benutzerdefinierten Erweiterungen den Befehl `magento-cloud local:build` , um die Abhängigkeiten von Composer vor der Bereitstellung zu überprüfen.

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

      - Überprüfen Sie den Indexerstatus und indizieren Sie ihn nach Bedarf neu. Siehe [Indexer verwalten](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=de) im _Konfigurationshandbuch_.

      - Überprüfen Sie die `cron` und die `cron_schedule` in der Adobe Commerce-Datenbank, um den Cron-Status zu überprüfen, und führen Sie Cron-Aufträge bei Bedarf erneut aus.
Siehe [Protokollierung](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=de#logging) im _Konfigurationshandbuch_.

   - Führen Sie nach dem Upgrade Benutzerakzeptanztests für Staging- und Produktionsumgebungen durch und beheben Sie alle Probleme im Zusammenhang mit Upgrades von Drittanbietern und benutzerdefinierten Erweiterungen.
