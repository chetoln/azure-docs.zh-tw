---
title: 關於 Azure 實驗室服務 | Microsoft Azure
description: 了解實驗室服務如何協助您輕鬆搭配虛擬機器建立、管理及保護實驗室，以供開發人員、測試人員、教育工作者、學生及其他人員使用。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: a4ca5cba924a3269f279469f26e68acdb0ad0659
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257615"
---
# <a name="an-introduction-to-azure-lab-services"></a>Azure 實驗室服務簡介
Azure 實驗室服務可讓您在雲端為團隊快速設定環境 (例如：開發環境、測試環境、教室實驗室環境)。 實驗室擁有者會建立實驗室，佈建 Windows 或 Linux 虛擬機器，安裝必要的軟體和工具，然後將它們提供給實驗室使用者使用。 實驗室使用者會連線至實驗室中的虛擬機器 (VM)，並使用它們來進行日常工作、短期專案，或是教室練習。 當使用者開始運用實驗室中的資源後，實驗室管理員便可以跨多個實驗室分析成本和使用情形，並設定整體性的原則來對組織或團隊的成本進行最佳化。

> [!IMPORTANT]
> 我們正在為 **Azure DevTest Labs** 擴展新的實驗室類型 (Azure 實驗室服務)！
>  
> Azure 實驗室服務可讓您建立受控實驗室類型，例如教室實驗室。 服務本身會處理受控實驗室類型的所有基礎結構管理，從啟動 VM 以處理錯誤，到調整基礎結構的規模。 現在，[DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) 和 [Azure 實驗室服務](https://azure.microsoft.com/services/lab-services/)將繼續成為 Azure 入口網站中的個別服務。 

## <a name="key-capabilities"></a>主要功能

Azure 實驗室服務支援下列主要能力/功能：

- **快速且彈的實驗室設定**。 透過使用 Azure 實驗室服務，實驗室擁有者可以快速地依需求設定實驗室。 該服務提供由系統處理受控實驗室類型的所有 Azure 基礎結構工作的選項，也可以讓實驗室擁有者在實驗室擁有者的訂用帳戶中自行管理及自訂基礎結構。 該服務針對實驗室提供由服務為您管理的內建基礎結構規模調整及復原功能。
- **簡化的實驗室使用者體驗**。 在受控實驗室類型 (例如教室實驗室) 中，實驗室使用者可以使用註冊代碼來註冊至實驗室，並隨時存取該實驗室以使用實驗室的資源。 在於 DevTest Labs 中建立的實驗室中，實驗室擁有者可以給予實驗室使用者建立及存取虛擬機器、管理及重新使用資料磁碟，以及設定可重複使用祕密的權限。  
- **成本最佳化和分析**。 實驗室擁有者可以設定實驗室排程，以自動關閉及啟動虛擬機器。 實驗室擁有者可以設定排程以指定實驗室虛擬機器可供使用者存取的時段、設定每個使用者或實驗室的使用原則來將成本最佳化，以及分析實驗室中的使用情況和活動趨勢。 針對如教室實驗室等的受控實驗室類型，目前有一些成本最佳化和分析選項子集可供使用。
- **內嵌安全性**。 實驗室使用者可以針對實驗室設定私人虛擬網路和子集，並提供共用的公用 IP 位址。 實驗室使用者可以使用已設定 ExpressRoute 或站對站 VPN 的虛擬網路，安全地存取資源。 (目前僅於 DevTest Labs 中提供)
- **與您的工作流程和工具整合**。 Azure 實驗室服務可讓您將實驗室整合至您組織的網站和管理系統。 您可以從您的持續整合/持續部署 (CI/CD) 工具內自動佈建環境。 (目前僅於 DevTest Labs 中提供)

> [!NOTE]
> 目前，Azure 實驗室服務僅支援從 Azure Marketplace 映像建立的 VM。 如果您想要在實驗室環境中使用自訂映像或建立其他 PaaS 資源，請使用 DevTest 實驗室。 如需詳細資訊，請參閱[在 DevTest 實驗室中建立自訂映像](devtest-lab-create-custom-image-from-vm-using-portal.md)及[使用 Resource Manager 範本建立實驗室環境](devtest-lab-create-environment-from-arm.md)。

## <a name="scenarios"></a>案例

以下為一些 Azure 實驗服務所支援的案例：

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>在雲端為您的教室設定可調整大小的電腦實驗室  

- 建立受控教室實驗室。 您只需將自己的需求告訴實驗室服務，它便會為您建立並管理實驗室的基礎結構，讓您可以專注在課堂的課程上，而不需處理實驗室的技術性細節。
- 為學生提供具有虛擬機器的實驗室，且虛擬機器已針對課堂需求來設定。 給予每個學生有限的時間，以使用 VM 來完成課堂作業。  
- 將學校的實體電腦實驗室移至雲端。 以您在實驗室上設定的使用量上限和成本閾值為限，以自動調整 VM 數目。
- 在結束之後，只需按一下便可以刪除實驗室。

### <a name="use-devtest-labs-for-development-environments"></a>針對開發環境使用 DevTest Labs

Azure DevTest Labs 可以用來實作許多重要案例，但是其中一個主要案例是使用 DevTest Labs 來託管開發人員的開發電腦。 在此案例中，DevTest Labs 提供以下優點：

- 可讓開發人員快速視需要佈建開發電腦。
- 使用可重複使用的範本及構件來佈建 Windows 和 Linux 環境。
- 開發人員可以隨時輕鬆自訂他們的開發電腦。
- 系統管理員可以透過確保開發人員無法取得超過其開發需求的 VM 數目，並使 VM 在未使用的情況下關閉來控制成本。

如需詳細資訊，請參閱[將 DevTest Labs 用於開發](devtest-lab-developer-lab.md)。

### <a name="use-devtest-labs-for-test-environments"></a>針對測試環境使用 DevTest Labs

您可以使用 Azure DevTest Labs 實作許多重要案例，但是主要案例要有關使用 DevTest Labs 來託管測試人員的電腦。 在此案例中，DevTest Labs 提供以下優點：

- 測試人員可以利用可重複使用的範本和構件，快速佈建 Windows 和 Linux 環境，來測試其最新版的應用程式。
- 測試人員可以佈建多個測試代理程式，相應增加其負載測試。
- 系統管理員可以透過確保測試人員無法取得超過其測試需求的 VM 數目，並使 VM 在未使用的情況下關閉來控制成本。

如需詳細資訊，請參閱[將 DevTest Labs 用於測試](devtest-lab-test-env.md)。

## <a name="types-of-labs"></a>實驗室類型
您可以建立的實驗室類型有兩種：使用 Azure 實驗室服務的**受控實驗室類型**，以及使用 Azure 實驗室服務的**實驗室**。 如果您要在實驗室中只放置需要的項目，並讓服務設定及管理實驗室所需的基礎結構，請選擇其中一個**受控實驗室類型**。 目前，**教室實驗室**是您可使用 Azure 實驗室服務建立的唯一受控實驗室類型。 如果您要管理自己的基礎結構，請使用 **Azure DevTest Labs** 建立實驗室。

下節提供更多這些實驗室的詳細資料。 

## <a name="managed-lab-types"></a>受控實驗室類型
Azure 實驗室服務可讓您建立基礎結構由 Azure 所管理的實驗室。 此文章將它們稱為受控實驗室類型。 受控實驗室類型提供符合您特定需求的不同實驗室類型。 目前唯一支援的受控實驗室類型是**教室實驗室**。 

受控實驗室類型讓您能夠進行最少的設定便立即開始使用。 服務本身會處理所有實驗室的基礎結構管理，從啟動 VM 來處理錯誤，至縮放基礎結構。 若要建立受控實驗室類型 (例如教室實驗室)，您需要先為您的組織建立實驗室帳戶。 實驗室帳戶是作為中心帳戶，並在其中管理組織內的所有實驗室。 

當您在這些受控實驗室類型中建立和使用 Azure 資源時，服務會在內部 Microsoft 訂用帳戶中建立和管理資源。 它們不是建立在您自己的 Azure 訂用帳戶中。 服務會在內部 Microsoft 訂用帳戶中繼續追蹤這些資源的使用量。 這些使用量會計費回到包含該實驗室帳戶的 Azure 訂用帳戶。   

以下是一些**受控實驗室類型的使用案例**： 

- 為學生提供具有虛擬機器的實驗室，且虛擬機器已針對課堂需求來設定。 提供每個學生使用 VM 處理作業或個人專案的有限時數。
- 設定高效能計算 VM 集區，以執行計算密集研究或圖形密集研究。 視需要執行 VM，並在完成時清除機器。 
- 將學校的實體電腦實驗室移至雲端。 以您在實驗室上設定的使用量上限和成本閾值為限，以自動調整 VM 數目。  
- 為裝載 Hackathon 快速佈建虛擬機器實驗室。 在結束之後，只需按一下便可以刪除實驗室。 


## <a name="devtest-labs"></a>DevTest Labs
在某些情況下您可能想要自行管理所有基礎結構和設定 (在您擁有的訂用帳戶內)。 若要這麼做，您可以在 Azure 入口網站中使用 Azure DevTest Labs 建立實驗室。 對於這些實驗室，您不需要建立實驗室帳戶。 這些實驗室不會顯示在實驗室帳戶 (為受控實驗室類型而存在) 中。  

以下是一些**使用 DevTest Labs 的使用案例**： 

- 快速佈建用於裝載 Hackathon 或會議實作課程的虛擬機器實驗室。 在結束之後，只需按一下便可以刪除實驗室。 
- 建立設定好您應用程式的 VM 集區，並讓您的小組輕鬆使用虛擬機器進行群眾挑錯。  
- 提供包含開發人員所需所有工具的虛擬機器給他們。 排定自動啟動和關機，以將成本降到最低。 
- 重複建立測試機器作為您部署的一部分。 測試您最新的位元，並於完成時清除測試機器。 
- 設定各種不同組態的虛擬機器和多個測試代理程式，以進行縮放和效能測試。 
- 使用以您的最新版產品設定的實驗室，為您的客戶提供訓練課程。 提供每位客戶使用實驗室的有限時數。 


## <a name="managed-lab-types-vs-devtest-labs"></a>受控實驗室類型與DevTest Labs
下表比較 Azure 實驗室服務支援的兩種實驗室類型： 

| 特性 | 受控實驗室類型 | DevTest Labs |
| -------- | ----------------- | ---------- |
| 在實驗室中管理 Azure 基礎結構。 |  由服務自動管理 | 您自行管理  |
| 基礎結構問題的內建恢復 | 由服務自動處理 | 您自行管理  |
| 訂用帳戶管理 | 服務處理支援服務的 Microsoft 訂用帳戶內的資源配置。 由服務自動處理縮放。 | 您使用自己的 Azure 訂用帳戶自行管理。 不會自動縮放訂用帳戶。 |
| 實驗室內的 Azure Resource Manager 部署 | 尚未提供 | 可用 |

## <a name="next-steps"></a>後續步驟

請參閱下列文章： 

- [關於教室實驗室](./classroom-labs/classroom-labs-overview.md)
- [關於 DevTest Labs](devtest-lab-overview.md)
