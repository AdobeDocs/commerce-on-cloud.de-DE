---
title: Pro Architektur
description: Erfahren Sie mehr über die von der Pro-Architektur unterstützten Umgebungen.
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 0%

---

# Pro Architektur

Ihre Adobe Commerce on Cloud Infrastructure Pro-Architektur unterstützt mehrere Umgebungen, die Sie zum Entwickeln, Testen und Starten Ihres Stores verwenden können.

- **Master** - Stellt eine `master` Verzweigung bereit, die in Platform as a Service (PaaS)-Containern bereitgestellt wird.
- **Integration** - Bietet eine einzige `integration` Verzweigung für die Entwicklung, Sie können jedoch eine zusätzliche Verzweigung erstellen. Dies ermöglicht bis zu zwei _aktive_ Verzweigungen, die in Platform as a Service (PaaS)-Containern bereitgestellt werden.
- **Staging**: Bietet eine einzelne `staging`, die in dedizierten IaaS-Containern (Infrastructure as a Service) bereitgestellt wird.
- **Produktion** - Bietet eine einzelne `production`, die in dedizierten IaaS-Containern (Infrastructure as a Service) bereitgestellt wird.

Die folgende Tabelle fasst die Unterschiede zwischen Umgebungen zusammen:

|                                        | INTEGRATION | STAGING | PRODUKTION |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| Unterstützt die Einstellungsverwaltung im [!DNL Cloud Console] | Ja | Limited | Limited |
| Unterstützt mehrere Verzweigungen | Ja | Nein (nur Staging) | Nein (nur Produktion) |
| Verwendet YAML-Dateien für die Konfiguration | Ja | Nein | Nein |
| Läuft auf dedizierter IaaS-Hardware | Nein | Ja | Ja |
| Enthält Fastly CDN | Nein | Ja | Ja |
| Umfasst New Relic-Service | Nein | APM | APM + NRI |
| Automatische Sicherungen | Nein | Ja | Ja |

>[!NOTE]
>
>Adobe stellt das Tool Cloud Docker für Commerce zum Bereitstellen in einer lokalen Cloud Docker-Umgebung bereit, damit Sie Adobe Commerce-Projekte entwickeln und testen können. Siehe [Docker-](../dev-tools/cloud-docker.md).

## Umgebungsarchitektur

Ihr Projekt ist ein einzelnes Git-Repository mit drei Hauptumgebungszweigen: `integration`, `staging` und `production`. Das folgende Diagramm zeigt die hierarchische Beziehung von Pro-Umgebungen:

![Allgemeine Ansicht der Pro-Umgebungsarchitektur](../../assets/pro-branch-architecture.png)

### Master-Umgebung

Bei Pro-Projekten bietet die `master`-Verzweigung eine aktive PaaS-Umgebung für Ihre Produktionsumgebung. Senden Sie immer eine Kopie des Produktions-Codes an die `master`, damit Sie die Produktionsumgebung debuggen können, ohne die Services zu unterbrechen.

**Einschränkungen:**

- Erstellen **&#x200B;**&#x200B;keine Verzweigung basierend auf der `master`. Verwenden Sie die Integrationsumgebung, um aktive Verzweigungen für die Entwicklung zu erstellen.

- Verwenden Sie die `master` nicht für Entwicklungs-, UAT- oder Leistungstests

### Integrationsumgebung

Die Integrationsumgebung wird in einem Linux-Container (LXC) auf einem Raster von Servern ausgeführt, die als PaaS bezeichnet werden. Jede Umgebung enthält einen Webserver und eine Datenbank zum Testen der Site. Unter [Regionale IP-Adressen](../project/regional-ip-addresses.md) finden Sie eine Liste der IP-Adressen von AWS und Azure.

**Empfohlene Anwendungsfälle:**

Integrationsumgebungen sind auf eingeschränkte Tests und Entwicklung ausgelegt, bevor Änderungen in Staging- und Produktionsumgebungen verschoben werden. Beispielsweise können Sie die Integrationsumgebung verwenden, um die folgenden Aufgaben auszuführen:

