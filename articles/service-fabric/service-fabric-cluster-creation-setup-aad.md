---
title: 設定 Azure Active Directory 以進行 Service Fabric 用戶端驗證 | Microsoft Docs
description: 了解如何設定 Azure Active Directory (Azure AD) 以驗證 Service Fabric 叢集的用戶端。
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/15/2019
ms.author: aljo
ms.openlocfilehash: c02e38880fdf8e8f1a2229f009b343d6431af853
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125131"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>設定用戶端驗用的 Azure Active Directory

針對在 Azure 上執行的叢集，建議您使用 Azure Active Directory (Azure AD) 來保護對管理端點的存取。  本文說明如何設定 Azure AD 來驗證 Service Fabric 叢集的用戶端，完成之後才能[建立叢集](service-fabric-cluster-creation-via-arm.md)。  Azure AD 可讓組織 (稱為租用戶) 管理使用者對應用程式的存取。 應用程式分成具有 Web 型登入 UI 的應用程式，以及具有原生用戶端體驗的應用程式。 

Service Fabric 叢集提供其管理功能的各種進入點 (包括 Web 型 [Service Fabric Explorer][service-fabric-visualizing-your-cluster] 和 [Visual Studio][service-fabric-manage-application-in-visual-studio])。 因此，您將建立兩個 Azure AD 應用程式來控制對叢集的存取：一個 Web 應用程式和一個原生應用程式。  建立應用程式之後，您可以將使用者指派給唯讀和系統管理員角色。

> [!NOTE]
> 建立叢集之前，您必須先完成下列步驟。 由於指令碼會預期叢集名稱和端點，因此這些值應該是計劃的值，而不是您已經建立的值。

## <a name="prerequisites"></a>必要條件
在本文中，我們假設您已經建立租用戶。 如果您尚未建立租用戶，請從閱讀[如何取得 Azure Active Directory 租用戶][active-directory-howto-tenant]開始進行。

為了簡化與設定 Azure AD 搭配 Service Fabric 叢集相關的一些步驟，我們建立了一組 Windows PowerShell 指令碼。

1. [將指令碼下載](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool)到您的電腦。
2. 在 zip 檔案上按一下滑鼠右鍵，選取 [屬性]、選取 [解除封鎖] 核取方塊，然後按一下 [套用]。
3. 解壓縮 zip 檔案。

## <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>建立 Azure AD 應用程式，並將使用者指派給角色
建立兩個 Azure AD 應用程式來控制對叢集的存取：一個 Web 應用程式和一個原生應用程式。 建立應用程式來代表您的叢集之後，請將使用者指派給 [Service Fabric 所支援的角色](service-fabric-cluster-security-roles.md)︰唯讀和系統管理員。

執行 `SetupApplications.ps1`，並提供租用戶識別碼、叢集名稱和 Web 應用程式回覆 URL 作為參數。  同時也應指定使用者的使用者名稱和密碼。  例如︰

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '0e3d2646-78b3-4711-b8be-74a381d9890c' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> 對於國家雲 (例如 Azure Government、Azure 中國、Azure 德國)，您也應指定 `-Location` 參數。

您可以執行 PowerShell 命令 `Get-AzureSubscription` 來找出您的 *TenantId*。 執行此命令會顯示每個訂用帳戶的 TenantId。

*ClusterName* 會用來為指令碼所建立的 Azure AD 應用程式加上前置詞。 它不需要與實際叢集名稱完全相符。 其用意只是要讓您更容易將 Azure AD 構件對應到與之搭配使用的 Service Fabric 叢集。

*WebApplicationReplyUrl* 是在您的使用者完成登入之後，Azure AD 傳回給他們的預設端點。 請將此端點設定為您叢集的 Service Fabric Explorer 端點，預設為︰

https://&lt;cluster_domain&gt;:19080/Explorer

系統會提示您登入具有 Azure AD 租用戶系統管理權限的帳戶。 在您登入之後，指令碼會建立 Web 和原生應用程式來代表 Service Fabric 叢集。 如果您在 [Azure 入口網站][azure-portal]中查看租用戶的應用程式，則應該會看到兩個新項目︰

   * *ClusterName*\_叢集
   * *ClusterName*\_用戶端

