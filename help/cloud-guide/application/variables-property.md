---
title: Variablen-Eigenschaft
description: Verwenden Sie die Variableneigenschaft, um Store-Konfigurationsoptionen für die [!DNL Commerce]-Anwendung anzupassen.
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: 'https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%
---
# Variablen-Eigenschaft

Sie können anwendungsbasierte Umgebungsvariablen verwenden, um Store-Konfigurationen anzupassen. Diese Variablen verwenden eine bestimmte Syntax. Siehe [Konfigurationseinstellungen überschreiben](https://experienceleague.adobe.com/de/docs/commerce-operations/configuration-guide/paths/override-config-settings) im _Konfigurationshandbuch_.

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

