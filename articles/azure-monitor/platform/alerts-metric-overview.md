---
title: 了解計量警示在 Azure 監視器中的運作方式。
description: 取得計量警示有何用途以及其在 Azure 監視器中如何運作的概觀。
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.subservice: alerts
ms.openlocfilehash: 59973d9530bf1c3ab3e77290b25e50860f9de0ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712860"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>了解計量警示在 Azure 監視器中的運作方式

Azure 監視器中的計量警示以多維度計量為基礎運作。 這些計量可能是[平台計量](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)、[自訂計量](../../azure-monitor/platform/metrics-custom-overview.md)、[來自 Azure 監視器並轉換為計量的常用記錄](../../azure-monitor/platform/alerts-metric-logs.md)，以及 Application Insights 計量。 計量警示會定期評估，檢查一或多個計量時間序列上的條件是否為真，並在評估符合時通知您。 計量警示具狀態，也就是說只會在狀態變更時傳送通知。

## <a name="how-do-metric-alerts-work"></a>計量警示的運作方式為何？

您可以藉由指定要監視的目標資源、計量名稱、條件類型 (靜態或動態) 以及條件 (運算子與閾值/敏感度) 來定義計量警示規則，以及要在警示規則引發時觸發的動作群組。 條件類型會影響決定閾值的方式。 [深入了解動態閾值條件類型和敏感度選項](alerts-dynamic-thresholds.md)。

### <a name="alert-rule-with-static-condition-type"></a>具有靜態條件類型的警示規則

假設您已建立簡單的靜態閾值計量警示規則，如下所示：

- 目標資源 (您要監視的 Azure 資源)：myVM
- 計量：Percentage CPU
- 條件類型：靜態
- 時間彙總 (針對未經處理的計量值執行的統計資料。 支援的時間彙總為最小、最大、平均、總計)：平均值
- 期間 (檢查計量值的回溯查看時段)：過去 5 分鐘
- 頻率 (計量警示檢查條件是否符合的頻率)：1 分鐘
- 運算子：大於
- 閾值：70

從警示規則建立後開始，監視器會每隔 1 分鐘執行一次，並查看過去 5 分鐘的計量值，以及檢查這些值的平均是否超過 70。 如果條件符合，也就是說過去 5 分鐘的 CPU 百分比平均超過 70，則警示規則會引發啟動通知。 如果您已在與警示規則相關聯的動作群組中設定電子郵件或 Webhook 動作，您會收到兩者的啟動通知。

### <a name="alert-rule-with-dynamic-condition-type"></a>具有動態條件類型的警示規則

假設您已建立簡單的動態閾值計量警示規則，如下所示：

- 目標資源 (您要監視的 Azure 資源)：myVM
- 計量：Percentage CPU
- 條件類型：動態
- 時間彙總 (針對未經處理的計量值執行的統計資料。 支援的時間彙總為最小、最大、平均、總計)：平均值
- 期間 (檢查計量值的回溯查看時段)：過去 5 分鐘
- 頻率 (計量警示檢查條件是否符合的頻率)：1 分鐘
- 運算子：大於
- 敏感度：中
- 回溯查看期間：4
- 違規次數：4

在警示規則建立後，動態閾值機器學習演算法會取得可用的歷史資料，計算最符合計量序列行為模式的閾值，並且根據新資料持續學習，讓閾值更準確。

從警示規則建立後開始，監視器會每隔 1 分鐘執行一次，並查看過去 20 分鐘 (以 5 分鐘的期間分組) 的計量值，以及檢查這 4 個期間的平均值是否超過預期的閾值。 如果條件符合，也就是說過去 20 分鐘 (四個 5 分鐘的期間) 的 CPU 百分比平均值超過偏離預期的行為四次，則警示規則會引發啟動的通知。 如果您已在與警示規則相關聯的動作群組中設定電子郵件或 Webhook 動作，您會收到兩者的啟動通知。

