---
title: Übersicht über Konfigurationsdateien
description: Erfahren Sie, wie Sie die Cloud-Infrastrukturumgebung konfigurieren, um die Bereitstellung und Verwaltung Ihres benutzerdefinierten Adobe Commerce-Stores zu unterstützen.
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Übersicht über Konfigurationsdateien

Umgebungen in Adobe Commerce auf Cloud-Infrastrukturen umfassen Container mit Programmen, Services und eine Datenbank, um ein vollständiges System für Ihre Adobe Commerce-Anwendungs-Code-Basis und -Dateien bereitzustellen.

Mit den folgenden Konfigurationsdateien können Sie Anwendungseinstellungen, Routen, Build- und Bereitstellungsaktionen sowie Benachrichtigungen zur Unterstützung Ihrer Projektumgebungen konfigurieren:

| Konfiguration | Dateiname | Beschreibung |
| ------------- | -------- | ----------- |
| [Anwendung](../application/configure-app-yaml.md) | `.magento.app.yaml` | Definiert, wie Adobe Commerce erstellt und bereitgestellt wird, einschließlich Services, Hooks und Cron-Aufträgen. |
| [Umgebung](configure-env-yaml.md) | `.magento.env.yaml` | Zentralisiert die Verwaltung von Build- und Bereitstellungsaktionen in allen Ihren Umgebungen, einschließlich Pro Staging und Produktion, mithilfe von Umgebungsvariablen. |
| [Routen](../routes/routes-yaml.md) | `.magento/routes.yaml` | Konfigurieren Sie das Caching, Umleitungen und serverseitige Includes. |
| [Service](../services/services-yaml.md) | `.magento/services.yaml` | Definiert die Services, die Adobe Commerce nach Name und Version verwendet. Diese Datei kann beispielsweise Versionen von MariaDB, PHP Extensions, Redis, RabbitMQ und Elasticsearch oder OpenSearch enthalten. Öffnen Sie ein Support-Ticket, um diese Änderungen in die Pro Plan Staging- und Produktionsumgebungen zu übertragen. |
| [PHP-Einstellungen](../application/php-settings.md#configure-php) | `php.ini` | Eine optionale Datei , die dem Projekt hinzugefügt werden kann. Die in dieser Datei enthaltenen Einstellungen werden an die Einstellungen angehängt, die von der Cloud-Infrastruktur verwaltet werden. |

{style="table-layout:auto"}

## Konfigurationsaktualisierungen für Pro-Umgebungen

Für Adobe Commerce in Cloud Infrastructure Pro Staging- und Produktionsumgebungen können Sie viele Konfigurationsoptionen in Ihrer lokalen Entwicklungsumgebung aktualisieren und die Änderungen übernehmen, um sie auf diese Umgebungen anzuwenden. Sie müssen jedoch [ein Adobe Commerce-Support-Ticket ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket), um die folgenden Konfigurationsoptionen zu aktualisieren:

- Installieren oder Aktualisieren von Diensten in der `.magento/services.yaml`.
- Ändern Sie die Konfiguration für die `mounts`- und `disk` in der `.magento.app.yaml`.

{{pro-self-service-warning}}
