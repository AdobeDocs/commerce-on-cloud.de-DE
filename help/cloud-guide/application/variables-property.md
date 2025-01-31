---
title: Variablen-Eigenschaft
description: Verwenden Sie die Variableneigenschaft, um Speicherkonfigurationsoptionen für die Anwendung  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Variablen-Eigenschaft

Sie können anwendungsbasierte Umgebungsvariablen verwenden, um Store-Konfigurationen anzupassen. Diese Variablen verwenden eine bestimmte Syntax. Siehe [Konfigurationseinstellungen überschreiben](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) im _Konfigurationshandbuch_.

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
