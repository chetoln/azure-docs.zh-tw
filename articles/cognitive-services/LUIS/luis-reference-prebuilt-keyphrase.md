---
title: KeyPhrase 預先建置的實體
titleSuffix: Azure
description: 本文包含 Language Understanding (LUIS) 中預先建置的 keyPhrase 實體資訊。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 5ef7ccb58533161d8397ad42e70de1999908dc36
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338309"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>LUIS 應用程式的 KeyPhrase 預先建置實體
keyPhrase 會從語句中擷取各種不同的關鍵片語。 您不需要將包含 keyPhrase 的範例語句新增到應用程式。 keyPhrase 實體會在[多種文化特性](luis-language-support.md#languages-supported)中受到支援，以作為[文字分析](../text-analytics/overview.md)功能的一部分。 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>解析預先建置的 keyPhrase 實體
下列範例示範如何解析 **builtin.keyPhrase** 實體。

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>後續步驟

了解 [percentage](luis-reference-prebuilt-percentage.md)、[number](luis-reference-prebuilt-number.md) 及 [age](luis-reference-prebuilt-age.md) 實體相關資訊。