- Sicherstellen, dass Änderungen an kontinuierlichen Integrationsprozessen (CI/CI) Cloud-kompatibel sind

- Testen Sie kritische Workflows auf wichtigen Seiten wie Startseite, Kategorie, Produktdetailseite (PDP), Checkout und Admin.

Befolgen Sie die folgenden Best Practices, um eine optimale Leistung in der Integrationsumgebung zu erzielen:

- Beschränken der Kataloggröße - Die Beispieldaten enthalten etwa 2.048 Produkte als Referenz. Reduzieren Sie Ihre Kataloggröße auf etwa 4.000-5.000 Produkte.
Um die Anzahl der Produkte im Katalog zu überprüfen, führen Sie die folgende MySQL-Abfrage aus:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Verringern der Anzahl von Kundengruppen - Zu viele Kundengruppen können die Indizierungsleistung und die Gesamtleistung beeinträchtigen.

- Verwendung auf einen oder zwei gleichzeitige Benutzer beschränken

- Deaktivieren Sie Cron-Aufträge und führen Sie sie nach Bedarf manuell aus

**Einschränkungen:**

- Fastly CDN- und New Relic-Services sind in einer Integrationsumgebung nicht verfügbar

- Die Architektur der Integrationsumgebung stimmt nicht mit der Staging- und Produktionsarchitektur überein

- Verwenden Sie die `integration`-Umgebung nicht für Entwicklungstests, Leistungstests oder Benutzerakzeptanztests (UAT)

- Verwenden Sie die `integration`-Umgebung nicht zum Testen von B2B auf Adobe Commerce-Funktionen

- Sie können die Datenbank nicht in der Integrationsumgebung von der Produktions- oder Staging-Datenbank aus wiederherstellen

{{enhanced-integration-envs}}

### Staging-Umgebung

Die Staging-Umgebung bietet eine produktionsnahe Umgebung zum Testen Ihrer Site. Diese Umgebung, die auf dedizierter IaaS-Hardware gehostet wird, umfasst alle Services wie Fastly CDN, New Relic APM und Suche.

**Empfohlene Anwendungsfälle:**

Die Umgebung entspricht der Produktionsarchitektur und ist für UAT, Staging von Inhalten und abschließende Überprüfung konzipiert, bevor Funktionen in die `production`-Umgebung verschoben werden. Beispielsweise können Sie die `staging` verwenden, um die folgenden Aufgaben auszuführen:

- Regressionstests mit Produktionsdaten

- Leistungstests mit aktiviertem Fastly-Caching

- Testen neuer Builds in der Produktionsumgebung statt eines Patches

- UAT-Tests für neue Builds

- Testen von B2B für Adobe Commerce

- Anpassen der Cron-Konfiguration und Testen von Cron-Aufträgen

