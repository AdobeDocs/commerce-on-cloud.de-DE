---
title: Starter-Architektur
description: Erfahren Sie mehr über die von der Starter-Architektur unterstützten Umgebungen.
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# Starter-Architektur

Ihre Starterarchitektur für die Adobe Commerce auf Cloud-Infrastruktur unterstützt bis zu **vier** Umgebungen, einschließlich einer `master`, die den anfänglichen Projekt-Code, die Staging-Umgebung und bis zu zwei Integrationsumgebungen enthält.

Alle Umgebungen befinden sich in PaaS-Containern (Platform as a Service). Diese Container werden in stark eingeschränkten Containern auf einem Raster von Servern bereitgestellt. Diese Umgebungen sind schreibgeschützt und akzeptieren bereitgestellte Code-Änderungen von Verzweigungen, die aus Ihrem lokalen Arbeitsbereich gepusht werden. Jede Umgebung stellt eine Datenbank und einen Webserver bereit.

Sie können eine beliebige Entwicklungs- und Verzweigungsmethode verwenden. Wenn Sie anfänglichen Zugriff auf Ihr Projekt erhalten, erstellen Sie eine `staging` Umgebung aus der `master`. Erstellen Sie dann die `integration` durch Verzweigen von `staging`.

## Starter-Umgebungsarchitektur

Das folgende Diagramm zeigt die hierarchischen Beziehungen der Starter-Umgebungen.

![Allgemeine Ansicht des Starter-Projekts](../../assets/starter/architecture.png)

## Produktionsumgebung

Die Produktionsumgebung stellt den Quell-Code bereit, um Adobe Commerce in der Cloud-Infrastruktur bereitzustellen, die Ihre öffentlichen Storefronts mit einer oder mehreren Sites ausführt. Die Produktionsumgebung verwendet Code aus der `master` Verzweigung, um den Webserver, die Datenbank, die konfigurierten Services und den Anwendungscode zu konfigurieren und zu aktivieren.

Da die `production`-Umgebung schreibgeschützt ist, verwenden Sie die `integration`-Umgebung, um Code-Änderungen vorzunehmen, sie in der gesamten Architektur von der `integration` bis zur `staging` und schließlich in der `production`-Umgebung bereitzustellen. Siehe [Bereitstellen Ihres Stores](../deploy/staging-production.md) und [Site-Launch](../launch/overview.md).

Adobe empfiehlt, das Testen vollständig in der `staging`-Verzweigung durchzuführen, bevor es zur `master`-Verzweigung übergeht, die in der `production` bereitgestellt wird.

## Staging-Umgebung

Adobe empfiehlt, von `master` aus eine Verzweigung mit dem Namen `staging` zu erstellen. Die `staging` stellt Code in der Staging-Umgebung bereit, um eine Vorproduktionsumgebung zum Testen von Code, Modulen und Erweiterungen, Zahlungs-Gateways, Versand, Produktdaten und vielem mehr bereitzustellen. Diese Umgebung stellt die Konfiguration für alle Services bereit, die mit der Produktionsumgebung übereinstimmen, einschließlich Fastly, New Relic APM und Suche.

Weitere Abschnitte in diesem Handbuch enthalten Anweisungen für die Bereitstellung von endgültigem Code und das Testen von Interaktionen auf Produktionsebene in einer sicheren Staging-Umgebung. Um eine optimale Leistung und Funktionstests zu erzielen, replizieren Sie Ihre Datenbank in die Staging-Umgebung.

>[!WARNING]
>
>Adobe empfiehlt, jede Händler- und Kundeninteraktion in der Staging-Umgebung zu testen, bevor sie in der Produktionsumgebung bereitgestellt werden. Siehe [Bereitstellen des Stores](../deploy/staging-production.md) und [Testen der Bereitstellung](../test/staging-and-production.md).

## Integrationsumgebung

Entwicklerinnen und Entwickler verwenden die `integration` Umgebung zum Entwickeln, Bereitstellen und Testen:

- Adobe Commerce-Anwendungscode

- Benutzerdefinierter Code

- Erweiterungen

- Dienste

**Empfohlene Anwendungsfälle:**

Integrationsumgebungen sind für eingeschränkte Tests und Entwicklung ausgelegt. Beispielsweise können Sie die Integrationsumgebung verwenden, um die folgenden Aufgaben auszuführen:

- Sicherstellen, dass Änderungen an kontinuierlichen Integrationsprozessen (CI/CI) Cloud-kompatibel sind

- Testen Sie kritische Workflows auf wichtigen Seiten wie Startseite, Kategorie, Produktdetailseite (PDP), Checkout und Admin.

Befolgen Sie die folgenden Best Practices, um eine optimale Leistung in der Integrationsumgebung zu erzielen:

- Beschränken der Kataloggröße - Die Beispieldaten enthalten etwa 2.048 Produkte als Referenz. Reduzieren Sie Ihre Kataloggröße auf etwa 4.000-5.000 Produkte.
Um die Anzahl der Produkte im Katalog zu überprüfen, führen Sie die folgende MySQL-Abfrage aus:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Verringern der Anzahl von Kundengruppen - Zu viele Kundengruppen können die Indizierungsleistung und die Gesamtleistung beeinträchtigen.

