---
title: 如何調整您的 Azure Time Series Insights 環境規模 | Microsoft Docs
description: 本文說明如何調整您的 Azure Time Series Insights 環境規模。 使用 Azure 入口網站在價格 SKU 內增加或減少容量。
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: f7d9827b11136370bcb401073a4d9cc4871c1cc9
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472779"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>如何調整您的 Time Series Insights 環境規模

本文說明如何使用 Azure 入口網站變更時間序列深入解析環境的容量。 容量是套用至輸入速率、儲存體容量以及與選取之 SKU 相關聯成本的乘數。

您可以使用 Azure 入口網站在給定的價格 SKU 內增加或減少容量。

不過，不允許變更定價層 SKU。 例如，擁有 S1 價格 SKU 的環境不能轉換成 S2，反之亦然。

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU 輸入速率和容量

| S1 SKU 容量 | 輸入速率 | 儲存體容量上限
| --- | --- | --- |
| 1 | 1 GB (1 百萬個事件) | 一個月 30 GB (3 千萬個事件) |
| 10 | 10 GB (1 千萬個事件) | 一個月 300 GB (3 億個事件) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU 輸入速率和容量

| S2 SKU 容量 | 輸入速率 | 儲存體容量上限
| --- | --- | --- |
| 1 | 10 GB (1 千萬個事件) | 一個月 300 GB (3 億個事件) |
| 10 | 100 GB (1 億個事件) | 一個月 3 TB (30 億個事件) |

容量是以線性方式擴充或減少，因此容量 2 的 S1 SKU 支援的輸入速率為一天 2 GB (2 百萬) 的事件，和 一個月 60 GB (6 千萬個事件)。

## <a name="change-the-capacity-of-your-environment"></a>變更您的環境容量

1. 在 Azure 入口網站中，找到並選取您的時間序列深入解析環境。

1. 在時間序列深入解析環境的功能表中，選取 [設定]。

   [![configure.png](media/scale-your-environment/configure.png)](media/scale-your-environment/configure.png#lightbox)

1. 調整 [容量] 滑桿來選取符合您所需輸入速率和儲存容量的容量。 請注意，**輸入速率**、**儲存容量**和**估計成本**會動態更新，以顯示變更的影響。

   [![滑桿](media/scale-your-environment/slider.png)](media/scale-your-environment/slider.png#lightbox)

   或者，您可以在滑桿右邊的文字方塊中，輸入容量乘數的數值。

1. 選取 [儲存] 來調整環境。 認可變更之後，系統就會立即顯示進度列指示器。

## <a name="next-steps"></a>後續步驟

- 請確定新容量[足夠以避免產生節流](time-series-insights-diagnose-and-solve-problems.md)。