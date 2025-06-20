---
title: Anzeigen und Verwalten von Protokollen
description: Machen Sie sich mit den in der Cloud-Infrastruktur verfügbaren Protokolldateitypen vertraut und erfahren Sie, wo Sie sie finden.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: 731cc36816afdb5374269e871d337e056a71c050
workflow-type: tm+mt
source-wordcount: '1205'
ht-degree: 0%

---

# Anzeigen und Verwalten von Protokollen

Protokolle für Adobe Commerce in Cloud-Infrastrukturprojekten sind nützlich, um Probleme im Zusammenhang mit [Build- und Bereitstellungs-](../application/hooks-property.md)), Cloud Services und der Adobe Commerce-Anwendung zu beheben.

Sie können die Protokolle aus dem Dateisystem, dem [!DNL Cloud Console] und der `magento-cloud` CLI anzeigen.

- **Dateisystem**: Das `/var/log` Systemverzeichnis enthält Protokolle für alle Umgebungen. Das `var/log/`-Verzeichnis enthält App-spezifische Protokolle, die für eine bestimmte Umgebung eindeutig sind. Diese Verzeichnisse werden nicht zwischen Knoten in einem Cluster freigegeben. In Pro-Produktions- und Staging-Umgebungen müssen Sie die Protokolle auf jedem Knoten überprüfen.

- **[!DNL Cloud Console]**: Protokollinformationen zu Erstellung, Bereitstellung und Nachbereitstellung werden in der Liste &quot;_&quot; der_ angezeigt.

- **Cloud-CLI** - Sie können lokale Umgebungsprotokolle mit dem Befehl `magento-cloud log` oder Remote-Umgebungsprotokolle mit dem Befehl `magento-cloud ssh` anzeigen.

## Speicherorte protokollieren

Systemprotokolle werden an den folgenden Speicherorten gespeichert:

- Integration: `/var/log/<log-name>.log`
- Pro-Staging: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Pro Production: `/var/log/platform/<project-ID>/<log-name>.log`

Der Wert von `<project-ID>` hängt vom Projekt ab und davon, ob es sich um eine Staging- oder eine Produktionsumgebung handelt. Bei einer Projekt-ID von `yw1unoukjcawe` wird beispielsweise der Benutzer der Staging-Umgebung `yw1unoukjcawe_stg` und der Benutzer der Produktionsumgebung `yw1unoukjcawe`.

Unter Verwendung dieses Beispiels lautet das Bereitstellungsprotokoll: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Remote-Umgebungsprotokolle anzeigen

Die meisten Protokolle enthalten Ereignisse, die in der Remote-Umgebung auftreten. Für Pro gibt es mehrere Knoten und jeder Knoten verfügt über eindeutige Protokolle. Verwenden Sie Folgendes, um eine Liste aller Hosts anzuzeigen:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Beispielantwort:

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**So zeigen Sie eine Liste der Remote-Umgebungsprotokolle an**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Beispiel für Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**So zeigen Sie ein Remote-Protokoll**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Beispiel für Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Für Pro Staging- und Pro-Produktionsumgebungen sind automatische Protokollrotation, -komprimierung und -entfernung für Protokolldateien mit festem Dateinamen aktiviert. Jeder Protokolldateityp hat ein rotierendes Muster und eine rotierende Lebensdauer.
>&#x200B;>Ausführliche Informationen zur Protokollrotation der Umgebung und zur Lebensdauer komprimierter Protokolle finden Sie in: `/etc/logrotate.conf` und `/etc/logrotate.d/<various>`.
>&#x200B;>Für Pro Staging- und Pro-Produktionsumgebungen müssen Sie [ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um Änderungen an der Protokollrotationskonfiguration anzufordern.

>[!TIP]
>
>Die Protokollrotation kann in Pro Integration-Umgebungen nicht konfiguriert werden.
>&#x200B;>Für die Pro-Integration müssen Sie eine benutzerdefinierte Lösung/ein benutzerdefiniertes Skript implementieren und [Ihren Cron konfigurieren](../application/crons-property.md), um das Skript nach Bedarf auszuführen.

>[!NOTE]
>
>Starter-Projektumgebungen haben keine Protokollrotation.

## Erstellen und Bereitstellen von Protokollen

Nachdem Sie Änderungen an Ihre Umgebung gepusht haben, können Sie die Protokollierung von jedem Hook in der `var/log/cloud.log`-Datei überprüfen. Das Protokoll enthält Start- und Stopp-Meldungen für jeden Hook. Im folgenden Beispiel lauten die Nachrichten &quot;`Starting post-deploy.`&quot; und &quot;`Post-deploy is complete.`&quot;

