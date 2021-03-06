---
title: Azure Service Fabric CLI- sfctl rpm| Microsoft Docs
description: 描述 Service Fabric CLI sfctl rpm 命令。
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 04080d75042bfa8a07533336936165e0abef051b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60556350"
---
# <a name="sfctl-rpm"></a>sfctl rpm
查詢命令，並將其傳送至修復管理員服務。

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
| approve-force | 強制核准指定的修復工作。 |
| delete | 刪除已完成的修復工作。 |
| list | 取得符合所指定篩選條件的修復工作清單。 |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm approve-force
強制核准指定的修復工作。

這個 API 支援 Service Fabric 平台；這不表示直接從您的程式碼使用。

### <a name="arguments"></a>引數

|引數|描述|
| --- | --- |
| --task-id [必要] | 修復工作的識別碼。 |
| --version | 修復工作的目前版本號碼。 如果不是零，則只有在此值符合修復工作的實際目前版本時，要求才會成功。 如果是零，則會不執行任何版本檢查。 |

### <a name="global-arguments"></a>全域引數

|引數|描述|
| --- | --- |
| --debug | 增加記錄詳細程度以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細程度。 使用 --debug 為完整偵錯記錄。 |

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
刪除已完成的修復工作。

這個 API 支援 Service Fabric 平台；這不表示直接從您的程式碼使用。

### <a name="arguments"></a>引數

|引數|描述|
| --- | --- |
| --task-id [必要] | 要刪除之已完成修復工作的識別碼。 |
| --version | 修復工作的目前版本號碼。 如果不是零，則只有在此值符合修復工作的實際目前版本時，要求才會成功。 如果是零，則會不執行任何版本檢查。 |

### <a name="global-arguments"></a>全域引數

|引數|描述|
| --- | --- |
| --debug | 增加記錄詳細程度以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細程度。 使用 --debug 為完整偵錯記錄。 |

## <a name="sfctl-rpm-list"></a>sfctl rpm list
取得符合所指定篩選條件的修復工作清單。

這個 API 支援 Service Fabric 平台；這不表示直接從您的程式碼使用。

### <a name="arguments"></a>引數

|引數|描述|
| --- | --- |
| --executor-filter | 其宣告的工作應該包含在清單中的修復執行程式名稱。 |
| --state-filter | 下列值的位元 OR，指定結果清單中應該包含哪些工作狀態。 <br> 1 - 已建立 <br>2 - 已宣告  <br>4 - 正在準備  <br>8 - 已核准  <br>16 - 正在執行  <br>32 - 正在還原  <br>64 - 已完成 |
| --task-id-filter | 要比對的修復工作識別碼前置詞。 |

### <a name="global-arguments"></a>全域引數

|引數|描述|
| --- | --- |
| --debug | 增加記錄詳細程度以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細程度。 使用 --debug 為完整偵錯記錄。 |


## <a name="next-steps"></a>後續步驟
- [設定](service-fabric-cli.md) Service Fabric CLI。
- 了解如何使用[範例指令碼](/azure/service-fabric/scripts/sfctl-upgrade-application)來使用 Service Fabric CLI。