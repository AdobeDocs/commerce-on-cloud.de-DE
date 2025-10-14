---
title: Szenariobasierte Bereitstellung
description: Erfahren Sie, wie Sie Adobe Commerce in Cloud-Infrastrukturbereitstellungen mithilfe von benutzerdefinierten Konfigurationsdateien anpassen können.
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Szenariobasierte Bereitstellung

Ab `ece-tools` 2002.1.0 können Sie die szenariobasierte Bereitstellungsfunktion verwenden, um das standardmäßige Bereitstellungsverhalten anzupassen.
Diese Funktion verwendet **Szenarien** und **Schritte** in der Konfiguration:

- **Szenariokonfiguration**-Jeder Bereitstellungs-Hook ist ein *Szenario*, bei dem es sich um eine XML-Konfigurationsdatei handelt, die die Sequenz und die Konfigurationsparameter zum Abschließen von Bereitstellungsaufgaben beschreibt. Sie konfigurieren die Szenarien im Abschnitt `hooks` der `.magento.app.yaml`.

- **Schrittkonfiguration**-Jedes Szenario verwendet eine Sequenz von *Schritten* die die Vorgänge programmgesteuert beschreiben, die zum Abschließen von Bereitstellungsaufgaben erforderlich sind. Die Schritte werden in einer XML-basierten Szenario-Konfigurationsdatei konfiguriert.

