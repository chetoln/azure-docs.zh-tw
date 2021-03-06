---
title: Azure 中的警示與通知監視概觀
description: Azure 中的警示概觀。 警示、傳統警示、警示介面。
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/28/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 6fb49baf8ab58ae6cfe7639cedcc4466810c8b96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347406"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft Azure 中的警示概觀 

本文章說明何謂警示、其優點，以及如何開始使用它們。  




## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft Azure 中的警示是什麼？
當您的監視資料中發現重要條件時，警示會主動通知您。 它們可讓您在系統使用者注意到問題之前，找出並解決問題。 

本文討論「Azure 監視器」中統一的警示體驗，此體驗現在包含 Log Analytics 和 Application Insights 所管理的警示。 [先前的警示體驗](alerts-classic.overview.md)和警示類型稱為**傳統警示**。 您可以透過按一下警示頁面上方的 [檢視傳統警ˋ] 來檢視此舊版體驗和舊版警示類型。 

## <a name="overview"></a>概觀

下圖代表警示流程。 

![警示流程](media/alerts-overview/Azure-Monitor-Alerts.svg)

警示規則分成警示和警示引發時所採取的動作。 

**警示規則** - 警規則會擷取用於警示的目標和準則。 警示規則可處於已啟用或已停用狀態。 警示只有在啟用時才會引發。 

警示規則的主要屬性包括：

**目標資源** - 定義用於警示的範圍和訊號。 目標可以是任何 Azure 資源。 範例目標：虛擬機器、儲存體帳戶、虛擬機器擴展集、Log Analytics 工作區或 Application Insights 資源。 針對某些資源 (例如虛擬機器)，您可以指定多個資源作為警示規則的目標。

**訊號** - 訊號是由目標資源所發出並可以是數種類型。 計量、Activity log、Application Insights 及記錄。

**準則** - 準則是目標資源上所套用訊號和邏輯的組合。 範例： 
   - 百分比 CPU > 70%
   - 伺服器回應時間 > 4 ms 
   - 記錄查詢的結果計數 > 100

**警示名稱** – 使用者所設定的警示規則特定名稱

**警示描述** – 使用者所設定的警示規則描述

**嚴重性** – 符合警示規則中指定的準則時的警示嚴重性。 嚴重性的範圍可從 0 到 4。

**動作** - 引發警示時所採取的動作。 如需詳細資訊，請參閱[動作群組](../../azure-monitor/platform/action-groups.md)。

## <a name="what-you-can-alert-on"></a>您可以發出警示的對象

您可以針對計量和記錄發出警示，如[監視資料來源](../../azure-monitor/platform/data-sources-reference.md)中所述。 包含但不限於：
- 計量值
- 記錄搜尋查詢
- 活動記錄事件
- 基礎 Azure 平台健康情況
- 網站可用性測試

之前，「Azure 監視器」計量、Application Insights、Log Analytics 及「服務健康狀態」具有個別的警示功能。 隨著時間進展，Azure 已改善並結合使用者介面與不同的警示方法。 這樣的整併仍在持續進行中。 因此，新的警示系統中仍可能沒有某些警示功能。  

| **監視來源** | **訊號類型**  | **說明** | 
|-------------|----------------|-------------|
| 服務健康情況 | 活動記錄檔  | 不支援。 請參閱[建立服務通知的活動記錄警示](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)。  |
| Application Insights | Web 可用性測試 | 不支援。 請參閱 [Web 測試警示](../../azure-monitor/app/monitor-web-app-availability.md)。 可供任何經檢測可傳送資料給 Application Insights 的網站使用。 當網站的可用性或回應能力低於預期時收到通知。 |

## <a name="manage-alerts"></a>管理警示
您可以設定警示的狀態來指定警示在解決流程中的位置。 當符合警示規則中指定的準則時，會建立或引發警示，該警示具有 [新] 狀態。 當您認可警示並將它關閉時，您可以變更狀態。 任何狀態變更都會儲存在警示的記錄中。

