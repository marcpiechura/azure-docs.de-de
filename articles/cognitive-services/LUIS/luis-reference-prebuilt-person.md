---
title: Vordefinierte PersonName-Entität – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten personName-Entität in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 768c719211e8a8f2133d3798343d076e795a3da0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273436"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Vordefinierte PersonName-Entität für eine LUIS-App
Die vordefinierte personName-Entität erkennt die Namen von Personen. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit personName hinzufügen. Die personName-Entität wird in den [Kulturen](luis-reference-prebuilt-entities.md) Englisch und Chinesisch unterstützt.

## <a name="resolution-for-personname-entity"></a>Auflösung für die personName-Entität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`Is Jill Jones in Cairo?`


#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)


Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ],
    "$instance": {
        "personName": [
            {
                "type": "builtin.personName",
                "text": "Jill Jones",
                "startIndex": 3,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
    }
}
```
#### <a name="v2-response"></a>[V2-Antwort](#tab/V2)

Im folgenden Beispiel wird die Auflösung der Entität **builtin.personName** veranschaulicht.

```json
"entities": [
{
    "entity": "Jill Jones",
    "type": "builtin.personName",
    "startIndex": 3,
    "endIndex": 12
}
]
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md).
