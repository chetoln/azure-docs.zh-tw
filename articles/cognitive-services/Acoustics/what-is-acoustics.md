---
title: 聲場專案概觀
titlesuffix: Azure Cognitive Services
description: 聲場專案是一種聲場引擎，可提供 3D 互動式體驗，整合聲場模擬波形物理特性模擬與互動式設計控制項。
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1fc125322b83a0eb51095fac21dee05c7ffb39c1
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313612"
---
# <a name="what-is-project-acoustics"></a>何謂 Project Acoustics？
聲場專案是可提供 3D 互動式體驗的聲場引擎。 它可模擬複雜場景中的繞射、傳送及殘響等聲波效果，完全不需要手動加上區域標記。 它也包含遊戲引擎和音訊中介軟體整合。 聲場專案的原理類似於靜態光照：離線詳細模擬物理特性以提供實際基準，並透過清楚表達的設計控制項，使用輕量的執行階段，實現您的藝術目標。

![顯示聲場體素的 Gears of War 4 螢幕擷取畫面](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>使用互動式聲場的波形物理特性
聲線聲場方法可使用單一音源對聆聽者的聲線投射，來檢查是否遭到阻隔，或透過使用一些聲線評估本機場景音量來促進殘響。 但這些技術並不可靠，因為小卵石與大圓石的阻隔程度相當。 聲線並不能說明聲音環繞物件的方式 (稱為繞射現象)。 聲場專案模擬使用波形模擬來獲得這些效果。 結果不僅更可預測，也更加可靠。

結合聲場模擬與傳統音效設計概念可說是聲場專案的重大創新。 它可將模擬結果轉譯為傳統的音訊 DSP參數，用於阻隔、傳送及殘響。 設計工具會透過此轉譯程序來使用控制項。 如需聲場專案背後的核心技術詳細資訊，請瀏覽[研究專案頁面](https://www.microsoft.com/en-us/research/project/project-triton/) \(英文\)。

![透過場景顯示聲波傳播水平 2D 配量的動畫](media/wave-simulation.gif)

## <a name="setup"></a>設定
[聲場專案 Unity 整合](unity-integration.md)採取拖放式作業，並包含 Unity 音訊引擎外掛程式。 透過在每個音訊物件附加聲場專案 C# 控制項元件，來加強 Unity 音訊來源控制項。

[聲場專案 Unreal 整合](unreal-integration.md)包含 Unreal 的編輯器與遊戲外掛程式，以及 Wwise 混音器外掛程式。 自訂的音訊元件可透過即時音場設計控制項，擴充 Unreal 內眾所熟悉的 Wwise 功能。 設計控制項也會在 Wwise 混音器外掛程式公開。

## <a name="workflow"></a>工作流程
* **預先模擬聲場：** 一開始先選取聲場的幾何回應　(例如忽略光束)，來設定聲場模擬。 接著，編輯自動指派的材質，然後選取要引導聆聽者取樣的導覽區域。 完全不必手動對殘響/傳送/空間區域加上標記。
* **聲場模擬：** 在本機執行的分析步驟，根據上述的選取範圍針對場景執行體素化或其他幾何分析。 在編輯器中將結果視覺化，以驗證場景設定。 在聲場模擬提交時，您會將體素資料送往 Azure，並取回聲場遊戲資產。
* **執行階段：** 在將資產載入您的層級後，便已準備好可以在您的層級接聽聲場。 根據原始程式碼控制細微調整，在編輯器中即時設計聲場。 您也可以從層級指令碼來驅動控制項。

## <a name="platforms"></a>平台
目前可將聲場專案執行階段外掛程式部署至下列平台：
* Windows
* Android
* Xbox One

## <a name="download"></a>下載
* [聲場專案 Unity 外掛程式與範例](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [聲場專案 Unreal 和 Wwise 外掛程式與範例](https://www.microsoft.com/download/details.aspx?id=58090)
  * 如需 Xbox 二進位檔與支援，請透過下面的「註冊」表單，與我們連絡

## <a name="contact-us"></a>與我們連絡
* [聲場專案論壇](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics)
* [註冊以接收聲場專案更新](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>後續步驟
* 試用 [Unreal](unreal-quickstart.md) 或 [Unity 聲場專案快速入門](unity-quickstart.md)
* 探索[聲場專案的音效設計原理](design-process.md)

