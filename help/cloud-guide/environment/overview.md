---
title: Übersicht über Konfigurationsdateien
description: Erfahren Sie, wie Sie die Cloud-Infrastrukturumgebung konfigurieren, um die Bereitstellung und Verwaltung Ihres benutzerdefinierten Adobe Commerce-Stores zu unterstützen.
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: 305380b0-1920-4037-a1db-80e72c6af333
TQID: 'https://experienceleague.adobe.com/mFjzrTN6R7LC3e9ADnzzulcWAwun4k-g3aCjc9Bo3gQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: 00451af3-7b97-5414-9992-3a6c269e413f
    internal-label: Paas
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
    internal-label: IaaS
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '286'
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
| [Service](../services/services-yaml.md) | `.magento/services.yaml` | Definiert die Services, die Adobe Commerce nach Name und Version verwendet. Diese Datei kann beispielsweise Versionen von MariaDB, PHP Extensions, Redis oder Valkey, RabbitMQ und Elasticsearch oder OpenSearch enthalten. Öffnen Sie ein Support-Ticket, um diese Änderungen in die Pro Plan Staging- und Produktionsumgebungen zu übertragen. |
| [PHP-Einstellungen](../application/php-settings.md#configure-php) | `php.ini` | Eine optionale Datei , die dem Projekt hinzugefügt werden kann. Die in dieser Datei enthaltenen Einstellungen werden an die Einstellungen angehängt, die von der Cloud-Infrastruktur verwaltet werden. |

{style="table-layout:auto"}

## Konfigurationsaktualisierungen für Pro-Umgebungen

Für Adobe Commerce in Cloud Infrastructure Pro Staging- und Produktionsumgebungen können Sie viele Konfigurationsoptionen in Ihrer lokalen Entwicklungsumgebung aktualisieren und die Änderungen übernehmen, um sie auf diese Umgebungen anzuwenden. Sie müssen jedoch [ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket), um die folgenden Konfigurationsoptionen zu aktualisieren:

- Installieren oder Aktualisieren von Diensten in der `.magento/services.yaml`.
- Ändern Sie die Konfiguration für die `mounts`- und `disk` in der `.magento.app.yaml`.

{{pro-self-service-warning}}