Überprüfen Sie die Zeitstempel in Protokolleinträgen, überprüfen Sie die Protokolle für eine bestimmte Bereitstellung und suchen Sie sie. Im Folgenden finden Sie ein zusammengefasstes Beispiel für die Protokollausgabe, die Sie zur Fehlerbehebung verwenden können:

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Wenn Sie Ihre Cloud-Umgebung konfigurieren, können Sie [protokollbasierte Slack- und E-Mail-Benachrichtigungen](../environment/set-up-notifications.md) für Build- und Bereitstellungsaktionen einrichten.

Die folgenden Protokolle haben einen gemeinsamen Speicherort für alle Cloud-Projekte:

- **Bereitstellungsprotokoll**: `var/log/cloud.log`
- **Letztes Bereitstellungsfehlerprotokoll**: `var/log/cloud.error.log`
- **Debug-Protokoll**: `var/log/debug.log`
- **Ausnahmenprotokoll**: `var/log/exception.log`
- **Systemprotokoll**: `var/log/system.log`
- **Support-Protokoll**: `var/log/support_report.log`
- **Berichte**: `var/report/`

Obwohl die `cloud.log`-Datei Feedback aus jeder Phase des Bereitstellungsprozesses enthält, sind die vom Bereitstellungs-Hook erstellten Protokolle für jede Umgebung eindeutig. Das umgebungsspezifische Bereitstellungsprotokoll befindet sich in den folgenden Verzeichnissen:

- **Starter- und Pro-Integration**: `/var/log/deploy.log`
- **Pro Staging**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro Production**: `/var/log/platform/<project-ID>/deploy.log`

### Protokoll bereitstellen

Das Protokoll für jede Bereitstellung wird mit der spezifischen `deploy.log` verkettet. Im folgenden Beispiel wird das Bereitstellungsprotokoll der aktuellen Umgebung im Terminal gedruckt:

```bash
magento-cloud log -e <environment-ID> deploy
```

Beispielantwort:

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Fehlerprotokoll

Während des Bereitstellungsprozesses generierte Fehler- und Warnmeldungen werden sowohl in die `var/log/cloud.log`- als auch in die `var/log/cloud.error.log` geschrieben. Die Cloud-Fehlerprotokolldatei enthält nur Fehler und Warnungen aus der neuesten Bereitstellung. Eine leere Datei bedeutet, dass die Bereitstellung ohne Fehler erfolgreich war.

