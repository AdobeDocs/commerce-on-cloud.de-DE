---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# Cloud-Snippets

## Elasticsearch-Warnung {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7 und höher wird für Adobe Commerce in der Cloud-Infrastruktur nicht unterstützt. Adobe Commerce 2.4.4 und höher unterstützt den OpenSearch-Service.

## Erweiterte Integration {#enhanced-integration-envs}

>[!NOTE]
>
>Projekte, die vor dem 5. Juni 2020 bereitgestellt wurden, wiesen mehrere kleinere Integrationsumgebungen auf. Wenn Sie eine größere Integrationsumgebung für Tests und Entwicklung benötigen, fordern Sie ein Upgrade auf die erweiterte Integrationsumgebung an. Weitere Informationen finden Sie [&#x200B; Artikel &#x200B;](https://experienceleague.adobe.com/de/docs/experience-cloud-kcs/kbarticles/ka-27242)Integrationsumgebungsanfrage“ im _Adobe Commerce_ Center.

## Zusammenführungsoptionen {#merge-options}

Standardmäßig überschreibt der Bereitstellungsprozess alle Einstellungen in der `env.php`-Datei. Sie können jedoch festlegen, dass ein oder mehrere Werte für eine Service-Konfiguration zusammengeführt werden, ohne dass alle Werte überschrieben werden.

Legen Sie die Option `_merge` auf einen der folgenden Werte fest:

- `true` - **Zusammenführen** der konfigurierten Service-Werte mit den Umgebungsvariablenwerten.
- `false` - **Überschreiben** der konfigurierten Service-Werte mit den Umgebungsvariablenwerten.

## Privates Repository {#private-repository}

>[!NOTE]
>
>Adobe empfiehlt die Verwendung eines privaten Repositorys für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt, um proprietäre Informationen oder Entwicklungsarbeiten wie Erweiterungen und vertrauliche Konfigurationen zu schützen.

## Pro Self-Service-Warnung {#pro-self-service-warning}

>[!WARNING]
>
>Einige **Pro** Projekte benötigen Unterstützung vom Adobe-Support, um die Routenkonfigurationen in der `routes.yaml`-Datei und die Cron-Konfigurationen in der `.magento.app.yaml`-Datei zu aktualisieren. Adobe empfiehlt, alle YAML-Konfigurationsänderungen zuerst in einer Integrationsumgebung vorzunehmen und zu validieren und dann in der Staging-Umgebung bereitzustellen.
>
>
>Wenn Ihre Änderungen nach der erneuten Bereitstellung nicht auf den Staging-Sites angezeigt werden und im Protokoll keine entsprechenden Fehlermeldungen enthalten sind, **Sie**&#x200B;[ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/de/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Beschreiben Sie im Ticket klar die Konfigurationsänderungen, die Sie versucht haben, und fügen Sie alle aktualisierten YAML-Konfigurationsdateien im Ticket hinzu.

## Pro Backups {#pro-backups}

>[!TIP]
>
>Um eine bestimmte Sicherung in Pro-Staging- und Produktionsumgebungen abzurufen, [reichen Sie ein Adobe Commerce-Support-Ticket ein &#x200B;](https://experienceleague.adobe.com/de/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) notieren Sie Datum, Uhrzeit und Zeitzone im Ticket.
>
>Adobe stellt **Umgebungen** einer automatischen Sicherung wieder her. Siehe [Wiederherstellen eines DB-Snapshots aus der Staging- oder &#x200B;](https://experienceleague.adobe.com/de/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production)), um Hilfe bei der Auswahl einer Methode zum Wiederherstellen eines Staging- oder Produktions-Snapshots zu erhalten.

## Warnung zur erneuten Bereitstellung {#redeploy-warning}

>[!WARNING]
>
>Der Bereitstellungsprozess beginnt, wenn Sie eine Zusammenführung, einen Push oder eine Synchronisierung Ihrer Umgebung durchführen oder eine manuelle erneute Bereitstellung durchführen, während der sich die [!DNL Commerce]-Anwendung im Wartungsmodus befindet. Für eine Produktionsumgebung empfiehlt Adobe, diese Arbeiten außerhalb der Spitzenzeiten durchzuführen, um Service-Unterbrechungen zu vermeiden.

## Routenplatzhalter {#route-placeholder}

>[!NOTE]
>
>In den folgenden Beispielen für die Routenkonfiguration werden Routenvorlagen mit Platzhaltern verwendet. Der `{default}` Platzhalter stellt die für Ihre Site konfigurierte Standard-Domain dar. Wenn Ihr Projekt über mehrere Domains verfügt, verwenden Sie den `{all}` Platzhalter, um das Routing für die Standard-Domain und alle Aliase zu konfigurieren. Siehe [Konfigurieren von Routen](/help/cloud-guide/routes/routes-yaml.md).

## SCD-Timing {#scd-timing-warning}

>[!WARNING]
>
>Wenn Sie nach der Bereitstellung Probleme mit statischen Inhaltsdateien in Ihrer Anwendung haben, z. B. fehlende benutzerdefinierte Design-Dateien, erhöhen Sie die erwartete maximale Ausführungszeit auf 900 Sekunden oder höher.

## Szenariobasierte Bereitstellung {#scenarios}

>[!NOTE]
>
>Ab [!DNL ECE-Tools] 2002.1.0 können Sie die szenarienbasierte Bereitstellungsfunktion verwenden, um die Erstellungs-, Bereitstellungs- und Nachbereitstellungsprozesse für Ihr Adobe Commerce in Cloud-Infrastrukturprojekt anzupassen. Siehe [Szenariobasierte Bereitstellung](/help/cloud-guide/deploy/scenario-based.md).

## Zweite Phase {#second-staging}

>[!NOTE]
>
>Einige Projekte erfordern einen komplexeren Entwicklungs-Workflow. Um diese Anforderung zu erfüllen, bietet Adobe eine [zusätzliche Staging](/help/cloud-guide/test/second-staging.md)Umgebung) als Add-on-Option für Ihre Cloud-Infrastruktur.

## Dienstanweisung {#service-instruction}

Verwenden Sie die folgenden Anweisungen für die Einrichtung des Services in Pro Integration-Umgebungen und Starter-Umgebungen, einschließlich der `master`.

>[!NOTE]
>
>Um die Service-Konfiguration in Pro-Produktions- und Staging-Umgebungen zu ändern, [&#x200B; Sie ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/de/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Informationen zu Zeitplananforderungen und zur Kundenverfügbarkeit finden Sie unter [Pro Services-Support](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) in _Services konfigurieren_.

## Service-Änderung {#service-change-tip}

>[!TIP]
>
>Nach der Ersteinrichtung des Service können Sie die Softwareversion für einen installierten Service ändern, indem Sie die `services.yaml` und `.magento.app.yaml` Konfigurationsdateien aktualisieren. Unter [Ändern der Service-Version](/help/cloud-guide/services/services-yaml.md#change-service-version) finden Sie Anleitungen zum Aktualisieren oder Herunterstufen eines Services. Diese Self-Service-Methode gilt nicht für Pro-Staging- oder Produktionsumgebungen. Weitere Informationen finden Sie unter [Pro-Services](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) in _Services konfigurieren_.

## Bereitstellungstip bleibt stecken {#stuck-deployment-tip}

>[!TIP]
>
>Hilfe bei blockierten Bereitstellungen erhalten Sie mit der Fehlerbehebung bei der [Adobe Commerce-Bereitstellung](https://experienceleague.adobe.com/de/docs/experience-cloud-kcs/kbarticles/ka-29640) im _Commerce-Hilfezentrum_.

## Aktualisierung auf ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Um veraltete Pakete aus Versionen von Adobe Commerce in der Cloud-Infrastruktur zu entfernen, die das `ece-tools` nicht enthalten, müssen Sie ein [einmaliges Upgrade](/help/cloud-guide/dev-tools/install-package.md) für Ihr Cloud-Projekt durchführen. Wenn Sie das `ece-tools` derzeit verwenden und aktualisieren müssen, finden Sie weitere Informationen unter [Aktualisieren des ECE-Tools-Pakets](/help/cloud-guide/dev-tools/update-package.md).

## Upgrade-Tipp {#upgrade-tip}

>[!TIP]
>
>Bevor Sie mit einem Upgrade oder einem Patch-Vorgang beginnen, erstellen Sie eine aktive Verzweigung aus der Integrationsumgebung und checken Sie die neue Verzweigung auf Ihrer lokalen Workstation aus. Wenn Sie dem Upgrade- oder Patch-Prozess eine Verzweigung zuweisen, vermeiden Sie Konflikte mit laufenden Arbeiten.

## Valkey in New Relic {#valkey-newrelic}

>[!NOTE]
>
>New Relic zeigt möglicherweise auch nach der Migration nach Valkey noch Redis.
>
>Es wird erwartet, dass New Relic den Cache-Service auch nach der Migration der Umgebung zu Valkey weiterhin als Redis bezeichnet.
>
>Valkey ist eine Open-Source-Form von Redis, und einige Tools und Integrationen identifizieren den Service weiterhin mit Redis-Namen anstatt einer eindeutigen Valkey-Kennzeichnung. Dieses Verhalten weist nicht unbedingt darauf hin, dass Redis noch installiert ist.

<!-- Fastly-related snippets begin -->

## Admin-Anmeldung {#admin-login-step}

1. [Anmelden](/help/get-started/onboarding.md#access-your-admin-panel) beim Administrator.

## Automatisieren der Bereitstellung benutzerdefinierter VCL-Snippets {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Anstatt benutzerdefinierte VCL-Snippets manuell hochzuladen, können Sie Snippets zum `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` in Ihrer Umgebung hinzufügen. Snippets in diesem Verzeichnis werden automatisch hochgeladen, wenn Sie im Commerce-Admin auf _VCL zu Fastly_ hochladen) klicken. Siehe [Automatisierte Bereitstellung benutzerdefinierter VCL-Snippets](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) im Fastly CDN-Modul für die Magento 2-Dokumentation.

<!-- Fastly-related snippets end -->
