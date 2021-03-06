---
title: 建立 Azure IoT Central 應用程式 | Microsoft Docs
description: 建立新的 Azure IoT Central 應用程式。 使用應用程式範本建立試用版或隨用隨付的應用程式。
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 6b53f091b63c79493f6bed78e111b50d33adbf13
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442186"
---
# <a name="create-an-azure-iot-central-application"></a>建立 Azure IoT Central 應用程式

「建置者」可使用 Azure IoT Central UI 來定義 Microsoft Azure IoT Central 應用程式。 本快速入門說明如何建立包含範例_裝置範本_和模擬_裝置_的 Azure IoT Central 應用程式。

## <a name="create-an-application"></a>建立應用程式

瀏覽至 Azure IoT Central 的[應用程式管理員](https://aka.ms/iotcentral)頁面。 您必須使用 Microsoft 的個人、公司或學校帳戶登入。

若要開始建立新的 Azure IoT Central 應用程式，請選取 [新增應用程式]。 您將會進入 [建立應用程式] 頁面。

![Azure IoT Central 的建立應用程式頁面](media/quick-deploy-iot-central/iotcentralcreate.png)

若要建立新的 Azure IoT Central 應用程式：

1. 選擇付款方案：
   - **試用版**應用程式可以免費試用 7 天。 此類應用程式可在到期前隨時轉換成預付型方案。 如果您建立**試用版**應用程式，則必須輸入連絡資訊，並選擇是否要收到來自 Microsoft 的資訊和提示。
   - **預付型方案**應用程式會依據裝置收費，前 5 個裝置免費。 如果您建立**隨用隨付**應用程式，則必須選取*目錄*、*Azure 訂用帳戶*和*區域*：
      - *目錄*是用來建立應用程式的 Azure Active Directory (AD)。 其中包含使用者識別、認證和其他組織資訊。 如果您沒有 Azure AD，系統會在您建立 Azure 訂用帳戶時為您建立。
      - *Azure 訂用帳戶*可讓您建立 Azure 服務執行個體。 IoT Central 會在您的訂用帳戶中佈建資源。 如果您沒有 Azure 訂用帳戶，您可以在 [Azure 註冊頁面](https://aka.ms/createazuresubscription)上建立一個。 建立 Azure 訂用帳戶之後，請瀏覽回到 [建立應用程式] 頁面。 新的訂用帳戶會出現在 [Azure 訂用帳戶] 下拉式清單中。
      - *區域*是您要在其中建立應用程式的實體位置。 一般而言，您應該選擇實際上最接近裝置的區域，以獲得最佳效能。您可以在 [依區域提供的產品](https://azure.microsoft.com/regions/services/) 頁面上檢視可使用 Azure IoT Central 的區域。 選擇區域後，即無法將應用程式移到不同的區域。

      在 [Azure IoT Central 定價頁面](https://azure.microsoft.com/pricing/details/iot-central/)上深入了解定價。

1. 選擇易記的應用程式名稱，例如 **Contoso IoT**。 Azure IoT Central 會為您產生唯一的 URL 前置詞。 您可以將其變更為更好記的 URL 前置詞。

1. 選擇應用程式範本。 應用程式範本可包含預先定義的項目 (例如裝置範本和儀表板)，協助您開始使用。

    | 應用程式範本 | 說明 |
    | -------------------- | ----------- |
    | Contoso 範例       | 建立一個應用程式，其中包含已為冷飲自動販賣機建立的裝置範本。 使用此範本來開始探索 Azure IoT 中心。 |
    | Devkits 範例       | 使用您可使用的裝置範本來建立應用程式，以連線 MXChip 或 Raspberry Pi 裝置。 如果您是以其中任一裝置進行試驗的裝置開發人員，請使用此範本。 |
    | 自訂應用程式   | 為您建立空的應用程式，以填入您自己的裝置範本和裝置。 |

1. 選取 [建立] 。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已建立 IoT Central 應用程式。 以下是建議的後續步驟：

> [!div class="nextstepaction"]
> [瀏覽 IoT Central](overview-iot-central-tour.md)
