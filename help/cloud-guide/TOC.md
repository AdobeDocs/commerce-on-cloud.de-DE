---
user-guide-title: Handbuch zu Commerce für Cloud
user-guide-description: Erfahren Sie, wie Sie die Adobe Commerce-Anwendung in der Cloud-Infrastruktur verwalten.
product: magento
feature: Cloud
source-git-commit: 3347ad0a5fe202cbd80d08b7289c20a1c98ed1e3
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 8%

---


# Commerce auf Cloud-Infrastruktur {#user-guide}

+ [Commerce](overview.md)
+ Architektur {#architecture}
   + [Cloud-Infrastruktur](architecture/cloud-architecture.md)
   + [Sicherheit](architecture/security.md)
   + [Technologie-Stack](architecture/tech-stack.md)
   + [Starter-Architektur](architecture/starter-architecture.md)
   + [Workflow starten](architecture/starter-develop-deploy-workflow.md)
   + [Pro Architektur](architecture/pro-architecture.md)
   + [Pro-Workflow](architecture/pro-develop-deploy-workflow.md)
   + [Skalierte Architektur](architecture/scaled-architecture.md)
   + [Automatische Skalierung](architecture/autoscaling.md)
+ [Erste Schritte](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html?lang=de)
+ Versionshinweise {#release-notes}
   + [Cloud-Tools-Suite](release-notes/cloud-tools-suite.md)
   + [ECE-Tools-Paket](release-notes/ece-tools-package.md)
   + [Cloud-Patches](release-notes/cloud-patches.md)
   + [Cloud Docker-Paket](release-notes/cloud-docker.md)
   + [Cloud-Komponenten](release-notes/cloud-components.md)
   + [Cloud-Pakete](release-notes/cloud-packages.md)
   + [Abwärtsinkompatible Änderungen](release-notes/backward-incompatible-changes.md)
   + [Archiv mit Versionshinweisen](release-notes/cloud-release-archive.md)
+ Cloud-Projekt {#project}
   + [Projektübersicht](project/overview.md)
   + [Projektstruktur](project/file-structure.md)
   + [Benutzerzugriff](project/user-access.md)
   + [Multi-Faktor-Authentifizierung](project/multi-factor-authentication.md)
   + [Aktivitäts-Stream](project/activity-stream.md)
   + [Ausgehende E-Mails](project/outgoing-emails.md)
   + [SendGrid-E-Mail-Service](project/sendgrid.md)
   + [Konsolen-Zweigstellenverwaltung](project/console-branches.md)
   + [Regionale IP-Adressen](project/regional-ip-addresses.md)
+ Entwickler-Tools {#dev-tools}
   + [Übersicht](dev-tools/overview.md)
   + Cloud-CLI {#cloud-cli}
      + [CLI-Übersicht](dev-tools/cloud-cli-overview.md)
      + [CLI-Referenz](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [Paketübersicht](dev-tools/package-overview.md)
      + [Einmaliges Upgrade auf ECE-Tools](dev-tools/install-package.md)
      + [Aktualisierung des Pakets ECE-Tools](dev-tools/update-package.md)
      + [CLI-Referenz](dev-tools/ece-tools-cli-reference.md)
      + [Fehlerreferenz](dev-tools/error-reference.md)
   + Integrationen {#integrations}
      + [Übersicht](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Statusbenachrichtigungen](integrations/health-notifications.md)
+ Entwicklung {#develop}
   + [Übersicht](development/overview.md)
   + [Authentifizierungsschlüssel](development/authentication-keys.md)
   + [CLI-Zweigstellenverwaltung](development/cli-branches.md)
   + [Sichere Verbindungen](development/secure-connections.md)
   + Bereitstellen {#deploy}
      + [Bereitstellungsprozess](deploy/process.md)
      + [Optimierung](deploy/optimization.md)
      + [Best Practices](deploy/best-practices.md)
      + [Szenariobasierte Bereitstellung](deploy/scenario-based.md)
      + [Keine Ausfallzeiten bei der Bereitstellung](deploy/reduce-downtime.md)
      + [Statische Inhaltsbereitstellung](deploy/static-content.md)
      + [Intelligente Assistenten](deploy/smart-wizards.md)
      + [Bereitstellung für Staging und Produktion](deploy/staging-production.md)
      + [Wiederherstellung nach Komponentenausfall](deploy/recover-failed-deployment.md)
   + Test {#test}
      + [Testanleitung](test/guidance.md)
      + [Protokolle](test/log-locations.md)
      + [xdebug](test/debug.md)
      + [Beispieldaten](test/sample-data.md)
      + [Staging und Produktion](test/staging-and-production.md)
      + [Zweite Staging-Umgebung](test/second-staging.md)
   + [Privater Link-Service](development/privatelink-service.md)
   + [Schutzblock](development/protective-block.md)
   + [Umgebung wiederherstellen](development/restore-environment.md)
   + Speicherung {#storage}
      + [Verwalten von Festplattenspeicher](storage/manage-disk-space.md)
      + [Abfragen der Profildatenbank](storage/profile-database-queries.md)
      + [Datenbank sichern](storage/database-dump.md)
      + [Backup-Verwaltung](storage/snapshots.md)
   + Aktualisierungen und Patches {#upgrade}
      + [Best Practices](development/best-practices.md)
      + [Commerce-Version aktualisieren](development/commerce-version.md)
      + [Patches anwenden](development/apply-patches.md)
+ Konfiguration {#configure}
   + [Übersicht](environment/overview.md)
   + Anwendung {#app}
      + [Konfigurieren der Anwendungsbereitstellung](application/configure-app-yaml.md)
      + [PHP-Einstellungen](application/php-settings.md)
      + Eigenschaften {#properties}
         + [Anwendungseigenschaften](application/properties.md)
         + [Crons](application/crons-property.md)
         + [Firewall (nur Starter)](application/firewall-property.md)
         + [Erweiterungspunkte](application/hooks-property.md)
         + [Variablen](application/variables-property.md)
         + [Web](application/web-property.md)
         + [Arbeiter](application/workers-property.md)
      + [Festlegen des Cache für statische Dateien](application/set-cache.md)
   + Umgebung {#env}
      + [Konfigurieren der Umgebungsbereitstellung](environment/configure-env-yaml.md)
      + [Variablenebenen und Optionen](environment/variable-levels.md)
      + Variablen überschreiben {#stage}
         + [Umgebungsvariablen](environment/variables-intro.md)
         + [ADMINISTRATOR](environment/variables-admin.md)
         + [Cloud-Variablen](environment/variables-cloud.md)
         + [Global](environment/variables-global.md)
         + [Build](environment/variables-build.md)
         + [Bereitstellen](environment/variables-deploy.md)
         + [Nach der Bereitstellung](environment/variables-post-deploy.md)
      + Konfigurieren von Benachrichtigungen {#log}
         + [Benachrichtigungen](environment/set-up-notifications.md)
         + [Protokoll-Handler](environment/log-handlers.md)
   + Routen {#routes}
      + [Konfigurieren von Routen](routes/routes-yaml.md)
      + [Caching](routes/caching.md)
      + [Redirects](routes/redirects.md)
      + [Server-seitige Includes](routes/server-side-includes.md)
   + Dienste {#service}
      + [Konfigurieren von Services](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Tal](services/valkey.md)
+ Fastly Services {#cdn}
   + [Übersicht](cdn/fastly.md)
   + Schnelle Einrichtung {#setup-fastly}
      + [Fastly-Services konfigurieren](cdn/fastly-configuration.md)
      + [Cache-Konfiguration anpassen](cdn/fastly-custom-cache-configuration.md)
      + [Anpassen von Fehler- und Wartungsseiten](cdn/fastly-custom-response.md)
   + [Web Application Firewall](cdn/fastly-waf-service.md)
   + [Bildoptimierung](cdn/fastly-image-optimization.md)
   + Anpassen mit VCL {#custom-vcl-snippets}
      + [Erste Schritte](cdn/fastly-vcl-custom-snippets.md)
      + [Anforderungen an ein CMS-Backend umleiten](cdn/fastly-vcl-wordpress.md)
      + [Spam für Verweise blockieren](cdn/fastly-vcl-badreferer.md)
      + [IP-Zulassungsliste](cdn/fastly-vcl-allowlist.md)
      + [IP-Blockierungsliste](cdn/fastly-vcl-blocking.md)
      + [Fastly-Cache umgehen](cdn/fastly-vcl-bypass-to-origin.md)
   + [Schnelle Fehlerbehebung](cdn/fastly-troubleshooting.md)
+ Store-Einstellungen {#configure-store}
   + [Übersicht](store/overview.md)
   + [Best Practices](store/best-practices.md)
   + [Benutzerdefiniertes Design](store/custom-theme.md)
   + [Erweiterungen](store/extensions.md)
   + [B2B-Modul](store/b2b-module.md)
   + [Mehrere Sites](store/multiple-sites.md)
   + [Sitemap und Suchmaschinenroboter](store/robots-sitemap.md)
   + [PayPal-Zahlungsmethoden](store/paypal.md)
   + [Konfigurationsverwaltung](store/store-settings.md)
+ Launch-Site {#launch}
   + [Übersicht](launch/overview.md)
   + [Checkliste starten](launch/checklist.md)
   + [Launch-Schritte](launch/steps.md)
+ Standort überwachen {#monitor}
   + [Leistung](monitor/performance.md)
   + [Betriebstelemetrie](monitor/operational-telemetry.md)
   + New Relic-Service {#new-relic}
      + [Übersicht über New Relic](monitor/new-relic-service.md)
      + [Konto- und Benutzerverwaltung](monitor/account-management.md)
      + Untersuchung der Leistung {#investigate}
         + [Richtlinien, Warnhinweise und Workflows](monitor/investigate-performance.md)
         + [Datenaufnahme](monitor/ingest-data.md)
         + [Tracking von Bereitstellungen](monitor/track-deployments.md)
      + [Protokollverwaltung](monitor/log-management.md)
