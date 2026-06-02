---
title: Launch-Schritte
description: Erfahren Sie, wie Sie den Site-Launch abschließen.
exl-id: e7a3cd6b-32de-4fd0-9fbd-da8299e77114
TQID: https://experienceleague.adobe.com/Nl8YFJUZUkxtsm0eqBgJklsTysKuUE31PkEDsJmvBMU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 0%

---

# Launch-Schritte

Nach dem Testen und Abschließen der Launch-Checkliste können Sie die letzten Schritte für den Launch starten. Zu diesen Schritten gehören das Eingeben von Tickets, der Zugriff auf die Transferseite und schließlich das Testen Ihres Stores, wenn er live ist.

Die Support-Mitarbeiter von Adobe unterstützen Sie während des gesamten Prozesses, prüfen den Status und helfen Ihnen bei Fragen oder Problemen. Alle Probleme sollten mit Tickets verfolgt werden, um am besten zu erfassen, was passiert ist und wie es gelöst wurde. Wenn Sie mit kontinuierlichen Iterationen beginnen, um Aktualisierungen für Ihren gestarteten Store bereitzustellen, können ähnliche Probleme erneut auftreten. Diese Tickets können dabei helfen, die Probleme zu identifizieren und Ihre Bereitstellungsaufgaben anzupassen.

## Adobe kontaktieren, um Ihre Site zu starten

Wenden Sie sich an den Adobe Commerce-Support und aktualisieren Sie alle Eintrittskarten für den Site-Start (Live-Schaltung) mit dem vorgesehenen Datum und der vorgesehenen Uhrzeit für den Wechsel und den Start Ihres Stores.

## DNS auf die neue Site wechseln

Der Wert Geänderte Zeit bis zur Live-Schaltung ist wichtig, um Ihre geänderte Domain zu überprüfen. Wenn Sie die A- und CNAME-Datensätze ändern, dauert die Aktualisierung der TTL-Konfiguration länger, bis sie korrekt aktualisiert wird. Weitere Informationen zu DNS-Einstellungen finden Sie unter [DNS-Konfigurationen](checklist.md#update-dns-configuration-with-production-settings).

### So wechseln Sie zur neuen Site:

1. Greifen Sie auf Ihren DNS-Service zu.

1. Aktualisieren Sie Ihre A- und CNAME-Einträge für Ihre Domains und Hostnamen.

1. Warten Sie, bis die TTL-Zeit verstrichen ist, und starten Sie Ihren Webbrowser neu.

1. Greifen Sie über die Storefront-Domain auf Ihren Store zu.

## Testen des Live Stores

Führen Sie einige UAT-Tests in Ihrem Live-Store durch, um zu bestätigen, dass alles geladen wird und Aktionen korrekt abgeschlossen werden. Eine Liste der Tests finden Sie unter [Testbereitstellung](../test/staging-and-production.md#complete-uat-testing).

## Nach dem Launch

Adobe überprüft und überwacht die Site, um sicherzustellen, dass alle Services und der Zugriff grün dargestellt werden. Wir stehen Ihnen nach Bedarf zur Verfügung, um alles durchzugehen und zu überprüfen, ob alle Systemprotokolle, Dienste, Caching und Funktionen so funktionieren, wie Sie und Ihre Kunden es benötigen.

Wenn Probleme auftreten, erstellen und verfolgen Sie Probleme mit dem Support. Geben Sie so viele Informationen wie möglich ein, einschließlich Datum/Uhrzeit, spezifischer Funktionen mit einem Problem, Symptome und seltsames Verhalten, Erweiterungen und mehr. Wir untersuchen die Protokolle, das Problem und arbeiten mit Ihnen zusammen, um das Problem so schnell wie möglich zu beheben.
