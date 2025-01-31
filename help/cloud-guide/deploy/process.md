---
title: Bereitstellungsprozess
description: Erfahren Sie, wie die Bereitstellung für Adobe Commerce in Cloud-Infrastrukturprojekten funktioniert.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Bereitstellungsprozess

Trigger Der Bereitstellungsprozess beginnt, wenn Sie eine Zusammenführung, einen Push oder eine Synchronisierung Ihrer Umgebung durchführen oder eine [manuelle Neubereitstellung) ](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Der Bereitstellungsprozess dauert seine Zeit. Es gibt jedoch Möglichkeiten, die Bereitstellung zu optimieren, je nachdem, ob Sie eine Live-Site entwickeln und testen oder mit ihr arbeiten. Insbesondere können Sie die „Bereitstellung [ statischen Inhalts“ ](static-content.md).

Es gibt drei verschiedene Phasen des Bereitstellungsprozesses: Erstellung, Bereitstellung und Nachbereitstellung. Jede Phase führt spezifische Aktionen mit begrenzten Ressourcen durch:

## ![Build-Phase](../../assets/status-build.png) Build-Phase

Die _build_-Phase stellt Container für die in den Konfigurationsdateien definierten Services zusammen, installiert Abhängigkeiten basierend auf der `composer.lock` und führt die in der `.magento.app.yaml`-Datei definierten Build-Hooks aus. Ohne die Möglichkeit, eine Verbindung zu Services herzustellen oder auf die Datenbank zuzugreifen, hängt die Build-Phase von den Ressourcen ab, die auf die Umgebung beschränkt sind.

## ![Bereitstellungsphase](../../assets/status-deploy.png) Bereitstellungsphase

Die _Bereitstellungs_-Phase hält eingehende Anfragen vorübergehend zurück und wechselt die Site in den [Wartungsmodus](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). In der Bereitstellungsphase werden die neuen Container verwendet. Nach dem Mounten des Dateisystems werden Netzwerkverbindungen geöffnet, die im Abschnitt `relationships` der `.magento.app.yaml`-Datei definierten Services aktiviert und die in der `.magento.app.yaml`-Datei definierten Bereitstellungs-Hooks ausgeführt. Alles ist _schreibgeschützt_ mit Ausnahme von Verzeichnissen, die in der `.magento.app.yaml`-Datei definiert sind. Standardmäßig umfasst die [`mounts`-Eigenschaft ](../application/properties.md#mounts) folgenden Verzeichnisse:

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
