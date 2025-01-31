---
title: Best Practices für die Bereitstellung
description: Entdecken Sie Best Practices für die Bereitstellung von Adobe Commerce auf Cloud-Infrastrukturen.
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Best Practices für die Bereitstellung

Skripte erstellen und bereitstellen, die beim Zusammenführen von Code in einer Remote-Umgebung aktiviert werden. Diese Skripte verwenden Umgebungs[Konfigurationsdateien ](../environment/overview.md) Anwendungs-Code, um eine Cloud-Infrastruktur mit entsprechenden Daten und Services bereitzustellen. Außerdem werden diese Skripte zum Installieren oder Aktualisieren des Adobe Commerce-Programms, von Drittanbieterdiensten und benutzerdefinierten Erweiterungen in der Cloud-Umgebung verwendet.

Der Build- und Bereitstellungsprozess unterscheidet sich für jeden Plan geringfügig:

- **Starterpläne** - Für die Integrationsumgebung erstellt jede aktive Verzweigung eine vollständige Umgebung und stellt diese bereit, um auf sie zuzugreifen und sie zu testen. Testen Sie den Code nach der Zusammenführung mit der `staging` Verzweigung vollständig. Um Ihre Site zu starten, pushen Sie `staging` auf `master`, um sie in der Produktionsumgebung bereitzustellen. Sie haben über die [!DNL Cloud Console] und die CLI-Befehle vollen Zugriff auf alle Verzweigungen.

- **Pro-Pläne** - Für die Integrationsumgebung erstellt jede aktive Verzweigung eine vollständige Umgebung und stellt diese bereit, um auf sie zuzugreifen und sie zu testen. Führen Sie Ihren Code mit der `integration`-Verzweigung zusammen, bevor Sie ihn mit der Staging- und Produktionsumgebung zusammenführen. Sie können mit der [!DNL Cloud Console] oder mit SSH- und `magento-cloud`-CLI-Befehlen mit der Staging- und Produktionsumgebung zusammenführen.

## Verfolgen des Prozesses

Sie können Build- und Bereitstellungsaktionen in Echtzeit verfolgen, indem Sie das Terminal oder die [!DNL Cloud Console] Statusmeldungen - `in-progress`, `pending`, `success` oder `failed` - verwenden, die während des Bereitstellungsprozesses angezeigt werden. Details können in den Protokolldateien angezeigt werden. Siehe [Protokolle anzeigen](../test/log-locations.md).

Wenn Sie externe GitHub-Repositorys verwenden, wird das Vorgangslog in der GitHub-Sitzung nicht angezeigt. Sie können jedoch weiterhin der Aktivität in der -Benutzeroberfläche für das externe Repository und die [!DNL Cloud Console] folgen. Siehe [Integrationen](../integrations/overview.md).

