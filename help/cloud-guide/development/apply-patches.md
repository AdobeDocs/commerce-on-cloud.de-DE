---
title: Patches anwenden
description: Erfahren Sie, wie Sie Patches im Infrastrukturprojekt "Adobe Commerce on Cloud“ anwenden.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Patches anwenden

[Cloud-Patches für Commerce](https://github.com/magento/magento-cloud-patches) und das [Quality Patches Tool](https://github.com/magento/quality-patches) stellen Patches für Ihre installierte Adobe Commerce-Anwendung bereit.

- Das Paket Cloud-Patches für Commerce stellt erforderliche Patches mit wichtigen Fehlerbehebungen bereit
- Qualitäts-Patches bieten optionale Qualitätskorrekturen mit geringer Auswirkung wie [einzelne Patches](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html?lang=de#individual-patch) die keine abwärtsinkompatiblen Änderungen enthalten

Siehe [Verfügbare Patches](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=de) im _Handbuch der Commerce Operations Tools_ um eine vollständige Liste der veröffentlichten Patches zu erhalten.

Beide Pakete verbessern die Integration aller Adobe Commerce-Versionen in Cloud-Umgebungen und unterstützen die schnelle Bereitstellung wichtiger, optionaler und benutzerdefinierter Fehlerbehebungen. Sie können diese Pakete verwenden, um allgemeine Informationen über alle individuellen Patches, die für Commerce verfügbar sind, anzuwenden, wiederherzustellen und anzuzeigen.

>[!TIP]
>
>Sie können das [Quality Patches Tool](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=de) und Cloud-Patches für Commerce als eigenständige Pakete für Magento Open Source- und Adobe Commerce-Projekte verwenden. Es wird empfohlen, für Nicht-Cloud-Projekte das Quality Patches Tool zu verwenden.

Wenn Sie Änderungen in der Remote-Umgebung bereitstellen, verwendet das `ece-tools`-Paket `magento/magento-cloud-patches` und `magento/quality-patches`, um nach ausstehenden Patches zu suchen, und wendet sie automatisch in der folgenden Reihenfolge an:

1. Wenden Sie alle erforderlichen Commerce-Patches an, die im Paket Cloud-Patches für Commerce enthalten sind.
1. Wenden Sie ausgewählte optionale Commerce-Patches an, die im Quality Patches Tool enthalten sind.
1. Wenden Sie benutzerdefinierte Patches im `/m2-hotfixes`-Verzeichnis in alphabetischer Reihenfolge nach Patch-Namen an.

>[!NOTE]
>
>Wenn Sie das `ece-tools`- oder das Cloud-Patches für Commerce-Paket aktualisieren, werden die neuesten erforderlichen Patches bei der nächsten Bereitstellung Ihres Projekts angewendet. Sie können sie aber auch sofort mithilfe des `ece-patches apply` CLI-Befehls bereitstellen und Ihre Cloud-Umgebung erneut bereitstellen. Sie können [erforderliche Patches](https://github.com/magento/magento-cloud-patches/tree/develop/patches) während des Bereitstellungsprozesses nicht überspringen.

## Voraussetzungen

{{upgrade-tip}}

Das Quality Patches Tool ist eine Abhängigkeit für die Cloud-Patches für Commerce und das `ece-tools`. Um die neuesten Patches anwenden zu können, muss [die neueste Version von ECE-Tools](../dev-tools/update-package.md) installiert sein. Die erforderliche Mindestversion von ECE-Tools ist 2002.1.2.

## Anzeigen verfügbarer Patches und Status

So zeigen Sie die Liste der verfügbaren einzelnen Patches an:

```bash
php ./vendor/bin/ece-patches status
```

Beispielantwort:

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

Die Statustabelle enthält die folgenden Arten von Informationen:

- **Typ**:
   - `Optional` - Alle Patches aus dem Quality Patches Tool und dem Cloud-Patches-Paket sind für Adobe Commerce- und Magento Open Source-Installationen optional. Für Adobe Commerce in der Cloud-Infrastruktur sind alle Patches optional.
   - `Required`: Alle Patches aus dem Cloud-Paket Patches für Commerce sind für Cloud-Kunden erforderlich.
   - `Deprecated` - Der einzelne Patch wird als veraltet markiert und wir empfehlen, ihn rückgängig zu machen, wenn Sie ihn angewendet haben. Nachdem Sie einen veralteten Patch rückgängig gemacht haben, wird er nicht mehr in der Statustabelle angezeigt.
   - `Custom` - Alle Patches aus dem Verzeichnis &#39;m2-hotfixes&#39;.

- **Status**:
   - `Applied` - Das Patch wurde angewendet.
   - `Not applied` - Das Patch wurde nicht angewendet.
   - `N/A` - Der Status des Patches kann aufgrund von Konflikten nicht definiert werden.

- **Details**:
   - `Affected components` - Die Liste der betroffenen Module.
   - `Required patches` - Die Liste der erforderlichen Patches (Abhängigkeiten).
   - `Recommended replacement` - Das Patch, das als Ersatz für einen veralteten Patch empfohlen wird.

## Patch in einer lokalen Umgebung anwenden

Sie können Patches manuell in einer lokalen Umgebung anwenden und sie vor der Bereitstellung testen.

**So wenden Sie einzelne Patches in einer lokalen Entwicklungsumgebung**:

1. Fügen Sie die Variable „QUALITY_PATCH&quot; zur `.magento.env.yaml` hinzu und listen Sie die erforderlichen Patches darunter auf.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Wenden Sie die Patches im Projektstamm an.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   Der Befehl `ece-patches apply` wendet Patches in der folgenden Reihenfolge an:
   - Erforderliche Patches
   - Optionale einzelne Patches
   - Benutzerdefinierte Patches aus dem `/m2-hotfixes`

1. Löschen Sie den Cache.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Testen Sie die Patches und nehmen Sie alle erforderlichen Änderungen an den benutzerdefinierten Patches vor.

## Patch in einer Remote-Umgebung anwenden

>[!WARNING]
>
>Es wird dringend empfohlen, alle Patches in einer Integrations- oder Staging-Umgebung zu testen, bevor sie in der Produktionsumgebung bereitgestellt werden.

**So wenden Sie Patches in einer Remote-Umgebung an**:

1. Fügen Sie die Variable `QUALITY_PATCHES` zur `.magento.env.yaml` hinzu und listen Sie die erforderlichen Patches darunter auf.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >Nach dem Upgrade auf eine neue Version von Adobe Commerce müssen Sie Patches erneut anwenden, wenn die Patches nicht in der neuen Version enthalten sind.

1. Fügen Sie die aktualisierte `.magento.env.yaml`-Datei hinzu, übertragen Sie sie und übertragen Sie sie.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Anwenden eines benutzerdefinierten Patches

Bei der Bereitstellung wendet ECE-Tools alle Adobe-Patches und alle benutzerdefinierten Patches an, die Sie dem `/m2-hotfixes` im Projektstamm hinzufügen.

>[!NOTE]
>
>Alle Patch-Dateinamen müssen mit der Erweiterung `.patch` enden.

**So wenden und testen Sie einen benutzerdefinierten Patch in einer Cloud-Umgebung**:

1. Erstellen Sie im Projektstammverzeichnis ein Verzeichnis mit dem Namen `m2-hotfixes`, falls es noch nicht vorhanden ist

   ```bash
   mkdir m2-hotfixes
   ```

1. Kopieren Sie die Patch-Datei in das `/m2-hotfixes`.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Stellen Sie sicher, dass Sie alle Patches in einer Vorproduktionsumgebung testen. Für Adobe Commerce in der Cloud-Infrastruktur können Sie mit dem `magento-cloud environment:branch <branch-name>` CLI-Befehl Verzweigungen erstellen.

## Wiederherstellen eines benutzerdefinierten Patches

So setzen Sie einen zuvor angewendeten benutzerdefinierten Patch zurück oder deinstallieren ihn:

1. Löschen Sie die Patch-Datei aus dem `/m2-hotfixes`.

1. Code-Änderungen hinzufügen, übertragen und per Push übertragen.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Stellen Sie sicher, dass Sie Tests in einer Vorproduktionsumgebung durchführen. Für Adobe Commerce in der Cloud-Infrastruktur können Sie mit dem `magento-cloud environment:branch <branch-name>` CLI-Befehl Verzweigungen erstellen.

## Anwenden von Patches auf ein Nicht-Cloud-Projekt

Verwenden Sie das [Quality Patches Tool](https://github.com/magento/quality-patches) für Magento Open Source- und Adobe Commerce-Projekte.

## Wiederherstellen eines Patches in einer lokalen Umgebung

Sie können alle zuvor angewendeten Patches in einer lokalen Entwicklungsumgebung mithilfe der `ece-patches` CLI zurücksetzen.

So setzen Sie alle angewendeten Patches zurück:

```bash
php ./vendor/bin/ece-patches revert
```

Mit diesem Befehl werden alle Patches in der folgenden Reihenfolge zurückgesetzt:

- Setzt alle angewendeten benutzerdefinierten Patches aus dem Verzeichnis /m2-hotfixes zurück.
- Stellt alle angewendeten optionalen einzelnen Patches wieder her.
- Stellt alle angewendeten erforderlichen Patches wieder her.

## Protokollierung

Das Quality Patches Tool protokolliert alle Vorgänge in der `<Project_root>/var/log/patch.log`.
