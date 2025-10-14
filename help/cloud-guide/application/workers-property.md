---
title: Arbeiter
description: Erfahren Sie, wie Sie die Worker-Eigenschaft in der Konfigurationsdatei  [!DNL Commerce] .application konfigurieren.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Workers-Eigenschaft

Sie können einen Worker definieren, der unabhängig von der Web-Instanz ausgeführt werden soll, ohne dass eine Nginx-Instanz ausgeführt wird. Der Worker verwendet jedoch denselben Netzwerkspeicher, der von der [!DNL Commerce]-Anwendung verwendet wird. Es ist nicht erforderlich, einen Webserver auf der Worker-Instanz einzurichten (mithilfe von Node.js oder Go), da der Router keine öffentlichen Anfragen an den Worker richten kann. Dadurch ist die Worker-Instanz ideal für Hintergrundaufgaben oder ständig ausgeführte Aufgaben, die eine Bereitstellung möglicherweise blockieren.

## Konfigurieren eines Sekundärs

Worker können nur mit Pro-Staging- und Produktionsumgebungen verwendet werden. Pro-Integrations- und Starter-Umgebungen können die Variable [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) verwenden.

Um einen Worker in Pro-Staging oder Produktion zu konfigurieren, [&#x200B; Sie ein Adobe Commerce-Support-Ticket &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) und geben Sie die folgenden Informationen ein:

- Projekt-ID
- Umgebungs-ID
- Worker-Name
- Startbefehle

Sie können einen Prozess pro Worker konfigurieren. Eine einfache, allgemeine Worker-Konfiguration in der `.magento.app.yaml` könnte wie folgt aussehen:

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

In diesem Beispiel wird ein einzelner Worker mit dem Namen `queue` mit einer kleinen Ressourcenzuordnungsebene (Größe S) definiert und der Befehl `php ./bin/magento` beim Start ausgeführt. Die Worker-`queue` wird dann auf jedem Knoten als Worker-Prozess ausgeführt. Wenn der Befehl beendet wird, wird er automatisch neu gestartet.

## Befehle und Überschreibungen

Die `commands.start` ist erforderlich, um Befehle mit dem Worker-Programm zu starten. Sie können jeden gültigen Shell-Befehl verwenden, obwohl es ideal ist, die Sprache Ihrer Anwendung zu verwenden. Wenn der durch die `start` angegebene Befehl beendet wird, wird er automatisch neu gestartet.

>[!IMPORTANT]
>
>Die `deploy`- und `post_deploy`-Hooks und `crons` werden nur im Web-Container ausgeführt, nicht in Worker-Instanzen.

### Vererbung

Definitionen für die Eigenschaften `size`, `relationships`, `access`, `disk` und `mount` sowie `variables` werden von einem Worker geerbt, es sei denn, sie werden explizit überschrieben.

Die folgenden Eigenschaften werden am häufigsten verwendet, um Einstellungen [&#x200B; oberster Ebene zu &#x200B;](properties.md):

- `size`: Weniger Ressourcen für einen einzelnen Hintergrundprozess zuweisen
- `variables` - Weist die Anwendung an, anders auszuführen

### Timing und Warteschlangen

Obwohl jeder Worker hintereinander in die Warteschlange gestellt wird, erzeugt die folgende Konfiguration eine konsistente Zwei-Sekunden-Trennung der Zeitstempel in der `var/time.txt`-Datei, unabhängig vom Acht-Sekunden-Ruhezustand im PHP-Code:

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
