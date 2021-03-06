---
title: 包含檔案
description: 包含檔案
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7cee3f534e4ab4973a87a9c373a5b83aa2ba9ce2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549934"
---
在 Azure Batch 中執行的工作，可能會在執行時產生輸出資料。 工作輸出的資料通常需要加以儲存，才能供作業中的其他工作、執行作業的用戶端應用程式 (或兩者) 擷取。 工作會將輸出資料寫入 Batch 計算節點的檔案系統，但當它重新安裝映像，或當節點離開集區時，節點上的所有資料都會遺失。 工作可能也會有檔案保留期，在此期間之後，會將工作所建立的檔案刪除。 基於這些理由，請務必將您稍後需要的工作輸出保存至資料存放區，例如 [Azure 儲存體](https://docs.microsoft.com/azure/storage/)。

如需 Batch 中的儲存體帳戶選項，請參閱 [Batch 功能概觀](../articles/batch/batch-api-basics.md#azure-storage-account)。