支援下列警示狀態：

| State | 描述 |
|:---|:---|
| 新增 | 已經偵測到問題，但尚未檢閱。 |
| 已認可 | 系統管理員已檢閱警示，且已開始處理。 |
| 關閉 | 已解決問題。 關閉警示之後，您可以將警示變更為另一個狀態以重新開啟它。 |

**警示狀態**與**監視條件**不同且無關。 警示狀態是由使用者所設定的。 監視條檢是由系統所設定的。 當引發警示時，警示的監視條件會設定為「已引發」。 當導致引發警示的根本條件清除時，監視條件就會設定為「已解決」。 警示狀態需等到使用者變更它之後才會變更。 了解[如何變更警示與智慧群組的狀態](https://aka.ms/managing-alert-smart-group-states)。

## <a name="smart-groups"></a>智慧群組 
智慧群組目前為預覽階段。 

智慧群組是以機器學習演算法為基礎的警示彙總，有助於減少警示干擾及協助疑難排解。 [深入了解智慧群組](https://aka.ms/smart-groups)和[如何管理智慧群組](https://aka.ms/managing-smart-groups)。


## <a name="alerts-experience"></a>警示體驗 
對於特定時間內建立的警示，預設的 [警示] 頁面提供警示的摘要。 它會顯示每個嚴重性的警示總計，且有欄顯示每個嚴重性和每個狀態的警示總數。 選取任何嚴重性以開啟依照該嚴重性篩選的 [所有警示](#all-alerts-page) 頁面。

它不會顯示或追蹤舊版[傳統警示](#classic-alerts)。 您可以變更訂用帳戶或篩選參數來更新頁面。 

![警示頁面](media/alerts-overview/alerts-page.png)

您可以選取頁面頂端下拉式功能表中的值來篩選此檢視。

| 欄 | 描述 |
|:---|:---|
| 訂用帳戶 | 選取最多五個 Azure 訂用帳戶。 檢視僅會包含所選訂用帳戶中出現的警示。 |
| 資源群組 | 選取單一資源群組。 檢視僅會包含所選資源群組中具有目標的警示。 |
| 時間範圍 | 只有在所選時間範圍內引發的警示才會包含在檢視中。 支援的值為過去 1 小時、過去 24 小時、過去 7 天和過去 30 天。 |

選取 [警示] 頁面頂端的下列值以開啟另一個頁面。

| Value | 描述 |
|:---|:---|
| 警示總計 | 符合所選準則的警示總數。 選取此值以開啟沒有任何篩選的 [所有警示] 檢視。 |
| 智慧群組 | 從符合所選準則之警示建立的智慧群組總數。 選取此值將開啟 [所有警示] 檢視中的智慧群組清單。
| 警示規則總計 | 所選訂用帳戶和資源群組中的警示規則總數。 選取此值以開啟在選取的訂用帳戶與資源群組上篩選的 [規則] 檢視。


## <a name="manage-alert-rules"></a>管理警示規則
按一下 [管理警示規則] 以顯示 [規則]頁面。 [規則] 是可管理各個 Azure 訂用帳戶之所有警示規則的單一位置。 它會列出所有警示規則，並可根據目標資源、資源群組、規則名稱或狀態來排序。 您也可以從這個頁面編輯、啟用或停用警示規則。  

 ![警示規則](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>建立警示規則
不論監視服務或訊號類型為何，都能以一致的方式編寫警示。 所有引發的警示和相關的詳細資料都在單一頁面中提供。
 
您可以使用下列三個步驟建立新的警示規則：
1. 挑選警示的_目標_。
1. 從目標的可用訊號中選取_訊號_。
1. 從訊號指定套用於資料的_邏輯_。
 
這個簡化的編寫程序讓您不再需要先知道監視來源或支援的訊號，就能選取 Azure 資源。 可用訊號的清單會自動根據您選取的目標資源進行篩選。 此外，也會根據該目標，自動逐步引導您定義警示規則的邏輯。  

您可以參閱[使用 Azure 監視器來建立、檢視及管理警示](../../azure-monitor/platform/alerts-metric.md)，深入了解如何建立警示規則。

警示可跨數個 Azure 監視服務使用。 如需如何和何時使用每個服務的資訊，請參閱[監視 Azure 應用程式和資源](../../azure-monitor/overview.md)。 


## <a name="all-alerts-page"></a>[所有警示] 頁面 
按一下 [警示總計] 可查看所有警示頁面。 您可在此處檢視在所選時間範圍內建立的警示清單。 您可以檢視個別警示的清單，或包含警示的智慧群組清單。 選取頁面頂端的橫幅以切換檢視。

![[所有警示] 頁面](media/alerts-overview/all-alerts-page.png)

您可以透過在頁面頂端的下拉式功能表中選取下列值來篩選檢視。

| 欄 | 描述 |
|:---|:---|
| 訂用帳戶 | 選取最多五個 Azure 訂用帳戶。 檢視僅會包含所選訂用帳戶中出現的警示。 |
| 資源群組 | 選取單一資源群組。 檢視僅會包含所選資源群組中具有目標的警示。 |
| 資源類型 | 選取一個或多個資源類型。 檢視僅會包含所選類型目標之具目標的警示。 指定資源群組之後，才可使用此欄。 |
| 資源 | 選取資源。 只有以該資源作為目標的警示才會包含在檢視中。 指定資源類型之後，才可使用此欄。 |
| 严重性 | 選取警示嚴重性，或選取 [所有] 以包含所有嚴重性的警示。 |
| 監視器條件 | 選取監視器條件，或選取 [所有] 以包含條件的警示。 |
| 警示狀態 | 選取警示狀態，或選取 [所有] 以包含狀態的警示。 |
| 監視器服務 | 選取服務，或選取 [所有] 以包含所有服務。 只會包含由使用服務作為目標之規則所建立的警示。 |
| 時間範圍 | 只有在所選時間範圍內引發的警示才會包含在檢視中。 支援的值為過去 1 小時、過去 24 小時、過去 7 天和過去 30 天。 |

選取頁面頂端的 [欄] 以選取要顯示的欄。 

## <a name="alert-detail-page"></a>警示詳細資料頁面
當您選取警示時，隨即顯示 [警示詳細資料] 頁面。 它會提供警示的詳細資料，且可讓您變更其狀態。

![警示詳細資料](media/alerts-overview/alert-detail2.png)

[警示詳細資料] 頁面包含下列區段。

| 區段 | 描述 |
|:---|:---|
| 基本資訊 | 顯示警示的內容和其他重要資訊。 |
| 歷程記錄 | 列出警示採取的每個動作，以及對警示所做的任何變更。 目前僅限於狀態變更。 |
| 智慧群組 | 內含警示之智慧群組的相關資訊。 「警示計數」是指智慧群組中包含的警示數目。 包括相同智慧群組中過去 30 天內所建立的其他警示，不論警示清單頁面中所設定的時間篩選條件為何。 選取警示以檢視其詳細資料。 |
| 其他詳細資訊 | 對於建立警示的來源類型一般特定的警示，顯示其他內容相關資訊。 |


## <a name="classic-alerts"></a>傳統警示 

2018 年 6 月之前的 Azure 監視器計量和活動記錄警示功能稱為「警示 (傳統)」。 

如需詳細資訊，請參閱[傳統警示](./../../azure-monitor/platform/alerts-classic.overview.md)


## <a name="next-steps"></a>後續步驟

- [深入了解智慧群組](https://aka.ms/smart-groups)
- [深入了解動作群組](../../azure-monitor/platform/action-groups.md)
- [在 Azure 中管理警示](https://aka.ms/managing-alert-instances)
- [管理智慧群組](https://aka.ms/managing-smart-groups)






