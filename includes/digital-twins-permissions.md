---
title: 包含檔案
description: 包含檔案
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 12/17/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: e8027e16cc1f58fbadbb35ae241ab29010005586
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2018
ms.locfileid: "53584725"
---
1. 在 [Azure 入口網站](https://portal.azure.com)中，從左側窗格開啟 **Azure Active Directory**，然後開啟 [屬性] 窗格。 將 [目錄識別碼] 複製到暫存檔。 您會使用此值來設定下一節的應用程式範例。

    ![Azure Active Directory 目錄識別碼](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. 開啟 [應用程式註冊] 窗格，然後選取 [新增應用程式註冊] 按鈕。

    ![應用程式註冊窗格](./media/digital-twins-permissions/aad-app-reg-start.png)

1. 在 [名稱] 方塊中為此應用程式註冊提供易記的名稱。 將 [應用程式類型] 選擇為 [原生]，然後將 [重新導向 URI] 選擇為 `https://microsoft.com`。 選取 [建立] 。

    ![建立窗格](./media/digital-twins-permissions/aad-app-reg-create.png)

1. 開啟已註冊的應用程式，並將 [應用程式識別碼] 欄位的值複製到暫存檔。 這個值可識別 Azure Active Directory 應用程式。 您會使用應用程式識別碼來設定後續章節的應用程式範例。

    ![Azure Active Directory 應用程式識別碼](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. 開啟應用程式註冊窗格。 選取 [設定] > [必要權限]，然後：

   a. 選取左上方的 [新增] 來開啟 [新增 API 存取權] 窗格。

   b. 選取 [選取 API]，並搜尋 **Azure Digital Twins**。 如果搜尋沒有找到 API，請改為搜尋 **Azure 智慧空間**。

   c. 選取 [Azure Digital Twins (Azure 智慧空間服務)] 選項，然後選擇 [選取]。

   d. 選擇 [選取權限]。 選取 [讀取/寫入權限] 委派權限核取方塊，然後選擇 [選取]。

   e. 在 [新增 API 存取權] 窗格中，選取 [完成]。

   f. 在 [所需的權限] 窗格中，選取 [授與權限] 按鈕，然後接受所出現的通知。

      ![必要權限窗格](./media/digital-twins-permissions/aad-app-req-permissions.png)
