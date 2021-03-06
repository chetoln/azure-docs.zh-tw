---
title: 在 Azure 上執行 SAP Hana (大型執行個體) 的作業系統升級 | Microsoft Docs
description: 在 Azure 上執行 SAP Hana on Azure (大型執行個體) 的作業系統升級
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7d451f3831309b4755170915b35a23da8910510
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60935631"
---
# <a name="operating-system-upgrade"></a>作業系統升級
本文件會詳述 Hana 大型執行個體作業系統升級的詳細資訊。

>[!NOTE]
>作業系統升級是客戶的責任，Microsoft 作業支援可以指引您在升級期間應特別注意的關鍵區域。 規劃升級之前，請先諮詢您的作業系統廠商。

佈建 HLI 單元時，Microsoft 作業小組即會安裝作業系統。 隨時間過去，會需要維護 HLI 單元上的作業系統 (例如：修補、調整、升級等)。

在您進行作業系統的重大變更之前 (例如，將 SP1 升級為 SP2)，請務必建立請求諮詢用的支援票證，藉以連絡 Microsoft 作業小組。

請在您的票證上註明：

* 您的 HLI 訂用帳戶識別碼。
* 您的伺服器名稱。
* 您打算套用的修補等級。
* 您打算進行變更的日期。 

建議您在合適的升級日期之前，開啟此票證至少一周，因為作業小組會檢查您的伺服器刀鋒視窗上是否需要進行韌體升級。


如需具有不同 Linux 版本之 SAP HANA 版本的支援矩陣，請參閱 [SAP 附註 #2235581](https://launchpad.support.sap.com/#/notes/2235581)。


## <a name="known-issues"></a>已知問題

以下是升級時一些已知的常見問題：
- II 型 SKU 類別的 SKU 進行作業系統升級之後，會移除 Software Foundation 軟體 (SFS)。 您必須在作業系統升級後重新安裝相容的 SFS。
- 乙太網路卡驅動程式 (ENIC 和 FNIC) 會復原至舊版本。 升級後必須重新安裝相容的驅動程式版本。

## <a name="next-steps"></a>後續步驟
- 請參閱類型 I 的 SKU 類別作業系統的[備份和還原](hana-overview-high-availability-disaster-recovery.md)。
- 請參閱類型 II 的 SKU 類別的[類型 II SKU 作業系統備份](os-backup-type-ii-skus.md)。
