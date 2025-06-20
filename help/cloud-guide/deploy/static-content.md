---
title: Statische Inhaltsbereitstellung
description: Erfahren Sie mehr über Strategien zur Bereitstellung von statischen Inhalten wie Bildern, Skripten und CSS in Adobe Commerce in Cloud-Infrastrukturprojekten.
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Strategien zur Bereitstellung statischer Inhalte

Die Bereitstellung statischer Inhalte (Static Content Deployment, SCD) hat einen erheblichen Einfluss auf den Prozess der Speicherbereitstellung, der davon abhängt, wie viel Inhalt generiert werden soll (z. B. Bilder, Skripte, CSS, Videos, Themen, Gebietsschemata und Web-Seiten) und wann der Inhalt generiert wird. Beispielsweise generiert die Standardstrategie statische Inhalte während der [Bereitstellungsphase](process.md#deploy-phase-deploy-phase) wenn sich die Site im Wartungsmodus befindet. Diese Bereitstellungsstrategie benötigt jedoch Zeit, um die Inhalte direkt in das bereitgestellte `pub/static`-Verzeichnis zu schreiben. Sie haben verschiedene Optionen oder Strategien, um die Bereitstellungszeit je nach Bedarf zu verbessern.

## JavaScript- und HTML-Inhalte optimieren

Sie können Bundling und Minimierung verwenden, um optimierte JavaScript- und HTML-Inhalte während der Bereitstellung statischer Inhalte zu erstellen.

### Inhalt minimieren

Sie können die Ladezeit der SCD während des Bereitstellungsprozesses verbessern, wenn Sie die statischen Ansichtsdateien im `var/view_preprocessed`-Verzeichnis überspringen und auf Anfrage _minimierte_ HTML generieren. Sie können dies aktivieren, indem Sie die globale Umgebungsvariable [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) auf `true` in der `.magento.env.yaml` festlegen.

>[!NOTE]
>
>Ab der `ece-tools`-Paketversion 2002.0.13 wird der Standardwert für die Variable SKIP_HTML_MINIFICATION auf `true` festgelegt.

Sie können **mehr** Bereitstellungszeit und Speicherplatz sparen, indem Sie die Anzahl der unnötigen Design-Dateien reduzieren. Sie können beispielsweise das `magento/backend` Design auf Englisch und ein benutzerdefiniertes Design in anderen Sprachen bereitstellen. Sie können diese Designeinstellungen mit der Umgebungsvariablen [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix) konfigurieren.

## Auswählen einer Bereitstellungsstrategie

Die Bereitstellungsstrategien unterscheiden sich je nachdem, ob Sie statische Inhalte während der _Build_-Phase, der _Bereitstellungs_-Phase oder _On-Demand-_ generieren. Wie im folgenden Diagramm dargestellt, ist das Generieren statischer Inhalte während der Bereitstellungsphase die am wenigsten optimale Wahl. Selbst bei minimierter HTML muss jede Inhaltsdatei in das bereitgestellte `~/pub/static`-Verzeichnis kopiert werden, was lange dauern kann. Die Erstellung statischer Inhalte nach Bedarf scheint die optimale Wahl zu sein. Wenn die Inhaltsdatei jedoch zum Zeitpunkt der Anforderung nicht im Cache vorhanden ist, wird sie generiert, was dem Benutzererlebnis eine Ladezeit hinzufügt. Daher ist die Generierung statischer Inhalte während der Build-Phase die optimalste.

![SCD-Lastvergleich](../../assets/scd-load-times.png)

### Festlegen der SCD beim Build

Das Generieren von statischem Inhalt während der Build-Phase mit minimiertem HTML ist die optimale Konfiguration für [**Bereitstellungen ohne Ausfallzeiten**, ](reduce-downtime.md) auch als **Idealzustand“**. Anstatt Dateien auf ein gemountetes Laufwerk zu kopieren, wird ein Symlink aus dem `./init/pub/static` Verzeichnis erstellt.

Zum Generieren statischer Inhalte ist Zugriff auf Designs und Gebietsschemata erforderlich. Adobe Commerce speichert Designs im Dateisystem, auf das während der Build-Phase zugegriffen werden kann. Adobe Commerce speichert jedoch Gebietsschemata in der Datenbank. Die Datenbank _während_ Erstellungsphase nicht verfügbar. Um den statischen Inhalt während der Build-Phase zu generieren, müssen Sie den `config:dump`-Befehl im `ece-tools`-Paket verwenden, um Gebietsschemata in das Dateisystem zu verschieben. Es liest die Gebietsschemata und speichert sie in der `app/etc/config.php`.

>[!NOTE]
>Nachdem Sie den `config:dump`-Befehl im `ece-tools`-Paket ausgeführt haben, werden die Konfigurationen, die in der `config.php`-Datei abgelegt werden [im Admin-Dashboard gesperrt (ausgegraut)](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin). Die einzige Möglichkeit, diese Konfigurationen in Admin zu aktualisieren, besteht darin, sie lokal aus der Datei zu löschen und das Projekt erneut bereitzustellen.
>>Darüber hinaus sollten Sie jedes Mal, wenn Sie Ihrer Instanz eine neue Store-/Store-Gruppe/-Website hinzufügen, daran denken, den `config:dump`-Befehl auszuführen, um sicherzustellen, dass die Datenbank synchronisiert ist. Sie können auch [welche Konfigurationen ausgegeben werden sollen](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en) in der `config.php`-Datei auswählen.
>>Wenn Sie die Konfiguration der Store-/Store-Gruppe/Website aus der `config.php` löschen, da die Felder ausgegraut sind, diesen Schritt jedoch nicht ausführen, werden die neuen Entitäten, die nicht gedumpt wurden, bei der nächsten Bereitstellung aus der Datenbank gelöscht.

**So konfigurieren Sie Ihr Projekt für die Generierung von SCD beim Build**:

1. Wechseln Sie auf Ihrer lokalen Workstation in Ihr Projektverzeichnis.
1. Verwenden Sie SSH, um sich bei der Remote-Umgebung anzumelden.

   ```bash
   magento-cloud ssh
   ```

1. Verschieben Sie Gebietsschemata in das Dateisystem und aktualisieren Sie dann die [`config.php` Datei](../development/commerce-version.md#create-a-configphp-file).

1. Die `.magento.env.yaml`-Konfigurationsdatei sollte die folgenden Werte enthalten:

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) ist `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) beim Build-Schritt ist `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) ist `compact`

