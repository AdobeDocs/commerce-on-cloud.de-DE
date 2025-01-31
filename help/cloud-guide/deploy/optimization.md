---
title: Optimieren der Cloud-Bereitstellung
description: Erfahren Sie, wie Sie den Bereitstellungsprozess für Adobe Commerce in Cloud-Infrastrukturprojekten optimieren können, einschließlich der Reduzierung von Ausfallzeiten, der Bereitstellung statischer Inhalte, der szenarienbasierten Bereitstellung und intelligenter Assistenten.
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Optimieren der Bereitstellung

Die Site-Leistung kann während des Bereitstellungsprozesses beeinträchtigt sein. Wie lange sich eine Site im Wartungsmodus befindet, wenn sie auf einer Produktions-Site bereitgestellt wird, hängt von vielen Faktoren ab, z. B. der Umgebungskonfiguration und der Menge an Inhalten, die eine Site enthält. Die erste Best Practice für die Optimierung Ihrer Cloud-Bereitstellung besteht darin[ ein Upgrade durchzuführen, um `ece-tools`](../dev-tools/install-package.md) für die Vorteile der Paketfunktionen zu verwenden, z. B. Befehle zum Erstellen eines Backups der Datenbank und zum Überprüfen der Umgebungskonfiguration.

Die folgenden Themen können Ihnen dabei helfen, besser zu verstehen, wie Sie den Bereitstellungsprozess optimieren können:

- [Cloud-Bereitstellungsprozess](process.md)
Der Cloud-Bereitstellungsprozess umfasst drei Phasen. Sie können die Stärken und Schwächen jeder Phase zu Ihrem Vorteil nutzen.

- [Keine Ausfallzeiten bei der Bereitstellung](reduce-downtime.md)
Erfahren Sie, was während der Bereitstellung passiert und wie Sie die Ausfallzeiten Ihres Stores während einer Aktualisierung der Produktionsumgebung reduzieren können.

- [Statische Inhaltsbereitstellung](static-content.md)
Die beste Möglichkeit, Ihre Cloud-Bereitstellung zu optimieren, besteht darin, zu steuern, wie und wann statische Inhalte generiert werden.

- [Intelligente Assistenten](smart-wizards.md)
Das `ece-tools`-Paket enthält die Befehle des intelligenten Assistenten, mit denen Sie Ihre Projektkonfiguration schnell auswerten können.

- [Verfolgen von Bereitstellungen mit New Relic](../monitor/track-deployments.md)
Verwenden Sie den New Relic-Service, um Bereitstellungsereignisse zu überwachen und die Auswirkungen der Bereitstellung auf die Gesamtleistung zu analysieren.