Siehe [Bereitstellungs-Workflow](pro-develop-deploy-workflow.md#deployment-workflow) und [Testbereitstellung](../test/staging-and-production.md).

**Einschränkungen:**

- Verwenden Sie nach dem Start der Produktions-Site die Staging-Umgebung hauptsächlich, um Patches für produktionskritische Fehlerkorrekturen zu testen.

- Aus der `staging` Verzweigung kann keine Verzweigung erstellt werden. Stattdessen übertragen Sie Code-Änderungen von der `integration` Verzweigung auf die `staging` Verzweigung.

{{second-staging}}

### Produktionsumgebung

Die Produktionsumgebung führt Ihre öffentlichen Storefronts mit einer und mehreren Sites aus. Diese Umgebung läuft auf dedizierter IaaS-Hardware mit redundanten, hochverfügbaren Knoten für den kontinuierlichen Zugriff und den Failover-Schutz für Ihre Kunden. Die Produktionsumgebung umfasst alle Services in der Staging-Umgebung sowie den [New Relic Infrastructure (NRI)](../monitor/new-relic-service.md#new-relic-infrastructure)-Service, der automatisch eine Verbindung zu den Anwendungsdaten und Leistungsanalysen herstellt, um eine dynamische Serverüberwachung zu ermöglichen.

**Vorbehalt:**

Aus der `production` Verzweigung kann keine Verzweigung erstellt werden. Stattdessen übertragen Sie Code-Änderungen von der `staging` Verzweigung auf die `production` Verzweigung.

### Produktionstechnologie-Stack

Die Produktionsumgebung verfügt über drei virtuelle Maschinen (VMs) hinter einem Elastic Load Balancer, der von einem HAProxy pro VM verwaltet wird. Jede VM umfasst die folgenden Technologien:

- **Fastly CDN** - HTTP-Caching und CDN

- **NGINX** - Webserver mit PHP-FPM, eine Instanz mit mehreren Workern

- **GlusterFS** - Dateiserver zur Verwaltung aller statischen Dateibereitstellungen und Synchronisierung mit vier Verzeichnis-Bereitstellungen:

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis** - ein Server pro virtuellem Rechner mit nur einem aktiven Server und die beiden anderen als Replikate

- **Elasticsearch** - Suchen Sie nach Adobe Commerce in Cloud Infrastructure 2.2 bis 2.4.3-p2

- **OpenSearch** - Suchen Sie nach Adobe Commerce in Cloud-Infrastruktur 2.3.7-p3, 2.4.3-p2, 2.4.4 und höher

- **Galera** - Datenbank-Cluster mit einer MariaDB MySQL-Datenbank pro Knoten mit einer Einstellung zum automatischen Inkrementieren von drei für eindeutige IDs in jeder Datenbank

Die folgende Abbildung zeigt die in der Produktionsumgebung verwendeten Technologien:

![Produktionstechnologie-Stack](../../assets/az-stack-diagram.png)

## Redundante Hardware

Anstatt eine herkömmliche Active/Passiv-`master` oder eine Einrichtung auf Primär-/Sekundär-Ebene auszuführen, führt Adobe Commerce in der Cloud-Infrastruktur eine _redundante Architektur_ auf der alle drei Instanzen Lese- und Schreibvorgänge akzeptieren. Diese Architektur bietet keine Ausfallzeiten bei der Skalierung und bietet garantierte Transaktionsintegrität.

Aufgrund der einzigartigen, redundanten Hardware kann Adobe drei Gateway-Server bereitstellen. Die meisten externen Services ermöglichen es Ihnen, einer Zulassungsliste mehrere IP-Adressen hinzuzufügen, sodass mehr als eine feste IP-Adresse kein Problem darstellt. Die drei Gateways sind den drei Servern im Cluster Ihrer Produktionsumgebung zugeordnet und behalten statische IP-Adressen bei. Es ist vollständig redundant und auf jeder Ebene hochverfügbar:

- DNS
- Content Delivery Network (CDN)
- Elastischer Lastausgleich (ELB)
- Dreiserver-Cluster, der alle Adobe Commerce-Services umfasst, einschließlich Datenbank und Webserver

## Sicherung und Notfallwiederherstellung

Adobe Commerce in Cloud-Infrastrukturen verwenden eine Hochverfügbarkeitsarchitektur, die jedes Pro-Projekt in drei separaten AWS- oder Azure Availability Zones repliziert, wobei jede Zone über ein separates Rechenzentrum verfügt. Zusätzlich zu dieser Redundanz erhalten Pro-Staging- und Produktionsumgebungen regelmäßige Live-Backups, die für den Einsatz bei &quot;_&quot;_.

**Automatische Backups** schließen persistente Daten aus allen laufenden Diensten ein, z. B. die MySQL-Datenbank und Dateien, die auf den bereitgestellten Volumes gespeichert sind. Die Backups werden in einem verschlüsselten Elastic Block Storage (EBS) in derselben Region wie die Produktionsumgebung gespeichert. Die automatischen Backups sind nicht öffentlich zugänglich, da sie in einem separaten System gespeichert sind.

>[!NOTE]
>
>Die bereitgestellten Volumes enthalten/beziehen sich nur auf [beschreibbare Bereitstellungen](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/configure/app/properties/properties#mounts) und enthalten nicht alle Ihre `app/`. Die anderen Dateien werden durch den Build- [&#x200B; Bereitstellungsprozess erstellt/generiert](https://experienceleague.adobe.com/de/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow) und Sie müssen außerdem Ihr Git-Repository auf verbleibende Dateien überprüfen.

{{pro-backups}}

Mithilfe von CLI **Befehlen können Sie eine** manuelle Sicherung) der Datenbank für Ihre Staging- und Produktionsumgebungen erstellen. Siehe [Datenbank sichern](../storage/database-dump.md). Für `integration` Umgebungen empfiehlt Adobe, als ersten Schritt nach dem Zugriff auf Ihr Adobe Commerce on Cloud-Infrastrukturprojekt und vor der Anwendung größerer Änderungen ein Backup zu erstellen. Siehe [Backup-Verwaltung](../storage/snapshots.md).

### Wiederherstellungspunkt-Ziel

Wenden Sie sich an Ihren Adobe Customer Success Manager, um weitere Informationen zur Wiederherstellungspunkt-Zielzeit bis zur letzten Sicherung zu erhalten. Die Häufigkeit der Backups hängt vom Backup-Zeitplan Ihres Plans und der Anzahl der Änderungen ab, die in den Speicher-Service geschrieben werden sollen.

### Aufbewahrungsrichtlinie

Adobe behält automatische Sicherungen gemäß der folgenden Datenaufbewahrungsrichtlinie bei:

| Zeitraum | Aufbewahrungsrichtlinie für Backups |
| ------------------ | ----------------------- |
| Tag 1 bis 3 | Ein Backup pro Stunde |
| Tage 4 bis 7 | Ein Backup pro Tag |
| Wochen 2 bis 6 | Ein Backup pro Woche |
| Wochen 8 bis 12 | Eine Sicherung alle zwei Wochen |
| 3. bis 5. Monat | Ein Backup pro Monat |

Diese Richtlinie kann je nach Cloud-Infrastrukturplan variieren.

### Recovery Time Objective

RTO hängt von der Größe des Speichers ab. Bei großen EBS-Volumes dauert die Wiederherstellung länger. Die Wiederherstellungszeiten können je nach Größe der Datenbank variieren. Weitere Informationen erhalten Sie von Ihrem Adobe Customer Success Manager.

## Pro Cluster-Skalierung

Die Größe des Pro-Clusters und _Compute_-Konfigurationen variieren je nach ausgewähltem Cloud-Anbieter (AWS, Azure), Region und Service-Abhängigkeiten. Die Adobe-Cloud-Infrastruktur kann Pro-Cluster skalieren, um Traffic-Erwartungen und Service-Anforderungen bei sich ändernden Anforderungen zu berücksichtigen.

Die redundante Architektur ermöglicht eine Hochskalierung der Adobe-Cloud-Infrastruktur ohne Ausfallzeiten. Beim Hochskalieren rotiert jede der drei Instanzen, um die Kapazität zu aktualisieren, ohne den Site-Betrieb zu beeinträchtigen. Sie können beispielsweise zusätzliche Webserver zu einem vorhandenen Cluster hinzufügen, wenn die Einschränkung auf PHP-Ebene statt auf Datenbankebene erfolgt. Dies bietet _horizontale Skalierung_ um die vertikale Skalierung zu ergänzen, die durch zusätzliche CPUs auf Datenbankebene bereitgestellt wird. Siehe [Skalierte Architektur](scaled-architecture.md).

Wenn Sie aus einem Ereignis oder einem anderen Grund einen signifikanten Traffic-Anstieg erwarten, können Sie eine temporäre Kapazitätssteigerung anfordern. Siehe [Anfordern einer temporären &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=de) im _Commerce-Hilfezentrum_.
