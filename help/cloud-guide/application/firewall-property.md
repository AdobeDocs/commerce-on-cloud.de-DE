---
title: Firewall-Eigenschaft
description: Siehe Beispiele zum Konfigurieren der Firewall-Eigenschaft in der Konfigurationsdatei der Commerce-Anwendung.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Firewall-Eigenschaft

>[!IMPORTANT]
>
>Nur Ausgangsprojekte

Bei Einstiegsprojekten fügt die `firewall`-Eigenschaft der Anwendung eine _ausgehende_ Firewall hinzu. Diese Firewall hat keine Auswirkungen auf eingehende Anfragen. Sie definiert, welche `tcp` ausgehenden Anfragen eine _-Site_ Adobe Commerce verlassen können. Dies wird als Ausgangs-Filterung bezeichnet. Die ausgehende Firewall filtert, was ausgehen kann: Verlassen oder Verlassen der Site. Durch die Begrenzung dessen, was entweichen kann, erhält der Server ein leistungsstarkes Sicherheitstool.

## Standardmäßige Einschränkungsrichtlinien

Die Firewall bietet zwei Standardrichtlinien zur Steuerung des ausgehenden Traffics: `allow` und `deny`. Die `allow` Richtlinie _lässt_ gesamten ausgehenden Traffic zu. Und die `deny`-Richtlinie _verweigert_ standardmäßig den gesamten ausgehenden Traffic. Wenn Sie jedoch eine Regel hinzufügen, wird die Standardrichtlinie außer Kraft gesetzt und die Firewall blockiert **alles** ausgehenden Traffic, der von der Regel nicht zugelassen ist.

Bei Starterplänen wird die Standardrichtlinie auf `allow` festgelegt. Mit dieser Einstellung wird sichergestellt, dass der gesamte ausgehende Traffic entsperrt bleibt, bis Sie Ihre Ausgangs-Filterregeln hinzufügen. Die Standardrichtlinie kann auf `deny` auf Anfrage festgelegt werden.

**So überprüfen Sie Ihre Standardrichtlinie**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

Sofern Sie keine `deny` für Ihre Richtlinie angefordert haben, sollte der Befehl den Richtliniensatz als `allow` anzeigen:

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**Wichtige Vorteile**: Wenn Sie eine ausgehende Regel hinzufügen, blockieren Sie den gesamten ausgehenden Traffic mit Ausnahme der Domains, IP-Adressen oder Ports, die Sie der Regel hinzufügen. Daher ist es wichtig, eine vollständige ausgehende Liste zu definieren und zu testen, bevor Sie sie zu Ihrer Produktions-Site hinzufügen.

## Firewall-Optionen

Die folgende Beispielkonfiguration in der `.magento.app.yaml` zeigt alle `firewall` Optionen, mit denen Sie Regeln für Ihre Ausgangs-Filterung hinzufügen können.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## Ausgangs-Filterregeln

Ausgehende Firewall-Konfigurationen bestehen aus Regeln. Sie können beliebig viele Regeln definieren. Die Vorschriften umfassen folgende Anforderungen.

**Jede Regel:**

- Muss mit einem Bindestrich (`-`) beginnen. Das Hinzufügen eines Kommentars in derselben Zeile hilft beim Dokumentieren und visuellen Trennen einer Regel von der nächsten.
- Muss mindestens eine der folgenden Optionen definieren: `domains`, `ips` oder `ports`.
- Muss das `tcp` Protokoll verwenden. Da dies das Standardprotokoll für alle Regeln ist, können Sie es aus der Regel auslassen.
- Kann `domains` oder `ips` definieren, aber nicht beide in derselben Regel.
- Kann `yaml` Kommentare (`#`) und Zeilenumbrüche enthalten, um die Domains, IP-Adressen und zulässigen Ports zu organisieren.

Jede Regel verwendet die folgenden Eigenschaften:

### `domains`

Die Option `domains` ermöglicht eine Liste vollständig qualifizierter Domain-Namen (FQDN).

Wenn eine Regel `domains` definiert, aber nicht `ports`, lässt die Firewall Domain-Anfragen an jedem Port zu.

### `ips`