- Verwendung auf einen oder zwei gleichzeitige Benutzer beschränken

- Deaktivieren Sie Cron-Aufträge und führen Sie sie nach Bedarf manuell aus

Sie können über bis zu **2** aktive Integrationsumgebungen verfügen. Sie erstellen eine Integrationsumgebung, indem Sie eine Verzweigung aus der `staging` Verzweigung erstellen. Wenn Sie eine Integrationsumgebung erstellen, entspricht der Umgebungsname dem Zweignamen. Eine Integrationsumgebung umfasst einen Webserver und eine Datenbank. Es sind nicht alle Services enthalten, z. B. sind Fastly CDN und New Relic nicht verfügbar.

Sie können eine unbegrenzte Anzahl inaktiver Verzweigungen für die Code-Speicherung haben. Um auf eine inaktive Verzweigung zuzugreifen, sie anzuzeigen und zu testen, müssen Sie sie aktivieren

{{enhanced-integration-envs}}

## Produktions- und Staging-Technologie-Stack

Die Produktions- und Staging-Umgebung umfasst die folgenden Technologien. Sie können diese Technologien über die [`.magento.app.yaml`](../application/configure-app-yaml.md)-Datei ändern und konfigurieren.

- Fastly für HTTP-Caching und CDN
- Nginx-Webserver, der mit PHP-FPM spricht, eine Instanz mit mehreren Workern
- Redis-Server
- Elasticsearch für die Katalogsuche für Adobe Commerce 2.2 bis 2.4.3-p2
- OpenSearch für die Katalogsuche für Adobe Commerce 2.3.7-p3, 2.4.3-p2 und 2.4.4 und höher
- Ausgangs-Filter (ausgehende Firewall)

### Dienste

Adobe Commerce in Cloud-Infrastrukturen unterstützt derzeit die folgenden Services: PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce 2.2 bis 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 und höher), Redis und [!DNL RabbitMQ].

Jeder Service wird in einem separaten, sicheren Container ausgeführt. Container werden im Projekt gemeinsam verwaltet. Einige Services sind standardmäßig, z. B. die folgenden:

- HTTP-Router (Verarbeitung eingehender Anfragen, aber auch Caching und Weiterleitungen)

- PHP-Anwendungsserver

- Git

- Secure Shell (SSH)

### Software-Versionen

Adobe Commerce in der Cloud-Infrastruktur verwendet das Debian GNU/Linux-Betriebssystem und den NGINX-Webserver. Sie können diese Software nicht aktualisieren, aber Sie können Versionen für Folgendes konfigurieren:

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

In den Staging- und Produktionsumgebungen verwenden Sie Fastly für CDN und Caching. Die neueste Version der Fastly CDN-Erweiterung wird während der ersten Bereitstellung Ihres Projekts installiert. Sie können die Erweiterung aktualisieren, um die neuesten Fehlerbehebungen und Verbesserungen zu erhalten. Siehe [Fastly CDN-Modul für Magento 2](https://github.com/fastly/fastly-magento2). Außerdem haben Sie Zugriff auf [New Relic](../monitor/account-management.md) zur Leistungsüberwachung.

Verwenden Sie die folgenden Dateien, um die Softwareversionen zu konfigurieren, die Sie in Ihrer Implementierung verwenden möchten.

- [&quot;.magento.app.yaml“](../application/configure-app-yaml.md)

- [`routes.yaml`](../routes/routes-yaml.md)

- [`services.yaml`](../services/services-yaml.md)

### Sicherung und Notfallwiederherstellung

Sie können eine Sicherung Ihrer Datenbank und Ihres Dateisystems mithilfe der [!DNL Cloud Console] oder der CLI erstellen. Siehe [Backup-Verwaltung](../storage/snapshots.md).

## Vorbereiten der Entwicklung

Im folgenden Workflow wird der Prozess zum Verzweigen des Codes sowie zum Entwickeln und Bereitstellen des Stores zusammengefasst:

1. Einrichten der lokalen Umgebung

1. Klonen Sie die `master` in Ihrer lokalen Umgebung

1. Erstellen einer `staging` Verzweigung aus `master`

1. Erstellen von Verzweigungen für die Entwicklung aus `staging`

1. Pushen von Code an Git, das eine Umgebung erstellt und zum Testen bereitstellt

In den folgenden Abschnitten finden Sie detaillierte Anweisungen und Anleitungen zum Entwickeln, Testen und Bereitstellen Ihres Stores:

- [Starter: Workflow zum Entwickeln und Bereitstellen](starter-develop-deploy-workflow.md)

- [Docker-Entwicklung](../dev-tools/cloud-docker.md) (lokale Entwicklungsumgebung, die von Cloud Docker für Commerce aktiviert wird)

- [Verzweigungen verwalten](../project/console-branches.md)

- [Bereitstellen des Stores](../deploy/staging-production.md)

- [Site-Launch](../launch/overview.md)
