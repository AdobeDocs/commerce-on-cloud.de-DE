---
title: Anforderungen an ein CMS-Backend umleiten
description: Erfahren Sie, wie Sie eingehende Anfragen von einem Adobe Commerce-Store mithilfe des Fastly-Edge-Moduls auf eine separate WordPress-Site umleiten können.
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '307'
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
