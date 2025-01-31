---
title: Hooks-Eigenschaft
description: Siehe Beispiele zum Konfigurieren der Hooks-Eigenschaft in der Konfigurationsdatei  [!DNL Commerce] .application.
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Hooks-Eigenschaft

Verwenden Sie den Abschnitt `hooks` , um Shell-Befehle während der Build-, Bereitstellungs- und Nachbereitstellungsphasen auszuführen:

- **`build`** - Führt Befehle aus _bevor_ Anwendung verpackt wird. Dienste wie die Datenbank oder Redis sind nicht verfügbar, da die Anwendung noch nicht bereitgestellt wurde. Fügen Sie benutzerdefinierte Befehle _vor_ dem Standardbefehl `php ./vendor/bin/ece-tools` hinzu, damit benutzergenerierter Inhalt in der Bereitstellungsphase fortgesetzt wird.

- **`deploy`**: Ausführen von Befehlen _nach_ Verpacken und Bereitstellen der Anwendung. An dieser Stelle können Sie auf andere Dienste zugreifen. Da der standardmäßige `php ./vendor/bin/ece-tools`-Befehl das `app/etc` an den richtigen Speicherort kopiert, müssen Sie (_)_ Bereitstellungsbefehl benutzerdefinierte Befehle hinzufügen, um Fehler bei benutzerdefinierten Befehlen zu vermeiden.

- **`post_deploy`** - Führt Befehle aus _nachdem_ Anwendung bereitgestellt wurde und _danach_ beginnt der Container Verbindungen zu akzeptieren. Der `post_deploy`-Hook löscht den Cache und lädt den Cache vorab (erwärmt). Sie können die Liste der Seiten mithilfe der `WARM_UP_PAGES` Variable im [Phase nach der Bereitstellung“ ](../environment/variables-post-deploy.md). Dies ist zwar nicht erforderlich, funktioniert aber zusammen mit der Umgebungsvariablen `SCD_ON_DEMAND` .

Das folgende Beispiel zeigt die Standardkonfiguration in der `.magento.app.yaml`. Fügen Sie CLI-Befehle unter den Abschnitten `build`, `deploy` oder `post_deploy` (_)_ `ece-tools` Befehl hinzu:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Außerdem können Sie die Build-Phase weiter anpassen, indem Sie die Befehle `generate` und `transfer` verwenden, um zusätzliche Aktionen speziell beim Erstellen von Code oder beim Verschieben von Dateien durchzuführen.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` - verursacht, dass Hooks beim ersten fehlgeschlagenen Befehl fehlschlagen, anstatt beim letzten fehlgeschlagenen Befehl.
- `build:generate`: Wendet Patches an, validiert die Konfiguration, generiert eine ID und generiert statische Inhalte, wenn SCD für die Build-Phase aktiviert ist.
- `build:transfer` - Überträgt generierten Code und statischen Inhalt an das endgültige Ziel.

Die Befehle werden im Anwendungsverzeichnis (`/app`) ausgeführt. Sie können den `cd`-Befehl verwenden, um das Verzeichnis zu ändern. Die Hooks schlagen fehl, wenn der letzte Befehl in ihnen fehlschlägt. Um sie beim ersten fehlgeschlagenen Befehl zum Fehlschlagen zu bringen, fügen Sie `set -e` am Anfang des Hooks hinzu.

**So kompilieren Sie Sass-Dateien mit grunt**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Kompilieren Sie Sass-Dateien mithilfe von `grunt` vor der Bereitstellung statischer Inhalte, die während des Builds erfolgt. Platzieren Sie den Befehl `grunt` vor dem Befehl `build` .

{{scenarios}}
