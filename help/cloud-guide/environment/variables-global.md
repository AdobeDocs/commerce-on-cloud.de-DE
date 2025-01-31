---
title: Globale Variablen
description: Siehe die Liste der Umgebungsvariablen, die Aktionen im Bereitstellungsprozess der Adobe Commerce-Cloud-Infrastruktur steuern.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Globale Variablen

Globale Variablen steuern Aktionen in jeder Phase des [!DNL Commerce]-Bereitstellungsprozesses: Erstellen, Bereitstellen und Nachbereitstellen. Da sich globale Variablen auf jede Phase auswirken, müssen Sie sie im `global` der `.magento.env.yaml` festlegen:

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Weitere Informationen zum Anpassen des Build- und Bereitstellungsprozesses finden Sie unter:

- [Bereitstellungskonfiguration](configure-env-yaml.md)
- [Bereitstellungsprozess](../deploy/process.md)

## `ENABLE_EVENTING`

- **default**-_not set_
- **Version**—Adobe Commerce 2.4.5 und höher

Bei Festlegung auf `true` wird Cron aktiviert, um die Nachrichtenwarteschlange von Nutzern auszuführen. Adobe I/O Events für Adobe Commerce verwendet Nachrichtenwarteschlangen, um den Versand kritischer Ereignisse zu beschleunigen.

Adobe empfiehlt, auch die Variable [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) zum `deploy` der `.magento.env.yaml` hinzuzufügen, wobei `cron_run` auf `true` gesetzt ist.

Das folgende Beispiel zeigt eine vollständig konfigurierte `ENABLE_EVENTING`.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **default**-_not set_
- **Version**—Adobe Commerce 2.4.4 und höher

