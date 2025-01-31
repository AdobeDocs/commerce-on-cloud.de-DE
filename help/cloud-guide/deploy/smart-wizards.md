---
title: Intelligente Assistenten
description: Erfahren Sie, wie Sie mithilfe von intelligenten Assistenten bewerten können, ob Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt die Best Practices für die Bereitstellung befolgt.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Intelligente Assistenten

Die intelligenten Assistenten können Ihnen dabei helfen festzustellen, ob Ihre Cloud-Konfiguration den Best Practices entspricht. Die verfügbaren Assistenten unterstützen die folgenden Konfigurationen:

- Idealer Status für minimale Bereitstellungsausfälle
- Konfiguration des Lastenausgleichs für Datenbank und Redis
- Statische Inhaltsbereitstellung (SCD) für die On-Demand-Phase, die Build-Phase oder die Bereitstellungsphase

Jeder der Smart Wizard-Befehle liefert eine Überprüfungsantwort und, falls zutreffend, eine Empfehlung für die korrekte Konfiguration.

| Befehl | Beschreibung |
| ------- | ------------|
| `wizard:ideal-state` | Vergewissern Sie sich, dass sich die SCD _Build_-Schritt befindet, dass die `SKIP_HTML_MINIFICATION`-Variable `true` ist und dass der Hook „post_deploy“ in der Cloud-Umgebung konfiguriert ist. Nicht zur Verwendung in der lokalen Entwicklungsumgebung. |
| `wizard:master-slave` | Überprüfen Sie, ob die `REDIS_USE_SLAVE_CONNECTION` Variable und die `MYSQL_USE_SLAVE_CONNECTION` Variable `true` sind. |
| `wizard:scd-on-demand` | Überprüfen Sie, ob die globale `SCD_ON_DEMAND`-Umgebungsvariable `true` ist. |
| `wizard:scd-on-build` | Überprüfen Sie, ob die `SCD_ON_DEMAND` globale Umgebungsvariable `false` ist und die `SKIP_SCD` Umgebungsvariable für den _Build_-Schritt `false` ist. Überprüft, ob die `config.php` Informationen zu Stores, Store-Gruppen und Websites enthält. |
| `wizard:scd-on-deploy` | Überprüfen Sie, ob die `SCD_ON_DEMAND` globale Umgebungsvariable `false` und die `SKIP_SCD` Umgebungsvariable für den _Bereitstellen_-Schritt `false` ist. Überprüft, ob die `config.php`-Datei _NOT_ die Liste der Stores, Store-Gruppen und Websites mit zugehörigen Informationen enthält. |

Beispielsweise können Sie überprüfen, ob Ihre Konfiguration die SCD-On-Demand-Funktion ordnungsgemäß aktiviert:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Eine erfolgreiche Konfiguration gibt zurück:

```
SCD on-demand is enabled
```

Eine fehlgeschlagene Konfiguration gibt zurück:

```
SCD on-demand is disabled
```

## Überprüfen einer idealen Konfiguration

Die _ideale_ Konfiguration für Ihr Cloud-Projekt trägt dazu bei, Bereitstellungsausfallzeiten zu minimieren, indem der Cache erwärmt und statische Inhalte generiert werden, wenn sie vom Benutzer angefordert werden. Dieser Assistent wird während der Bereitstellung automatisch ausgeführt. Wenn Ihre Cloud nicht für diesen _idealen Status_ konfiguriert ist, erhalten Sie eine Nachricht ähnlich der folgenden:

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

Je nach Ausgabe müssen Sie die folgenden Korrekturen an Ihrer Konfiguration vornehmen:

1. Aktivieren Sie die Variable HTML-Minimierung überspringen .

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Konfigurieren Sie den Hook nach der Bereitstellung.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Pushen Sie Ihre Code-Änderungen und führen Sie den Test erneut aus. Wenn Ihre Konfiguration _ideal_ ist, erhalten Sie die folgende Meldung.

   ```
   Ideal state is configured
   ```
