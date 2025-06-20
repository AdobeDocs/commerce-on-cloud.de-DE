---
title: Verwalten von Festplattenspeicher
description: Erfahren Sie, wie Sie den Speicherplatz mithilfe der Befehlszeilenschnittstelle verwalten.
feature: Cloud, Storage
exl-id: 1d13dc4e-56eb-4153-a8b1-48d2263ebc4c
source-git-commit: b8cabaad4b7805858563cecbe5ffc2fdb9aeac58
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Verwalten von Festplattenspeicher

Die gesamte Speicherkapazität für Ihr Cloud-Projekt finden Sie in Ihrem Adobe Commerce auf Cloud Infrastructure Contract und auf Ihrer [Kontoseite](https://accounts.magento.cloud/user). Jede Projektkarte in Ihrem Konto enthält die Anzahl _Umgebungen_, die _Speicher_-Kapazität in GB und die Anzahl _Benutzer_. Alternativ können Sie den folgenden Cloud-Befehl verwenden:

```bash
magento-cloud subscription:info | grep storage
```

Beispielantwort:

```
| storage              | 51200
```

Trigger Wenn eine Pro-Produktions- oder Staging-Umgebung 95 % der Speicherkapazität erreicht oder überschreitet, gibt das Cloud Infrastructure Monitoring Tool einen Support-Warnhinweis aus, der Sie über eine automatische Erhöhung der Speicherkapazität informiert.

Beispielbenachrichtigung:

>[!BEGINSHADEBOX]

_„Unsere Überwachung hat festgestellt, dass der Dateispeicher in Ihrem Cluster (project-id-environment) fast voll ist. Die Festplattenauslastung liegt derzeit bei kritischen Nutzungsraten, wobei weniger als 1 GiB übrig bleibt. Das gemeinsam genutzte Speichervolumen wird derzeit von 60 GiB auf 70 GiB aktualisiert, um den Betrieb Ihrer Services aufrechtzuerhalten. Bitte schauen Sie sich die Verwendung von Produktions- und Staging-Dateien an, um zu sehen, ob Sie etwas Platz freimachen können.“_

>[!ENDSHADEBOX]

>[!TIP]
>
>Adobe empfiehlt, die Speicherkapazität regelmäßig zu überwachen und unter 90 % zu halten, um diese automatischen Erhöhungen zu vermeiden. Nach der Zuweisung ist die Speichererhöhung für Pro-Staging und Produktion dauerhaft und kann nicht rückgängig gemacht werden.

## Integrationsumgebung überprüfen

Sie können die Speicherplatznutzung für Ihre Integrationsumgebung mithilfe der `magento-cloud` CLI überprüfen.

**So überprüfen Sie die ungefähre Speicherplatzauslastung**:

```bash
magento-cloud db:size
```

Beispielantwort:

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Alle Halterungen verwenden gemeinsam eine Festplatte. Sie können die Speicherplatznutzung für Bereitstellungen über die `magento-cloud` CLI überprüfen.

**So prüfen Sie die ungefähre Speicherplatzauslastung für Bereitstellungen**:

```bash
magento-cloud mount:size
```

Beispielantwort:

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## Überprüfen dedizierter Cluster

Bei Pro-Staging- und Produktionsumgebungen können Sie die Speicherplatznutzung in jeder Umgebung mit dem Befehl `disk free` überprüfen, der den vom Dateisystem belegten Speicherplatz angibt. Sie müssen SSH verwenden, um sich bei einer Remote-Umgebung anzumelden.

```bash
df -h
```

Die Option `-h` zeigt den Bericht in einem für Menschen lesbaren Format an (KB, MB oder GB).

In der folgenden Beispielantwort zeigt die `/data/exports`-Bereitstellung den Speicherplatz für Medien und `/data/mysql/`-Bereitstellung den Speicherplatz für die Datenbank:

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

Sie können die Antwort einschränken, indem Sie ein Verzeichnis angeben. Beispiel:

```bash
df -h var/
```

Beispielantwort:

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Speicherplatz zuweisen

Zwei [Konfigurationsdateien](../environment/overview.md) steuern die Zuordnung von Speicherplatz in den Cloud-Umgebungen: die `.magento.app.yaml`- und die `.magento/services.yaml`. Jede Datei enthält die `disk`-Eigenschaft, die den Wert der Festplattengröße in MB für die jeweilige Konfiguration definiert. Sie können die Speicherplatzzuweisung nur in der Pro-Integrations- und Starter-Umgebung ändern.

>[!IMPORTANT]
>
>Bei Pro-Produktions- und Staging-Umgebungen müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) um die Speicherplatzzuweisung zu ändern. Eine Vergrößerung der Pro-Produktions- und Staging-Umgebungen kann nur in bestimmten Intervallen erfolgen. Abhängig von Ihrer aktuellen Speicherplatznutzung empfiehlt der Support daher möglicherweise, die Speicherplatzzuweisung um mindestens 10 GB zu erhöhen. Nach der Zuweisung kann die Speichererhöhung für Pro-Staging und Produktion nicht mehr rückgängig gemacht werden. Speicher kann nicht neu zugewiesen oder zwischen Ressourcen umverteilt werden. Um mehr Dateispeicherplatz hinzuzufügen, reduzieren Sie den für MySQL zugewiesenen Speicherplatz.

### Anwendungsspeicherplatz

Die `.magento.app.yaml` steuert den [persistenten Speicherplatz](../application/properties.md#disk) der für die Anwendung verfügbar ist.

**So erhöhen Sie den Speicherplatz für Ihre Anwendung**:

1. Öffnen Sie in Ihrer lokalen Entwicklungsumgebung die `.magento.app.yaml`.

1. Legen Sie einen neuen Wert für die `disk`-Eigenschaft fest (in MB).

   ```yaml
   disk: <value-mb>
   ```

1. Speichert Änderungen in der Datei.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   Die Änderungen werden wirksam, nachdem Sie die aktualisierte YAML-Datei in die Remote-Umgebung gepusht haben.

### Dienstspeicherplatz

Die `.magento/services.yaml`-Datei steuert den für jeden Dienst verfügbaren Speicherplatz, z. B. MySQL und Redis.

**So erhöhen Sie den Festplattenspeicher für einen Service**:

1. Öffnen Sie in Ihrer lokalen Entwicklungsumgebung die `.magento/services.yaml`.

1. Fügen Sie einen Dienst in der Datei hinzu oder suchen Sie ihn. Siehe [Weitere Informationen zum Konfigurieren von Services](../services/services-yaml.md).

1. Legen Sie einen neuen Wert für die Datenträgereigenschaft fest (in MB).

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. Speichert Änderungen in der Datei.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   Die Änderungen werden wirksam, nachdem Sie die aktualisierte YAML-Datei in die Remote-Umgebung gepusht haben.

## Überwachen des Festplattenspeichers

In Pro-Produktionsumgebungen können Sie den Festplattenspeicher und andere Leistungsindikatoren mithilfe der Warnmeldungsrichtlinie „Verwaltete Warnhinweise für Adobe Commerce&quot; für New Relic überwachen. Weitere Informationen finden Sie unter [Überwachen der Leistung mit verwalteten Warnhinweisen](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Weitere Anleitungen finden Sie unter [Best Practices zum Beheben von Problemen mit der Datenbankleistung](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html).

## Kein Platz übrig

Der Build-Cache kann im Laufe der Zeit wachsen. Wenn Sie eine Warnung mit dem Status &quot;`No space left on device`&quot; erhalten, versuchen Sie, den Build-Cache zu löschen und erneut bereitzustellen:

```bash
magento-cloud project:clear-build-cache
```
