---
title: Azure 容器映像的 Sku |Azure Marketplace
description: 設定 Azure 容器的 SKU。
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 6953329bfabe99fc4bb28f2494cb412ba9cbbba0
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942916"
---
# <a name="container-skus-tab"></a>容器 SKU 索引標籤

可使用 [新增供應項目] 頁面的 [SKU] 索引標籤，建立一個或一個以上的 SKU，並建立 SKU 與新供應項目之間的關聯性。  您可以使用不同的 SKU，依功能集、帳單模式或其他特性來區分解決方案。

## <a name="sku-settings"></a>SKU 設定

開始建立新的供應項目時，沒有任何與該供應項目相關聯的 SKU。 若要建立新的 SKU，請依照下列步驟進行：

1. 在 [SKU] 索引標籤中，選取 [新增 SKU]

   ![新增 SKU 提示](./media/containers-sku-settings.png)

2. 提供所需的 SKU 和容器資訊。 每個 SKU 都會對應至容器映像。 SKU 有兩個部分：

    -   SKU 中繼資料
    -   容器中繼資料


### <a name="sku-metadata"></a>SKU 中繼資料

SKU 中繼資料包含容器清單的店面顯示資訊。

![SKU 中繼資料](./media/containers-sku-details.png)


### <a name="container-metadata"></a>容器中繼資料

容器中繼資料擁有 Azure Container Registry (ACR) 中映像存放庫詳細資料的參考資訊。 Azure Marketplace 會將此映像複製到 Marketplace 專用的公用登錄中，然後在認證之後提供給客戶使用。 Azure 使用者取用 Azure Marketplace 容器映像的所有要求，都會從 Marketplace 的公用登錄處理，而不是 ACR。

![容器中繼資料](./media/containers-image-repository.png)
    
**映像儲存機制詳細資料**前一個螢幕擷取包含下列欄位。  必要欄位由星號 (*) 標示。

-   **訂用帳戶 ID\***  -ACR 所在的 Azure 訂用帳戶 ID。
-   **資源群組名稱\*** -ACR 的資源群組名稱。
-   **登錄名稱\*** -ACR 名稱。
-   **存放庫名稱\*** -存放庫名稱。 設定此名稱之後，就無法變更這個值。 使用唯一的名稱避免與您帳戶中的其他供應項目發生衝突。
-   **使用者名稱\*** -與 ACR 映像相關聯的使用者名稱 （系統管理員使用者名稱）。
-   **密碼\*** -與 ACR 映像相關聯的密碼。

    >[!NOTE]
    >必須提供使用者名稱和密碼，以確保合作夥伴在發佈程序中能存取先前所述的 ACR。


### <a name="image-version"></a>映像版本

發佈容器映像時，您可以提供一個或多個映像標籤以及 SHA 訊息摘要。

**映像標記\*或摘要**
 
- 此標籤或訊息摘要必須包含 `latest` 標籤和版本標籤 (例如，以 `xx.xx.xx-` 開頭，其中 xx 是數字)， 且應是[資訊清單標籤](https://github.com/estesp/manifest-tool)，以將多個平台設為目標。 也請務必新增資訊清單標籤參考的所有標籤，以便上傳。 
- 您可以使用標籤來新增數個版本的容器。 所有資訊清單標籤 (除了 `latest` 以外) 必須以 `X.Y-` 或 `X.Y.Z-` 開頭，其中 X、Y、Z 是整數。 <br/> 例如，如果 `latest` 標籤指向 `1.0.1-linux-x64`、`1.0.1-linux-arm32`，以及 `1.0.1-windows-arm32`，則需要在此處加入這些標籤。

>[!NOTE]
>請記得將**測試標籤**新增到您的映像，以便您在測試期間識別該映像。


## <a name="next-steps"></a>後續步驟

使用 [Marketplace](./cpp-marketplace-tab.md) 索引標籤建立供應項目的 Marketplace 說明。 
