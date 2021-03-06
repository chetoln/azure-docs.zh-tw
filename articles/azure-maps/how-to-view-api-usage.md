---
title: 了解如何查檢視Azure 地圖服務 API 使用量 | Microsoft Docs
description: 了解如何在入口網站中檢視您 Azure 地圖服務 API 呼叫的計量。
author: dsk-2015
ms.author: dkshir
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: d14088ed940ab83be29756a26f8612704bb9aebd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770083"
---
# <a name="view-azure-maps-api-usage"></a>檢視 Azure 地圖服務 API 使用量

此文章說明如何在[入口網站](https://portal.azure.com)中檢視您 Azure 地圖服務帳戶的 API 使用量計量。 計量是以方便檢視的圖形格式顯示，並包含可自訂的持續時間。

## <a name="view-metric-snapshot"></a>檢視計量快照集

您可以在您地圖服務帳戶的 [概觀] 頁面查看一些常見的計量。 它目前顯示可選取之持續時間內的 [要求總數]、[錯誤總數] 和 [可用性]。

![Azure 地圖服務計量概觀](media/how-to-view-api-usage/portal-overview.png)

如果您需要針對特定分析來自訂這些圖表，請繼續閱讀下一節。

## <a name="view-detailed-metrics"></a>檢視詳細計量

1. 在[入口網站](https://portal.azure.com)中登入您的 Azure 訂用帳戶。

2. 按一下左側的 [所有資源] 功能表項目，並瀏覽到您的 [Azure 地圖服務帳戶]。

3. 您的地圖服務帳戶開啟之後，按一下左邊的 [計量] 功能表。

4. 在 [計量] 窗格上，選擇下列其中一個項目：

   1. [可用性] - 顯示持續時間內 API 可用性的「平均值」。
   2. [使用量] - 顯示您帳戶的使用量「計數」為何。

      ![Azure 地圖服務計量窗格](media/how-to-view-api-usage/portal-metrics.png)

5. 接下來，您可以按一下 [過去 24 小時 (自動)] 來選取「時間範圍」。 根據預設，時間範圍會設定為 24 小時。 按一下之後，您會看到所有可選取的時間範圍。 在同一個下拉式功能表中，您也可以選取 [時間細微性]，並選擇以 [當地] 或 [GMT] 顯示時間。 按一下 [套用]。

    ![Azure 地圖服務計量時間範圍](media/how-to-view-api-usage/time-range.png)

6. 新增您的計量之後，接著您可以從和該計量相關的屬性間 [新增篩選]，然後針對該屬性選擇您想要在圖表看到的值。

    ![Azure 地圖服務計量篩選器](media/how-to-view-api-usage/filter.png)

7. 您也可以根據所選的計量屬性 [套用分割]。 這樣可讓圖表分割成多個圖表，即該屬性的每個值都會有一個圖表。 在下列圖片中，每個圖表顏色都對應到顯示在圖表底部的屬性值。

    ![Azure 地圖服務計量分割](media/how-to-view-api-usage/splitting.png)

8. 您也可以在同一個圖表上觀察多個計量，只要按一下頂端的 [新增計量] 按鈕即可。

## <a name="next-steps"></a>後續步驟

深入了解您要追蹤其使用量的 Azure 地圖服務 API：

> [!div class="nextstepaction"]
> [Azure 地圖服務 REST API 文件](https://docs.microsoft.com/rest/api/maps) \(英文\)
