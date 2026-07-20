---
title: Anforderungen an ein CMS-Backend umleiten
description: Erfahren Sie, wie Sie eingehende Anfragen von einem Adobe Commerce-Store mithilfe des Fastly-Edge-Moduls auf eine separate WordPress-Site umleiten können.
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
TQID: https://experienceleague.adobe.com/zRM-iTFGNPgSmT5xu1B9Lo3-onUtCHh-tVY-WPPiVC8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: bcac8986e748f6e513d4db22ee7eb7f64bac1a3a
workflow-type: tm+mt
source-wordcount: 322
ht-degree: 0%

---

# Anforderungen an ein CMS-Backend umleiten

Eingehende Anfragen von einem Adobe Commerce-Store auf eine separate WordPress-Website umleiten, indem Sie das Fastly Edge-Modul _Andere CMS-/Backend-Integration_ mit einem Edge-Wörterbuch verwenden. Sie können einen ähnlichen Prozess ausführen, um Anfragen an andere CMS-Backends weiterzuleiten.

Verwenden Sie die Fastly Edge-Module, um benutzerdefinierten VCL-Code vom Admin zu erstellen und hochzuladen, anstatt den VCL-Code manuell zu schreiben und ihn mit der Fastly-API hochzuladen.

>[!NOTE]
>
>Es wird empfohlen, benutzerdefinierte VCL-Konfigurationen zu einer Staging-Umgebung hinzuzufügen, in der Sie sie testen können, bevor Sie die Fastly-Service-Konfiguration in der Produktionsumgebung aktualisieren.

**Voraussetzungen**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**So leiten Sie Anfragen von Adobe Commerce an WordPress weiter**:

1. Aktivieren Sie Fastly Edge-Module in der Staging- oder Produktionsumgebung.

   - Melden Sie sich beim Administrator an.

   - Navigieren Sie zu **Stores** > Einstellungen > **Konfiguration** > **Erweitert** > **System** > **Vollständiger Seitencache** > **Fastly Configuration** > **Erweiterte Konfiguration**.

   - Setzen Sie den Wert für **Fastly Edge Modules** auf **Ja**.

   - Speichern Sie die Konfiguration.

1. Identifizieren Sie die URL-Pfade, die zum WordPress-Backend umgeleitet werden sollen.

1. Führen Sie die folgenden Aufgaben aus, um den Fastly-Service zu konfigurieren und den benutzerdefinierten VCL-Code für die Umleitung von Anfragen an das WordPress-Backend zu erstellen.

   - Erstellen Sie ein Edge-Wörterbuch, das die Pfade angibt, die vom Adobe Commerce-Store zum Backend umgeleitet werden sollen.

   - Fügen Sie das WordPress-Backend zur Fastly-Service-Konfiguration hinzu und fügen Sie die Anfragebedingung für die URL-Neuschreibungen hinzu.

   - Konfigurieren Sie das Edge-Modul _Sonstige CMS/Backend_ Integration, um das Umschreiben von URLs von Adobe Commerce in das WordPress-Backend zu verarbeiten.

     Detaillierte Anweisungen finden Sie unter [Fastly Edge Modules - Other CMS/Backend Integration](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) in der Dokumentation _Fastly CDN Module for Magento 2_.

1. Nach dem Aktualisieren der Fastly-Service-Konfiguration testen Sie Ihren Adobe Commerce-Store, um sicherzustellen, dass die angegebenen URL-Anfragen für WordPress korrekt umgeleitet werden.

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
