---
title: Azure API 管理原則範例 - 將錯誤傳送至 Stackify 進行記錄 | Microsoft Docs
description: Azure API 管理原則範例 - 示範如何新增錯誤記錄原則，以將錯誤傳送到 Stackify 進行記錄。
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 07cc83830fe2d467c611622bb66dfbb8c9429c2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60860533"
---
# <a name="send-errors-to-stackify-for-logging"></a>將錯誤傳送至 Stackify 進行記錄

本文會說明 Azure API 管理原則範例，示範如何新增錯誤記錄原則，以將錯誤傳送到 Stackify 進行記錄。 若要設定或編輯原則程式碼，請依照[設定或編輯原則](../set-edit-policies.md)中所述的步驟執行。 若要查看其他範例，請參閱[原則範例](../policy-samples.md)。

## <a name="policy"></a>原則

將程式碼貼至 [on-error] 區塊。

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>後續步驟

深入了解 APIM 原則：

+ [轉換原則](../api-management-transformation-policies.md)
+ [原則範例](../policy-samples.md)

