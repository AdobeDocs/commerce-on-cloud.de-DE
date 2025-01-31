---
title: Schutzblock
description: Erfahren Sie mehr über die Schutzblockfunktion von Adobe Commerce in der Cloud-Infrastruktur und wie sie Ihre Site vor bekannten Sicherheitslücken schützt.
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
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
