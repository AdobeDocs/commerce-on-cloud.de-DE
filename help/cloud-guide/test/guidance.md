---
title: Testanleitung
description: Erfahren Sie mehr über Testtypen und Best Practices für den Start von Adobe Commerce in Cloud-Infrastrukturen.
exl-id: 70fdfbbd-1763-4b1b-9ffd-9ffdc92f4f91
TQID: https://experienceleague.adobe.com/uCBGY5eP-R4zbUWeD-WE95F49k-U-nI058FR-rOJfsU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 371
ht-degree: 0%

---

# Testanleitung

Nach der Konfiguration und Anpassung Ihres Adobe Commerce on Cloud-Infrastrukturprojekts ist es Best Practice, Ihre Anwendung gründlich zu testen, bevor Sie die Store-Website starten. Tests helfen, die Erwartungen an die Cluster-Größe richtig zu verwalten und für zukünftige Geschäftsanforderungen angemessen zu skalieren.

## Funktionstests

Während der Entwicklung ist es wichtig, End-to-End-Funktionstests für Ihr Adobe Commerce in einem Cloud-Infrastrukturprojekt durchzuführen. Siehe die folgende Anleitung zum Ausführen von Funktionstests in der Docker-Umgebung:

- **Anwendungstests**: Verwenden Sie das [Magento Functional Testing Framework (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing) zum Testen von Anwendungen in der Cloud Docker-Umgebung.

- **Code-**: Verwenden Sie das [Codeception Testing Framework für PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing) zur Validierung von Code, der für den Beitrag zu Cloud-Paket-Repositorys vorgesehen ist.

## Best Practices vor dem Launch

Beachten Sie die folgenden Testtypen als Best Practice vor dem Start der Site:

- **Belastungstest** Führen Sie einen Belastungstest durch, um das Verhalten des Systems unter einer erwarteten Last zu verstehen. Testen Sie beispielsweise eine gleichzeitige Anzahl aktiver Benutzer im Programm, wobei jeder Benutzer innerhalb der festgelegten Dauer eine bestimmte Anzahl von Transaktionen ausführen muss. Dieser Test zeigt die Reaktionszeit wichtiger geschäftskritischer Transaktionen, z. B. das Verhalten der Datenbank oder des Anwendungs-Servers. Ein Belastungstest kann dabei helfen, Engpässe zu identifizieren.

- **Belastungstest** - Fordern Sie die oberen Kapazitätsgrenzen innerhalb des Systems an, um festzustellen, ob das System ausreichend funktioniert, wenn die aktuelle Last deutlich über dem erwarteten Maximum liegt.

- **Sicherheitsprüfung** - Adobe bietet ein kostenloses [Sicherheitsprüfungstool](../launch/overview.md#set-up-the-security-scan-tool) für Ihre Standorte.

- **Penetrationstest** - Ist ein autorisierter simulierter Cyberangriff auf ein Computersystem, der die Sicherheit des Systems bewertet. Der Penetrationstest hilft dabei, Schwachstellen oder Schwachstellen zu identifizieren, einschließlich des Potenzials für nicht autorisierte Parteien, Zugriff auf Systemfunktionen und -daten zu erhalten.

>[!WARNING]
>
>Kunden sind nicht berechtigt, Sicherheitsbewertungen der AWS-Infrastruktur oder der AWS-Services selbst durchzuführen. Wenn Sie bei einem der AWS-Services in Ihrer Sicherheitsbewertung ein Sicherheitsproblem feststellen, wenden Sie sich [&#x200B; an AWS Security](mailto:aws-security@amazon.com). Siehe [AWS-Kundenrichtlinien für Penetrationstests](https://aws.amazon.com/security/penetration-testing/).