Adobe Commerce in Cloud-Infrastrukturen bietet eine Reihe [Standardszenarien](https://github.com/magento/ece-tools/tree/2002.1/scenario) und [Standardschritte](https://github.com/magento/ece-tools/tree/2002.1/src/Step) im `ece-tools`. Sie können das Bereitstellungsverhalten anpassen, indem Sie benutzerdefinierte XML-Konfigurationsdateien erstellen, um die Standardkonfiguration zu überschreiben oder anzupassen. Sie können auch Szenarien und Schritte verwenden, um Code aus benutzerdefinierten Modulen auszuführen.

## Hinzufügen von Szenarien mithilfe von Build- und Bereitstellungs-Hooks

Sie fügen die Szenarien zum Erstellen und Bereitstellen von Adobe Commerce zum Abschnitt `hooks` der `.magento.app.yaml` hinzu. Jeder Hook gibt die Szenarien an, die während jeder Phase ausgeführt werden sollen. Das folgende Beispiel zeigt die Standardszenario-Konfiguration.

> `magento.app.yaml`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Mit der Veröffentlichung von `ece-tools` 2002.1.x gibt es ein neues [Hooks-Konfiguration](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html?lang=de)-Format. Das Legacy-Format `ece-tools` Versionen 2002.0.x wird weiterhin unterstützt. Sie müssen jedoch auf das neue Format aktualisieren, um die szenariobasierte Bereitstellungsfunktion verwenden zu können.

## Schritte des Überprüfungsszenarios

In der Hook-Konfiguration ist jedes Szenario eine XML-Datei, die Schritte zum Ausführen von Build-, Bereitstellungs- oder Post-Deploy-Aufgaben enthält. Die `scenario/transfer`-Datei umfasst beispielsweise drei Schritte: `compress-static-content`, `clear-init-directory` und `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Erweitern eines Standardszenarios

Im folgenden Beispiel wird das standardmäßige Bereitstellungsszenario erweitert, indem zusätzliche Bereitstellungskonfigurationsdateien an die Hook-Konfiguration angehängt werden.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Während der Bereitstellung werden die benutzerdefinierten Szenarien anhand der folgenden Regeln mit dem Standardszenario zusammengeführt:

- Szenarien werden basierend auf ihrer Sequenz in der Hook-Definition priorisiert, wobei das letzte aufgelistete Szenario die höchste Priorität hat.

  Im Beispiel haben die Szenarien die folgende Priorität:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (Standard- oder Basisszenario)

- Die Schritte im Szenario mit der höchsten Priorität überschreiben Schritte mit demselben Namen in den anderen Szenarien. Der Konfiguration werden neue Schritte hinzugefügt. Dieselben Regeln gelten für mehr als zwei Szenarien, wobei jedes Szenario von rechts nach links priorisiert wird, z. B. (C → B → A).

### Standardschritte entfernen

Schritte werden mithilfe des `skip`-Parameters aus Standardszenarien entfernt.

Um beispielsweise die `enable-maintenance-mode`- und `set-production-mode` im Standardbereitstellungsszenario zu überspringen, erstellen Sie eine Konfigurationsdatei, die die folgende Konfiguration enthält.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Um die benutzerdefinierte Konfigurationsdatei zu verwenden, aktualisieren Sie die standardmäßige `.magento.app.yaml`.

> `.magento.app.yaml` mit benutzerdefiniertem Bereitstellungsszenario

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Standardschritte ersetzen

Benutzerdefinierte Szenarien können Standardschritte ersetzen, um eine benutzerdefinierte Implementierung bereitzustellen. Verwenden Sie dazu den Standardnamen des Schritts als Namen für den benutzerdefinierten Schritt.

Im Szenario [Standardbereitstellung“ führt ] Schritt `enable-maintenance-mode` beispielsweise das standardmäßige Skript [EnableMaintenanceMode PHP] aus.

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Um diesen Schritt zu überschreiben, erstellen Sie eine benutzerdefinierte Szenariokonfigurationsdatei, um beim Ausführen des `enable-maintenance-mode` ein anderes Skript auszuführen.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Ändern der Schrittpriorität

Benutzerdefinierte Szenarien können die Priorität von Standardschritten ändern. Der folgende Schritt ändert die Priorität des `enable-maintenance-mode` von `300` in `10`, sodass der Schritt früher im Bereitstellungsszenario ausgeführt wird.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

In diesem Beispiel wird der `enable-maintenance-mode` Schritt an den Anfang des Szenarios verschoben, da er eine niedrigere Priorität hat als alle anderen Schritte im Standardbereitstellungsszenario.

### Beispiel: Bereitstellungsszenario erweitern

Im folgenden Beispiel wird das [Standardbereitstellungsszenario] mit den folgenden Änderungen angepasst:

- Ersetzt den `remove-deploy-failed-flag` Schritt durch einen benutzerdefinierten Schritt
- Überspringt den `clean-redis-cache` Unterschritt im Schritt vor der Bereitstellung
- Überspringt den `unlock-cron-jobs` Schritt
- Überspringt den `validate-config` Schritt zum Deaktivieren kritischer Validatoren
- Fügt einen neuen Schritt vor der Bereitstellung hinzu

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Um dieses Skript in Ihrem Projekt zu verwenden, fügen Sie der `.magento.app.yaml` für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt die folgende Konfiguration hinzu:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Sie können die [Standardszenarien](https://github.com/magento/ece-tools/tree/2002.1/scenario) und [Standardschrittkonfiguration) &#x200B;](https://github.com/magento/ece-tools/tree/2002.1/src/Step) GitHub-Repository von `ece-tools` überprüfen, um zu bestimmen, welche Szenarien und Schritte für die Erstellung, Bereitstellung und Nachbereitstellung Ihres Projekts angepasst werden sollen.

## Hinzufügen eines benutzerdefinierten Moduls zum Erweitern von `ece-tools`

Das `ece-tools`-Paket stellt API-Standardschnittstellen bereit, die den Standards der Semantic-Version entsprechen. Alle API-Schnittstellen sind mit der Anmerkung {@api}**gekennzeichnet.** Sie können die standardmäßige API-Implementierung durch Ihre eigene ersetzen, indem Sie ein benutzerdefiniertes Modul erstellen und den Standard-Code nach Bedarf ändern.

Um das benutzerdefinierte Modul mit Adobe Commerce in der Cloud-Infrastruktur verwenden zu können, müssen Sie Ihr Modul in der Liste der Erweiterungen für das `ece-tools`-Paket registrieren. Der Registrierungsprozess ähnelt dem Prozess, den Sie zum Registrieren von Modulen in Adobe Commerce verwenden.

**So registrieren Sie ein Modul beim `ece-tools`-Paket**:

1. Erstellen oder erweitern Sie die `registration.php`-Datei im Stammverzeichnis Ihres Moduls.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Aktualisieren Sie den Abschnitt `autoload` für Ihre Modulkonfigurationsdatei, um die `registration.php`-Datei zum automatischen Laden von Moduldateien in `composer.json` einzuschließen.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Fügen Sie die `config/services.xml` Datei in Ihr Modul ein. Diese Konfiguration wird über `config/services.xml` aus `ece-tools` Paket zusammengeführt.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Weitere Informationen zur Injektion von Abhängigkeiten finden Sie unter [Symfony Dependency Injection](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[Standard-Bereitstellungsszenario]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode-PHP-Skript]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