### <a name="view-and-resolution-of-fired-alerts"></a>檢視並解決觸發的警示

前述警示規則引發範例也可在 Azure 入口網站的 [所有警示] 刀鋒視窗中檢視。

假設在後續檢查中，"myVM" 上的使用量一直高於閾值，除非解決該情況，否則不會再次引發警示規則。

經過一段時間後，如果 "myVM" 上的使用量下降恢復至正常，也就是低於閾值。 警示規則會再監視該條件兩次，以傳送解決通知。 當連續三個期間的警示條件都不符合時，警示規則會傳送已解決/停用訊息，以減少不穩定情況下的雜訊。

透過 Webhook 或電子郵件傳送已解決通知時，Azure 入口網站中警示執行個體的狀態 (稱為監視器狀態)，也會設定為已解決。

### <a name="using-dimensions"></a>使用維度

Azure 監視器中的計量警示也支援以一個規則監視多個維度值組合。 讓我們借助範例，來了解為什麼您可能會使用多個維度組合。

假設您有要用於網站的 App Service 方案。 您想要針對執行您網站/應用程式的多個執行個體監視其 CPU 使用量。 您可以使用計量警示規則，如下所示：

- 目標資源：myAppServicePlan
- 計量：Percentage CPU
- 條件類型：靜態
- 維度
  - 執行個體 = InstanceName1、InstanceName2
- 時間彙總：平均值
- 期間：過去 5 分鐘
- 頻率：1 分鐘
- 運算子：GreaterThan
- 閾值：70

和之前一樣，此規則會監視過去 5 分鐘的平均 CPU 使用量是否超過 70%。 不過，使用相同的規則，您可以監視兩個執行您網站的執行個體。 每個執行個體都是個別監視，而您也會個別得到通知。

假設您的 Web 應用程式預計會面臨大量需求，而您需要新增更多執行個體。 上述規則仍只會監視兩個執行個體。 不過，您可以建立規則，如下所示：

- 目標資源：myAppServicePlan
- 計量：Percentage CPU
- 條件類型：靜態
- 維度
  - 執行個體 = *
- 時間彙總：平均值
- 期間：過去 5 分鐘
- 頻率：1 分鐘
- 運算子：GreaterThan
- 閾值：70

此規則會自動監視執行個體的所有值，也就是說 您可以在執行個體顯示時監視，而不需要再次修改計量警示規則。

監視多個維度時，動態閾值警示規則可同時為數百個計量序列建立合適的閾值。 動態閾值可減少需要管理的警示規則，且在警示規則的管理和建立方面都可省下大量時間。

假設您的 Web 應用程式有多個執行個體，而您不知道最適合的閾值為何。 上述規則一律會使用 70% 的閾值。 不過，您可以建立規則，如下所示：

- 目標資源：myAppServicePlan
- 計量：Percentage CPU
- 條件類型：動態
- 維度
  - 執行個體 = *
- 時間彙總：平均值
- 期間：過去 5 分鐘
- 頻率：1 分鐘
- 運算子：GreaterThan
- 敏感度：中
- 回溯查看期間：1
- 違規次數：1

此規則會監視過去 5 分鐘的平均 CPU 使用量是否超過每個執行個體的預期行為。 同樣地，您可以在執行個體顯示時監視，而不需要再次修改計量警示規則。 每個執行個體都會取得符合計量序列行為模式的閾值，並且根據新資料持續變更，使閾值更為精確。 同樣地，每個執行個體都個別受到監視，而您也會個別得到通知。

