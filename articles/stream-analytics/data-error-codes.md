---
title: Datenfehlercodes – Azure Stream Analytics
description: Beheben Sie Probleme in Azure Stream Analytics mit Datenfehlercodes.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: f7383a56a11ac9b567c80e73cc84944174c30ac8
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594089"
---
# <a name="azure-stream-analytics-configuration-error-codes"></a>Konfigurationsfehlercodes in Azure Stream Analytics

Unerwartetes Verhalten Ihres Azure Stream Analytics-Auftrags können Sie mithilfe von Aktivitätsprotokollen und Ressourcenprotokollen debuggen. In diesem Artikel ist die Beschreibung für jeden Datenfehlercode aufgeführt. Datenfehler treten auf, wenn ungültige Daten im Stream vorhanden sind, z. B. ein unerwartetes Datensatzschema.

## <a name="inputdeserializationerror"></a>InputDeserializationError

* **Ursache:** Fehler beim Deserialisieren von Eingabedaten.

## <a name="inputeventtimestampnotfound"></a>InputEventTimestampNotFound

* **Ursache:** Stream Analytics kann keinen Zeitstempel für die Ressource abrufen. 

## <a name="inputeventtimestampbyovervaluenotfound"></a>InputEventTimestampByOverValueNotFound

* **Ursache:** Stream Analytics kann den Wert von `TIMESTAMP BY OVER COLUMN` nicht abrufen.

## <a name="inputeventlatebeyondthreshold"></a>InputEventLateBeyondThreshold

* **Ursache:** Ein Eingabeereignis wurde später als in der konfigurierten Toleranz gesendet.

## <a name="inputeventearlybeyondthreshold"></a>InputEventEarlyBeyondThreshold

* **Ursache:** Die Eingangszeit eines Eingabeereignisses liegt vor dem Grenzwert für den Anwendungszeitstempel des Eingabeereignisses.

## <a name="azurefunctionmessagesizeexceeded"></a>AzureFunctionMessageSizeExceeded

* **Ursache:** Die Nachrichtenausgabe an Azure Functions überschreitet das Größenlimit.

## <a name="eventhuboutputrecordexceedssizelimit"></a>EventHubOutputRecordExceedsSizeLimit

* **Ursache:** Ein Ausgabedatensatz überschreitet das Größenlimit beim Schreiben an Event Hub.

## <a name="cosmosdboutputinvalidid"></a>CosmosDBOutputInvalidId

* **Ursache:** Der Wert oder der Typ einer bestimmten Spalte ist ungültig.
* **Empfehlung**: Geben Sie eindeutige, nicht leere Zeichenfolgen an, die nicht länger als 255 Zeichen sind.

## <a name="cosmosdboutputinvalididcharacter"></a>CosmosDBOutputInvalidIdCharacter

* **Ursache:** Die Dokument-ID des Ausgabedatensatzes enthält ein ungültiges Zeichen.

## <a name="cosmosdboutputmissingid"></a>CosmosDBOutputMissingId

* **Ursache:** Der Ausgabedatensatz enthält nicht die Spalte \[id], die als Primärschlüsseleigenschaft verwendet werden soll.

## <a name="cosmosdboutputmissingidcolumn"></a>CosmosDBOutputMissingIdColumn

* **Ursache:** Der Ausgabedatensatz enthält nicht die Dokument-ID-Eigenschaft. 
* **Empfehlung**: Stellen Sie sicher, dass die Abfrageausgabe die Spalte mit einer eindeutigen, nicht leeren Zeichenfolge enthält, die kleiner als 255 Zeichen ist.

## <a name="cosmosdboutputmissingpartitionkey"></a>CosmosDBOutputMissingPartitionKey

* **Ursache:** Im Ausgabedatensatz fehlt eine Spalte, die als Partitionsschlüsseleigenschaft verwendet werden soll.

## <a name="cosmosdboutputsinglerecordtoolarge"></a>CosmosDBOutputSingleRecordTooLarge

* **Ursache:** Der Schreibvorgang für einen einzelnen Datensatz in Cosmos DB ist zu groß.

## <a name="sqldatabaseoutputdataerror"></a>SQLDatabaseOutputDataError

* **Ursache:** Aufgrund von Problemen in den Daten kann Stream Analytics keine Ereignisse in die SQL-Datenbank schreiben.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für Eingangsverbindungen](stream-analytics-troubleshoot-input.md)
* [Problembehandlung von Azure Stream Analytics-Ausgaben](stream-analytics-troubleshoot-output.md)
* [Problembehandlung von Azure Stream Analytics-Abfragen](stream-analytics-troubleshoot-query.md)
* [Problembehandlung von Azure Stream Analytics mit Ressourcenprotokollen](stream-analytics-job-diagnostic-logs.md)
* [Datenfehler in Azure Stream Analytics](data-errors.md)
