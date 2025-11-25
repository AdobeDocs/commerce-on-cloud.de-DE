---
title: Entwicklungsübersicht
description: Bereiten Sie sich mit einem Adobe Commerce on Cloud-Infrastrukturprojekt auf die lokale Entwicklung vor.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Entwicklungsübersicht

Adobe Commerce in Remote-Umgebungen der Cloud **Infrastruktur sind schreibgeschützt** einschließlich aller Starter-Umgebungen und aller Pro-Integrations-, Staging- und Produktionsumgebungen. In einer lokalen Entwicklungsumgebung können Sie Code schreiben und testen, bevor Sie ihn in eine Integrationsumgebung pushen, um weitere Tests durchzuführen und ihn in der Staging- und Produktionsumgebung bereitzustellen.

Stellen Sie vor der Vorbereitung Ihres lokalen Arbeitsbereichs sicher, dass Sie über Ihre [Anmeldeinformationen](../../get-started/prepare-workspace.md) verfügen. Für die lokale Entwicklung ist die Installation von PHP und Composer erforderlich, es sei denn, Sie verwenden [Cloud Docker für Commerce](#docker-environment).

## Erforderliche Pakete

Adobe Commerce in der Cloud-Infrastruktur verwendet Composer, um die Abhängigkeiten und Upgrades für Projekte zu verwalten. Für die lokale Entwicklung müssen Sie die PHP- und Composer-Versionen installieren, die mit Ihrem Cloud-Projekt kompatibel sind. Wenn Sie beispielsweise die Cloud-Vorlage [!DNL Commerce] 2.4.8 verwenden, können Sie sehen, dass die [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml) Konfigurationsdatei **PHP 8.4** und **Composer 2.8.4** verwendet.

Composer installiert die erforderlichen Bibliotheken und Abhängigkeiten für Ihr Projekt im `vendor`. Die folgenden erforderlichen Composer-Dateien befinden sich im Stammverzeichnis des Projekts:

- `composer.json` - Verwenden Sie die `composer.json`, um Produktinstallationen und Upgrades zu verwalten.
- `composer.lock` - Die `composer.lock`-Datei speichert einen Satz exakter Versionsabhängigkeiten, die die Versionsbeschränkungen jeder Anforderung für jedes Paket in der Abhängigkeitsstruktur des Projekts erfüllen.

**Allgemeine Befehle:**

| Befehl | Beschreibung |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Aktualisierungen der neuesten Versionen der Abhängigkeiten, die in der `composer.json`-Datei enthalten sind. Dadurch wird die `composer.lock` aktualisiert. |
| `composer install` | Liest die `composer.lock` zum Herunterladen von Abhängigkeiten. Es gilt als Best Practice, eine aktuelle Kopie von `composer.lock` in Ihrem Projekt-Repository zu führen. |

{style="table-layout:auto"}

Nachdem Sie den aktualisierten Code hinzugefügt, übertragen und gepusht haben, führt der Bereitstellungsprozess den `composer install`-Befehl während der Build[Phase automatisch &#x200B;](../deploy/process.md#build-phase-build-phase).

### Cloud-Metapaket

Adobe Commerce in Cloud-Infrastrukturen verwendet ein Metapaket, für das `magento/product-enterprise-edition` erforderlich ist. Verwenden Sie die folgende Einschränkungssyntax, um die neuesten Aktualisierungen für die neueste Version von Commerce zu erhalten:

```text
>=current_version <next_version
```

Um beispielsweise die neueste Adobe Commerce-Version 2.4.9 zu verwenden, legen Sie `2.4.8` als „aktuelle“ Version und `2.4.9` als „nächste“ Version in der `composer.json` fest:

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

Die Hauptpakete dieses Metapakets sind die folgenden:

- **Vendor/magento/ece-tools**: Das `ece-tools`-Paket ist mit Adobe Commerce Version 2.1.4 und höher kompatibel und bietet zahlreiche Funktionen, mit denen Sie Ihr Adobe Commerce in Cloud-Infrastrukturprojekt verwalten können. Es enthält Skripte und Adobe Commerce-Befehle für die Cloud-Infrastruktur, die Sie bei der Verwaltung Ihres Codes und der automatischen Erstellung und Bereitstellung Ihrer Projekte unterstützen. Siehe [`ece-tools` Paket - Übersicht](../dev-tools/package-overview.md).
- **Vendor/magento/product-enterprise-edition**: Dieses Metapaket erfordert Anwendungskomponenten, einschließlich Module, Frameworks, Designs und mehr.
- **provider/fastly2/magento2**: Dieses Modul verwaltet das Fastly CDN und die Services für die Pro Staging- und Produktions- und Starter-Produktionsumgebungen. Siehe [Fastly Services](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **Vendor/magento/module-paypal-on-boarding** - Dieses Modul ermöglicht den PayPal-Bezahlungs-Gateway-Checkout durch die Verbindung mit Ihrem PayPal-Händlerkonto. Siehe [PayPal-Onboarding-Tool](../store/paypal.md).
- **provider/aem/rum**: Dieses Modul verwaltet das Datenerfassungs[Tool für die &#x200B;](../monitor/operational-telemetry.md) (Operational Telemetry).

>[!TIP]
>
>Unter [Cloud-Pakete für Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) in den _Versionshinweisen_ Commerce finden Sie eine Liste der Abhängigkeiten und Drittanbieterlizenzen.

## Docker-Umgebung

Sie können das Tool Cloud Docker für Commerce verwenden, um die Adobe Commerce in Cloud-Infrastrukturproduktions- und -Entwicklungsumgebungen für die lokale Entwicklung zu emulieren. Cloud Docker für Commerce erfordert keine lokale Installation von PHP und Composer.

- [Lokale Entwicklung mit Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup) auf der Adobe Developer-Site
- [Docker-Architektur und allgemeine Befehle](../dev-tools/cloud-docker.md)
- [Versionshinweise zu Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Informationen zur Verwendung von Git-basierten Hosting-Services mit Adobe Commerce in der Cloud-Infrastruktur finden Sie unter [Integrationen](../integrations/overview.md).
