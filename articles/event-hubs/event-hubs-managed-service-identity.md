---
title: Azure 資源的受控識別 - Azure 事件中樞 | Microsoft Docs
description: 本文提供有關如何將 Azure 資源的受控識別與「Azure 事件中樞」搭配使用的資訊
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 784d8c9280aeff7224f90ecee0b16c9c30381aeb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746880"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Azure 資源的受控識別與事件中樞

[Azure 資源的受控識別](../active-directory/managed-identities-azure-resources/overview.md)是一個跨 Azure 功能，可讓您建立與應用程式程式碼執行所在之部署相關聯的安全識別。 然後您可以將該識別與存取控制角色產生關連，該角色會授與用來存取應用程式所需之特定 Azure 資源的自訂權限。 

使用受控識別，Azure 平台就能管理此執行階段識別。 您不需要為了識別本身或您需要存取的資源，在應用程式程式碼或設定中儲存及保護存取金鑰。 若事件中樞用戶端應用程式在 Azure App Service 應用程式內或已啟用 Azure 資源的受控識別支援的虛擬機器中執行，則不需處理 SAS 規則和金鑰，或任何其他存取權杖。 用戶端應用程式只需要事件中樞命名空間的端點位址。 當應用程式連線時，事件中樞會將受控識別的內容繫結至作業 (會在此文章稍後的範例中顯示) 中的用戶端。

一旦與受控識別產生關聯之後，事件中樞用戶端就可以執行所有授權的作業。 授權會藉由將受控識別與事件中樞角色產生關聯來授與。 

## <a name="event-hubs-roles-and-permissions"></a>事件中樞角色和權限

您只能將受控識別新增至事件中樞命名空間的「擁有者」或「參與者」角色，以便為命名空間中的所有實體授與識別完整控制。 不過，變更命名空間拓撲的管理作業一開始只能透過 Azure Resource Manager 來支援。 它不能透過原生的事件中樞 REST 管理介面來支援。 此支援也表示您無法在受控識別內使用 .NET Framework 用戶端 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 物件。 
 
## <a name="use-event-hubs-with-managed-identities-for-azure-resources"></a>搭配使用事件中樞與 Azure 資源的受控識別

下一節將說明下列步驟：

1. 建立及部署要在受控識別底下執行的範例應用程式。
2. 為該識別授與存取事件中樞命名空間的權限。
3. 應用程式如何使用該識別來與事件中樞進行互動。

本簡介說明 [Azure App Service](https://azure.microsoft.com/services/app-service/) 中裝載的 Web 應用程式。 虛擬機器裝載之應用程式所需的步驟類似。

### <a name="create-an-app-service-web-application"></a>建立 App Service Web 應用程式

第一個步驟是建立 App Service ASP.NET 應用程式。 如果您不熟悉如何在 Azure 中執行這項操作，請遵循[本操作說明指南](../app-service/app-service-web-get-started-dotnet-framework.md)。 不過，並非如教學課程所示的建立 MVC 應用程式，而是建立 Web Form 應用程式。

### <a name="set-up-the-managed-identity"></a>設定受控識別

一旦您建立應用程式，在 Azure 入口網站瀏覽至新建立的 Web 應用程式 (也會在操作說明中顯示)，然後瀏覽至 [受控服務識別] 分頁，並啟用功能： 

![[受控服務識別] 頁面](./media/event-hubs-managed-service-identity/msi1.png)
 
一旦您啟用此功能，新的服務識別會在您的 Azure Active Directory 中建立，並設定到 App Service 主機。

### <a name="create-a-new-event-hubs-namespace"></a>建立新的事件中樞命名空間

接下來，在具有 Azure 資源之受控識別預覽支援的其中一個 Azure 區域中[建立事件中樞命名空間](event-hubs-create.md)：**美國東部**、**美國東部 2** 或**西歐**。 

瀏覽至入口網站上的命名空間 [存取控制 (IAM)] 頁面，然後按一下 [新增角色指派] 以將受控識別新增至 [擁有者] 角色。 若要這樣做，請在 [新增權限] 面板 [選取] 欄位中搜尋 Web 應用程式的名稱，然後按一下項目。 然後按一下 [儲存] 。 Web 應用程式的受控識別現在具有事件中樞命名空間的存取權，以及您先前建立之事件中樞的存取權。 

### <a name="run-the-app"></a>執行應用程式

現在修改您建立之 ASP.NET 應用程式的預設分頁。 您也可以使用來自[這個 GitHub 存放庫](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp)的 Web 應用程式程式碼。 

一旦您啟動應用程式，將您的瀏覽器指向 EventHubsMSIDemo.aspx。 您也可以將它設為您的起始頁面。 可以在 EventHubsMSIDemo.aspx.cs 檔案中找到程式碼。 結果是最小的 Web 應用程式，具有數個項目欄位，以及具有連線到事件中樞的 [傳送] 和 [接收] 按鈕，以傳送或接收事件。 

請注意 [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) 物件如何初始化。 程式碼不會使用共用存取權杖 (SAS) 權杖提供者，而會改用 `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` 呼叫來建立受控識別的權杖提供者。 因此，不會儲存及使用祕密。 受控識別內容到事件中樞的流程以及授權交握，都會由權杖提供者自動處理，這是比使用 SAS 還要簡單的模型。

當您進行這些變更之後，請發行並執行應用程式。 您可以藉由下載然後在 Visual Studio 中匯入發行設定檔，來取得正確的發行資料：

![匯入發行設定檔](./media/event-hubs-managed-service-identity/msi3.png)
 
若要傳送或接收訊息，請輸入命名空間名稱和您建立之實體的名稱，然後按一下 [傳送] 或 [接收]。 
 
受控識別只適用於 Azure 環境內部，而且只適用於您在其中設定它的 App Service 部署。 受控識別目前不會使用 App Service 部署位置。

## <a name="next-steps"></a>後續步驟

如需事件中樞的詳細資訊，請造訪下列連結：

* 開始使用[事件中樞教學課程](event-hubs-dotnet-standard-getstarted-send.md)
* [事件中樞常見問題集](event-hubs-faq.md)
* [事件中樞價格詳細資料](https://azure.microsoft.com/pricing/details/event-hubs/)
* [使用事件中樞的完整範例應用程式](https://github.com/Azure/azure-event-hubs/tree/master/samples)
