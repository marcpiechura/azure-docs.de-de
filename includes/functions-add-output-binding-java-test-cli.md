---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 6a359cdd44cc0c0cfbd93bd23b69a67a641c7fbb
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673278"
---
## <a name="update-the-tests"></a>Aktualisieren der Tests

Da der Archetyp auch eine Reihe von Tests erstellt, müssen Sie diese Tests aktualisieren, um den neuen Parameter `msg` in der Signatur der Methode `run` zu behandeln.  

Navigieren Sie zum Speicherort Ihres Testcodes (unter _src/test/java_), öffnen Sie die Projektdatei *Function.java*, und ersetzen Sie die Codezeile unter `//Invoke` durch den folgenden Code:

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::