增加回溯查看期間和違規次數，也可以讓篩選警示僅對您的重大偏差定義發出警示。 [深入了解動態閾值進階選項](alerts-dynamic-thresholds.md#what-do-the-advanced-settings-in-dynamic-thresholds-mean)。

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>在 Azure 監視器中使用計量警示大規模進行監視

到目前為止，您已經了解如何使用單一計量警示來監視與單一 Azure 資源相關的一或多個計量時間序列。 很多時候，您可能想將相同的警示規則套用至許多資源。 Azure 監視器也支援以單一計量警示規則監視多個資源。 這項功能目前僅在虛擬機器上支援使用。 此外，單一計量警示也可以監視一個 Azure 區域中的資源。

您可以透過下列三種方式之一，指定單一計量警示所監視的範圍：

- 作為訂用帳戶中的一系列虛擬機器 (位於一個 Azure 區域)
- 作為訂用帳戶中一或多個資源群組的所有虛擬機器 (位於一個 Azure 區域)
- 作為訂用帳戶中的所有虛擬機器 (位於一個 Azure 區域)

建立監視多個資源的計量警示規則，類似於[建立監視單一資源的任何其他計量警示](alerts-metric.md)。 唯一的區別是您會選取要監視的所有資源。 您也可以透過 [Azure Resource Manager 範本](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-metric-alert-that-monitors-multiple-resources)建立這些規則。 您會收到每個虛擬機器的個別通知。

## <a name="typical-latency"></a>一般延遲

針對計量警示，如果您將警示規則頻率設定為 1 分鐘，通常可在 5 分鐘內得到通知。在通知系統負載過重時，則可能會延遲較久。

## <a name="supported-resource-types-for-metric-alerts"></a>計量警示支援的資源類型

您可以在此[文章](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)中找到所支援資源類型的完整清單。

如果您目前使用傳統計量警示，並且正在查看計量警示是否支援您正在使用的所有資源類型，下表顯示了傳統計量警示支援的資源類型，以及它們現在是否受到計量警示的支援。

|傳統計量警示支援的資源類型 | 受計量警示支援 |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | 是 |
| Microsoft.Batch/batchAccounts| 是|
|Microsoft.Cache/redis| 是 |
|Microsoft.ClassicCompute/virtualMachines | 否 |
|Microsoft.ClassicCompute/domainNames/slots/roles | 否|
|Microsoft.CognitiveServices/accounts | 否 |
|Microsoft.Compute/virtualMachines | 是|
|Microsoft.Compute/virtualMachineScaleSets| 是|
|Microsoft.ClassicStorage/storageAccounts| 否 |
|Microsoft.DataFactory/datafactories | 是|
|Microsoft.DBforMySQL/servers| 是|
|Microsoft.DBforPostgreSQL/servers| 是|
|Microsoft.Devices/IotHubs | 否|
|Microsoft.DocumentDB/databaseAccounts| 是|
|Microsoft.EventHub/namespaces | 是|
|Microsoft.Logic/workflows | 是|
|Microsoft.Network/loadBalancers |是|
|Microsoft.Network/publicIPAddresses| 是|
|Microsoft.Network/applicationGateways| 是|
|Microsoft.Network/expressRouteCircuits| 是|
|Microsoft.Network/trafficManagerProfiles | 是|
|Microsoft.Search/searchServices | 是|
|Microsoft.ServiceBus/namespaces| 是 |
|Microsoft.Storage/storageAccounts | 是|
|Microsoft.StreamAnalytics/streamingjobs| 是|
|Microsoft.TimeSeriesInsights/environments | 是|
|Microsoft. Web/serverfarms | 是 |
|Microsoft. Web/sites (不包括函式) | 是|
|Microsoft. Web/hostingEnvironments/multiRolePools | 否|
|Microsoft. Web/hostingEnvironments/workerPools| 否 |
|Microsoft.SQL/Servers | 否 |

## <a name="next-steps"></a>後續步驟

- [了解如何在 Azure 中建立、檢視及管理計量警示](alerts-metric.md)
- [了解如何使用 Azure Resource Manager 範本部署計量警示](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [深入了解動作群組](action-groups.md)
- [深入了解動態閾值條件類型](alerts-dynamic-thresholds.md)
