---
title: Festlegen des Cache für statische Dateien
description: Erfahren Sie, wie Sie Cache-Speicheroptionen in der  [!DNL Commerce] -Konfigurationsdatei festlegen.
feature: Cloud, Configuration, Cache, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Festlegen des Cache für statische Dateien

Die TTL (Time-to-Live) für den Cache für Ihre Medien- und statischen Dateien wird in der `.magento.app.yaml`-Konfigurationsdatei mithilfe des `expires` festgelegt.

>[!NOTE]
>
>Bevor Sie Ihre Produktionsumgebung aktualisieren, müssen Sie die Änderungen in Ihrer Staging-Umgebung testen. [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um Hilfe bei der Aktualisierung der Konfiguration in diesen Umgebungen zu erhalten.

1. Geben Sie die TTL-Zeit (in Sekunden) in der [`web`-Eigenschaft ](web-property.md) der `.magento.app.yaml`-Datei an. Sie können den `expires` Schlüssel unter `locations` oder unter `"/media"` und `"/static"` hinzufügen.

   Um ein Ablaufen des Cache zu verhindern, verwenden Sie das Schlüssel-Wert-Paar `expires: -1` . Siehe folgendes Beispiel:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```