此指令碼會列印您[建立叢集](service-fabric-cluster-creation-create-template.md#add-azure-ad-configuration-to-use-azure-ad-for-client-access)時 Azure Resource Manager 範本所需的 JSON，因此建議讓 PowerShell 視窗保持開啟。

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>設定 Azure Active Directory 的疑難排解說明
設定 Azure AD 並加以使用並不容易，因此以下提供一些指標，供您對問題進行偵錯。

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer 會提示您選取憑證
#### <a name="problem"></a>問題
在 Service Fabric Explorer 中順利登入 Azure AD 之後，瀏覽器會返回首頁，但是會出現提示您選取憑證的訊息。

![SFX 憑證對話方塊][sfx-select-certificate-dialog]

#### <a name="reason"></a>原因
使用者未獲指派 Azure AD 叢集應用程式中的角色。 因此，Azure AD 驗證在 Service Fabric 叢集上發生失敗。 Service Fabric Explorer 會回復到憑證驗證。

#### <a name="solution"></a>解決方法
請依照設定 Azure AD 的指示進行操作，然後指派使用者角色。 另外，建議您如 `SetupApplications.ps1` 所做的一樣，開啟 [存取應用程式需要使用者指派]。

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>使用 PowerShell 進行連線時失敗，發生錯誤：「指定的認證無效」
#### <a name="problem"></a>問題
在您順利登入 Azure AD 之後，於使用 PowerShell 以 “AzureActiveDirectory” 安全性模式連線到叢集時連線失敗，發生錯誤：「指定的認證無效」。

#### <a name="solution"></a>解決方法
此解決方案與前一個相同。

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer 在您登入時傳回失敗："AADSTS50011"
#### <a name="problem"></a>問題
當您在 Service Fabric Explorer 中嘗試登入 Azure AD 時，頁面傳回失敗：「AADSTS50011:回覆地址 &lt;url&gt; 與為應用程式 &lt;guid&gt; 設定的回覆地址不符」。

![SFX 回覆地址不相符][sfx-reply-address-not-match]

#### <a name="reason"></a>原因
代表 Service Fabric Explorer 的叢集 (Web) 應用程式嘗試對照 Azure AD 來進行驗證，而它在要求中提供重新導向傳回 URL。 但該 URL 並未列在 Azure AD 應用程式 [回覆 URL] 清單中。

#### <a name="solution"></a>解決方法
在 AAD 頁面中選取 [應用程式註冊]，選取您的叢集應用程式，然後選取 [回覆 URL] 按鈕。 在 [回覆 URL] 頁面上，將 Service Fabric Explorer 的 URL 新增到清單中，或取代清單內的其中一個項目。 完成時，請儲存變更。

![Web 應用程式回覆 url][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>透過 PowerShell 使用 Azure AD 驗證來連接叢集
若要連接 Service Fabric 叢集，請使用下列 PowerShell 命令範例︰

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

若要深入了解，請參閱 [Connect-ServiceFabricCluster Cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster)。

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>我是否可以在多個叢集中重複使用相同的 Azure AD 租用戶？
是。 但是請務必將 Service Fabric Explorer 的 URL 新增到叢集 (Web) 應用程式。 否則 Service Fabric Explorer 無法運作。

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>為什麼在已啟用 Azure AD 的情況下仍然需要伺服器憑證？
FabricClient 和 FabricGateway 會執行相互驗證。 在 Azure AD 驗證期間，Azure AD 整合會將用戶端身分識別提供給伺服器，而伺服器憑證則用來驗證伺服器身分識別。 如需有關 Service Fabric 憑證的詳細資訊，請參閱 [X.509 憑證和 Service Fabric][x509-certificates-and-service-fabric]。

## <a name="next-steps"></a>後續步驟
設定 Azure Active Directory 應用程式並為使用者設定角色之後，請[設定和部署叢集](service-fabric-cluster-creation-via-arm.md)。


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
