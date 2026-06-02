---
title: Variablen-Eigenschaft
description: Verwenden Sie die Variableneigenschaft, um Speicherkonfigurationsoptionen für die Anwendung  [!DNL Commerce] .
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 85
ht-degree: 0%

---

# Variablen-Eigenschaft

Sie können anwendungsbasierte Umgebungsvariablen verwenden, um Store-Konfigurationen anzupassen. Diese Variablen verwenden eine bestimmte Syntax. Siehe [Konfigurationseinstellungen überschreiben](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=de) im _Konfigurationshandbuch_.

Die folgenden in der `.magento.app.yaml`-Datei enthaltenen Umgebungsvariablen sind für bestimmte Versionen des [!DNL Commerce]-Programms erforderlich.

Erforderlich für Adobe Commerce 2.2.x bis 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Legen Sie für Adobe Commerce 2.4.x die folgenden Variablen fest:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
