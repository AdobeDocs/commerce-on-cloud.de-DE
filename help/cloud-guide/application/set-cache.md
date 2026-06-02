---
title: Festlegen des Cache für statische Dateien
description: Erfahren Sie, wie Sie Cache-Speicheroptionen in der  [!DNL Commerce] -Konfigurationsdatei festlegen.
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 0%

---

# Festlegen des Cache für statische Dateien

Die TTL (Time-to-Live) für den Cache für Ihre Medien- und statischen Dateien wird in der `.magento.app.yaml`-Konfigurationsdatei mithilfe des `expires` festgelegt.

>[!NOTE]
>
>Bevor Sie Ihre Produktionsumgebung aktualisieren, müssen Sie die Änderungen in Ihrer Staging-Umgebung testen. [Senden eines Adobe Commerce-Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um Hilfe bei der Aktualisierung der Konfiguration in diesen Umgebungen zu erhalten.

1. Geben Sie die TTL-Zeit (in Sekunden) in der [`web`-Eigenschaft &#x200B;](web-property.md) der `.magento.app.yaml`-Datei an. Sie können den `expires` Schlüssel unter `locations` oder unter `"/media"` und `"/static"` hinzufügen.

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
