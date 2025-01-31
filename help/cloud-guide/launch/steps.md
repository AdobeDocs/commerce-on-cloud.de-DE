---
title: Launch-Schritte
description: Erfahren Sie, wie Sie den Site-Launch abschließen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Launch-Schritte

Nach dem Testen und Abschließen der Launch-Checkliste können Sie die letzten Schritte für den Launch starten. Zu diesen Schritten gehören das Eingeben von Tickets, der Zugriff auf die Transferseite und schließlich das Testen Ihres Stores, wenn er live ist.

Adobe-Support-Mitarbeiter arbeiten mit Ihnen durch den Prozess, prüfen den Status und helfen Ihnen, wenn Fragen oder Probleme auftreten. Alle Probleme sollten mit Tickets verfolgt werden, um am besten zu erfassen, was passiert ist und wie es gelöst wurde. Wenn Sie mit kontinuierlichen Iterationen beginnen, um Aktualisierungen für Ihren gestarteten Store bereitzustellen, können ähnliche Probleme erneut auftreten. Diese Tickets können dabei helfen, die Probleme zu identifizieren und Ihre Bereitstellungsaufgaben anzupassen.

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

Adobe überprüft und überwacht die Website, um sicherzustellen, dass alle Services und der Zugriff grün sind. Wir stehen Ihnen nach Bedarf zur Verfügung, um alles durchzugehen und zu überprüfen, ob alle Systemprotokolle, Dienste, Caching und Funktionen so funktionieren, wie Sie und Ihre Kunden es benötigen.

Wenn Probleme auftreten, erstellen und verfolgen Sie Probleme mit dem Support. Geben Sie so viele Informationen wie möglich ein, einschließlich Datum/Uhrzeit, spezifischer Funktionen mit einem Problem, Symptome und seltsames Verhalten, Erweiterungen und mehr. Wir untersuchen die Protokolle, das Problem und arbeiten mit Ihnen zusammen, um das Problem so schnell wie möglich zu beheben.
