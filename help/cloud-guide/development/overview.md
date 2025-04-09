---
title: Entwicklungsübersicht
description: Bereiten Sie sich mit einem Adobe Commerce on Cloud-Infrastrukturprojekt auf die lokale Entwicklung vor.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Entwicklungsübersicht

Adobe Systems Commerce in Cloud-Infrastruktur Remote-Umgebungen ist **schreibgeschützt, einschließlich aller Starter-Umgebungen und aller Pro-Integrations-, Staging-** und Produktionsumgebungen. In einer lokalen Entwicklungs Umgebung können Sie Code schreiben und Test, bevor Sie ihn zur weiteren Prüfung und Implementierung für Staging und Produktion an eine Integrationsumgebung übertragen.

Bevor Sie Ihre lokaler Arbeitsplatz vorbereiten, stellen Sie sicher, dass Sie über Ihre [Anmeldedaten](../../get-started/prepare-workspace.md) verfügen. Für die lokale Entwicklung sind die Installation von PHP und Composer erforderlich, es sei denn, Sie entscheiden sich für die Verwendung von [Cloud Docker for Commerce](#docker-environment).

## Erforderlich Pakete

Adobe Systems Commerce on Cloud-Infrastruktur verwendet Composer, um die Abhängigkeiten und Upgrades für Projekte zu managen. Für die lokale Entwicklung müssen Sie die PHP- und Composer-Versionen installieren, die mit Ihrem Cloud-Projekt kompatibel sind. Wenn Sie beispielsweise die [!DNL Commerce] Vorlage 2.4.8 Cloud verwenden, können Sie sehen, dass die [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml) Konfigurationsdatei PHP 8.4 **und** Composer 2.8.4 **verwendet**.

Composer installiert die erforderlichen Bibliotheken und Abhängigkeiten für Ihr Projekt im `vendor`. Die folgenden erforderlichen Composer-Dateien befinden sich im Stammverzeichnis des Projekts:

- `composer.json` - Verwenden Sie die `composer.json`, um Produktinstallationen und Upgrades zu verwalten.
- `composer.lock` - Die `composer.lock`-Datei speichert einen Satz exakter Versionsabhängigkeiten, die die Versionsbeschränkungen jeder Anforderung für jedes Paket in der Abhängigkeitsstruktur des Projekts erfüllen.

**Allgemeine Befehle:**

| Befehl | Beschreibung |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Aktualisierungen auf die neuesten Versionen der Abhängigkeiten, die sich in der `composer.json` Datei widerspiegeln. Dadurch wird die `composer.lock` Datei aktualisiert. |
| `composer install` | Liest die Datei, um Abhängigkeiten `composer.lock` zu herunterladen. Es ist eine Best Practice, eine Kopie davon `composer.lock` in Ihrem Projekt Repository auf dem neuesten Stand zu halten. |

{style="table-layout:auto"}

Nachdem Sie den aktualisierten Code hinzugefügt, festgeschrieben und gepusht haben, führt der Implementierung-Prozess den `composer install` Befehl während der [Build-Phase](../deploy/process.md#build-phase-build-phase) automatisch aus.

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

- **Vendor/magento/ece-tools**: Das `ece-tools`-Paket ist mit Adobe Commerce Version 2.1.4 und höher kompatibel und bietet zahlreiche Funktionen, mit denen Sie Ihr Adobe Commerce in Cloud-Infrastrukturprojekt verwalten können. Es enthält Skripts und Adobe Systems Commerce on Cloud-Infrastruktur-Befehle, die Ihnen dabei helfen, Ihren Code zu managen und Ihre Projekte automatisch zu Build und bereitzustellen. Weitere Informationen finden Sie in der [`ece-tools` Paketübersicht](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition**: Dieses Metapaket erfordert Applikation Komponenten, einschließlich Modulen, Frameworks, Themen und mehr.
- **vendor/fastly2/magento2**—Dieses Modul verwaltet die Fastly CDN und Services für die Pro Staging-, Produktions- und Starter-Produktionsumgebungen. Weitere Informationen finden Sie unter [Fastly Services](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **vendor/magento/Modul-PayPal-on-boarding** - Dieses Modul bietet PayPal Zahlungsgateway-Checkout, indem eine Verbindung zu Ihrem PayPal Händler Konto hergestellt wird. Siehe [PayPal Onboarding-Tool](../store/paypal.md).

>[!TIP]
>
>Eine Liste zu Abhängigkeiten und Drittanbieterlizenzen finden Sie unter [Cloud-Pakete für Adobe Systems Commerce](/help/cloud-guide/release-notes/cloud-packages.md) in den _Commerce-Versionshinweisen_ .

## Docker-Umgebung

Sie können das Tool Cloud Docker für Commerce verwenden, um die Adobe Commerce in Cloud-Infrastrukturproduktions- und -Entwicklungsumgebungen für die lokale Entwicklung zu emulieren. Cloud Docker für Commerce erfordert keine lokale Installation von PHP und Composer.

- [Lokale Entwicklung mit Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) auf der Adobe Developer-Site
- [Docker-Architektur und allgemeine Befehle](../dev-tools/cloud-docker.md)
- [Cloud-Docker-Versionshinweise](../release-notes/cloud-docker.md)

>[!TIP]
>
>Informationen zur Verwendung von Git-basierten Hostingdiensten mit Adobe Systems Commerce on Cloud-Infrastruktur finden Sie unter [Integrationen](../integrations/overview.md).
