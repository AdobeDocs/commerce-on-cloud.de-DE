---
title: Schutzblock
description: Erfahren Sie mehr über die Schutzblockfunktion von Adobe Commerce in der Cloud-Infrastruktur und wie sie Ihre Site vor bekannten Sicherheitslücken schützt.
feature: Cloud, Configuration, Security
topic: Security
exl-id: 4a470e75-0b42-4ab7-b3dc-9f50b63bea14
TQID: https://experienceleague.adobe.com/E0lyCu6cFEaHR0KoauRFmQi9QThzoGDmNetnzYv3hVg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 0%

---

# Schutzblock

Adobe Commerce in der Cloud-Infrastruktur verfügt über eine Schutzsperre, die unter bestimmten Umständen den Zugriff auf Websites mit Sicherheitslücken einschränkt. Diese partielle Sperrmethode verhindert die Ausnutzung bekannter Sicherheitslücken. Veraltete Software enthält häufig Exploits. Daher ist es wichtig, diese Exploits zu schützen, indem der Zugriff auf diese Sites teilweise blockiert wird.

## Funktionsweise des Schutzblocks

Adobe Commerce unterhält eine Signaturdatenbank bekannter Sicherheitslücken in Open-Source-Software, die häufig in der Cloud-Infrastruktur bereitgestellt werden. Die Sicherheitsprüfung analysiert nur bekannte Schwachstellen in Open-Source-Projekten und kann keine von Ihnen geschriebenen Anpassungen untersuchen.

Die Sicherheitsprüfung wird ausgeführt:

- Wenn Sie neuen Code an Git pushen
- Wenn neue Sicherheitslücken zur Datenbank hinzugefügt werden

Wenn in Ihrer Anwendung eine kritische Sicherheitslücke entdeckt wird, wird die Git-Push-Benachrichtigung abgelehnt.

Es gibt zwei Arten von Blöcken, die ausgeführt werden:

1. **Komplettblock** - für Entwicklungs-Websites. Die `git push` beigefügte Fehlermeldung enthält detaillierte Informationen zur Sicherheitslücke.

1. **Teilblockierung** - für Produktions-Websites, mit denen die Website größtenteils online bleiben kann. Je nach Art der Sicherheitslücke können Teile einer Anfrage, wie z. B. eine Abfragezeichenfolge, Cookies oder zusätzliche Header, aus GET-Anfragen entfernt werden. Alle anderen Anfragen können vollständig blockiert werden, z. B. Anmeldung, Formularübermittlung oder Produktkasse.

Die Aufhebung der Blockierung erfolgt automatisch bei Behebung des Sicherheitsrisikos. Die Sperre wird kurz nach der Anwendung eines Sicherheits-Upgrades entfernt, das die Sicherheitslücke entfernt.

## Deaktivieren des Schutzblocks

Der Schutzblock dient dazu, Sie vor bekannten Sicherheitslücken in der Software zu schützen, die Sie in der Cloud-Infrastruktur von Adobe Commerce bereitstellen.

Sie können den Schutzblock jedoch deaktivieren, indem Sie Folgendes zu [`.magento.app.yaml`](../application/configure-app-yaml.md) hinzufügen:

```yaml
   preflight:
      enabled: false
```

Sie können eine bestimmte Prüfung explizit deaktivieren, z. B.:

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
