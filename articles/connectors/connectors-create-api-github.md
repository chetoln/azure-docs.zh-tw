---
title: 連線到 GitHub - Azure Logic Apps | Microsoft Docs
description: 使用 GitHub REST API 和 Azure Logic Apps 監視 GitHub 事件
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0d2ff9368bc244a5afd6fafc40cf476b90a80a52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462521"
---
# <a name="connect-to-github"></a>連線至 GitHub

GitHub 是網頁式 Git 存放庫主機服務，它提供 Git 中所有分散式版本控制和原始程式碼管理 (SCM) 功能，以及其他功能。

若要開始使用 GitHub 連接器，[請先建立邏輯應用程式](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

## <a name="create-a-connection-to-github"></a>建立 GitHub 的連線

若要在邏輯應用程式中使用 GitHub 連接器，您必須先建立「連線」，然後提供這些屬性的詳細資料： 

| 屬性 | 必要項 | 描述 | 
| -------- | -------- | ----------- | 
| token | 是 | 提供您的 GitHub 認證。 |

建立連線後，您就可以執行動作，並接聽本文所述的觸發程序。

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>連接器特定的詳細資料

如需 Swagger 中定義的觸發程序和動作及任何限制，請參閱[連接器詳細資料](/connectors/github/)。

## <a name="find-more-connectors"></a>尋找其他連接器

* 檢閱[連接器清單](apis-list.md)。