Wenn auf `true` gesetzt, werden Commerce-Webhooks aktiviert. Der Webhook wird über einen externen Endpunkt ausgeführt, z. B. eine App Builder-Laufzeitaktion oder ein Inventarverwaltungssystem eines Drittanbieters. Im [_Webhooks-Handbuch_](https://developer.adobe.com/commerce/extensibility/webhooks) wird diese Funktion ausführlich beschrieben.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Überschreibt die minimale Protokollierungsstufe für alle Ausgabe-Streams, ohne den Code zu ändern, was bei der Fehlerbehebung bei Bereitstellungsproblemen hilfreich ist. Wenn beispielsweise Ihre Bereitstellung fehlschlägt, können Sie diese Variable verwenden, um die Protokollierungsgranularität global zu erhöhen. Siehe [Protokollebenen](log-handlers.md#log-levels). Der `min_level` Wert in Logging Handlern überschreibt diese Einstellung.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>Die Einstellung für die Variable `MIN_LOGGING_LEVEL` ändert nichts an der Konfiguration auf Protokollebene für den Datei-Handler, der standardmäßig auf `debug` festgelegt ist.

## `SCD_ON_DEMAND`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Aktivieren der Generierung statischer Inhalte auf Anfrage eines Benutzers (SCD). Statische Inhalte bei Bedarf eignen sich ideal für den Entwicklungs- und Test-Workflow, da sie die Bereitstellungszeit verkürzen.

Vorausfüllen des Cache mithilfe des [`post_deploy` Hooks ](../application/hooks-property.md) die Ausfallzeit der Site. Der Cache-Warming ist nur für Pro-Projekte verfügbar, die Staging- und Produktionsumgebungen im [!DNL Cloud Console] enthalten, sowie für Starter-Projekte. Fügen Sie die Umgebungsvariable `SCD_ON_DEMAND` zum `global` in der `.magento.env.yaml` hinzu:

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

Die Variable `SCD_ON_DEMAND` überspringt die SCD in beiden Phasen (Erstellen und Bereitstellen), löscht die `pub/static` und `var/view_preprocessed` Ordner und schreibt Folgendes in die `app/etc/env.php`:

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.2.0 und höher

Ermöglicht die Verlängerung der maximal erwarteten Ausführungszeit für die Bereitstellung statischer Inhalte.

Standardmäßig legt Adobe Commerce die maximal erwartete Ausführung auf 900 Sekunden fest. In einigen Szenarien benötigen Sie jedoch möglicherweise mehr Zeit, um die statische Inhaltsbereitstellung für ein Cloud-Projekt abzuschließen.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.4.2 und höher

Wenn auf `true` gesetzt, wird verhindert, dass während der Build- und Bereitstellungsphase statische Inhalte für übergeordnete Designs generiert werden. Wenn diese Option auf `true` festgelegt ist, wird weniger statischer Inhalt generiert, wodurch die Erstellungs- und Bereitstellungszeiten insgesamt verbessert werden.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.3.0 und höher

[Baler](https://github.com/magento/baler) ist ein Modul, das Ihren generierten JavaScript-Code scannt und ein optimiertes JavaScript-Bundle erstellt. Durch die Bereitstellung des optimierten Bundles auf Ihrer Site kann die Anzahl der Netzwerkanfragen beim Laden Ihrer Site reduziert und die Seitenladezeiten verbessert werden.

Legen Sie diese Option auf `true` fest, um die Ballenpresse nach der Bereitstellung statischer Inhalte auszuführen.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Installieren und konfigurieren Sie das Ballenmodul, bevor Sie diese Funktion verwenden. Da Baller in der Alpha-Version vorliegt, aktivieren Sie diese Option nur in Staging-Umgebungen.

## `SKIP_HTML_MINIFICATION`

- **Standard**:
   - `true`—für `ece-tools` 2002.0.13 und höher
   - `false` - für frühere Versionen von `ece-tools`
- **Version**—Adobe Commerce 2.1.4 und höher

Aktiviert oder deaktiviert das Kopieren von statischen Ansichtsdateien in das `<magento_root>/init/` am Ende des Erstellungsvorgangs. Wenn auf `true` festgelegt, werden die Dateien nicht kopiert und eine HTML-Minimierung ist auf Anfrage verfügbar. Legen Sie diesen Wert auf `true` fest, um Ausfallzeiten bei der Bereitstellung in Staging- und Produktionsumgebungen zu reduzieren.

- **`false`** - Kopiert den `view_preprocessed` Ordner am Ende der Build-Phase in den `<magento_root>/init/` Ordner und stellt den Ordner im `<magento_root>/var` zu Beginn der Bereitstellungsphase wieder her.
- **`true`** - Aktiviert die On-Demand-HTML-Minimierung; kopiert _nicht_ die `<magento_root>var/view_preprocessed` am Ende der Erstellungsphase in das `<magento_root>/init/`.

Fügen Sie die Umgebungsvariable `SKIP_HTML_MINIFICATION` zum `global` in der `.magento.env.yaml` hinzu:

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Standard**—_Nicht festgelegt_
- **Version**—Adobe Commerce 2.1.4 und höher

Verwenden Sie die Variable `X_FRAME_CONFIGURATION` , um die Konfiguration des [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html)-Headers für Ihre Adobe Commerce-Site zu ändern. Diese Konfiguration steuert, wie der Browser eine Seite in einem `<frame>`, `<iframe>` oder `<object>` rendert. Verwenden Sie eine der folgenden Optionen:

- `DENY` - Die Seite kann nicht in einem Frame angezeigt werden.
- `SAMEORIGIN`-(Die Standardeinstellung für Adobe Commerce.) Die Seite kann nur in einem Frame angezeigt werden, der auf demselben Ursprung wie die Seite selbst liegt.

>[!WARNING]
>
>Die `ALLOW-FROM <uri>`-Option wird nicht mehr unterstützt, da sie von Adobe Commerce unterstützte Browser nicht mehr unterstützen. Siehe [Browser-Kompatibilität](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Fügen Sie die Umgebungsvariable `X_FRAME_CONFIGURATION` zum `global` in der `.magento.env.yaml` hinzu:

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
