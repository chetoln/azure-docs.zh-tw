---
title: 將存取原則指派給 Azure Service Fabric 服務端點 | Microsoft Docs
description: 了解如何將安全性存取原則指派給 Service Fabric 服務中的 HTTP 或 HTTPS 端點。
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: atsenthi
ms.openlocfilehash: 3e892e443f5e3309add48f939f26ba14eaf5a51b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614195"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>為 HTTP 和 HTTPS 端點指派安全性存取原則
如果您套用執行身分原則，而且服務資訊清單宣告 HTTP 端點資源，您就必須指定 **SecurityAccessPolicy**。  **SecurityAccessPolicy** 可確保配置給這些端點的連接埠受到正確限制，唯有執行服務的使用者帳戶才能使用。 否則， **http.sys** 無法存取服務，而且您從用戶端呼叫時將會失敗。 下列範例會將 Customer1 帳戶套用到名為 **EndpointName** 的端點，這會給予它完整的存取權限。

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

針對 HTTPS 端點，請同時指出要傳回給用戶端的憑證名稱。 您可以使用 **EndpointBindingPolicy** 參考憑證。  憑證的定義位於應用程式資訊清單的 [憑證] 區段。

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies>
```

> [!WARNING] 
> 使用 HTTPS 時，請勿對部署至相同節點的不同服務執行個體 (獨立於應用程式) 使用相同連接埠和憑證。 在不同的應用程式執行個體中，使用相同連接埠來升級兩個不同的服務，將會導致升級失敗。 如需詳細資訊，請參閱[使用 HTTPS 端點來升級多個應用程式](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints)。
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
若要了解後續步驟，請閱讀下列文章：
* [了解應用程式模型](service-fabric-application-model.md)
* [在服務資訊清單中指定資源](service-fabric-service-manifest-resources.md)
* [部署應用程式](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
