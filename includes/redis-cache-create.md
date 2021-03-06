---
title: 包含檔案
description: 包含檔案
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 0f1decae5fb3ec4a07f01c5bff7475f3d73a3cbb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60527837"
---
1. 若要建立快取，請先登入 [Azure 入口網站](https://portal.azure.com)。 然後選取 [建立資源] > [資料庫] > [Azure Cache for Redis]。

    ![New cache](media/redis-cache-create/redis-cache-new-cache-menu.png)

2. 在 [新的 Azure Cache for Redis] 中，配置新快取的設定。

    | 設定      | 建議的值  | 說明 |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS 名稱** | 全域唯一的名稱 | 快取名稱。 該名稱必須是介於 1 到 63 個字元的字串，而且只能包含數字、字母和 `-` 字元。 快取名稱的開頭或結尾不能是 `-` 字元，且連續的 `-` 字元無效。  | 
    | **訂用帳戶** | 您的訂用帳戶 | 這個新建立的 Azure Cache for Redis 執行個體所在的訂用帳戶。 | 
    | **資源群組** |  *TestResources* | 容納新建立快取的新資源群組名稱。 將應用程式的所有資源放在群組中，藉此同時管理。 例如，刪除資源群組會刪除所有與應用程式相關聯的資源。 | 
    | **位置** | 美國東部 | 選擇其他將使用快取之服務的鄰近[區域](https://azure.microsoft.com/regions/)。 |
    | **[定價層](https://azure.microsoft.com/pricing/details/cache/)** |  基本 C0 (250 MB 快取) |  快取的可用大小、效能和功能取決於定價層。 如需詳細資訊，請參閱 [Azure Cache for Redis 概觀](../articles/azure-cache-for-redis/cache-overview.md)。 |
    | **釘選到儀表板** |  已選取 | 將新快取釘選到儀表板，以便尋找。 |

    ![Create cache](media/redis-cache-create/redis-cache-cache-create.png) 

3. 配置新快取設定之後，請選取 [建立]。 

    建立快取可能需要數分鐘的時間。 若要檢查狀態，您可以監視儀表板上的進度。 建立快取之後，其顯示的狀態為 [執行中]，而且已經可供使用。

    ![Cache created](media/redis-cache-create/redis-cache-cache-created.png)

