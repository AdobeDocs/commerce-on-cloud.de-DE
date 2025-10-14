---
title: PHP-Einstellungen
description: Erfahren Sie mehr über die optimalen PHP-Einstellungen für die Konfiguration von Commerce-Anwendungen in der Cloud-Infrastruktur.
feature: Cloud, Configuration, Extensions
exl-id: 83094c16-7407-41fa-ba1c-46b206aa160d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# PHP-Einstellungen

Sie können auswählen, welche [Version von PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=de) in Ihrer `.magento.app.yaml`-Datei ausgeführt werden soll:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Wenn Sie auf PHP 8.1 und höher aktualisieren, entfernen Sie JSON aus der [`runtime: extensions:`-Eigenschaft &#x200B;](properties.md#runtime) der `.magento.app.yaml`-Datei und stellen Sie erneut bereit. Die JSON-Erweiterung wird seit PHP 8.0 in der Cloud-Umgebung installiert.

## Konfigurieren von PHP

Sie können die PHP-Einstellungen für Ihre Umgebung mithilfe einer `php.ini`-Datei anpassen, die an die von Adobe Commerce gepflegte Konfiguration angehängt wird.

Fügen Sie in Ihrem Repository die `php.ini` Datei zum Stamm des Programms (dem Repository-Stamm) hinzu.

>[!TIP]
>
>Die fehlerhafte Konfiguration von PHP-Einstellungen kann Probleme verursachen, sodass nur erfahrene Administratoren diese Optionen festlegen sollten.

### PHP-Speicherlimit erhöhen

Um die PHP-Speicherbegrenzung zu erhöhen, fügen Sie der `php.ini`-Datei die folgende Einstellung hinzu:

```ini
memory_limit = 1G
```

Erhöhen Sie zum Debuggen den Wert auf 2G.

### Optimieren der realpath_cache-Konfiguration

Legen Sie die folgenden `realpath_cache` fest, um die Anwendungsleistung zu verbessern.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Diese Einstellungen ermöglichen es PHP-Prozessen, Pfade zu Dateien zwischenzuspeichern, anstatt sie bei jedem Laden der Seite zu suchen. Siehe [Leistungsoptimierung](https://www.php.net/manual/en/ini.core.php) in der PHP-Dokumentation.

>[!NOTE]
>
>Eine Liste der empfohlenen PHP-Konfigurationseinstellungen finden Sie unter [Erforderliche PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html?lang=de) im _Installationshandbuch_.

### Überprüfen der benutzerdefinierten PHP-Einstellungen

Nachdem Sie die `php.ini` Änderungen in Ihre Cloud-Umgebung übertragen haben, können Sie überprüfen, ob die benutzerdefinierte PHP-Konfiguration zu Ihrer Umgebung hinzugefügt wurde. Verwenden Sie beispielsweise SSH, um sich bei der Remote-Umgebung anzumelden, PHP-Konfigurationsinformationen anzuzeigen und nach der `register_argc_argv`-Direktive zu filtern:

```bash
php -i | grep register_argc_ar
```

Beispielausgabe:

```text
register_argc_argv => On => On
```

>[!WARNING]
>
>Wenn Sie Cloud Docker für Commerce für die lokale Entwicklung verwenden, finden Sie [Docker-Service-Container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) Informationen zur Verwendung einer benutzerdefinierten `php.ini` in einer Docker-Umgebung.

## Erweiterungen aktivieren

Sie können PHP-Erweiterungen im Abschnitt `runtime:extension` aktivieren oder deaktivieren. Außerdem werden die angegebenen Erweiterungen in den Docker-PHP-Containern verfügbar.

>[!IMPORTANT]
>
>Bevor Sie Erweiterungen aktivieren, müssen Sie wissen, dass die PHP-Version mit dem Betriebssystem kompatibel sein muss, das das Projekt hostet. Bevor Sie fortfahren können, muss Ihre Projektumgebung möglicherweise vom Infrastruktur-Team auf das Betriebssystem aktualisiert werden.

Beispiel in `.magento.app.yaml` Datei:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Verwenden Sie SSH, um sich bei einer Umgebung anzumelden und die PHP-Erweiterungen aufzulisten.

```bash
php -m
```

Einzelheiten zu einer bestimmten PHP-Erweiterung finden Sie in der [PHP Extension List](https://www.php.net/manual/en/extensions.alphabetical.php).

Die folgende Tabelle zeigt die unterstützten PHP-Erweiterungen bei der Bereitstellung von Adobe Commerce auf der Cloud-Plattform.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

Die PHP-Modulvoraussetzungen sind an die Adobe Commerce-Version gebunden. Siehe [PHP-](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html?lang=de).

### Unterstützung von Erweiterungen

Für Pro-Projekte benötigen die folgenden Erweiterungen zusätzliche Unterstützung für die Installation:

- `ioncube`
- `sourceguardian`

Um beispielsweise PHP so einzurichten, dass nur SourceGuardian-geschützte Skripte in allen Umgebungen ausgeführt werden, muss die folgende Option in der `php.ini`-Datei festgelegt werden:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Siehe [.5 der SourceGuardian-Dokumentation](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Dies ist ein Link zu einer PDF_.

[Senden Sie ein Adobe Commerce Support Ticket](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=de#submit-ticket) um Hilfe bei der Installation dieser PHP-Erweiterungen in allen Produktionsumgebungen und Pro-Staging-Umgebungen zu erhalten. Fügen Sie Ihre aktualisierte `.magento/services.yaml`-Datei, `.magento.app.yaml`-Datei mit der aktualisierten PHP-Version und alle weiteren PHP-Erweiterungen ein. Bei Änderungen an einer Live-Produktionsumgebung müssen Sie mindestens 48 Stunden im Voraus angeben. Es kann bis zu 48 Stunden dauern, bis das Cloud-Infrastruktur-Team Ihr Projekt aktualisiert.

>[!WARNING]
>
>PHP, das mit debug kompiliert wurde, wird nicht unterstützt und die Probe kann mit [!DNL XDebug] oder [!DNL XHProf] in Konflikt stehen. Deaktivieren Sie diese Erweiterungen, wenn Sie den Prüfpunkt aktivieren. Die Probe steht in Konflikt mit einigen PHP-Erweiterungen wie [!DNL Pinba] oder IonCube.

<!-- Last updated from includes: 2025-04-14 09:39:27 -->
