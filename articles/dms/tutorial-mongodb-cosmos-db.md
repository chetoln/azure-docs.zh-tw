---
title: 教學課程：使用 Azure 資料庫移轉服務在離線狀態下將 MongoDB 遷移至 Azure Cosmos DB 的 Mongo 版 API | Microsoft Docs
description: 了解如何使用 Azure 資料庫移轉服務，在離線狀態下從內部部署 MongoDB 遷移至 Azure Cosmos DB 的 Mongo 版 API。
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ad0d990554d9ff49bed3e9da7097c87c06c7152f
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415557"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>教學課程：使用 DMS 在離線狀態下將 MongoDB 遷移至 Azure Cosmos DB 的 Mongo 版 API
您可以使用 Azure 資料庫移轉服務，在離線狀態下將資料庫從內部部署或雲端的 MongoDB 執行個體 (單次) 移轉至 Azure Cosmos DB 的 Mongo 版 API。

在本教學課程中，您了解如何：
> [!div class="checklist"]
> * 建立 Azure 資料庫移轉服務的執行個體。
> * 使用 Azure 資料庫移轉服務來建立移轉專案。
> * 執行移轉。
> * 監視移轉。

在本教學課程中，您會使用 Azure 資料庫移轉服務，從裝載在 Azure 虛擬機器中的 MongoDB，將其中的某個資料集遷移至 Azure Cosmos DB 的 Mongo 版 API。 如果您尚未設定 MongoDB 來源，請參閱[在 Azure 中的 Windows VM 上安裝及設定 MongoDB](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb) 一文。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程，您需要：

