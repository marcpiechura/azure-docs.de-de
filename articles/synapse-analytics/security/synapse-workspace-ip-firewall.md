---
title: Konfigurieren von IP-Firewallregeln in Azure Synapse Analytics
description: In diesem Artikel lernen Sie, wie Sie IP-Firewallregeln in Azure Synapse Analytics konfigurieren.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 1dfac9a56e3bc299fbb0651ae1cd5644eba83267
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83645813"
---
# <a name="azure-synapse-analytics-ip-firewall-rules-preview"></a>IP-Firewallregeln in Azure Synapse Analytics (Vorschau)

In diesem Artikel werden IP-Firewallregeln erläutert, und Sie erfahren, wie Sie diese in Azure Synapse Analytics konfigurieren.

## <a name="ip-firewall-rules"></a>IP-Firewallregeln

IP-Firewallregeln gewähren oder verweigern den Zugriff auf Ihren Synapse-Arbeitsbereich basierend auf der IP-Quelladresse der jeweiligen Anforderung. Sie können IP-Firewallregeln für Ihren Arbeitsbereich konfigurieren. Auf Arbeitsbereichsebene konfigurierte IP-Firewallregeln gelten für alle öffentlichen Endpunkte des Arbeitsbereichs (SQL-Pools, SQL On-Demand und Entwicklung).

## <a name="create-and-manage-ip-firewall-rules"></a>Erstellen und Verwalten von IP-Firewallregeln

Es gibt zwei Möglichkeiten, um einem Synapse-Arbeitsbereich IP-Firewallregeln hinzuzufügen. Um eine IP-Firewall zu Ihrem Arbeitsbereich hinzuzufügen, wählen Sie **Sicherheit + Netzwerk** aus, und aktivieren beim Erstellen des Arbeitsbereichs die Option **Verbindungen von allen IP-Adressen zulassen**.

![IP-Konfiguration für einen Synapse-Arbeitsbereich im Azure-Portal.](./media/synpase-workspace-ip-firewall/ip-firewall-1.png)

![IP-Konfiguration für einen Synapse-Arbeitsbereich im Azure-Portal.](./media/synpase-workspace-ip-firewall/ip-firewall-2.png)

Sie können einem Synapse-Arbeitsbereich auch nach der Erstellung IP-Firewallregeln hinzufügen. Wählen Sie dazu im Azure-Portal im Bereich **Sicherheit** die Option **Firewalls** aus. Um eine neue IP-Firewallregel hinzuzufügen, geben Sie einen Namen sowie eine Start-IP-Adresse und eine End-IP-Adresse an. Wenn Sie abschließend **Speichern** aus.

![IP-Konfiguration für einen Azure Synapse-Arbeitsbereich im Azure-Portal.](./media/synpase-workspace-ip-firewall/ip-firewall-3.png)

## <a name="connecting-to-synapse-from-your-own-network"></a>Herstellen einer Verbindung mit Synapse aus dem eigenen Netzwerk

Sie können über Synapse Studio eine Verbindung mit Ihrem Synapse-Arbeitsbereich herstellen. Sie können auch SQL Server Management Studio (SSMS) verwenden, um eine Verbindung mit den SQL-Ressourcen (SQL-Pools und SQL On-Demand) in Ihrem Arbeitsbereich herzustellen.

Stellen Sie sicher, dass die Firewall in Ihrem Netzwerk und auf Ihrem lokalen Computer ausgehende TCP-Kommunikation für Synapse Studio über die TCP-Ports 80, 443 und 1443 zulässt.

Sie müssen auch die ausgehende Kommunikation über den UDP-Port 53 für Synapse Studio zulassen. Um eine Verbindung mithilfe von Tools wie SSMS oder Power BI herzustellen, müssen Sie die ausgehende Kommunikation über den TCP-Port 1433 zulassen.

Wenn Sie die standardmäßige Richtlinieneinstellung für die Umleitung von Verbindungen verwenden, müssen Sie möglicherweise die ausgehende Kommunikation über weitere Ports zulassen. Mehr über Verbindungsrichtlinien erfahren Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy).

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Azure Synapse-Arbeitsbereichs](../quickstart-create-workspace.md)

Erstellen eines Azure Synapse-Arbeitsbereichs mit einem [verwalteten Arbeitsbereich-VNET](./synapse-workspace-managed-vnet.md)