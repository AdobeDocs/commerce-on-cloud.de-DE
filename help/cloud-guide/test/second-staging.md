---
title: Zweite Staging-Umgebung
description: Erfahren Sie mehr über die Vorteile und Einsatzmöglichkeiten einer zweiten Staging-Umgebung für parallele Tests, die Isolierung von Problemen, die Versionskontrolle und mehr.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Zweite Staging-Umgebung

In der Adobe Commerce Cloud-Infrastruktur stellt die Staging-Umgebung umfassende Tests und Validierungen in einer Umgebung sicher, die der Produktionsumgebung entspricht. In dieser Umgebung können Sie Fehler sicher identifizieren und beheben und gleichzeitig sicherstellen, dass alle neuen Funktionen oder Änderungen nahtlos integriert werden, bevor sie sich auf Ihre Live-Site auswirken.

Einige Projekte erfordern einen komplexeren Entwicklungs-Workflow. Um diese Anforderung zu erfüllen, bietet Adobe eine zusätzliche Staging-Umgebung als Add-on-Option für Ihre Cloud-Infrastruktur. Zwei Staging-Umgebungen sind für komplexe Projekte und die gleichzeitige Zusammenarbeit zwischen Teams von Vorteil.

Eine sekundäre Staging-Umgebung bietet die folgenden Vorteile:

- **Paralleles Testen**: Mit zwei Staging-Umgebungen können Teams neue Funktionen und wichtige Updates gleichzeitig testen, ohne sich gegenseitig in der Entwicklung zu behindern. Beispielsweise können Sie eine Umgebung für Funktionstests reservieren, während Sie die andere Umgebung für Leistungs- und Belastungstests reservieren.

- **Isolierung von Problemen**: Wenn Fehler oder Probleme in der primären Staging-Umgebung auftreten, können Sie diese in der sekundären Umgebung replizieren und diagnostizieren, ohne den Testfortschritt zu stoppen. Dadurch wird sichergestellt, dass laufende Tests nicht durch die Fehlerbehebung beeinträchtigt werden.

- **Versionskontrolle**: Verschiedene Versionen der Anwendung können effektiver verwaltet werden. In der primären Staging-Umgebung kann der neueste stabile Build ausgeführt werden, während die sekundären Testfunktionen aufweisen. Dadurch können Sie Versionszyklen besser verwalten und sicherstellen, dass experimentelle Änderungen stabile Builds nicht stören.

- **Erweiterte Rollout-Planung** Sie können verschiedene Bereitstellungsszenarien simulieren. Die primäre Staging-Umgebung kann die aktuelle Produktionseinrichtung nachahmen, während die sekundäre Umgebung zum Testen kommender Versionen konfiguriert werden kann.

- **User Acceptance Testing (UAT)**: Wenn Sie die primäre Umgebung für die laufende Entwicklung verwenden, können Sie der UAT eine sekundäre Umgebung zuweisen. Auf diese Weise können Benutzer oder Clients in einem kontrollierten Umfeld Tests durchführen und Feedback zu neuen Funktionen geben, ohne dass Tests betroffen sind.

- **Regressionstests**: Sie können eine Umgebung für Tests in die Zukunft und die andere für Regressionstests verwenden, um sicherzustellen, dass neue Änderungen die vorhandenen Funktionen nicht beeinträchtigen.

- **Schulung und Demonstrationen**: Verwenden Sie eine Umgebung, um neue Team-Mitglieder zu schulen oder neue Funktionen für Stakeholder zu demonstrieren. Dadurch wird sichergestellt, dass Tests durch Trainingsaktivitäten nicht unterbrochen werden.

- **Einrichtung privater Links**: Master- und Integrationsumgebungen unterstützen nicht die Einrichtung privater Links. Wenn Ihr Entwicklungs-Workflow zusätzliche Umgebungen erfordert, die über einen privaten Link für Entwicklung, Tests oder einen anderen Zweck verbunden sind, sollten Sie das Hinzufügen einer zusätzlichen Staging-Umgebung erwägen.

Mit zwei Staging-Umgebungen können Sie die Effizienz Ihres Workflows, das Risikomanagement und die Gesamtqualität Ihrer Anwendung erheblich verbessern. Diese Umgebungen bieten Sicherheit und Flexibilität, die eine Staging-Umgebung nicht bieten könnte.

>[!NOTE]
>
>Dieses Setup ist ein Add-on. Kunden, die eine sekundäre Umgebung wünschen, müssen sich an ihren Vertriebsmitarbeiter wenden, um eine solche Umgebung anzufordern. Der Vertriebsmitarbeiter kann Informationen über Preise und den Bereitstellungsprozess bereitstellen.

Wenn Ihr Projekt bereits über eine zusätzliche Staging-Umgebung verfügt oder Sie ein Upgrade Ihres aktuellen Projekts in Betracht ziehen, sollten Sie die folgenden Bereitstellungszeitpläne und Zuständigkeiten berücksichtigen:

- Der Bereitstellungsprozess kann bis zu zwei Wochen dauern, nachdem Sie die Anfrage bei Ihrem Adobe-Vertriebsmitarbeiter bestätigt haben.

- Neue Staging-Umgebungen sind nur in derselben Region verfügbar wie Ihre aktuellen Staging- und Produktionsumgebungen.

- Sie müssen entweder Ihre Integrations- oder die Master-Umgebung aktualisieren und angeben, auf welchen Ihrer Umgebungen Ihre sekundäre Staging-Umgebung basieren wird. Die sekundäre Staging-Umgebung kann nicht aus der Staging- oder Produktionsumgebung kopiert werden.

- Nachdem Sie Ihre neue Staging-Umgebung erhalten haben, können Sie sie in Ihren Entwicklungsablauf einbeziehen. Wie bei anderen Umgebungen liegt die Verantwortung bei Code- und Datenbankaktualisierungen.

## Umgebung anfordern

Gehen Sie wie folgt vor, um Ihre Anfrage zu vereinfachen:

1. Aktualisieren Sie Ihre Verzweigung.

   Aktualisieren Sie Ihre Integrations- oder Master-Verzweigung mit dem Code und der Datenbank, die Sie für die neue Umgebung verwenden möchten.

1. Wenden Sie sich an Ihren Vertriebsmitarbeiter.

   Wenden Sie sich an Ihren Adobe-Vertriebsmitarbeiter und geben Sie ihm die spezifische Verzweigung an, die Sie als Grundlage für Ihre neue Staging-Umgebung (Integration oder Master) verwenden möchten.

1. Details bestätigen.

   Warten Sie nach der Bestätigung der Details bei Ihrem Vertriebsmitarbeiter auf die Bestätigung, dass die Bereitstellungsanfrage empfangen wurde und verarbeitet wird. Wenn der Bereitstellungsprozess abgeschlossen ist, sendet Ihnen das Adobe-Team eine Bestätigung.

1. Stellen Sie in Ihrer neuen Umgebung bereit.

   Befolgen Sie die [Standardbereitstellungsschritte](../deploy/staging-production.md), um Ihren Code und Ihre Datenbank in der neuen Staging-Umgebung bereitzustellen.