* [完成移轉前](../cosmos-db/mongodb-pre-migration.md)步驟，例如估計輸送量、選擇分割索引鍵和索引編製原則。
* [建立 Azure Cosmos DB 的 Mongo 版 API 帳戶](https://ms.portal.azure.com/#create/Microsoft.DocumentDB)。
* 使用 Azure Resource Manager 部署模型建立 Azure 資料庫移轉服務的 Azure 虛擬網路 (VNet)，以使用 [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) 或 [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) 為您的內部部署來源伺服器提供站對站連線能力。 如需建立 VNet 的詳細資訊，請參閱[虛擬網路文件](https://docs.microsoft.com/azure/virtual-network/)，特別是快速入門文章，裡面會提供逐步操作詳細資料。

    > [!NOTE]
    > 在 VNet 設定期間，如果您搭配與 Microsoft 對等互連的網路使用 ExpressRoute，請將下列服務[端點](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)新增至將佈建服務的子網路：

    > * 目標資料庫端點 (例如，SQL 端點、Cosmos DB 端點等)
    > * 儲存體端點
    > * 服務匯流排端點
    >
    > 此為必要設定，因為 Azure 資料庫移轉服務沒有網際網路連線。

* 確定您的 VNet 網路安全性群組規則不會對 Azure 資料庫移轉服務封鎖下列輸入通訊埠：443、53、9354、445 及 12000。 如需 Azure VNet NSG 流量篩選的詳細資訊，請參閱[使用網路安全性群組來篩選網路流量](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)。
* 開啟您的 Windows 防火牆以允許 Azure 資料庫移轉服務存取來源 MongoDB 伺服器 (依預設會使用 TCP 連接埠 27017)。
* 使用來源資料庫前面的防火牆應用裝置時，您可能必須新增防火牆規則，才能讓 Azure 資料庫移轉服務存取來源資料庫，以進行移轉。

## <a name="register-the-microsoftdatamigration-resource-provider"></a>註冊 Microsoft.DataMigration 資源提供者

1. 登入 Azure 入口網站，選取 [所有服務]，然後選取 [訂用帳戶]。

   ![顯示入口網站訂用帳戶](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)

2. 選取您要在其中建立 Azure 資料庫移轉服務執行個體的訂用帳戶，然後選取 [資源提供者]。

    ![顯示資源提供者](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)

3. 搜尋移轉，然後在 [Microsoft.DataMigration] 的右邊，選取 [註冊]。

    ![註冊資源提供者](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>建立執行個體

1. 在 Azure 入口網站中，選取 [+ 建立資源]，搜尋「Azure 資料庫移轉服務」，然後從下拉式清單選取 [Azure 資料庫移轉服務]。

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2. 在 [Azure 資料庫移轉服務] 畫面上，選取 [建立]。

    ![建立 Azure 資料庫移轉服務執行個體](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3. 在 [建立移轉服務] 畫面上，指定服務的名稱、訂用帳戶，以及新的或現有的資源群組。

4. 選取您要在其中建立 Azure 資料庫移轉服務執行個體的位置。 

5. 選取現有 VNet 或建立新的 VNet。

    VNet 會為 Azure 資料庫移轉服務提供來源 MongoDB 執行個體和目標 Azure Cosmos DB 帳戶的存取權。

    如需有關如何在 Azure 入口網站中建立 VNet 的詳細資訊，請參閱[使用 Azure 入口網站建立虛擬網路](https://aka.ms/DMSVnet)一文。

6. 選取定價層。

    如需成本和定價層的詳細資訊，請參閱[定價分頁](https://aka.ms/dms-pricing)。

    ![設定 Azure 資料庫移轉服務執行個體設定](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7. 選取 [建立] 以建立服務。

## <a name="create-a-migration-project"></a>建立移轉專案

建立服務之後，請在 Azure 入口網站中找出該服務，然後建立新的移轉專案。

1. 在 Azure 入口網站中，選取 [所有服務]，搜尋 Azure 資料庫移轉服務，然後選取 [Azure 資料庫移轉服務]。

      ![找出 Azure 資料庫移轉服務的所有執行個體](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. 在 [Azure 資料庫移轉服務] 畫面上，搜尋您建立的 Azure 資料庫移轉服務執行個體名稱，然後選取該執行個體。

3. 選取 [+ 新增移轉專案]。

4. 在 [新增移轉專案] 畫面上指定專案名稱、在 [來源伺服器類型] 文字方塊中選取 [MongoDB]、在 [目標伺服器類型] 文字方塊中選取 [CosmosDB (MongoDB API)]，然後針對 [選擇活動類型]，選取 [離線資料移轉]。 

    ![建立資料庫移轉服務專案](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5. 選取 [建立及執行活動]，以建立專案並執行移轉活動。

## <a name="specify-source-details"></a>指定來源詳細資料

1. 在 [來源詳細資料] 畫面上，指定來源 MongoDB 伺服器的連線詳細資料。

   您也可以使用連接字串模式，並為 Blob 存放區檔案容器 (您已在其中傾印您想要移轉的集合資料) 提供位置。

   > [!NOTE]
   > Azure 資料庫移轉服務也可以將 bson 文件或 json 文件遷移至 Azure Cosmos DB 的 Mongo 版 API 集合。

   如果無法解析 DNS 名稱，您也可以使用 IP 位址。

   ![指定來源詳細資料](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. 選取 [ **儲存**]。

## <a name="specify-target-details"></a>指定目標詳細資料

1. 在 [移轉目標詳細資料] 畫面上，對目標 Azure Cosmos DB 帳戶指定連線詳細資料，此帳戶就是要作為 MongoDB 資料遷移目的地的預先佈建 Azure Cosmos DB Mongo 版 API 帳戶。

    ![指定目標詳細資料](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. 選取 [ **儲存**]。

## <a name="map-to-target-databases"></a>對應到目標資料庫

1. 在 [Map to target databases] \(對應到目標資料庫\) 畫面上，對應要進行移轉的來源資料庫和目標資料庫。

    如果目標資料庫包含與來源資料庫相同的資料庫名稱，Azure 資料庫移轉服務依預設會選取目標資料庫。

    如果字串 **Create** 出現在資料庫名稱旁邊，則代表 Azure 資料庫移轉服務未找到目標資料庫，且服務會為您建立該資料庫。

    在進行到這個移轉階段時，您可以[佈建輸送量](https://docs.microsoft.com/azure/cosmos-db/set-throughput)。 在 Cosmos DB 中，您可以在資料庫層級或以個別進行的方式，為每個集合佈建輸送量。 輸送量會以[要求單位](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU) 來測量。 深入了解 [Azure Cosmos DB 定價](https://azure.microsoft.com/pricing/details/cosmos-db/)。

    ![對應到目標資料庫](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. 選取 [ **儲存**]。
3. 在 [集合設定] 畫面上，展開集合清單，然後檢閱要遷移的集合清單。

    請注意，Azure 資料庫移轉服務會自動選取所有存在於來源 MongoDB 執行個體上，卻不存在於目標 Azure Cosmos DB 帳戶上的集合。 如果您想要重新移轉已包含資料的集合，就必須在此刀鋒視窗上明確地選取集合。

    您可以指定您想要讓集合使用的 RU 數量。 Azure 資料庫移轉服務會根據集合大小來建議智慧的預設值。

    您也可以指定分區索引鍵以便利用 [Azure Cosmos DB 中的資料分割](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)，從而獲得最佳延展性。 請務必檢閱[選取分區/資料分割索引鍵的最佳做法](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)。

    ![選取集合資料表](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. 選取 [ **儲存**]。

5. 在 [移轉摘要] 畫面上的 [活動名稱] 文字方塊中，指定移轉活動的名稱。

    ![移轉摘要](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>執行移轉

* 選取 [執行移轉]。

    [移轉活動] 視窗隨即出現，而且活動的 [狀態] 為 [未啟動]。

    ![活動狀態](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>監視移轉

* 在移轉活動畫面上，選取 [重新整理] 以更新顯示，直到移轉的**狀態**顯示為 [已完成] 為止。

   > [!NOTE]
   > 您可以選取活動來取得資料庫層級和集合層級移轉計量的詳細資料。

    ![活動狀態已完成](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>確認 Cosmos DB 中的資料

* 在移轉完成之後，您可以檢查 Azure Cosmos DB 帳戶，以確認所有集合都已成功移轉。

    ![活動狀態已完成](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="post-migration-optimization"></a>移轉後最佳化

將 MongoDB 資料庫中儲存的資料遷移至 Azure Cosmos DB 的 MongoDB 版 API 之後，您就可以連線到 Azure Cosmos DB 並管理這些資料。 您也可以執行其他的移轉後最佳化步驟，例如將索引編製原則最佳化、更新預設的一致性層級或設定 Azure Cosmos DB 帳戶的全域散發。 如需詳細資訊，請參閱[移轉後最佳化](../cosmos-db/mongodb-post-migration.md)一文。 

## <a name="additional-resources"></a>其他資源

* [Cosmos DB 服務資訊](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>後續步驟

* 在 Microsoft [資料庫移轉指南](https://datamigration.microsoft.com/)中檢閱其他案例的移轉指導方針。
