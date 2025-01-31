---
title: Umgebungsvariablen
description: Anzeigen einer Liste der für Adobe Commerce spezifischen Umgebungsvariablen in der Cloud-Infrastruktur.
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Umgebungsvariablen

Mit Adobe Commerce in der Cloud-Infrastruktur können Sie Umgebungsvariablen zuweisen, um Konfigurationsoptionen zu überschreiben. Das `ece-tools` legt Werte in der `env.php` anhand von Werten aus [Cloud-Variablen](variables-cloud.md), in der [!DNL Cloud Console] festgelegten Variablen und der `.magento.env.yaml`-Konfigurationsdatei fest.

Die Umgebungsvariablen in der `.magento.env.yaml`-Datei passen die Cloud-Umgebung an, indem sie Ihre bestehende Commerce-Konfiguration überschreiben. Wenn ein Standardwert `Not Set` ist, ergreift das `ece-tools`-Paket die Aktion **NEIN** und verwendet den [!DNL Commerce] Standardwert oder den Wert aus der `MAGENTO_CLOUD_RELATIONSHIPS`. Wenn der Standardwert festgelegt ist, setzt das `ece-tools`-Paket diesen Standardwert.

Zu den Arten von Umgebungsvariablen gehören:

- [ADMIN](variables-admin.md) - Variablen überschreiben Projekt-ADMIN-Variablen
- [MAGENTO_CLOUD](variables-cloud.md) - Cloud-Infrastrukturspezifische Variablen
- In der `.magento.env.yaml` verwendete Variablen:
   - [Global](variables-global.md) - Variablen wirken sich auf die Phasen der Erstellung, Bereitstellung und Nachbereitstellung aus
   - [Build](variables-build.md) - Variablen steuern Buildaktionen
   - [Bereitstellen](variables-deploy.md) - Variablen steuern Bereitstellungsaktionen
   - [Nach der Bereitstellung](variables-post-deploy.md) - Variablen steuern Aktionen nach der Bereitstellung

Variablen sind _hierarchisch_ was bedeutet, dass eine Variable von der übergeordneten Umgebung übernommen wird, wenn sie nicht überschrieben wird.

Sie können [ADMIN-Variablen](variables-admin.md) über die [!DNL Cloud Console] oder die Adobe Commerce-CLI festlegen. Sie können andere Umgebungsvariablen aus der [`.magento.env.yaml`](configure-env-yaml.md)-Datei verwalten, um Build- und Bereitstellungsaktionen in allen Ihren Umgebungen zu verwalten, einschließlich Pro-Staging und Produktion, ohne dass ein Support-Ticket erforderlich ist.

>[!TIP]
>
>Bei YAML-Dateien wird zwischen Groß- und Kleinschreibung unterschieden und es werden keine Registerkarten zugelassen. Achten Sie darauf, in der gesamten `.magento.env.yaml`-Datei konsistente Einzüge zu verwenden. Andernfalls funktioniert Ihre Konfiguration möglicherweise nicht wie erwartet. Die Beispiele in dieser Dokumentation und in der Beispieldatei verwenden _Einzug mit_ Leerzeichen. Verwenden Sie den Befehl [ece-tools validate](configure-env-yaml.md#validate-configuration-file), um Ihre Konfiguration zu überprüfen.