>[!NOTE]
>
>In Integrationsumgebungen können Sie die Bereitstellungsprotokolle nicht über die [!DNL Cloud Console] anzeigen. Diese Funktion ist nur für Produktions- und Staging-Umgebungen verfügbar. Sie können jedoch Protokolle für jede Phase der Bereitstellung in jeder Umgebung mithilfe der Protokolle [Erstellen und Bereitstellen](../test/log-locations.md#build-and-deploy-logs) anzeigen. Informationen zur Fehlerbehebung finden Sie unter [Bereitstellungsfehlerreferenz](../dev-tools/error-reference.md).

Sie können &quot;[ mit New Relic&quot; aktivieren](../monitor/track-deployments.md) um Bereitstellungsereignisse zu überwachen und die Leistung zwischen Bereitstellungen zu analysieren.

## Best Practices für Builds und die Bereitstellung

Lesen Sie diese Best Practices und Überlegungen für Ihren Bereitstellungsprozess:

- **Stellen Sie sicher, dass Sie die aktuelle Version des `ece-tools` ausführen**

  Siehe [Versionshinweise für ECE-Tools](../release-notes/ece-tools-package.md).

- **Befolgen Sie den Build- und Bereitstellungsprozess**

  Stellen Sie sicher, dass Sie in jeder Umgebung den richtigen Code haben, um zu vermeiden, dass Konfigurationen beim Zusammenführen von Code zwischen Umgebungen überschrieben werden. Um beispielsweise Konfigurationsänderungen auf alle Umgebungen anzuwenden, ändern und testen Sie die Änderungen in der lokalen Umgebung, bevor Sie sie in der Remote-Integrationsumgebung bereitstellen. Stellen Sie dann die Änderungen in der Staging-Umgebung bereit und testen Sie sie, bevor Sie sie in der Produktion bereitstellen. Beim Zusammenführen von einer Umgebung in eine andere überschreibt die Bereitstellung den gesamten Code in der Umgebung, mit Ausnahme der umgebungsspezifischen Konfiguration und Einstellungen.

- **Verwenden Sie dieselben Variablen in allen Umgebungen**

  Die Werte für diese Variablen können je nach Umgebung unterschiedlich sein. Normalerweise benötigen Sie jedoch dieselben Variablen in jeder Umgebung. Siehe [Konfigurationsverwaltung für Store-](../store/store-settings.md).

- **Vertrauliche Konfigurationswerte und Daten in umgebungsspezifischen Variablen beibehalten**

  Zu diesen Werten gehören Variablen, die über die Cloud-CLI oder die [!DNL Cloud Console] angegeben oder der `env.php`-Datei hinzugefügt wurden. Siehe [Variablenebenen](../environment/variable-levels.md).

- **Stellen Sie sicher, dass der gesamte Code in der Umgebungsverzweigung verfügbar ist**

  Verweise auf Code aus anderen Verzweigungen, z. B. einer privaten Verzweigung, können Probleme während des Build- und Bereitstellungsprozesses verursachen. Wenn Sie beispielsweise auf ein Design aus einer privaten Verzweigung verweisen, ist das Design nicht zugänglich und kann nicht mit dem Anwendungs-Code erstellt werden.

- **Hinzufügen von Erweiterungen, Integrationen und Code in iterierten Verzweigungen**

  Nehmen Sie Änderungen vor und testen Sie sie lokal, pushen Sie auf `integration` und dann auf `staging` und `production`. Testen und Beheben von Problemen in jeder Umgebung, bevor die Aktualisierungen in der nächsten Umgebung zusammengeführt werden. Einige Erweiterungen und Integrationen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge aktiviert und konfiguriert werden. Das Hinzufügen und Testen in Gruppen kann den Build- und Bereitstellungsprozess erheblich vereinfachen und bei der Ermittlung auftretender Probleme helfen.

- **Überprüfen von Service-Versionen und -Beziehungen und der Möglichkeit, eine Verbindung herzustellen**

  Überprüfen Sie, welche Dienste für Ihr Programm verfügbar sind, und stellen Sie sicher, dass Sie die aktuelle, kompatible Version verwenden. Empfohlene Versionen finden Sie [Service](../services/services-yaml.md#service-relationships)Beziehungen und [](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)Systemanforderungen im _Installationshandbuch_.

- **Testen Sie lokal und in der Integrationsumgebung, bevor Sie sie in der Staging- und Produktionsumgebung bereitstellen**

  Identifizieren und beheben Sie Probleme in Ihren lokalen und Integrationsumgebungen, um längere Ausfallzeiten bei der Bereitstellung in Staging- und Produktionsumgebungen zu verhindern.

  >[!TIP]
  >
  >Es gibt [Smart Wizard](../deploy/smart-wizards.md)-Befehle, mit denen Sie überprüfen können, ob Ihre Cloud-Projektkonfiguration Best Practices für die Build- und Bereitstellungskonfiguration entspricht, einschließlich der Strategie zur Bereitstellung statischer Inhalte (SCD).

- **Nach Abschluss der Tests in lokalen Umgebungen und Integrationsumgebungen können Sie diese in der Staging-Umgebung bereitstellen und testen**

  Siehe [Staging- und Produktionstests](../test/staging-and-production.md).

- **Konfiguration der Produktionsumgebung überprüfen**

  Führen Sie vor der Bereitstellung in der Produktion die folgenden Aufgaben aus:

   - Stellen Sie sicher, dass Sie mit „SSH“ eine Verbindung zu allen drei Knoten in [ Produktionsumgebung ](../development/secure-connections.md) können.

   - Stellen Sie sicher, dass die Indexer auf _Planmäßig aktualisieren_ eingestellt sind. Siehe [Indizierungsmodi](https://developer.adobe.com/commerce/php/development/components/indexing/) im _Entwicklerhandbuch für Erweiterungen_.

   - Bereiten Sie die Umgebung vor, indem Sie alle umgebungsspezifischen Variablen im Produktions-Code aktualisieren, die Verfügbarkeit und Kompatibilität der Services überprüfen und alle anderen erforderlichen Konfigurationsänderungen vornehmen.

- **Überwachen des Bereitstellungsprozesses**

  Überprüfen Sie die Bereitstellungsstatusmeldungen und beheben Sie Probleme nach Bedarf. Überprüfen Sie die Cloud [Protokolle](../test/log-locations.md#) auf detaillierte Protokollmeldungen.

## Fünf Phasen der Integration, Erstellung und Bereitstellung

Die folgenden Phasen treten in Ihrer lokalen Entwicklungsumgebung und der Integrationsumgebung auf. Bei Pro-Plänen wird der Code in diesen Anfangsphasen nicht in den Staging- oder Produktionsumgebungen bereitgestellt.

### Phase 1: Validierung von Code und Konfiguration

Beim erstmaligen Einrichten eines Projekts ([ Cloud-Infrastrukturvorlage](https://github.com/magento/magento-cloud) stellt eine Grundlage für die Code-Dateien bereit. Dieses Code-Repository wird in Ihr Projekt als `master` geklont.

- **Für**: `master` Verzweigung ist Ihre Produktionsumgebung.
- **Für Pro** - `master` beginnt als Ursprungsverzweigung für die Integrationsumgebung.

Erstellen Sie eine Verzweigung aus `master` für benutzerdefinierten Code, Erweiterungen und Module sowie Integrationen von Drittanbietern. Es gibt eine Remote-Integrationsumgebung zum Testen Ihres Codes in der Cloud.

Wenn Sie Ihren Code vom lokalen Arbeitsbereich in das Remote-Repository pushen, wird eine Reihe von Prüfungen und Code-Validierungen abgeschlossen, bevor die Erstellung und Bereitstellung von Skripten beginnt. Der integrierte Git-Server überprüft, was Sie per Push übertragen, und nimmt Änderungen vor. Wenn Sie beispielsweise einen OpenSearch-Service hinzufügen, überprüft der integrierte Git-Server, ob die Topologie Ihres Clusters entsprechend geändert wird.

Wenn in einer Konfigurationsdatei ein Syntaxfehler vorhanden ist, lehnt der Git-Server die Push-Benachrichtigung ab. Siehe [Schutzblock](../development/protective-block.md).

Diese Phase wird auch `composer install` ausgeführt, um Abhängigkeiten abzurufen.

### Phase 2: Build

>[!NOTE]
>
>Während der Build-Phase befindet sich die Site nicht im Wartungsmodus und wenn Fehler oder Probleme auftreten, wird nicht heruntergefahren. Es wird nur der Code erstellt, der sich seit dem vorherigen Build geändert hat.

In dieser Phase werden die Codebasis erstellt und Hooks im `build` Abschnitt von `.magento.app.yaml` ausgeführt. Der standardmäßige Build-Hook ist der `php ./vendor/bin/ece-tools`-Befehl und führt Folgendes aus:

- Wendet Patches in `vendor/magento/ece-patches` und optionale projektspezifische Patches in `m2-hotfixes` an
- Erstellt den Code und die [Dependency Injection](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)-Konfiguration (d. h. das `generated/`-Verzeichnis, das `generated/code` und `generated/metapackage` enthält) mithilfe von `bin/magento setup:di:compile` neu.
- Überprüft, ob die [`app/etc/config.php`](../store/store-settings.md) Datei in der Codebasis vorhanden ist. Adobe Commerce generiert diese Datei automatisch, wenn es sie während der Build-Phase nicht erkennt und eine Liste von Modulen und Erweiterungen enthält. Wenn sie vorhanden ist, wird die Build-Phase wie gewohnt fortgesetzt, komprimiert statische Dateien mithilfe von GZIP und stellt bereit, was die Ausfallzeiten in der Bereitstellungsphase reduziert. Unter [Build-Optionen](../environment/variables-build.md) erfahren Sie, wie Sie die Dateikomprimierung anpassen oder deaktivieren.

>[!WARNING]
>
>Zu diesem Zeitpunkt wurde der Cluster nicht erstellt. Versuchen Sie daher nicht, eine Verbindung zu einer Datenbank herzustellen, und gehen Sie nicht davon aus, dass ein aktiver Daemon-Prozess vorhanden ist.

Nachdem die Anwendung erstellt wurde, wird sie auf einem **schreibgeschützten Dateisystem)**. Sie können bestimmte Bereitstellungspunkte konfigurieren, die gelesen/geschrieben werden sollen. Es ist nicht möglich, FTP auf den Server zu übertragen und Module hinzuzufügen. Stattdessen müssen Sie Code zu Ihrem lokalen Repository hinzufügen und `git push` ausführen, das die Umgebung erstellt und bereitstellt. Informationen zur Projektstruktur finden Sie unter [Lokale Projektverzeichnisstruktur](../project/file-structure.md).

### Phase 3: Vorbereiten des Slug

Das Ergebnis der Build-Phase ist ein schreibgeschütztes Dateisystem, das als &quot;_&quot; bezeichnet_. In dieser Phase wird ein Archiv erstellt und der Slug dauerhaft gespeichert. Wenn Sie das nächste Mal Code pushen, verwendet ein Service, der sich nicht geändert hat, den Slug aus dem Archiv.

- Beschleunigt den Aufbau der kontinuierlichen Integration durch die Wiederverwendung von unverändertem Code
- Wenn sich der Code ändert, aktualisiert den Slug für den nächsten Build, um ihn wiederzuverwenden
- Ermöglicht bei Bedarf das sofortige Zurücksetzen einer Bereitstellung
- Enthält statische Dateien, wenn die `app/etc/config.php` in der Codebasis vorhanden ist

Der Slug umfasst alle Dateien und Ordner **mit Ausnahme der folgenden** Bereitstellungen, die in `magento.app.yaml` konfiguriert sind:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Phase 4: Bereitstellen von Slugs und Clustern

Ihre Anwendungen und alle [Backend](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)Services stellen Folgendes bereit:

- mountet jeden Dienst in einem Container, z. B. Webserver, OpenSearch, [!DNL RabbitMQ]
- Bereitstellung des Lese-/Schreib-Dateisystems (bereitgestellt in einem hochverfügbaren verteilten Speicherraster)
- Konfiguriert das Netzwerk so, dass Dienste einander (und nur einander) „sehen“ können

>[!NOTE]
>
>Nehmen Sie Ihre Änderungen in einer Git-Verzweigung vor, nachdem der Build und die Bereitstellung abgeschlossen sind, und übertragen Sie sie erneut. Alle Umgebungsdateisysteme sind _schreibgeschützt_. Ein schreibgeschütztes System garantiert deterministische Bereitstellungen und verbessert die Sicherheit Ihrer Site erheblich, da kein Prozess in das Dateisystem schreiben kann. Außerdem wird sichergestellt, dass der Code in der Integrations-, Staging- und Produktionsumgebung identisch ist.

### Phase 5: Bereitstellungs-Hooks

>[!NOTE]
>
>In dieser Phase befindet sich die [!DNL Commerce] Anwendung im Wartungsmodus, bis die Bereitstellung abgeschlossen ist.

Im letzten Schritt wird ein Bereitstellungsskript ausgeführt, mit dem Sie Daten in Entwicklungsumgebungen anonymisieren, Caches löschen und externe Tools für die kontinuierliche Integration abfragen können. Wenn dieses Skript ausgeführt wird, haben Sie Zugriff auf alle Services in Ihrer Umgebung, z. B. Redis.

Wenn die `app/etc/config.php`-Datei nicht in der Codebasis vorhanden ist, werden statische Dateien mithilfe von `gzip` komprimiert und während dieser Phase bereitgestellt, wodurch die Bereitstellungsphase und die Site-Wartung verlängert werden.

>[!NOTE]
>
>Unter [Variablen bereitstellen](../environment/variables-deploy.md) erfahren Sie mehr über das Anpassen oder Deaktivieren der Dateikomprimierung.

Es gibt zwei Bereitstellungs-Hooks. Der `pre-deploy.php`-Hook schließt die erforderliche Bereinigung und das Abrufen von Ressourcen und Code ab, die im Build-Hook generiert wurden. Der `php ./vendor/bin/ece-tools deploy`-Hook führt eine Reihe von Befehlen und Skripten aus:

- Wenn Adobe Commerce **nicht installiert** wird mit `bin/magento setup:install` installiert, aktualisiert die Bereitstellungskonfiguration, die `app/etc/env.php` und die Datenbank für Ihre angegebene Umgebung, z. B. Redis und Website-URLs. **Wichtig:** Nach Abschluss der [Erstbereitstellung](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html) während des Setups wurde Adobe Commerce in allen Umgebungen installiert und bereitgestellt.

- Wenn Adobe Commerce **installiert ist** führen Sie alle erforderlichen Upgrades durch. Das Bereitstellungsskript wird `bin/magento setup:upgrade` ausgeführt, um das Datenbankschema und die Daten zu aktualisieren (was nach der Aktualisierung der Erweiterung oder des Kern-Codes erforderlich ist) und auch die Bereitstellungskonfiguration, die `app/etc/env.php` und die Datenbank für Ihre Umgebung zu aktualisieren. Schließlich löscht das Bereitstellungsskript den Adobe Commerce-Cache.

- Das Skript generiert optional statischen Webinhalt mithilfe der `magento setup:static-content:deploy`.

- Verwendet Bereiche (`-s` Flag in Build-Skripten) mit der Standardeinstellung `quick` für die Strategie zur Bereitstellung statischer Inhalte. Sie können die Strategie mithilfe der Umgebungsvariablen [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy) anpassen. Weitere Informationen zu diesen Optionen und Funktionen finden Sie unter [Strategien zur Bereitstellung statischer Dateien](../deploy/static-content.md) und `-s`-Flag für [Statische Ansichtsdateien bereitstellen](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>Das Bereitstellungsskript verwendet die Werte, die von den Konfigurationsdateien im Verzeichnis `.magento` definiert werden. Anschließend löscht das Skript das Verzeichnis und dessen Inhalte. Ihre lokale Entwicklungsumgebung ist davon nicht betroffen.

### Nach der Bereitstellung: Routing konfigurieren

Während der Bereitstellung stoppt der Prozess eingehenden Traffic am Einstiegspunkt für 60 Sekunden und konfiguriert das Routing neu, sodass Ihr Web-Traffic in Ihrem neu erstellten Cluster ankommt.

Bei erfolgreicher Bereitstellung wird der Wartungsmodus entfernt, um einen normalen Zugriff zu ermöglichen, und es werden Backup-Dateien (BAK) für die `app/etc/env.php`- und `app/etc/config.php`-Konfigurationsdateien erstellt.

Aktivieren Sie die statische Inhaltserstellung mit der `SCD_ON_DEMAND`-Variablen und konfigurieren Sie den [`post_deploy`-Hook](../application/hooks-property.md) sodass der Cache gelöscht und der Cache vorgeladen (erwärmt) wird, _nachdem_ Container beginnt, Verbindungen zu akzeptieren und (_während_ normalen eingehenden Traffics.

Informationen zum Überprüfen von Build- und Bereitstellungsprotokollen finden Sie unter [Protokolle anzeigen](../test/log-locations.md#view-and-manage-logs).
