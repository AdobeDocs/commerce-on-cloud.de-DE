---
title: Bereitstellungsprozess
description: Erfahren Sie, wie die Bereitstellung für Adobe Commerce in Cloud-Infrastrukturprojekten funktioniert.
feature: Cloud, Build, Deploy, SCD
exl-id: 76806381-0ecc-4d76-974a-f203d3bf44da
TQID: 'https://experienceleague.adobe.com/mSJOsLfNVGbkSNSrUzJgszxsqc07c-4KFhrJxm5I72U'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
subfeature_v2:
  - id: adedf3b3-e153-47a3-ae73-b5d65067b544
    internal-label: Build system
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
    internal-label: Optimization
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%
---
# Bereitstellungsprozess

Der Bereitstellungsprozess beginnt, wenn Sie eine Zusammenführung, einen Push oder eine Synchronisierung Ihrer Umgebung durchführen oder eine [manuelle Neubereitstellung) ](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Der Bereitstellungsprozess dauert seine Zeit. Es gibt jedoch Möglichkeiten, die Bereitstellung zu optimieren, je nachdem, ob Sie eine Live-Site entwickeln und testen oder mit ihr arbeiten. Insbesondere können Sie die „Bereitstellung [ statischen Inhalts“ ](static-content.md).

Es gibt drei verschiedene Phasen des Bereitstellungsprozesses: Erstellung, Bereitstellung und Nachbereitstellung. Jede Phase führt spezifische Aktionen mit begrenzten Ressourcen durch:

## ![Build-Phase](../../assets/status-build.png) Build-Phase

Die _build_-Phase stellt Container für die in den Konfigurationsdateien definierten Services zusammen, installiert Abhängigkeiten basierend auf der `composer.lock` und führt die in der `.magento.app.yaml`-Datei definierten Build-Hooks aus. Ohne die Möglichkeit, eine Verbindung zu Services herzustellen oder auf die Datenbank zuzugreifen, hängt die Build-Phase von den Ressourcen ab, die auf die Umgebung beschränkt sind.

## ![Bereitstellungsphase](../../assets/status-deploy.png) Bereitstellungsphase

Die _Bereitstellungs_-Phase hält eingehende Anfragen vorübergehend zurück und wechselt die Site in den [Wartungsmodus](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/setup/application-modes). In der Bereitstellungsphase werden die neuen Container verwendet. Nach dem Mounten des Dateisystems werden Netzwerkverbindungen geöffnet, die im Abschnitt `relationships` der `.magento.app.yaml`-Datei definierten Services aktiviert und die in der `.magento.app.yaml`-Datei definierten Bereitstellungs-Hooks ausgeführt. Alles ist _schreibgeschützt_ mit Ausnahme von Verzeichnissen, die in der `.magento.app.yaml`-Datei definiert sind. Standardmäßig umfasst die [`mounts`-Eigenschaft ](../application/properties.md#mounts) folgenden Verzeichnisse:

- `app/etc` - Enthält die `env.php` und `config.php` Konfigurationsdateien
- `pub/media` - Enthält alle Mediendaten, wie Produkte oder Kategorien
- `pub/static` - Enthält generierte statische Dateien
- `var` - Enthält temporäre Dateien, die während der Laufzeit erstellt werden

Alle anderen Ordner haben schreibgeschützte Berechtigungen. Die neue Site wird am Ende der Bereitstellungsphase aktiv, sobald sie aus dem Wartungsmodus wechselt, und gibt den temporären Haltestatus für eingehende Anfragen frei.

In der Bereitstellungsphase werden Kopien der `app/etc/config.php`- und `app/etc/env.php`-Bereitstellungskonfigurationsdateien mit der BAK-Erweiterung gespeichert. Weitere Informationen [ Wiederherstellen dieser Dateien finden ](../store/store-settings.md#restore-configuration-files) unter „Einstellungen speichern.

## ![Phase nach der Bereitstellung](../../assets/status-post-deploy.png) Phase nach der Bereitstellung

In _Phase „post_ deploy“ werden die in der `.magento.app.yaml`-Datei definierten Hooks nach der Bereitstellung ausgeführt. Die Durchführung einer Aktion in dieser Phase kann sich auf die Leistung der Site auswirken. Sie können jedoch die Umgebungsvariable [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) verwenden, um den Cache zu füllen.

## ![Status überprüfen](../../assets/status-verify.png) Konfigurationen überprüfen

Sie können die optimale Konfiguration für den Status Ihres Projekts testen, indem Sie die [Smart-Assistenten](smart-wizards.md) ausführen.

>[!NOTE]
>
>Ab `ece-tools` 2002.1.0 können Sie die szenarienbasierte Bereitstellungsfunktion verwenden, um die Erstellungs-, Bereitstellungs- und Nachbereitstellungsprozesse für Ihr Adobe Commerce in Cloud-Infrastrukturprojekt anzupassen. Siehe [Szenariobasierte Bereitstellung](scenario-based.md).