Sie können die Protokolldatei mit der [Cloud-CLI SSH](#view-remote-environment-logs) anzeigen oder ECE-Tools verwenden, um die Fehler mit Vorschlägen anzuzeigen:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Beispielantwort:

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

Die meisten Fehlermeldungen enthalten eine Beschreibung und empfohlene Maßnahmen. Verwenden Sie die [Fehlermeldungsreferenz für ECE-Tools](../dev-tools/error-reference.md), um den Fehlercode für weitere Anleitungen zu suchen. Weitere Anleitungen finden Sie in der Fehlerbehebung bei der Bereitstellung von [Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=de).

## Anwendungsprotokolle

Ähnlich wie bei der Bereitstellung von Protokollen sind auch Anwendungsprotokolle für jede Umgebung eindeutig:

| Protokolldatei | Starter- und Pro-Integration | Beschreibung |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Protokoll bereitstellen** | `/var/log/deploy.log` | Aktivität aus dem [Bereitstellungs-Hook](../application/hooks-property.md). |
| **Protokoll nach der Bereitstellung** | `/var/log/post_deploy.log` | Aktivität aus dem [Hook nach der Bereitstellung](../application/hooks-property.md). |
| **Cron-Protokoll** | `/var/log/cron.log` | Ausgabe aus Cron-Aufträgen. |
| **Nginx-Zugriffsprotokoll** | `/var/log/access.log` | Beim Start von Nginx werden HTTP-Fehler für fehlende Verzeichnisse und ausgeschlossene Dateitypen angezeigt. |
| **Nginx-Fehlerprotokoll** | `/var/log/error.log` | Startmeldungen, die zum Debuggen von Konfigurationsfehlern im Zusammenhang mit Nginx nützlich sind. |
| **PHP-Zugriffsprotokoll** | `/var/log/php.access.log` | Anfragen an den PHP-Dienst. |
| **PHP FPM-Protokoll** | `/var/log/app.log` | |

Bei Pro-Staging- und Produktionsumgebungen sind die Protokolle „Bereitstellen“, „Nach der Bereitstellung“ und „Cron“ nur auf dem ersten Knoten im Cluster verfügbar:

| Protokolldatei | Pro-Staging | Pro Produktion |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Protokoll bereitstellen** | Nur erster Knoten:<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | Nur erster Knoten:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Protokoll nach der Bereitstellung** | Nur erster Knoten:<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | Nur erster Knoten:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Cron-Protokoll** | Nur erster Knoten:<br>`/var/log/platform/<project-ID>_stg*/cron.log` | Nur erster Knoten:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx-Zugriffsprotokoll** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Nginx-Fehlerprotokoll** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP-Zugriffsprotokoll** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM-Protokoll** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Archivierte Protokolldateien

Die Anwendungsprotokolle werden einmal täglich komprimiert und archiviert und standardmäßig **365 Tage** (für Pro-Staging- und Produktions-Cluster) - und die Protokollrotation ist nicht in allen Integrations-/Starter-Umgebungen verfügbar. Die komprimierten Protokolle werden mit einer eindeutigen ID benannt, die dem -`Number of Days Ago + 1` entspricht. In Pro-Produktionsumgebungen wird beispielsweise ein PHP-Zugriffsprotokoll für 21 Tage gespeichert und wie folgt benannt:

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

Die archivierten Protokolldateien werden immer in dem Verzeichnis gespeichert, in dem sich die Originaldatei vor der Komprimierung befand.

Sie können [ein Support-Ticket ](https://experienceleague.adobe.com/home?lang=de&support-tab=home#support), um Änderungen an Ihrer Protokollaufbewahrungsdauer oder Ihrer Protokollkonfiguration anzufordern. Sie können die Aufbewahrungsdauer auf maximal 365 Tage erhöhen, sie reduzieren, um das Speicherkontingent zu erhalten, oder zusätzliche Protokollpfade zur logrotate-Konfiguration hinzufügen. Diese Änderungen sind für Pro-Staging- und Produktions-Cluster verfügbar.

Wenn Sie beispielsweise einen benutzerdefinierten Pfad erstellen, um Protokolle im `var/log/mymodule`-Verzeichnis zu speichern, können Sie eine Protokollrotation für diesen Pfad anfordern. Die aktuelle Infrastruktur erfordert jedoch konsistente Dateinamen für Adobe, um die Protokollrotation ordnungsgemäß zu konfigurieren. Adobe empfiehlt, die Protokollnamen konsistent zu halten, um Konfigurationsprobleme zu vermeiden.

>[!NOTE]
>
>**Bereitstellen** und **Nach der Bereitstellung** werden Protokolldateien nicht rotiert und archiviert. In diese Protokolldateien wird der gesamte Bereitstellungsverlauf geschrieben.

## Service-Protokolle

Da jeder Service in einem separaten Container ausgeführt wird, sind die Service-Protokolle nicht in der Integrationsumgebung verfügbar. Adobe Commerce in der Cloud-Infrastruktur bietet nur in der Integrationsumgebung Zugriff auf den Webserver-Container. Die folgenden Speicherorte für Service-Protokolle sind für die Pro-Produktions- und Staging-Umgebungen vorgesehen:

- **Redis-Protokoll**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Elasticsearch-Protokoll**: `/var/log/elasticsearch/elasticsearch.log`
- **Java Garbage Collection Log**: `/var/log/elasticsearch/gc.log`
- **Mail-Protokoll**: `/var/log/mail.log`
- **MySQL-Fehlerprotokoll**: `/var/log/mysql/mysql-error.log`
- **MySQL-Protokoll**: `/var/log/mysql/mysql-slow.log`
- **RabbitMQ-Protokoll**: `/var/log/rabbitmq/rabbit@host1.log`

Service-Protokolle werden je nach Protokolltyp für unterschiedliche Zeiträume archiviert und gespeichert. Beispielsweise haben MySQL-Protokolle die kürzeste Lebensdauer - sie werden nach sieben Tagen entfernt.

>[!TIP]
>
>Die Speicherorte der Protokolldateien in der skalierten Architektur hängen vom Knotentyp ab. Siehe [Protokollspeicherorte im Thema „Skalierte Architektur](../architecture/scaled-architecture.md#log-locations).

## Protokolldaten für Pro Produktion und Staging

Verwenden Sie in Pro-Produktions- und Staging-Umgebungen das in Ihr Projekt [&#128279;](../monitor/log-management.md) New Relic-Protokollmanagement, um aggregierte Protokolldaten aus allen Protokollen zu verwalten, die mit Ihrem Adobe Commerce in Cloud-Infrastrukturprojekt verknüpft sind.

Die Anwendung &quot;New Relic-Protokolle“ bietet ein zentralisiertes Protokollmanagement-Dashboard zur Fehlerbehebung und Überwachung von Adobe Commerce in Cloud-Produktions- und Staging-Umgebungen. Das Dashboard bietet außerdem Zugriff auf Protokolldaten für Fastly CDN, Bildoptimierung und WAF-Services (Web Application Firewall). Siehe [New Relic-Services](../monitor/new-relic-service.md).