Die Option `ips` ermöglicht eine Liste von IP-Adressen in der CIDR-Notation. Sie können einzelne IP-Adressen oder IP-Adressbereiche angeben.

Um eine einzelne IP-Adresse anzugeben, fügen Sie das `/32` CIDR-Präfix am Ende Ihrer IP-Adresse hinzu:

```
172.217.11.174/32  # google.com
```

Um einen Bereich von IP-Adressen anzugeben, verwenden Sie den [IP-Bereich zu CIDR](https://ipaddressguide.com/cidr)-Rechner.

Wenn eine Regel `ips` definiert, aber nicht `ports`, lässt die Firewall IP-Anfragen an jedem Port zu.

### `ports`

Die Option `ports` ermöglicht eine Liste der Ports von 1 bis 65535. Für die meisten Regeln im Beispiel erlauben die Ports `80` und `443` sowohl HTTP- als auch HTTPS-Anfragen. Für New Relic erlauben die Regeln jedoch nur den Zugriff auf Domains und IP-Adressen auf Port `443`, wie in der New Relic-Dokumentation unter [Netzwerk-Traffic](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents) empfohlen.

Wenn eine Regel nur `ports` definiert, ermöglicht die Firewall den Zugriff auf alle Domains und IP-Adressen für die definierten Ports.

>[!NOTE]
>
>Port `25`, der SMTP-Port zum Senden von E-Mails, wird ausnahmslos immer blockiert.

### `protocol`

Wie bereits erwähnt, ist TCP das standardmäßige und einzige zulässige Protokoll für Regeln. UDP und seine Ports sind nicht zulässig. Aus diesem Grund können Sie die Option `protocol` in allen Regeln auslassen. Wenn Sie sie trotzdem einbeziehen möchten, müssen Sie den Wert auf `tcp` festlegen, wie in der ersten Regel des Beispiels gezeigt.

## Suchen von zuzulassenden Domain-Namen

Verwenden Sie den folgenden Befehl, um die Domains zu identifizieren, die in Ihre Ausgangs-Filterregeln aufgenommen werden sollen, um die `dns.log`-Datei Ihres Servers zu analysieren und eine Liste aller DNS-Anfragen anzuzeigen, die von Ihrer Site protokolliert wurden:

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

Dieser Befehl zeigt auch DNS-Anfragen an, die zwar gestellt, aber von Ihren Ausgangs-Filterregeln blockiert wurden. Die Ausgabe zeigt nicht an, welche Domains blockiert wurden, sondern nur, dass -Anfragen gestellt wurden. Die Ausgabe zeigt keine Anfragen an, die über eine IP-Adresse gesendet wurden.

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

Domains sind im Gegensatz zu IP-Adressen normalerweise spezifischer und sicherer für die Ausgangs-Filterung. Wenn Sie beispielsweise eine IP-Adresse für einen Service hinzufügen, der ein CDN verwendet, lassen Sie die IP-Adresse für das CDN zu, das von Hunderten oder Tausenden anderer Domains verwendet werden kann. Mit einer IP-Adresse können Sie ausgehenden Zugriff auf Tausende anderer Server zulassen.

## Testen von Ausgangs-Filterregeln

Nachdem Sie Zugriffsregeln für die Domains und IP-Adressen erfasst und konfiguriert haben, die Ihre Site benötigt, ist es an der Zeit, sie zu pushen und zu testen.

So testen Sie Ihre Ausgangs-Filterregeln:

1. Erstellen Sie ein Shell-Skript mit `curl` Befehlen, um auf die Domains und IP-Adressen in Ihren Regeln zuzugreifen. Dazu gehören Befehle, die den Zugriff auf Domains und IP-Adressen testen, die blockiert werden sollen.

1. Konfigurieren Sie einen `post_deploy` Hook in Ihrer `.magento.app.yaml`, um das Skript auszuführen.

1. Pushen Sie die `firewall`-Konfiguration und das Testskript in die `integration`.

1. Überprüfen Sie die `post_deploy` Ausgabe Ihrer `curl`.

1. Verfeinern Sie Ihre `firewall`, aktualisieren Sie Ihr `curl`, übertragen Sie, senden Sie eine Push-Benachrichtigung und wiederholen Sie den Vorgang.

### Beispiel für `curl`

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy` Beispiel

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
