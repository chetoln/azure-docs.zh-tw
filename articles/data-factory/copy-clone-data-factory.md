---
title: 複製或重製 Azure Data Factory 中的資料處理站 | Microsoft Docs
description: 了解如何複製或重製 Azure Data Factory 中的資料處理站
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 96ea8142e2f7794d3c15c6efb436eafa585bc8fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60780918"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>如何複製或重製 Azure Data Factory 中的資料處理站

此文章說明如何複製或重製 Azure Data Factory 中的資料處理站。

## <a name="use-cases-for-cloning-a-data-factory"></a>重製資料處理站的使用案例

以下是一些您在複製或重製資料處理站時可能會發現很實用的情況：

-   **重新命名資源**。 Azure 不支援重新命名資源。 若要重新命名資料處理站，您可以使用不同的名稱重製資料處理站，然後刪除現有的資料處理站。

-   **針對變更進行偵錯** (當偵錯功能不敷使用時)。 有時候，若要測試您的變更，您可以在不同的處理站測試您的變更，然後再將它們套用到您的主處理站。 在大部分情況中，您都可以使用「偵錯」。 不過，觸發程序中的變更 (例如您的變更在自動叫用觸發程序時的行為方式或您的變更在一段期間內的行為) 可能無法輕鬆地在未簽入的情況下測試。 在這些案例中，重製處理站並在該處套用您的變更非常有意義。 由於 Azure Data Factory 主要依回合數目計費，第二個處理站不會導致產生任何額外的費用。

## <a name="how-to-clone-a-data-factory"></a>如何重製資料處理站

1. Azure 入口網站中的 Data Factory UI 可讓您隨著可讓您定義任何想要在重製您的資料處理站時變更之值的參數檔案將您整個資料處理站的承載匯出到 Resource Manager 範本。

1. 前提是您必須從 Azure 入口網站建立您的目標資料處理站。

1. 如果您將 SelfHosted IntegrationRuntime 在您的來源處理站時，您需要先它建立具有相同名稱的目標處理站中。 如果您想要共用 SelfHosted IRs 之間不同的處理站，您可以使用已發佈的模式[此處](author-visually.md#best-practices-for-git-integration)。

1. 若您處於 GIT 模式，每次您從入口網站發行時，處理站的 Resource Manager 範本都會儲存到 GIT 中 (存放庫的 adf_publish 分支)。

1. 針對其他案例，您可以按一下入口網站中的 [匯出 Resource Manager 範本] 按鈕，以下載 Resource Manager 範本。

1. 下載 Resource Manager 範本之後，您可以透過標準 Resource Manager 範本部署方法來部署它。

1. 基於安全性理由，產生的 Resource Manager 範本不包含任何祕密資訊，例如連結服務的密碼。 因此，您必須以部署參數方式提供這些密碼。 若不想要提供參數，您必須從 Azure Key Vault 取得連結服務的連接字串與密碼。

## <a name="next-steps"></a>後續步驟

檢閱[使用Azure Data Factory UI 建立資料處理站](quickstart-create-data-factory-portal.md)以取得在 Azure 入口網站中建立資料處理站的指導方針。