1. Überprüfen Sie die Konfiguration [ Hooks „Nach der Bereitstellung](../application/hooks-property.md) in der `.magento.app.yaml`.

1. Überprüfen Sie Ihre Einstellungen, indem Sie den [Smart-Assistenten für den idealen Status](smart-wizards.md) ausführen.

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### SCD bei Bedarf einstellen

Die Erstellung von SCD on demand ist optimal für einen Entwicklungs-Workflow in der Integrationsumgebung. Dadurch wird die Bereitstellungszeit verkürzt, sodass Sie Ihre Implementierungen schnell überprüfen und Integrationstests ausführen können. Aktivieren Sie [ Umgebungsvariable SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) im globalen Schritt der `.magento.env.yaml`. Die Variable SCD_ON_DEMAND überschreibt alle anderen Konfigurationen im Zusammenhang mit SCD und löscht vorhandene Inhalte aus dem `~/pub/static`.

Bei Verwendung der SCD-On-Demand-Strategie ist es hilfreich, den Cache vorab mit Seiten zu laden, die Sie erwarten, z. B. die -Startseite. Fügen Sie Ihre Liste der erwarteten Seiten in [ Umgebungsvariablen WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) in der Phase nach der Bereitstellung der `.magento.env.yaml` hinzu.

>[!WARNING]
>
>Verwenden Sie die SCD-On-Demand-Strategie nicht in der Produktionsumgebung.

### SCD wird übersprungen

Manchmal können Sie die Generierung statischer Inhalte vollständig überspringen. Sie können die Umgebungsvariable [SKIP_SCD](../environment/variables-build.md#skipscd) im globalen Schritt so einstellen, dass andere Konfigurationen im Zusammenhang mit SCD ignoriert werden. Vorhandene Inhalte im `~/pub/static` sind davon nicht betroffen.
