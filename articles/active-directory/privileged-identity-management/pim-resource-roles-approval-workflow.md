---
title: 核准或拒絕要求的 PIM-Azure Active Directory 中的 Azure 資源角色 |Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中核准或拒絕 Azure 資源角色的要求。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f645b7077ef43dc7eb4d70261b6b601b5e4af1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60288428"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>在 PIM 中核准或拒絕 Azure 資源角色的要求

使用 Azure Active Directory (Azure AD) Privileged Identity Management (PIM)，您可以將角色設定為需要核准才能啟用，並選擇一或多個使用者或群組做為委派核准者。 委派核准者會有 24 小時的時間來核准要求。 如果未在 24 小時內核准要求，符合資格的使用者就必須重新提交新要求。 24 小時核准時間範圍是不可設定的。

依照本文中的步驟核准或拒絕 Azure 資源角色的要求。

## <a name="view-pending-requests"></a>檢視擱置的要求

身為委派核准者，當有 Azure 資源角色要求正等待您的核准時，您會收到電子郵件通知。 您可以在 PIM 中檢視這些擱置的要求。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. 開啟 **Azure AD Privileged Identity Management**。

1. 按一下 [核准要求]。

    ![Azure 資源 - 核准要求](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    在 [要求啟用角色] 區段中，您會看見正等待您核准的要求清單。

## <a name="approve-requests"></a>核准要求

1. 尋求並按一下您想要核准的要求。 核准窗格隨即出現。

    ![核准要求窗格](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. 在 [理由] 方塊中，輸入理由。

1. 按一下 [核准]。

    表示您核准的通知隨即顯示。

    ![核准通知](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>拒絕要求

1. 尋求並按一下您想要拒絕的要求。 核准窗格隨即出現。

    ![核准要求窗格](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. 在 [理由] 方塊中，輸入理由。

1. 按一下 [拒絕]。

    表示您拒絕的通知隨即顯示。

## <a name="workflow-notifications"></a>工作流程通知

以下是一些工作流程通知相關資訊：

- 當有角色要求正在等待他們檢閱時，核准者清單中的所有成員都會收到電子郵件通知。 電子郵件通知包含可供核准者核准或拒絕要求的直接連結。
- 清單內第一個核准或拒絕的成員會解決要求。
- 當核准者回應要求時，核准者清單的所有成員都會得知該動作。
- 已核准的成員變成有效角色時，資源管理員會收到通知。

>[!Note]
>當資源系統管理員認為獲得核准的成員不應該成為有效角色時，可在 PIM 中移除作用中角色指派。 雖然資源系統管理員必須是核准者清單的成員才會收到待核准要求的通知，但他們可以透過檢視 PIM 中的待核准要求，來檢視和取消所有使用者的待核准要求。 

## <a name="next-steps"></a>後續步驟

- [在 PIM 中延長或更新 Azure 資源角色](pim-resource-roles-renew-extend.md)
- [PIM 中的電子郵件通知](pim-email-notifications.md)
- [核准或拒絕要求的 Azure AD PIM 中的角色](azure-ad-pim-approval-workflow.md)
