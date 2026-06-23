---
title: Optimieren der Cloud-Bereitstellung
description: Erfahren Sie, wie Sie den Bereitstellungsprozess für Adobe Commerce in Cloud-Infrastrukturprojekten optimieren können, einschließlich der Reduzierung von Ausfallzeiten, der Bereitstellung statischer Inhalte, der szenarienbasierten Bereitstellung und intelligenter Assistenten.
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 0%

---

# Optimieren der Bereitstellung

Die Site-Leistung kann während des Bereitstellungsprozesses beeinträchtigt sein. Wie lange sich eine Site im Wartungsmodus befindet, wenn sie auf einer Produktions-Site bereitgestellt wird, hängt von vielen Faktoren ab, z. B. der Umgebungskonfiguration und der Menge an Inhalten, die eine Site enthält. Die erste Best Practice für die Optimierung Ihrer Cloud-Bereitstellung besteht darin[ ein Upgrade durchzuführen, um `ece-tools`](../dev-tools/install-package.md) für die Vorteile der Paketfunktionen zu verwenden, z. B. Befehle zum Erstellen eines Backups der Datenbank und zum Überprüfen der Umgebungskonfiguration.

Die folgenden Themen können Ihnen dabei helfen, besser zu verstehen, wie Sie den Bereitstellungsprozess optimieren können:

- [Cloud-Bereitstellungsprozess](process.md)
Der Cloud-Bereitstellungsprozess umfasst drei Phasen. Sie können die Stärken und Schwächen jeder Phase zu Ihrem Vorteil nutzen.

- [Keine Ausfallzeiten bei der Bereitstellung](reduce-downtime.md)
Erfahren Sie, was während der Bereitstellung passiert und wie Sie die Ausfallzeiten Ihres Stores während einer Aktualisierung der Produktionsumgebung reduzieren können.

- [Bereitstellung statischer Inhalte](static-content.md)
Die beste Möglichkeit, Ihre Cloud-Bereitstellung zu optimieren, besteht darin, zu steuern, wie und wann statische Inhalte generiert werden.

- [Intelligente Assistenten](smart-wizards.md)
Das `ece-tools`-Paket enthält die Befehle des intelligenten Assistenten, mit denen Sie Ihre Projektkonfiguration schnell auswerten können.

- [Verfolgen von Bereitstellungen mit New Relic](../monitor/track-deployments.md)
Verwenden Sie den New Relic-Service, um Bereitstellungsereignisse zu überwachen und die Auswirkungen der Bereitstellung auf die Gesamtleistung zu analysieren.

