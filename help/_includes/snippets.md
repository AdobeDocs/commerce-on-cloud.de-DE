---
source-git-commit: c8effbdb82060e2a2cbcbdef577fed7249a76799
workflow-type: tm+mt
source-wordcount: '1313'
ht-degree: 0%

---
# Cloud-Snippets

## Elasticsearch-Warnung {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11 und höher wird für Adobe Commerce in Cloud-Infrastrukturen nicht unterstützt. Die Adobe Commerce-Versionen 2.3.7-p3, 2.4.3-p2 und 2.4.4 und höher unterstützen den OpenSearch-Service. Die lokalen Installationen unterstützen weiterhin Elasticsearch.

## Erweiterte Integration {#enhanced-integration-envs}

>[!NOTE]
>
>Projekte, die vor dem 5. Juni 2020 bereitgestellt wurden, wiesen mehrere kleinere Integrationsumgebungen auf. Wenn Sie eine größere Integrationsumgebung für Tests und Entwicklung benötigen, fordern Sie ein Upgrade auf die erweiterte Integrationsumgebung an. Weitere Informationen finden Sie [&#x200B; Artikel &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html?lang=de)Integrationsumgebungsanfrage“ im _Adobe Commerce_ Center.

## Zusammenführungsoptionen {#merge-options}

Standardmäßig überschreibt der Bereitstellungsprozess alle Einstellungen in der `env.php`-Datei. Sie können jedoch festlegen, dass ein oder mehrere Werte für eine Service-Konfiguration zusammengeführt werden, ohne dass alle Werte überschrieben werden.

Legen Sie die Option `_merge` auf einen der folgenden Werte fest:

- `true` - **Zusammenführen** der konfigurierten Service-Werte mit den Umgebungsvariablenwerten.
- `false` - **Überschreiben** der konfigurierten Service-Werte mit den Umgebungsvariablenwerten.

## Privates Repository {#private-repository}

>[!NOTE]
>
>Adobe empfiehlt dringend die Verwendung eines privaten Repositorys für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt, um proprietäre Informationen oder Entwicklungsarbeiten wie Erweiterungen und vertrauliche Konfigurationen zu schützen.

## Pro Self-Service-Warnung {#pro-self-service-warning}

>[!WARNING]
>
>Einige **Pro** Projekte benötigen Unterstützung vom Adobe-Support, um die Routenkonfigurationen in der `routes.yaml`-Datei und die Cron-Konfigurationen in der `.magento.app.yaml`-Datei zu aktualisieren. Adobe empfiehlt, alle YAML-Konfigurationsänderungen zuerst in einer Integrationsumgebung vorzunehmen und zu validieren und dann in der Staging-Umgebung bereitzustellen.
>
>
>Wenn Ihre Änderungen nach der erneuten Bereitstellung nicht auf den Staging-Sites angezeigt werden und keine entsprechenden Fehlermeldungen im Protokoll enthalten sind, **Sie**&#x200B;[ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket). Beschreiben Sie im Ticket klar die Konfigurationsänderungen, die Sie versucht haben, und fügen Sie alle aktualisierten YAML-Konfigurationsdateien im Ticket hinzu.

## Pro Services-Support {#pro-update-service}

>[!BEGINSHADEBOX]

- Bei Pro-Projekten müssen Sie [ein Adobe Commerce-Support-Ticket einreichen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um [Services](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html?lang=de) nur in `Staging` und `Production` zu installieren oder zu aktualisieren.

- Geben Sie an, welche Service-Änderungen erforderlich sind, schließen Sie Ihre aktualisierten `.magento.app.yaml`- und `services.yaml`-Dateien ein und geben Sie die PHP-Version im Ticket an. Informationen zu Self-Service-Änderungen an PHP-Versionen, Erweiterungen oder Umgebungseinstellungen finden Sie unter [PHP-Einstellungen](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html?lang=de) unter _Anwendungskonfiguration_.

  >[!IMPORTANT]
  >
  >Verwenden Sie bei der Auswahl des Felds Umgebung im neuen Ticketformular die Umgebungsbenennung von Adobe. Wählen Sie beispielsweise Staging aus, auch wenn Sie diese Umgebung intern **dev** aufrufen. Sie können Ihren internen Namen in der Beschreibung angeben, aber das Umgebungsfeld selbst muss die Nomenklatur von Adobe verwenden.

- Bei Änderungen an einer Live-Produktionsumgebung **nur Pro** ist eine Vorankündigung von mindestens 48 Stunden erforderlich. Dadurch hat das Cloud-Infrastruktur-Team genügend Zeit, Ressourcen zu mobilisieren und ein sicheres Upgrade durchzuführen. Die Kündigungsfrist beginnt, wenn das Infrastruktur-Team die Anfrage bestätigt und das Upgrade plant, mit Ausnahme der Wochenenden. Damit beispielsweise Service-Upgrades an einem Montag abgeschlossen werden, muss bis Mittwoch eine Bestätigung des geplanten Upgrades empfangen werden. In Spitzenzeiten kann die Verarbeitung Ihrer Anfrage länger dauern.

  >[!NOTE]
  >
  >Alle geplanten Wartungsfenster müssen im UTC-Format bereitgestellt werden, um Klarheit und Konsistenz in der gesamten Kommunikation zu gewährleisten. Service-Upgrades können nicht in der Staging-Umgebung geplant werden. In den meisten Fällen werden Upgrades in der Staging-Umgebung am selben Tag wie die Anfrage durchgeführt.
  >
  >Wenn Sie ein RabbitMQ-Upgrade anfordern, stellen Sie die Umgebung nach Abschluss des Upgrades erneut bereit, damit die Nachrichtenwarteschlangen erneut initialisiert werden.

- **Zweiteiliger Handshake-Prozess für die Planung von Upgrades**

  Um einen reibungslosen und koordinierten Upgrade-Prozess sicherzustellen, folgt der Adobe Commerce-Support einem zweiteiligen Handshake-Prozess für alle Upgrades der Produktionsumgebung:

   1. **Kundenbestätigung**: Der Adobe-Support fordert den Kunden zunächst auf, das gewünschte Datum und die gewünschte Uhrzeit für das Upgrade zu bestätigen. Mit diesem Schritt wird sichergestellt, dass das Timing den Geschäftsanforderungen und Wartungsfenstern des Kunden entspricht.
   2. **Planung und endgültige Bestätigung**: Sobald der Kunde den Zeitpunkt bestätigt, sendet der Adobe-Support die Anfrage an das Infrastruktur-Team, das dann die Anfrage prüft und die endgültige Bestätigung des geplanten Upgrade-Fensters bereitstellt.

Das Upgrade gilt erst dann als geplant, wenn das Infrastruktur-Team eine endgültige Bestätigung vorgelegt hat. Kunden wird empfohlen, mindestens 48 Stunden vor dem Upgrade-Fenster umgehend zu antworten, um Verzögerungen zu vermeiden und eine angemessene Ankündigung zu ermöglichen.

>[!ENDSHADEBOX]

## Pro Backups {#pro-backups}

>[!TIP]
>
>In Pro-Staging- und Produktionsumgebungen müssen Sie [ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket), um eine bestimmte Sicherung abzurufen, in der Datum, Uhrzeit und Zeitzone des Tickets angegeben sind.
>
>Adobe stellt **Umgebungen** einer automatischen Sicherung wieder her. Siehe [Wiederherstellen eines DB-Snapshots aus der Staging- oder &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html?lang=de)), um Hilfe bei der Auswahl einer Methode zum Wiederherstellen eines Staging- oder Produktions-Snapshots zu erhalten.

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
>[Senden eines Adobe Commerce Support-Tickets](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um die Service-Konfiguration in Pro-Produktions- und Staging-Umgebungen zu ändern.

## Service-Änderung {#service-change-tip}

>[!TIP]
>
>Nach der Ersteinrichtung des Service können Sie die Softwareversion für einen installierten Service ändern, indem Sie die `services.yaml` und `.magento.app.yaml` Konfigurationsdateien aktualisieren. Unter [Ändern der Service-Version](/help/cloud-guide/services/services-yaml.md#change-service-version) finden Sie Anleitungen zum Aktualisieren oder Herunterstufen eines Services.

## Bereitstellungstip bleibt stecken {#stuck-deployment-tip}

>[!TIP]
>
>Hilfe bei blockierten Bereitstellungen erhalten Sie mit der Fehlerbehebung bei der [Adobe Commerce-Bereitstellung](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=de) im _Commerce-Hilfezentrum_.

## Aktualisierung auf ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Wenn Sie eine Version von Adobe Commerce in der Cloud-Infrastruktur verwenden, die nicht das `ece-tools` enthält, müssen Sie ein [einmaliges Upgrade“ &#x200B;](/help/cloud-guide/dev-tools/install-package.md) Ihr Cloud-Projekt durchführen, um veraltete Pakete zu entfernen. Wenn Sie das `ece-tools` derzeit verwenden und aktualisieren müssen, finden Sie weitere Informationen unter [Aktualisieren des ECE-Tools-Pakets](/help/cloud-guide/dev-tools/update-package.md).

## Upgrade-Tipp {#upgrade-tip}

>[!TIP]
>
>Bevor Sie mit einem Upgrade oder einem Patch-Vorgang beginnen, erstellen Sie eine aktive Verzweigung aus der Integrationsumgebung und checken Sie die neue Verzweigung auf Ihrer lokalen Workstation aus. Wenn Sie dem Upgrade- oder Patch-Prozess eine Verzweigung zuweisen, vermeiden Sie Konflikte mit laufenden Arbeiten.

<!-- Fastly-related snippets begin -->

## Admin-Anmeldung {#admin-login-step}

1. [Anmelden](/help/get-started/onboarding.md#access-your-admin-panel) beim Administrator.

## Automatisieren der Bereitstellung benutzerdefinierter VCL-Snippets {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Anstatt benutzerdefinierte VCL-Snippets manuell hochzuladen, können Sie Snippets zum `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` in Ihrer Umgebung hinzufügen. Snippets in diesem Verzeichnis werden automatisch hochgeladen, wenn Sie im Commerce-Admin auf _VCL zu Fastly_ hochladen) klicken. Siehe [Automatisierte Bereitstellung benutzerdefinierter VCL-Snippets](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) im Fastly CDN-Modul für die Magento 2-Dokumentation.

<!-- Fastly-related snippets end -->
