---
title: 快速入門：辨識語音，Java (Android) - 語音服務
titleSuffix: Azure Cognitive Services
description: 了解如何使用語音 SDK 在 Android 上以 Java 辨識語音
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: dabae62b9e308bc97850ee36fa735b8a11c4d6f0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020974"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>快速入門：使用語音 SDK 在 Android 上以 Java 辨識語音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

在本文中，您將了解如何使用認知服務語音 SDK 將語音轉譯成文字，以開發適用於 Android 的 Java 應用程式。
應用程式以語音 SDK Maven 套件 1.5.0 版和 Android Studio 3.3 為基礎。
語音 SDK 目前與使用 32/64 位元 ARM 和 Intel x86/x64 相容處理器的 Android 裝置相容。

> [!NOTE]
> 針對語音裝置 SDK 和 Roobo 裝置，請參閱[語音裝置 SDK](speech-devices-sdk.md)。

## <a name="prerequisites"></a>必要條件

您需要語音服務訂用帳戶金鑰，才能完成本快速入門。 您可以免費取得一個金鑰。 如需詳細資訊，請參閱[免費試用語音服務](get-started.md)。

## <a name="create-and-configure-a-project"></a>建立和設定專案

1. 啟動 Android Studio，然後在 [歡迎使用] 視窗中選擇 [開始新的 Android Studio 專案]。

    ![Android Studio 歡迎使用視窗的螢幕擷取畫面](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. 在 [選擇您的專案] 精靈出現時，選取活動選取方塊中的 [手機和平板電腦] 和 [空白活動]。 選取 [下一步] 。

   ![選擇專案精靈的螢幕擷取畫面](media/sdk/qs-java-android-02-target-android-devices.png)

1. 在 [設定您的專案] 畫面中，輸入 **Quickstart** 作為**名稱**，並輸入 **samples.speech.cognitiveservices.microsoft.com** 作為**套件名稱**，然後選擇專案目錄。 針對 [最低 API 層級]，選擇 **[API 23：Android 6.0 (Marshmallow)]**，並將所有其他核取方塊保留為未核取，然後選取 [完成]。

   ![設定專案精靈的螢幕擷取畫面](media/sdk/qs-java-android-03-create-android-project.png)

Android Studio 需要一些時間來準備您新的 Android 專案。 接著，請設定專案以了解語音 SDK 並使用 Java 8。

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

認知服務語音 SDK 目前的版本為 `1.5.0`。

適用於 Android 的語音 SDK 會封裝成 [AAR (Android 程式庫)](https://developer.android.com/studio/projects/android-library) \(英文\)，其中包含必要的程式庫及所需的 Android 權限。
它會裝載在位於 https:\//csspeechstorage.blob.core.windows.net/maven/ 的 Maven 存放庫中。

將您的專案設定為使用語音 SDK。 從 Android Studio 功能表列中選擇 [檔案] > [專案結構]，以開啟 [專案結構] 視窗。 在 [專案結構] 視窗中，進行下列變更：

1. 在位於視窗左側的清單中，選取 [專案]。 在單引號中附加逗號和我們的 Maven 存放庫 URL，以編輯 [預設程式庫存放庫]。 'https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![專案結構視窗的螢幕擷取畫面](media/sdk/qs-java-android-06-add-maven-repository.png)

1. 在相同畫面中的左側，選取 [應用程式]。 然後，選取視窗頂端的 [相依性] 索引標籤。 選取綠色加號 (+)，然後從下拉式功能表中選擇 [程式庫相依性]。

   ![專案結構視窗的螢幕擷取畫面](media/sdk/qs-java-android-07-add-module-dependency.png)

1. 在出現的視窗中，輸入適用於 Android 之語音 SDK 的名稱和版本 `com.microsoft.cognitiveservices.speech:client-sdk:1.5.0`。 然後選取 [確定]。
   語音 SDK 現在應該會加入至相依性的清單，如下所示：

   ![專案結構視窗的螢幕擷取畫面](media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. 選取 [屬性] 索引標籤。針對 [來源相容性] 和 [目標相容性] 兩者，都選取 [1.8]。

   ![](media/sdk/qs-java-android-09-dependency-added.png)

1. 選取 [確定] 以關閉 [專案結構] 視窗，並將您的變更套用至專案。

## <a name="create-user-interface"></a>建立使用者介面

我們將建立應用程式的基本使用者介面。 編輯您主要活動的版面配置 `activity_main.xml`。 最初，版面配置會包含具有您應用程式名稱的標題列，以及包含 "Hello World!" 文字的 TextView。

* 按一下 TextView 元素。 在 `hello` 的右上角，變更其 ID 屬性。

* 從 `activity_main.xml` 視窗左上方的 [調色盤] 中，將按鈕拖曳至文字上方的空白處。

* 在右側按鈕的屬性中，針對 `onClick` 屬性的值，輸入 `onSpeechButtonClicked`。 我們將以此名稱撰寫用來處理按鈕事件的方法。  在 `button` 的右上角，變更其 ID 屬性。

* 使用設計工具頂端的魔術棒圖示，推斷版面配置條件約束。

  ![魔術棒圖示的螢幕擷取畫面](media/sdk/qs-java-android-10-infer-layout-constraints.png)

UI 的文字和圖形化表示法現在應會顯示如下：

![](media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>新增範例程式碼

1. 開啟來源檔案 `MainActivity.java`。 以下列程式碼取代此檔案中的所有程式碼。

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` 方法包含要求麥克風和網際網路權限，以及初始化原生平台繫結的程式碼。 原生平台繫結只需要設定一次。 此設定應該在應用程式初始化期間即已完成。

   * 如先前所述，方法 `onSpeechButtonClicked` 是按鈕點擊處理常式。 按下按鈕就會觸發語音轉換文字的轉譯。

1. 在相同檔案中，以您的訂用帳戶金鑰取代 `YourSubscriptionKey` 字串。

1. 同時以與您的訂用帳戶 (例如，免費試用訂用帳戶的 `westus`) 相關聯的[區域](regions.md)取代 `YourServiceRegion` 字串。

## <a name="build-and-run-the-app"></a>建置並執行應用程式

1. 將 Android 裝置連接到開發電腦。 請確定您已在裝置上啟用[開發模式和 USB 偵錯](https://developer.android.com/studio/debug/dev-options)。

1. 若要建置應用程式，請按 Ctrl+F9，或從功能表列中選擇 [建置] > [建立專案]。

1. 若要啟動應用程式，請按 Shift+F10，或選擇 [執行] > [執行應用程式]。

1. 在出現的 [部署目標] 視窗中，選擇您的 Android 裝置。

   ![選取部署目標視窗的螢幕擷取畫面](media/sdk/qs-java-android-12-deploy.png)

按下應用程式中的按鈕，開始使用 [語音辨識] 區段。 接下來 15 秒的英文語音會傳送到語音服務，並進行轉譯。 結果會出現在 Android 應用程式中，以及 Android Studio 的 Logcat 視窗中。

![Android 應用程式的螢幕擷取畫面](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [探索 GitHub 上的 Java 範例](https://aka.ms/csspeech/samples) \(英文\)

## <a name="see-also"></a>另請參閱

- [自訂原音模型](how-to-customize-acoustic-models.md)
- [自訂語言模型](how-to-customize-language-model.md)
