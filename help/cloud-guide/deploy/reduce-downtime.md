---
title: Keine Ausfallzeiten bei der Bereitstellung
description: Erfahren Sie, wie Sie bei der Bereitstellung von Adobe Commerce in Cloud-Infrastrukturprojekten die Ausfallzeiten insgesamt reduzieren können.
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Keine Ausfallzeiten bei der Bereitstellung

Adobe Commerce in der Cloud-Infrastruktur führt die Anwendung während [_Bereitstellungsphase im_-](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=de#production-mode) aus. Dadurch wird Ihre Site offline geschaltet, bis die Bereitstellung abgeschlossen ist. Wie lange sich Ihre Produktions-Site im Wartungsmodus befindet, hängt von der Größe der Site, der Anzahl der während der Bereitstellung vorgenommenen Änderungen und der Konfiguration für die Bereitstellung statischer Inhalte ab. Es ist möglich, Ihr Projekt so zu konfigurieren, dass es mit einem **Ausfallzeiteffekt bereitgestellt**.

Während des Bereitstellungsprozesses stehen alle Verbindungen für bis zu 5 Minuten in der Warteschlange, wobei alle aktiven Sitzungen und ausstehenden Aktionen beibehalten werden, z. B. das Hinzufügen zum Warenkorb oder der Checkout. Nach der Bereitstellung wird die Warteschlange freigegeben und die Verbindungen werden ohne Unterbrechung fortgesetzt. Um diese _Verbindung zu Ihrem Vorteil_ halten und die Bereitstellung auf _null_ Ausfallzeiten zu reduzieren, müssen Sie Ihr Projekt so konfigurieren, dass es die effizienteste Bereitstellungsstrategie verwendet.

>[!NOTE]
>
>Um sicherzustellen, dass Ihr Cloud-Projekt optimal konfiguriert ist, um Bereitstellungsausfälle zu minimieren, verwenden Sie den [Smart Wizard](smart-wizards.md). Der intelligente Assistent überprüft Ihre aktuelle Einrichtung und führt Sie durch empfohlene Konfigurationsanpassungen, um Best Practices für Bereitstellungen ohne Ausfallzeiten zu ermöglichen.

Führen Sie die folgenden Schritte aus, um die Zeit zu reduzieren, die Ihr Store für die Bereitstellung eines Updates für die Produktion benötigt:

1. [Aktualisieren Sie auf das `ece-tools`-](../dev-tools/install-package.md) oder [aktualisieren Sie die `ece-tools` Version](../dev-tools/update-package.md)
Ihr Adobe Commerce in Cloud-Infrastrukturprojekt muss über das neueste `ece-tools` verfügen, damit Sie über die Tools zur Konfiguration einer optimalen Bereitstellung verfügen. Wenn Sie über die neuesten `ece-tools` verfügen, fahren Sie mit dem nächsten Schritt fort.

   >[!NOTE]
   >
   >Obwohl es sich um eine Best Practice handelt, das neueste `ece-tools`-Paket zu verwenden, funktioniert die Bereitstellungsmethode ohne Ausfallzeiten mit `ece-tools` ([.0.13](../release-notes/cloud-release-archive.md#v2002013) und höher.

1. [Konfigurieren der Bereitstellung statischer Inhalte](static-content.md)
Wenn die Bereitstellung statischer Inhalte in der Bereitstellungsphase fehlschlägt, bleibt Ihre Site im Wartungsmodus hängen. Wenn während der Build-Phase ein Fehler auftritt, vermeidet der Prozess Ausfallzeiten, da er nie in die Bereitstellungsphase eintritt. [Das Generieren von statischem Inhalt während der Build-Phase mit minimiertem HTML](static-content.md#setting-the-scd-on-build), auch als idealer Status bezeichnet, ist die optimale Konfiguration für Bereitstellungen ohne Ausfallzeiten und _Ausfallzeiten_, wenn ein Fehler auftritt.

1. [Konfigurieren des Hooks nach der Bereitstellung](../application/hooks-property.md)
Sie müssen den Hook nach der Bereitstellung konfigurieren, um den Cache zu bereinigen und zu wärmen. Standardmäßig erfolgt die Cache-Bereinigung während der Bereitstellungsphase, wenn die Site ausgefallen ist. Wenn Sie den Cache bereinigen in die Phase nach der Bereitstellung verschieben, bleibt Ihr Cache bis zum Abschluss der Bereitstellungsphase aktiv, und Sie können dann den Cache sicher bereinigen.

   Passen Sie die Liste der Seiten an, die zum Vorausfüllen des Caches verwendet werden, indem Sie die [WARM_UP_PAGES-Umgebungsvariable](../environment/variables-post-deploy.md#warmuppages) verwenden.

1. [Reduzieren von Design-Dateien](../environment/variables-deploy.md#scdmatrix)
Sie können die Anzahl unnötiger Design-Dateien reduzieren, indem Sie die Umgebungsvariable SCD\_MATRIX konfigurieren.

1. [Beschleunigung der Bereitstellung statischer Inhalte](../environment/variables-deploy.md#scdthreads)
Sie können den Bereitstellungsprozess beschleunigen, indem Sie die Umgebungsvariable SCD\_THREADS aktualisieren, um die Anzahl der Threads für die Bereitstellung statischer Inhalte zu erhöhen.

>[!NOTE]
>
>Sie können Ihre Projektkonfiguration für eine optimale Bereitstellung überprüfen, indem Sie [den Assistenten für den Idealzustand ausführen](smart-wizards.md#verifying-an-ideal-configuration).
