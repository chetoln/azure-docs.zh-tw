---
title: 快速入門 - 使用 Azure 入口網站建立服務匯流排主題和訂用帳戶 | Microsoft Docs
description: 在本快速入門中，您會了解如何使用 Azure 入口網站來建立服務匯流排主題和該主題的訂用帳戶。
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: 2af346b2c9fa5c46593aa9421c3a762bda78dc2f
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59610148"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>快速入門：使用 Azure 入口網站建立服務匯流排主題和主題的訂用帳戶
在本快速入門中，您會使用 Azure 入口網站來建立服務匯流排主題，然後建立該主題的訂用帳戶。 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>什麼是服務匯流排主題和訂用帳戶？
服務匯流排主題和訂用帳戶支援「發佈/訂閱」  訊息通訊模型。 使用主題和訂用帳戶時，分散式應用程式的元件彼此不直接通訊，相反的，他們會透過扮演中繼角色的主題來交換訊息。

![主題概念](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

有別於服務匯流排佇列，服務匯流排佇列中的每個訊息只會由單一取用者處理，主題和訂用帳戶採用發佈/訂閱模式，提供一對多的通訊形式。 一個主題可以登錄多個訂用帳戶。 當訊息傳送至主題時，每個訂用帳戶都可取得訊息來個別處理。 主題的訂用帳戶類似於虛擬佇列，同樣可接收已傳送到主題的訊息複本。 您可以選擇為個別訂用帳戶登錄主題的篩選規則，以篩選或限制主題的哪些訊息由哪些主題訂用帳戶接收。

服務匯流排主題和訂用帳戶可讓您擴大處理非常多使用者和應用程式上大量的訊息。

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]



## <a name="next-steps"></a>後續步驟
若要了解如何傳送訊息至主題，並透過訂用帳戶接收那些訊息，請參閱下列文章：選取 TOC 中的程式設計語言。 

> [!div class="nextstepaction"]
> [發佈和訂閱訊息](service-bus-dotnet-how-to-use-topics-subscriptions.md)
