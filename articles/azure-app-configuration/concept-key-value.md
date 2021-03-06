---
title: Azure 應用程式設定索引鍵/值存放區 | Microsoft Docs
description: 概略說明設定資料如何儲存在 Azure 應用程式設定中
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 4c741bb86242abfb03d01c902dbaa84d83491dd9
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408733"
---
# <a name="keys-and-values"></a>索引鍵和值

Azure 應用程式設定會將設定資料儲存為索引鍵/值組。 索引鍵/值組以簡單卻彈性的方式，呈現開發人員熟悉的各種應用程式設定。

## <a name="keys"></a>金鑰

索引鍵會作為索引鍵/值組的名稱，並用來儲存和擷取對應的值。 常見的方法是使用字元分隔符號 (例如 `/` 或 `:`)，來將索引鍵組織到階層命名空間。 使用最適合您應用程式的慣例。 應用程式設定會將索引鍵視為一個整體。 其不會剖析索引鍵，以找出索引鍵名稱的結構，或對索引鍵強制執行任何規則。

若設定存放區在應用程式架構內，則其使用方式可能會要求特定的索引鍵值命名配置。 例如，Java 的 Spring Cloud 架構會定義 `Environment` 資源，以提供 Spring 應用程式的設定，讓其可透過變數 (包括應用程式名稱和設定檔) 來設置參數。 Spring Cloud 相關設定資料的索引鍵通常會以這兩個元素開頭，並以分隔符號隔開。

儲存在應用程式設定中的索引鍵是 Unicode 字串 (需區分大小寫)。 在應用程式設定存放區中，索引鍵 app1 和 App1 是不同的。 當您在應用程式內使用組態設定時，務必記得這一點，因為部分架構會以不區分大小寫的方式處理設定索引鍵。 例如，ASP.NET Core 設定系統會將索引鍵視為不區分大小寫的字串。 當您在 ASP.NET Core 應用程式內查詢應用程式設定時，若要避免無法預期的行為，請不要使用只有大小寫不同的索引鍵。

您可以在輸入到應用程式設定的索引鍵名稱中使用任何 Unicode 字元，但 `*`、`,` 和 `\` 除外。 這些都是保留字元。 如果您需要包含保留字元，您必須使用 `\{Reserved Character}` 來將其逸出。 索引鍵/值組的合併大小限制為 10,000 個字元。 此限制包括索引鍵中的所有字元、索引鍵的值及所有相關聯的選擇性屬性。 在此限制中，您可以有許多索引鍵階層層級。

### <a name="design-key-namespaces"></a>設計索引鍵命名空間

命名設定資料所使用的索引鍵一般有兩種方法：平面式或階層式。 從應用程式使用方式的角度來看，這些方法會很類似，但階層式命名可提供許多優點：

* 易於閱讀。 比起一長串的字元，階層式索引鍵名稱中的分隔符號可作為句子中的空格。 還會在文字之間提供自然的中斷點。
* 易於管理。 索引鍵名稱的階層表示設定資料的邏輯群組。
* 易於使用。 您可以更輕鬆地撰寫查詢，以透過模式來比對階層式結構中的索引鍵，並只擷取一部分的設定資料。 此外，許多較新的程式設計架構都有階層式設定資料的原生支援，可讓您的應用程式使用特定設定集。

您可以使用許多方式來將應用程式設定中的索引鍵組織成階層。 這類索引鍵可作為 [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)。 每個階層式索引鍵都是「路徑」資源，由一個或多個以分隔符號聯結的元件組成。 請根據應用程式、程式設計語言或架構需求，來選擇要當作分隔符號使用的字元。 針對應用程式設定中的不同索引鍵，使用多個分隔符號。

以下是如何將索引鍵名稱建構至階層的幾個範例：

* 以元件服務為基礎

        AppName:Service1:ApiEndpoint
        AppName:Service2:ApiEndpoint

* 以部署區域為基礎

        AppName:Region1:DbEndpoint
        AppName:Region2:DbEndpoint

### <a name="label-keys"></a>標籤索引鍵

應用程式設定中的索引鍵值可選擇性地使用標籤屬性。 標籤會用來區分具有相同索引鍵的索引鍵值。 具有標籤 A 和 B 的索引鍵 app1，會在應用程式設定存放區中形成兩個個別的索引鍵。 根據預設，索引鍵值的標籤會是空的，或是 `null`。

標籤可提供便利的方式來建立索引鍵變體。 標籤的常見用法是為相同索引鍵指定多個環境：

    Key = AppName:DbEndpoint & Label = Test
    Key = AppName:DbEndpoint & Label = Staging
    Key = AppName:DbEndpoint & Label = Production

### <a name="version-key-values"></a>版本索引鍵值

應用程式設定不會在索引鍵值受到修改時自動編排版本。 請使用標籤作為建立多個索引鍵值版本的方式。 例如，您可以在標籤內輸入應用程式版本號碼或 Git 認可識別碼，以識別與特定軟體組建相關聯的索引鍵值。

您可以在標籤中使用任何 Unicode 字元，但 `*`、`,` 及 `\` 除外。 這些都是保留字元。 若要包含保留字元，您必須使用 `\{Reserved Character}` 來將其逸出。

### <a name="query-key-values"></a>查詢索引鍵值

透過索引鍵和標籤 (可以是 `null`)，即可識別每個唯一的索引鍵值。 您可以藉由指定模式來查詢應用程式設定存放區的索引鍵值。 應用程式設定存放區會傳回符合模式的索引鍵值，以及其對應的值和屬性。 在對應用程式設定發出的 REST API 呼叫中，請使用下列索引鍵模式：

| Key | |
|---|---|
| 省略 `key` 或使用 `key=*` | 符合所有索引鍵 |
| `key=abc` | 完全符合索引鍵名稱 **abc** |
| `key=abc*` | 符合以 **abc** 開頭的索引鍵名稱 |
| `key=*abc` | 符合以 **abc** 結尾的索引鍵名稱 |
| `key=*abc*` | 符合包含 **abc** 的索引鍵名稱 |
| `key=abc,xyz` | 符合索引鍵名稱 **abc** 或 **xyz** (限制為五個 CSV) |

您也可以包含下列標籤模式：

| 標籤 | |
|---|---|
| 省略 `label` 或使用 `label=*` | 符合任何標籤，包括 `null` 在內 |
| `label=%00` | 符合 `null` 標籤 |
| `label=1.0.0` | 完全符合標籤 **1.0.0** |
| `label=1.0.*` | 符合以 **1.0.** 開頭的標籤 |
| `label=*.0.0` | 符合以 **.0.0** 結尾的標籤 |
| `label=*.0.*` | 符合包含 **.0.** 的標籤 |
| `label=%00,1.0.0` | 符合標籤 `null` 或 **1.0.1** (限制為五個 CSV) |

## <a name="values"></a>值

指派給索引鍵的值也是 Unicode 字串。 您可以使用所有 Unicode 字元來作為值。 每個值都可以與選擇性的使用者定義內容類型建立關聯。 請使用此屬性來儲存值的相關資訊，例如編碼配置，以協助應用程式正確地處理值。

儲存在應用程式設定存放區的設定資料 (包括所有索引鍵和值)，在待用和傳輸時都會進行加密。 應用程式設定並不是 Azure Key Vault 的替代方案。 請勿在其中儲存應用程式祕密。

## <a name="next-steps"></a>後續步驟

* [時間點快照集](./concept-point-time-snapshot.md)  
* [功能管理](./concept-feature-management.md)  
