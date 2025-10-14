---
title: Fehlermeldungen für das ECE-Tools-Paket
description: Hier finden Sie eine Liste der Fehler-Codes und Meldungen, die während des Erstellungs-, Bereitstellungs- und Nachbereitstellungsprozesses von Adobe Commerce in der Cloud-Infrastruktur auftreten können.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Fehlermeldungen für ECE-Tools

Diese Referenz zu Fehlermeldungen enthält Informationen zur Fehlerbehebung bei Fehlern, die während der Prozesse zum Erstellen, Bereitstellen und Nachbereitstellen von Adobe Commerce in der Cloud-Infrastruktur auftreten können.

Alle kritischen Meldungen und Warnfehlermeldungen, die während der Bereitstellung auftreten, werden sowohl in die `var/log/cloud.log`- als auch in die `/var/log/cloud.error.log`-Dateien geschrieben. Die Cloud-Fehlerprotokolldatei enthält nur Fehler aus der neuesten Bereitstellung. Eine leere Datei bedeutet, dass die Bereitstellung ohne Fehler erfolgreich war.

In der `cloud.error.log`-Datei wird jeder Eintrag als JSON-Zeichenfolge formatiert, um das Analysieren zu erleichtern:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

Fehlermeldungen werden nach einer der Bereitstellungsphasen kategorisiert: Erstellen, Bereitstellen und Nachbereitstellen. Jeder Abschnitt enthält eine Liste der zugehörigen Fehler mit den folgenden Informationen für jeden Fehler:

- **Fehlercode**: Die von Adobe Commerce zugewiesene Kennung für die Fehlermeldung
- **Staging**: Gibt an, ob der Fehler während der Build-, Bereitstellungs- oder Nach-Bereitstellungsphase aufgetreten ist
- **Schritt**: Gibt den Schritt im Bereitstellungsszenario an, der den Fehler zurückgeben kann. Wenn die _Schritt_-Spalte leer ist, handelt es sich bei dem Fehler um einen allgemeinen Fehler, der in mehreren Schritten oder bei Vorab-Verarbeitungsvorgängen zurückgegeben werden kann. Weitere [&#x200B; zu den Schritten &#x200B;](../deploy/scenario-based.md) Erstellen, Bereitstellen und Nachbereitstellen finden Sie unter „Szenariobasierte Bereitstellung“.
- **Vorschlag**: Bietet Anleitungen zur Fehlerbehebung und Fehlerbehebung
- **Titel (Fehlerbeschreibung)**: Eine Beschreibung, die die Ursache des Fehlers zusammenfasst
- **Typ**: Gibt an, ob der Fehler ein kritischer Fehler oder eine Warnung ist

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
