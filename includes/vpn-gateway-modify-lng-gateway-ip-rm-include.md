---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6505b12b35ee436930ba6571c27db30c12030041
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419120"
---
### <a name="gwipnoconnection"></a>修改區域網路閘道 'GatewayIpAddress' - 沒有閘道連線

如果您想要連線的 VPN 裝置已變更其公用 IP 位址，您需要修改區域網路閘道，以反映該變更。 使用範例來修改沒有閘道連線的區域網路閘道。

修改此值時，您也可以同時修改位址首碼。 務必使用區域網路閘道的現有名稱，以便覆寫目前的設定。 如果您使用不同的名稱，您會建立新的區域網路閘道，而不是覆寫現有閘道。

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="gwipwithconnection"></a>修改區域網路閘道 'GatewayIpAddress' - 現有閘道連線

如果要连接的 VPN 设备已更改其公共 IP 地址，则需根据该更改修改本地网关。 如果网关连接已存在，首先需要删除该连接。 删除连接后，可修改网关 IP 地址并重新创建一个新的连接。 您也可以同時修改位址首碼。 這會導致您 VPN 連線的停機時間。 修改閘道 IP 位址時，您不需要刪除 VPN 閘道。 您只需要移除連線。
 

1. 移除連線。 您可以使用 'Get-AzVirtualNetworkGatewayConnection' Cmdlet 來找到連線的名稱。

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1
   ```
2. 修改 'GatewayIpAddress' 值。 您也可以同時修改位址首碼。 請務必使用區域網路閘道的現有名稱來覆寫目前的設定。 否則，您會建立新的區域網路閘道，而不是覆寫現有閘道。

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name Site1 `
   -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
   -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
   ```
3. 建立連線。 在此範例中，我們會設定 IPsec 連線類型。 當您重新建立連線時，請使用針對設定指定的連線類型。 對於其他連線類型，請參閱 [PowerShell Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) 頁面。  若要取得 VirtualNetworkGateway 名稱，您可以執行 'Get-AzVirtualNetworkGateway' Cmdlet。
   
    設定變數。

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1

   $vnetgw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   ```
   
    建立連線。

   ```azurepowershell-interactive 
   New-AzVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
   -Location "East US" `
   -VirtualNetworkGateway1 $vnetgw `
   -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```