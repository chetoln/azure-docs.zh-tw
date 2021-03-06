---
title: 自動調整和在 Azure 中的區域備援應用程式閘道
description: 本文介紹 Azure 應用程式 Standard_v2 和 WAF_v2 SKU，其中包括自動調整和區域備援功能。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 5/16/2019
ms.author: victorh
ms.openlocfilehash: 0da5d8a3eec0faa4001ccf229c6748c253f1b6e5
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827432"
---
# <a name="autoscaling-and-zone-redundant-application-gateway"></a>自動調整和區域備援應用程式閘道 

應用程式閘道和 Web 應用程式防火牆 (WAF) 也會是下 Standard_v2 和 WAF_v2 SKU 提供項目。 V2 SKU 提供效能增強功能，並且加入了重要的新功能，例如自動調整、 區域備援，以及支援靜態 Vip 支援。 下的標準和 WAF SKU 的現有功能會繼續在新的 v2 SKU，支援有一些例外狀況中所列[比較](#differences-with-v1-sku)一節。

新的 v2 SKU 會包含下列增強功能：

- **自動調整**：自動調整 SKU 下的「應用程式閘道」或 WAF 部署可以根據變動的流量負載模式來相應增加或相應減少。 自動調整規模也可讓您在佈建時，無須選擇部署大小或執行個體計數。 此 SKU 提供，則為 true 的彈性。 在 Standard_v2 和 WAF_v2 SKU 中，應用程式閘道能在固定的容量 （已停用自動調整），以啟用自動調整規模模式。 固定容量模式適用於工作負載一致且可預測的案例。 自動調整模式是有幫助，請參閱中的應用程式流量的變異數的應用程式中。
- **區域備援**：部署 WAF 的應用程式閘道可以跨越多個可用性區域，而不需要佈建每個區域使用流量管理員中的個別應用程式閘道執行個體。 您可以選擇單一區域或多個區域部署的應用程式閘道執行個體的位置，讓區域失敗更有彈性。 應用程式的後端集區可以均等分散在可用性區域間。

  區域備援可只所在 Azure 區域可用。 在其他區域，支援所有其他功能。 如需詳細資訊，請參閱[什麼是 Azure 中可用性區域？](../availability-zones/az-overview.md#services-support-by-region)
- **靜態 VIP**：應用程式閘道 v2 SKU 支援靜態 VIP 以獨佔方式輸入。 這可確保應用程式閘道相關聯的 VIP 不會變更生命週期的部署，即使重新啟動。
- **標頭重寫**:應用程式閘道可讓您新增、 移除或更新 HTTP 要求和回應標頭與 v2 SKU。 如需詳細資訊，請參閱[重寫的 HTTP 標頭，與應用程式閘道](rewrite-http-headers.md)
- **金鑰保存庫整合 （預覽）**:應用程式閘道 v2 支援與整合 Key Vault （公開預覽） 中的之伺服器憑證附加到啟用 HTTPS 接聽程式。 如需詳細資訊，請參閱 < [Key Vault 憑證與 SSL 終止](key-vault-certs.md)。
- **Azure Kubernetes 服務輸入控制器 （預覽）**:應用程式閘道 v2 輸入控制器可讓 Azure 應用程式閘道，來作為輸入的 Azure Kubernetes Service (AKS) 稱為 AKS 叢集。 如需詳細資訊，請參閱 <<c0> [ 文件頁面](https://azure.github.io/application-gateway-kubernetes-ingress/)。
- **效能增強功能**：V2 SKU 提供最多 5 個 X 更好 SSL 卸載相較於標準/WAF SKU 的效能。
- **更快速的部署和更新時間**v2 SKU 可提供更快速的部署和更新時間相較於標準/WAF SKU。 這也包括 WAF 組態變更。

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>支援區域

Standard_v2 和 WAF_v2 SKU 會在以下區域上市：美國中北部、美國中南部、美國西部、美國西部 2、美國東部、美國東部 2、美國中部、北歐、西歐、東南亞、法國中部、英國西部、日本東部、日本西部。 將在未來新增其他區域。

## <a name="pricing"></a>價格

V2 SKU 的定價模型驅動耗用量與不再附加至執行個體計數或大小。 V2 SKU 價格有兩個元件：

- **固定的價格**-這是每小時 （或一小時部分） 來佈建 Standard_v2 或 WAF_v2 閘道的價格。
- **容量單位價格**-這是除了固定成本的計費取用量為基礎的費用。 容量單位費用也計算每小時或部分每小時。 有三個容量單位的維度-計算單位、 持續的連線和輸送量。 計算單位所耗用的處理器容量的量值。 因素會影響計算單位是 TLS 連線數/秒 」、 「 URL 重寫的計算和 「 WAF 規則處理。 持續連線是在給定的計費時間間隔中的應用程式閘道建立 TCP 連線量值。 輸送量會平均 Mb/秒在給定的計費時間間隔處理系統。

每個容量單位最多組成：1 個計算單位或 2500年持續連線或 2.22 Mbps 輸送量。

計算單位的指引：

- **Standard_v2** -每個計算單位可以使用 RSA 2048 位元金鑰的 TLS 憑證每秒大約 50 個連線。
- **WAF_v2** -每個計算單位可以支援大約每 10 個並行要求第二個 70-30%與 70%的流量混用要求小於 2 KB GET/POST 和剩餘的更高版本。 WAF 效能不會受到回應大小目前。

> [!NOTE]
> 每個執行個體目前都能支援大約 10 個容量單位。
> 計算單位可以處理的要求數目取決於各種不同的準則，TLS 憑證金鑰大小、 金鑰交換演算法、 標頭重寫，以及發生 WAF 傳入的要求大小。 我們建議您執行應用程式測試，以判斷每個計算單位的要求率。 容量單位和計算單位都可以做為計量之前開始的計費。

**在美國東部定價**:

|              SKU 名稱                             | 固定的價格 （$/ 小時）  | 容量單位價格 （$/ CU-小時）   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0.20             | 0.0080                          |
| WAF_v2                                            |    0.36             | 0.0144                          |

如需定價資訊，請參閱[定價頁面](https://azure.microsoft.com/pricing/details/application-gateway/)。 已排定於 2019 年 7 月 1 日開始計費。

**範例 1**

應用程式閘道 Standard_v2 會佈建而不需要手動調整模式中的自動調整，固定的容量為五個執行個體。

固定的價格 = 744(hours) * 美金 $0.20 元 = $148.8 <br>
容量單位 = 每個執行個體的 744 （小時） 10 容量單位 * 5 個執行個體 * 每個容量單位時數 $0.008 = $297.6

總價 = $148.8 + $297.6 = $446.4

**範例 2**

應用程式閘道 standard_v2 是針對每個月，並且在這段期間中，會收到 25 新 SSL 連線數/秒，8.88 Mbps 資料傳輸的平均。 假設連接很短，您的價格會是：

固定的價格 = 744(hours) * 美金 $0.20 元 = $148.8

容量單位價格 = 744(hours) * Max (8.88/2.22 輸送量的容量單位中的 25/50 計算單位的連線數/秒） * $0.008 = 744 * 4 * $0.008 = $23.81

總價 = $148。 8 + 23.81 = $172.61

**範例 3**

每個月的佈建應用程式閘道 WAF_v2。 在此期間，它會接收 25 新 SSL 連線數/秒，8.88 Mbps 資料傳輸的平均，並沒有 80 秒的要求。 假設連接很短的存留時間，以及該應用程式的計算單位計算會支援 10 個 RPS，每個計算單位，您的價格會是：

固定的價格 = 744(hours) * $0.36 = $267.84

容量單位價格 = 744(hours) * Max (計算單位 Max(25/50 for connections/sec, 80/10 WAF RPS)，8.88/2.22 輸送量的容量單位) * $0.0144 = 744 * 8 * 0.0144 = $85.71

總價 = $267.84 + $85.71 = $353.55

[定價頁面](https://azure.microsoft.com/pricing/details/application-gateway/)將會更新以反映於 2019 5 月 14 日的地區的價格。 已排定於 2019 年 6 月 1 日開始計費。

## <a name="scaling-application-gateway-and-waf-v2"></a>調整應用程式閘道和 WAF v2

您可以設定應用程式閘道和 WAF 用於相應縮小兩種模式：

- **自動調整**-已啟用自動調整，應用程式閘道和 WAF v2 Sku 相應增加或減少依據應用程式的流量需求。 此模式為提供更佳的彈性，您的應用程式，並且讓您不需要猜測應用程式閘道大小或執行個體計數。 此模式也可讓您不需要執行閘道在預期的最大流量負載的尖峰佈建容量來節省成本。 客戶必須指定最小值和選擇性的最大執行個體計數。 最小容量可確保您的應用程式閘道和 WAF v2 不會因為下列指定，即使在流量不存在的最小執行個體計數。 您將支付此最小容量，即使在沒有任何流量。 您可以選擇性地指定最大執行個體計數，以確保應用程式閘道無法擴充超過指定的執行個體數目。 您將會繼續由閘道的流量數量向您收費。 執行個體計數的範圍可以從 0 到 125。 如果未指定，則執行個體計數上限的預設值為 20。
- **手動**-您也可以選擇手動模式中，閘道將不會自動調整規模。 在此模式中，如果沒有更多的流量，比什麼應用程式閘道或 WAF 可處理，它可能會導致流量遺失。 手動模式中，指定執行個體計數是必要的。 執行個體計數從 1 變更到 125 的執行個體。

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>SKU v1 與 v2 SKU 之間的功能比較

下表比較每個 SKU 提供的功能。

|                                                   | v1 SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| 自動調整規模                                       |          | &#x2713; |
| 區域備援                                   |          | &#x2713; |
| 靜態的 VIP                                        |          | &#x2713; |
| Azure Kubernetes Service (AKS) 輸入控制器 |          | &#x2713; |
| Azure Key Vault 整合                       |          | &#x2713; |
| 請重寫 HTTP (S) 標頭                           |          | &#x2713; |
| URL 型路由                                 | &#x2713; | &#x2713; |
| 多網站裝載                             | &#x2713; | &#x2713; |
| 流量重新導向                               | &#x2713; | &#x2713; |
| Web 應用程式防火牆 (WAF)                    | &#x2713; | &#x2713; |
| 安全通訊端層 (SSL) 終止            | &#x2713; | &#x2713; |
| 端對端 SSL 加密                         | &#x2713; | &#x2713; |
| 工作階段親和性                                  | &#x2713; | &#x2713; |
| 自訂錯誤頁面                                | &#x2713; | &#x2713; |
| WebSocket 支援                                 | &#x2713; | &#x2713; |
| HTTP/2 支援                                    | &#x2713; | &#x2713; |
| 清空連線                               | &#x2713; | &#x2713; |

> [!NOTE]
> SKU 現在支援自動調整 v2[預設健康情況探查](application-gateway-probe-overview.md#default-health-probe)自動監視它的後端集區中的所有資源的健全狀況，並反白顯示那些被視為狀況不良的後端成員。 沒有任何自訂探查組態的後端會自動設定預設健全狀況探查。 若要進一步了解，請參閱[健康情況探查應用程式閘道中](application-gateway-probe-overview.md)。

## <a name="differences-with-v1-sku"></a>使用 v1 SKU 的差異

|差異|詳細資料|
|--|--|
|驗證憑證|不支援。<br>如需詳細資訊，請參閱[應用程式閘道端對端 SSL 的概觀](ssl-overview.md#end-to-end-ssl-with-the-v2-sku)。|
|在相同子網路上混合使用 Standard_v2 和標準應用程式閘道|不支援|
|應用程式閘道子網路上的使用者定義路由 (UDR)|不支援|
|輸入連接埠範圍的 NSG| - Standard_v2 SKU 適用 65200 至 65535<br>- 標準 SKU 適用 65503 至 65534<br>如需詳細資訊，請參閱[常見問題集](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet)。|
|Azure 診斷中的效能記錄|不支援。<br>應該使用 Azure 計量。|
|計費|排程為於 2019 年 6 月 1 日開始計費。|
|FIPS 模式|目前不支援。|
|純 ILB 模式|目前不支援。 支援將公用與 ILB 模式搭配使用。|
|Netwatcher 整合|不支援。|
|Azure 支援中心整合|尚未提供。

## <a name="next-steps"></a>後續步驟

- [快速入門：使用 Azure 應用程式閘道-Azure 入口網站的直接網路流量](quick-create-portal.md)
- [使用 Azure PowerShell 建立具有保留虛擬 IP 位址的自動調整規模、區域備援應用程式閘道](tutorial-autoscale-ps.md)
- 深入了解[應用程式閘道](overview.md)。
- 深入了解 [Azure 防火牆](../firewall/overview.md)。