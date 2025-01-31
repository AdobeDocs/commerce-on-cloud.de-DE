---
title: Erste Schritte mit benutzerdefinierten VCL-Snippets
description: Erfahren Sie mehr über die Verwendung von Sprachcodeausschnitten in Varnish Control, um die Fastly-Service-Konfiguration für Adobe Commerce anzupassen.
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1947'
ht-degree: 0%

---

# Erste Schritte mit benutzerdefinierter VCL

Fastly unterstützt eine angepasste Version der Varnish Configuration Language (VCL), um die Fastly-Service-Konfiguration an Ihre Anforderungen anzupassen.

Benutzerdefinierte VCL-Snippets sind Blöcke von VCL-Logik, die zur aktiven VCL-Version hinzugefügt werden, die auf Ihre Adobe Commerce-Site hochgeladen wird. Ein benutzerdefiniertes VCL-Fragment ändert, wie Fastly-Caching-Services auf Anfrage-Traffic reagieren. Sie können beispielsweise ein benutzerdefiniertes VCL-Fragment hinzufügen, um Anfrage-Traffic nur von angegebenen Client-IP-Adressen zuzulassen. Oder erstellen Sie einen Code-Ausschnitt, um den Traffic von Websites zu blockieren, die als Spam für Verweise an Ihre Adobe Commerce-Sites bekannt sind.

Benutzerdefinierte VCL-Snippets - generiert, kompiliert und an alle Fastly-Caches übertragen - werden geladen und aktiviert, ohne dass der Server ausfällt.

>[!NOTE]
>
>Bevor Sie benutzerdefinierten VCL-Code, Edge-Wörterbücher und ACLs zu Ihrer Fastly-Modulkonfiguration hinzufügen, überprüfen Sie, ob der Fastly-Caching-Service mit der Standardkonfiguration funktioniert. Siehe [Fastly-Services konfigurieren](fastly-configuration.md).

Fastly unterstützt zwei Arten von benutzerdefinierten VCL-Snippets:

- [Reguläre Snippets](https://docs.fastly.com/en/guides/about-vcl-snippets) - Benutzerdefinierte reguläre VCL-Snippets werden für bestimmte VCL-Versionen codiert. Sie können reguläre VCL-Ausschnitte über die Admin- oder die Fastly-API erstellen, ändern und bereitstellen.

- [Dynamische Snippets](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) - VCL-Snippets, die mit der Fastly-API erstellt wurden. Sie können dynamische Snippets ändern und bereitstellen, ohne die Fastly VCL-Version für Ihren Dienst aktualisieren zu müssen.

Es wird empfohlen, benutzerdefinierte VCL-Snippets mit Edge Dictionaries und Zugriffssteuerungslisten (ACL) zu verwenden, um die in Ihrem benutzerdefinierten Code verwendeten Daten zu speichern.

- [**Edge-Wörterbuch**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) - Speichert Daten als Schlüssel-Wert-Paare in einem Wörterbuch-Container, der von benutzerdefinierten VCL-Ausschnitten referenziert werden kann

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) - Speichert die Client-IP-Adressdaten, die die Zugriffssteuerungsliste für Block- oder Zulassungsregeln definieren, die mit benutzerdefinierten VCL-Ausschnitten implementiert wurden

Das Wörterbuch und die ACL-Daten werden auf den Fastly Edge-Knoten bereitgestellt, auf die über Netzwerkregionen zugegriffen werden kann. Außerdem können die Daten dynamisch über das Netzwerk aktualisiert werden, ohne dass Sie den VCL-Code für Ihre Staging- oder Produktionsumgebung erneut bereitstellen müssen.

>[!NOTE]
>
>Sie können benutzerdefinierte VCL-Snippets nur dann zu einer Staging- oder Produktionsumgebung hinzufügen, wenn Sie [Fastly-Services](fastly-configuration.md) für diese Umgebung konfiguriert haben.

## Tutorial

In diesem Tutorial und den Beispielen wird die Verwendung regulärer benutzerdefinierter VCL-Snippets mit Edge-Wörterbüchern und Edge-ACLs veranschaulicht, um die Fastly-Service-Konfiguration für Adobe Commerce anzupassen. Weitere Informationen finden Sie in der Fastly-Dokumentation:

- [Handbuch zu Fastly VCL](https://docs.fastly.com/guides/vcl/guide-to-vcl) - Informationen über die Implementierung von Fastly Varnish, Erweiterungen von Fastly VCL und Ressourcen, um mehr über Varnish und VCL zu erfahren.
- [Fastly VCL-Referenz](https://docs.fastly.com/guides/vcl/) - Detaillierte Programmierreferenz zur Entwicklung und Fehlerbehebung von Fastly benutzerdefinierten VCL- und benutzerdefinierten VCL-Snippets.

Sie können benutzerdefinierte VCL-Snippets über Adobe Commerce Admin oder die Fastly-API erstellen und verwalten:

- [Adobe Commerce Admin](#manage-custom-vcl-from-admin) - Es wird empfohlen, den Adobe Commerce Admin zum Verwalten benutzerdefinierter VCL-Snippets zu verwenden, da dies den Prozess zum Validieren, Hochladen und Anwenden der VCL-Änderungen auf die Fastly-Service-Konfiguration automatisiert. Außerdem können Sie die benutzerdefinierten VCL-Snippets, die zur Fastly-Service-Konfiguration hinzugefügt wurden, über den Admin anzeigen und bearbeiten.

- [Fastly API](#manage-vcl-using-the-api) - Wenn Sie nicht auf den Admin zugreifen können, verwenden Sie die Fastly-API, um benutzerdefinierte VCL-Snippets zu verwalten. Verwenden Sie beispielsweise die API zur Fehlerbehebung bei der Fastly-Service-Konfiguration, wenn die Site nicht verfügbar ist, oder um ein benutzerdefiniertes VCL-Snippet hinzuzufügen. Außerdem können einige Vorgänge nur mithilfe der API abgeschlossen werden. Beispielsweise müssen Sie die API verwenden, um eine ältere VCL-Version zu reaktivieren oder alle in einer angegebenen VCL-Version enthaltenen VCL-Snippets anzuzeigen. Siehe [API-Schnellreferenz für VCL-Snippets](#api-quick-reference-for-vcl-snippets).

### Beispiel für VCL-Code-Ausschnitt

Das folgende Beispiel zeigt das benutzerdefinierte VCL-Snippet (JSON-Format), das Traffic nach Client-IP-Adresse filtert:

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>In diesem Beispiel wird der VCL-Code als JSON-Payload formatiert, die in einer Datei gespeichert und in einer Fastly-API-Anfrage gesendet werden kann. Um JSON-Validierungsfehler beim Senden des Ausschnitts als JSON für eine API-Anfrage zu vermeiden, verwenden Sie einen Backslash, um Sonderzeichen im Code zu umgehen. Siehe [Verwenden dynamischer VCL-Snippets](https://docs.fastly.com/vcl/vcl-snippets/) in der Fastly-VCL-Dokumentation. Wenn Sie das VCL-Snippet vom Administrator senden, müssen Sie keine Sonderzeichen auslassen.

Die VCL-Logik im `content` führt die folgenden Aktionen aus:

- Überprüft bei jeder Anfrage `client.ip` eingehende IP-Adresse.

- Blockiert alle Anfragen mit einer IP-Adresse, die in der *ACLNAME*-Edge-ACL enthalten sind, und gibt einen `403 Forbidden` Fehler zurück

Die folgende Tabelle enthält Details zu Schlüsseldaten für benutzerdefinierte VCL-Snippets. Eine detailliertere Referenz finden Sie unter der Referenz [VCL-Snippets](https://docs.fastly.com/api/config#api-section-snippet) in der Fastly-Dokumentation.

| Wert | Beschreibung |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | Der API-Schlüssel für den Zugriff auf Ihr Fastly-Konto. Siehe [Abrufen von Anmeldeinformationen](fastly-configuration.md). |
| `active` | Aktiver Status des Snippets oder der Version. Gibt `true` oder `false` zurück. Wenn „true“, wird der Ausschnitt oder die Version verwendet. Klonen Sie einen aktiven Code-Ausschnitt mithilfe der Versionsnummer. |
| `content` | Der auszuführende VCL-Code-Ausschnitt. Fastly unterstützt nicht alle VCL-Sprachfunktionen. Außerdem bietet Fastly Erweiterungen mit benutzerdefinierten Funktionen. Weitere Informationen zu unterstützten Funktionen finden Sie in der [Fastly VCL-Programmierreferenz](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | Dynamischer Status eines Snippets. Gibt `false` für [reguläre Snippets](https://docs.fastly.com/en/guides/about-vcl-snippets) zurück, die in der versionierten VCL für die Fastly-Service-Konfiguration enthalten sind. Gibt `true` für ein [dynamisches Snippet“ zurück](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) das geändert und bereitgestellt werden kann, ohne dass eine neue VCL-Version erforderlich ist. |
| `number` | VCL-Versionsnummer, in der der Ausschnitt enthalten ist. Fastly verwendet *bearbeitbare Version #* in ihren Beispielwerten. Wenn Sie benutzerdefinierte Snippets aus der API hinzufügen, schließen Sie die Versionsnummer in die API-Anfrage ein. Wenn Sie benutzerdefinierte VCL von der Admin-Instanz hinzufügen, wird die Version für Sie bereitgestellt. |
| `priority` | Ein numerischer Wert von `1` bis `100`, der angibt, wann der benutzerdefinierte VCL-Code ausgeführt wird. Ausschnitte mit niedrigeren Prioritätswerten werden zuerst ausgeführt. Wenn kein Wert angegeben ist, wird der `priority` standardmäßig auf `100` gesetzt.<p>Jedes benutzerdefinierte VCL-Fragment mit einem Prioritätswert von `5` wird sofort ausgeführt, was am besten für VCL-Code geeignet ist, der das Anforderungsrouting (Block und Zulassungslisten und Umleitungen) implementiert. Die Priorität `100` eignet sich am besten zum Überschreiben des standardmäßigen VCL-Code-Ausschnitts.<p>Alle [standardmäßigen VCL-Snippets](fastly-configuration.md#upload-vcl-snippets) die im Magento-Fastly-Modul enthalten sind, haben `priority=50`.<ul><li>Weisen Sie eine hohe Priorität wie `100` zu, um benutzerdefinierten VCL-Code nach allen anderen VCL-Funktionen auszuführen, und überschreiben Sie den standardmäßigen VCL-Code.</li></ul> |
| `service_id` | Die Fastly Service-ID für eine bestimmte Staging- oder Produktionsumgebung. Diese ID wird zugewiesen, wenn Ihr Projekt zum Adobe Commerce in der Cloud-Infrastruktur hinzugefügt wird [Fastly Service-Konto](fastly.md#fastly-service-account-and-credentials). |
| `type` | Gibt den Ort an, an dem der generierte Ausschnitt eingefügt werden soll, z. B. `init` (über den Unterprogrammen) und `recv` (innerhalb der Unterprogramme). Weitere Informationen finden Sie in der Fastly [VCL-Snippets](https://docs.fastly.com/api/config#api-section-snippet)-Referenz. |

## Benutzerdefinierte VCL von Admin aus verwalten

Sie können [benutzerdefinierte VCL-Snippets hinzufügen](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) im Abschnitt *Fastly-Konfiguration* > *Benutzerdefinierte VCL-Snippets* in Admin hinzufügen.

![Verwalten benutzerdefinierter VCL-Snippets](../../assets/cdn/fastly-edit-snippets.png)

Die *Benutzerdefinierte VCL-*&quot; zeigt nur Ausschnitte an, die über die Admin hinzugefügt wurden. Wenn Snippets mit der Fastly-API hinzugefügt werden, verwenden Sie die API, um [sie zu verwalten](#manage-vcl-using-the-api).

Die folgenden Beispiele zeigen, wie Sie benutzerdefinierte VCL-Snippets vom Administrator erstellen und verwalten und Fastly Edge-Module und Edge-Wörterbücher verwenden:

- [Anforderungen an ein CMS-Backend umleiten](fastly-vcl-wordpress.md)
- [Spam für Verweise blockieren](fastly-vcl-badreferer.md)
- [Spam für Verweise blockieren](fastly-vcl-badreferer.md)
- [Benutzerdefinierte VCL für IP-Zulassungsliste](fastly-vcl-allowlist.md)
- [Benutzerdefinierte VCL für IP-Blockierungsliste](fastly-vcl-blocking.md)
- [Fastly-Cache umgehen](fastly-vcl-bypass-to-origin.md)

## Verwalten von VCL mithilfe der API

Die folgende Anleitung zeigt Ihnen, wie Sie mit der Fastly-API reguläre VCL-Snippet-Dateien erstellen und zu Ihrer Fastly-Service-Konfiguration hinzufügen können. Sie können die Snippets über das Programm *Terminal* erstellen und verwalten. Sie benötigen keine SSH-Verbindung zu einer bestimmten Umgebung.

**Voraussetzungen:**

- Konfigurieren Sie Ihre Adobe Commerce in der Cloud-Infrastrukturumgebung für Fastly Services. Siehe [Schnelles Setup](fastly-configuration.md).

- [Abrufen von Fastly API-Anmeldeinformationen](fastly-configuration.md) zum Authentifizieren von Anfragen an die Fastly-API. Stellen Sie sicher, dass Sie die Anmeldeinformationen für die richtige Umgebung erhalten: Staging oder Produktion.

- Speichern Sie die Fastly-Service-Anmeldeinformationen als Bash-Umgebungsvariablen, die Sie in cURL-Befehlen verwenden können:

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  Die exportierten Umgebungsvariablen sind nur in der aktuellen Bash-Sitzung verfügbar und gehen beim Schließen des Terminals verloren. Sie können Variablen neu definieren, indem Sie einen neuen Wert exportieren. So zeigen Sie die Liste der exportierten Variablen im Zusammenhang mit Fastly an:

  ```bash
  export | grep FASTLY
  ```

## VCL-Snippets hinzufügen

In diesem Tutorial werden die grundlegenden Schritte zum Hinzufügen benutzerdefinierter Snippets mithilfe der Fastly-API beschrieben.

>[!NOTE]
>
>Informationen zum Verwalten benutzerdefinierter VCL-Snippets über den Adobe Commerce-Administrator finden Sie unter [Verwalten von VCL über den Adobe Commerce-Administrator](#manage-custom-vcl-from-admin).


**Voraussetzungen**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### Schritt 1: Suchen der aktiven VCL-Version

Verwenden Sie den Fastly-API[Vorgang „Version abrufen](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987), um die aktive VCL-Versionsnummer abzurufen:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

Beachten Sie in der JSON-Antwort die aktive VCL-Versionsnummer, die im `number` zurückgegeben wird, z. B. `"number": 99`. Sie benötigen die Versionsnummer, wenn Sie die VCL zur Bearbeitung klonen.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Speichern Sie die aktive Versionsnummer in einer Bash-Umgebungsvariablen zur Verwendung in nachfolgenden API-Anfragen:

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### Schritt 2: Klonen Sie die aktive VCL-Version und alle Snippets

Bevor Sie benutzerdefinierte VCL-Snippets hinzufügen oder ändern können, müssen Sie eine Kopie der aktiven VCL-Version zur Bearbeitung erstellen. Verwenden Sie den Fastly-API[Klon](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709)-Vorgang:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

In der JSON-Antwort wird die Versionsnummer erhöht und der *active*-Schlüsselwert wird `false`. Sie können die neue, inaktive VCL-Version lokal ändern.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Speichern Sie die neue Versionsnummer in einer Bash-Umgebungsvariablen zur Verwendung in nachfolgenden Befehlen:

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### Schritt 3: Erstellen eines benutzerdefinierten VCL-Snippets

Erstellen und speichern Sie den benutzerdefinierten VCL-Code in einer JSON-Datei mit folgendem Inhalt und Format:

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

Die Werte umfassen:

- `name` - Name für den VCL-Ausschnitt.

- `dynamic` - Gibt an, ob es sich um einen [regulären Ausschnitt](https://docs.fastly.com/en/guides/about-vcl-snippets) oder einen [dynamischen Ausschnitt](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets) handelt.

- `type` () - Gibt den Ort an, an dem das erzeugte Snippet eingefügt werden soll, z. B. `init` (über den Unterprogrammen) und `recv` (innerhalb der Unterprogramme). Informationen zu [ Werten finden Sie unter ](https://docs.fastly.com/api/config#snippet)Fastly VCL-Snippet-Objektwerte .

- `priority` - Ein Wert von `1` bis `100`, der bestimmt, wann der benutzerdefinierte VCL-Code ausgeführt wird. Benutzerdefinierte VCL-Ausschnitte mit niedrigeren Werten werden zuerst ausgeführt.

  Der gesamte Standard-VCL-Code aus dem Fastly-VCL-Modul hat eine `priority` von `50`. Wenn eine Aktion zuletzt ausgeführt werden soll oder der standardmäßige VCL-Code überschrieben werden soll, verwenden Sie eine höhere Zahl, z. B. `100`. Wenn Sie benutzerdefinierten VCL-Code sofort ausführen möchten, legen Sie für die Priorität einen niedrigeren Wert fest, z. B. `5`.

- `content` - Der Ausschnitt des VCL-Codes, der in einer Zeile ohne Zeilenumbrüche ausgeführt werden soll. Siehe [Beispiel für ein benutzerdefiniertes VCL-Fragment](#example-vcl-snippet-code).

### Schritt 4: Hinzufügen eines VCL-Snippets zur Fastly-Konfiguration

Verwenden Sie den Fastly-API[Snippet erstellen](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d)-Vorgang, um das benutzerdefinierte VCL-Snippet zur VCL-Version hinzuzufügen.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

Der `<filename.json>` ist der Name der Datei, die Sie im vorherigen Schritt vorbereitet haben. Wiederholen Sie diesen Befehl für jeden VCL-Code-Ausschnitt.

Wenn Sie eine `500 Internal Server Error` Antwort vom Fastly-Service erhalten, überprüfen Sie die JSON-Dateisyntax, um sicherzustellen, dass Sie eine gültige Datei hochladen.

### Schritt 5: Validieren und Aktivieren benutzerdefinierter VCL-Snippets

Nachdem Sie ein benutzerdefiniertes VCL-Snippet hinzugefügt haben, fügt Fastly das Snippet in die VCL-Version ein, die Sie bearbeiten. Um Änderungen anzuwenden, führen Sie die folgenden Schritte aus, um den VCL-Code-Ausschnitt zu validieren und die VCL-Version zu aktivieren.

1. Verwenden Sie den Vorgang Fastly-API [VCL-Version validieren](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6), um den aktualisierten VCL-Code zu überprüfen.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Wenn die Fastly-API einen Fehler zurückgibt, beheben Sie das Problem und überprüfen Sie die aktualisierte VCL-Version erneut.

1. Verwenden Sie den Fastly-API-[aktivieren](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5), um die neue VCL-Version zu aktivieren.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## API-Schnellreferenz für VCL-Snippets

Diese API-Anfragebeispiele verwenden exportierte Umgebungsvariablen, um die Anmeldeinformationen bereitzustellen, mit denen sich Fastly authentifizieren kann. Weitere Informationen zu diesen Befehlen finden Sie in der [Fastly-API-Referenz](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Verwenden Sie diese Befehle, um Snippets zu verwalten, die Sie mit der Fastly-API hinzugefügt haben. Wenn Sie Snippets von der Admin-Instanz hinzugefügt haben, lesen Sie [Verwalten von VCL-Snippets mit der Admin-Instanz](#manage-vcl-using-the-api).

- **Aktive VCL-Versionsnummer abrufen**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **Listet alle regulären VCL-Snippets auf, die an einen Dienst angehängt sind**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **Überprüfen eines einzelnen Snippets**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  Der `<snippet_name>` ist der Name eines Snippets, z. B. `my_regular_snippet`.

- **Aktualisieren eines Snippets**

  Ändern Sie die [vorbereitete JSON-Datei](#step-3-create-a-custom-vcl-snippet) und senden Sie die folgende Anfrage:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **Löschen eines einzelnen VCL-Snippets**

  Rufen Sie eine Liste von Snippets ab und verwenden Sie den folgenden `curl`-Befehl mit dem spezifischen Snippet-Namen, der gelöscht werden soll:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **Überschreiben von Werten im [Standard-VCL-Code von Fastly](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  Erstellen Sie einen Ausschnitt mit aktualisierten Werten und weisen Sie `100` Priorität zu.
