---
title: '[!DNL ECE-Tools]'
description: Erfahren Sie mehr über das  [!DNL ECE-Tools] -Paket und wie es bei der Verwaltung und Bereitstellung von Adobe Commerce hilft.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# ECE-Tools-Paket

Das [!DNL ECE-Tools] Paket besteht aus einer Reihe von Skripten und Tools, die zur Verwaltung und Bereitstellung der [!DNL Commerce] Anwendung entwickelt wurden. Das `ece-tools` vereinfacht viele Prozesse, z. B. die Verwaltung von Cron-Aufträgen, die Überprüfung der Projektkonfiguration und das Anwenden von Adobe-Patches und Hotfixes. Sie können das [Open-Source-Code-Repository  [!DNL ECE-Tools]  GitHub anzeigen und zu ihm ][ece-repo].

{{ece-tools-package}}

Das `ece-tools`-Paket ist mit Adobe Commerce kompatibel (ab Version 2.1.4) und enthält Skripte und Adobe Commerce-Befehle für die Cloud-Infrastruktur, die zur Verwaltung des Codes und zum automatischen Erstellen und Bereitstellen Ihrer Projekte entwickelt wurden.

Im Folgenden sind die verfügbaren `ece-tools`-Befehle aufgeführt:

```bash
php ./vendor/bin/ece-tools list
```

## Erstellen und Bereitstellen

Das `ece-tools`-Paket enthält Befehle zum Ausführen von Vorgängen für die Build-, Bereitstellungs- und Post-Bereitstellungsphasen des Starts Ihres Adobe Commerces auf einer Cloud-Infrastrukturanwendung. Beispielsweise startet der Befehl `php ./vendor/bin/ece-tools build` den Anwendungserstellungsprozess.

Standardmäßig befinden sich diese `ece-tools`-Befehle in der [hooks](../application/hooks-property.md) der `.magento.app.yaml`.

## Docker-Konfigurationsgenerator

Das `ece-tools`-Paket enthält eine Abhängigkeit für das Paket [magento/magento-cloud-docker], das Funktionen und Konfigurationsdateien für Docker-Images bereitstellt, um eine Docker-Entwicklungsumgebung für Adobe Commerce in der Cloud-Infrastruktur zu starten. Sie können Cloud Docker für Commerce auch als eigenständiges Paket ausführen. Siehe [Docker-](../dev-tools/cloud-docker.md).

## Dienste, Routen und Variablen

Sie können das `ece-tools`-Paket verwenden, um detaillierte Informationen zu den Base64-kodierten [Cloud-Variablen](../environment/variables-cloud.md) anzuzeigen, die in jeder Cloud-Umgebung verwendet werden. Der folgende Befehl zeigt alle Services, Routen und Variablen.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Verwenden Sie das folgende Format, um einen bestimmten Satz von Informationen anzuzeigen:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - Zeigt die Beziehungsdaten aus der `MAGENTO_CLOUD_RELATIONSHIPS` Umgebungsvariablen an, die in der `services.yaml`-Datei definiert sind.
- `routes` - Zeigt die konfigurierten Routen für das Projekt mithilfe der `MAGENTO_CLOUD_ROUTES` Umgebungsvariablen an.
- `variables` - Zeigt die konfigurierten Variablen für das Projekt mithilfe der `MAGENTO_CLOUD_VARIABLES` Umgebungsvariablen an.

Beispielausgabe für die `services`:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Überprüfen der Umgebungskonfiguration

Es steht eine Reihe von Überprüfungsbefehlen zur Verfügung, mit denen Sie die Konfiguration Ihres Projekts überprüfen können. Siehe [Intelligente Assistenten](../deploy/smart-wizards.md) im Abschnitt _Bereitstellung optimieren_ für eine ausführliche Beschreibung jedes Assistenten-Befehls. Der Befehl `wizard:ideal-state` wird während der Build-Phase automatisch ausgeführt. So überprüfen Sie den idealen Zustand Ihres Projekts:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Sie müssen den `wizard:ideal-state`-Befehl in der Remote-Cloud-Umgebung ausführen. Der Befehl gibt bei der Ausführung in der lokalen Entwicklungsumgebung immer den `The configured state is not ideal` Fehler zurück.

Beispielausgabe:

```
Ideal state is configured
```

Siehe [Versionshinweise für ECE-Tools](../release-notes/cloud-tools-suite.md).

## Adobe von Patches und benutzerdefinierten Patches

Das `ece-tools`-Paket enthält eine Abhängigkeit für das Paket [magento/magento-cloud-patches], das Adobe-Patches und Hotfixes bereitstellt, die die Integration aller Adobe Commerce-Versionen mit Cloud-Umgebungen verbessern und die schnelle Bereitstellung wichtiger Fehlerbehebungen unterstützen. „stellt auch benutzerdefinierte Patches bereit, die Sie Ihrem Adobe Commerce in einem Cloud-Infrastrukturprojekt hinzufügen. Siehe [Patches anwenden](../development/apply-patches.md).

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[Magento/Magento-Cloud-Docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
