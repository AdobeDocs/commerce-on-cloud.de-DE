---
title: Variablenebenen und Optionen
description: Erfahren Sie mehr über die verschiedenen Variablenebenen und Optionen, die beim Anpassen Ihrer Laufzeitumgebung des Adobe Commerce in Cloud-Infrastrukturprojekts verwendet werden.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Variablenebenen

Projektvariablen gelten für alle Umgebungen innerhalb des Projekts. Umgebungsvariablen gelten für eine bestimmte Umgebung oder Verzweigung. Eine Umgebung _Variablendefinitionen_ der übergeordneten Umgebung ab (erbt).

Sie können einen geerbten Wert überschreiben, indem Sie die Variable speziell für die Umgebung definieren. Um beispielsweise Variablen für die Entwicklung festzulegen, definieren Sie die Variablenwerte in der `.magento.env.yaml` in der Integrationsumgebung. Alle Umgebungen, die aus der Integrationsumgebung verzweigen, übernehmen diese Werte. Siehe [Bereitstellungskonfiguration](configure-env-yaml.md) für Details zur Konfiguration Ihrer Umgebung mithilfe der `.magento.env.yaml`.

>[!BEGINTABS]

>[!TAB CLI]

**So legen Sie Variablen mithilfe der Cloud-CLI**:

- **Projektspezifische Variablen** - Zum Festlegen desselben Werts für _alle_ Umgebungen in Ihrem Projekt. Diese Variablen stehen zur Build- und Laufzeit in allen Umgebungen zur Verfügung.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **Umgebungsspezifische Variablen** - Zum Festlegen eines eindeutigen Werts für eine _Umgebung_. Diese Variablen sind zur Laufzeit verfügbar und werden von untergeordneten Umgebungen übernommen. Geben Sie die Umgebung in Ihrem Befehl mithilfe der Option `-e` an.

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

Nachdem Sie projektspezifische Variablen festgelegt haben, müssen Sie die Remote-Umgebung manuell neu bereitstellen, damit die Änderung wirksam wird. Pushen Sie die neuen Commits zum Trigger einer Neubereitstellung.

>[!TAB Konsole]

**So legen Sie Variablen mithilfe der[!DNL Cloud Console]** fest:

1. Klicken Sie in der _[!DNL Cloud Console]_auf das Konfigurationssymbol auf der rechten Seite der Projektnavigation.

   ![Projekt konfigurieren](../../assets/icon-configure.png){width="36"}

1. Klicken Sie zum Festlegen einer Variablen auf Projektebene unter _Projekteinstellungen_ auf **Variablen**.

   ![Projektvariablen](../../assets/ui-project-variables.png)

1. Um eine Variable auf Umgebungsebene festzulegen, wählen Sie in der Liste _Umgebungen_ eine Umgebung aus und klicken Sie auf die Registerkarte **[!UICONTROL Variables]** .

   ![Registerkarte „Umgebungsvariablen“](../../assets/ui-environment-variables.png)

1. Klicken Sie auf **[!UICONTROL Create variable]**.

1. Geben Sie einen Namen und einen Wert für die Variable an. Wählen Sie aus den Optionen:

   - Zur Laufzeit verfügbar
   - Verfügbar während der Erstellungszeit
   - JSON-Wert
   - Sensible Variable (Wert in der Konsole und in CLI-Antworten ausgeblendet)
   - Vererben (untergeordnete Umgebungen können Variablen auf Umgebungsebene erben)

1. Klicken Sie auf **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>Das Festlegen umgebungsspezifischer Variablen im [!DNL Cloud Console] stellt die Umgebung automatisch erneut bereit.

>[!ENDTABS]

## Sichtbarkeit

Sie können die Sichtbarkeit einer Variablen während des Builds oder zur Laufzeit mit dem `--visible-<build|runtime>`-Befehl einschränken. Außerdem gibt es Optionen zum Festlegen von Vererbung und Vertraulichkeit.

Verwenden Sie die folgenden Optionen, um zu verhindern, dass eine Variable angezeigt oder vererbt wird:

- `--inheritable false` - Deaktiviert die Vererbung für untergeordnete Umgebungen. Dies ist nützlich, um reine Produktionswerte für die `master` Verzweigung festzulegen und es allen anderen Umgebungen zu ermöglichen, eine Variable desselben Namens auf Projektebene zu verwenden.
- `--sensitive true` - Markiert die Variable im [!DNL Cloud Console] als _nicht_. Sie können die Variable nicht in der Benutzeroberfläche anzeigen. Sie können die Variable jedoch wie jede andere Variable aus dem Anwendungs-Container anzeigen.

Im folgenden Beispiel wird ein spezieller Fall veranschaulicht, in dem verhindert werden soll, dass eine Variable angezeigt oder vererbt wird. Sie können diese Optionen nur in der CLI angeben. Dieser Fall bezieht sich nicht auf alle verfügbaren Umgebungsvariablen.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## Überprüfen von Variablenebenen und -werten

Sie können eine Liste vorhandener Variablen mithilfe der Cloud-CLI anzeigen.

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
