---
title: Azure 中 Windows VM 的相關常見問題集 | Microsoft Docs
description: 針對以 Resource Manager 模型建立的 Windows 虛擬機器，提供一些相關常見問題的解答。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 61f24b3c13a53b23538327cd1458a54756b7caa5
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466358"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows 虛擬機器的常見問題
本文可解決在 Azure 中使用 Resource Manager 部署模型建立之 Windows 虛擬機器的一些常見問題。 本主題的 Linux 版本，請參閱[關於 Linux 虛擬機器的常見問題集](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

## <a name="what-can-i-run-on-an-azure-vm"></a>我可以在 Azure VM 上运行什么程序？
所有订户都可以在 Azure 虚拟机上运行服务器软件。 在 Azure 中執行的 Microsoft 伺服器軟體的支援原則的相關資訊，請參閱[Microsoft 伺服器軟體支援 Azure 虛擬機器的](https://support.microsoft.com/kb/2721672)。

特定版本的 Windows 7、Windows 8.1 及 Windows 10 可供 MSDN Azure 權益訂閱者和 MSDN 開發和測試隨用隨付訂閱者 (針對開發與測試工作) 使用。 如需詳細資訊 (包括指示和限制)，請參閱 [MSDN 訂閱者的 Windows 用戶端映像](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)。 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>我可以使用多少的儲存體搭配虛擬機器？
每個資料磁碟最多可達 4 TB (4,095 GB)。 可使用的資料磁碟數量取決於虛擬機器的大小。 如需詳細資訊，請參閱 [虛擬機器的大小](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

Azure 受控磁碟是受歡迎磁碟儲存體供應項目，可用在 Azure 虛擬機器當作資料的用續性儲存體。 每部虛擬機器可使用多部受控磁碟。 受控磁碟提供兩種耐久的儲存體選項：進階與標準受控磁碟。 如需定價資訊，請參閱[受控磁碟定價](https://azure.microsoft.com/pricing/details/managed-disks)。

Azure 儲存體帳戶也提供作業系統磁碟和任何資料磁碟的儲存空間。 每個磁碟是以分頁 Blob 方式儲存的 .vhd 檔案。 如需定價的詳細資料，請參閱 [儲存體定價詳細資料](https://azure.microsoft.com/pricing/details/storage/)。

## <a name="how-can-i-access-my-virtual-machine"></a>如何访问我的虚拟机？
使用遠端桌面連線 (RDP) 為 Windows VM 建立遠端連線。 如需相關指示，請參閱 <<c0> [ 如何連接和登入執行 Windows 的 Azure 虛擬機器](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。 除非將伺服器設定為遠端桌面服務工作階段主機，否則最多支援兩個並行連線。  

如果您在使用「遠端桌面」時遇到問題，請參閱 [疑難排解以 Windows 為基礎之 Azure 虛擬機器的遠端桌面連線](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。 

如果您熟悉 Hyper-V，您可能正在尋找類似 VMConnect 的工具。 Azure 沒有提供類似的工具，因為並不支援主控台存取虛擬機器。

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>我可以使用暫存磁碟 (預設為 D: 磁碟機) 儲存資料嗎？
請勿使用暫存磁碟來儲存資料。 暫存磁碟僅提供暫存空間，因此您會有遺失資料且無法復原的風險。 當虛擬機器移動到不同的主機時就可能發生資料遺失。 可能要移動虛擬機器的一些原因是調整虛擬機器的大小、更新主機，或主機上的硬體故障等等。

如果應用程式需要使用 D: 磁碟機代號，您可以重新指派磁碟機代號，讓暫存磁碟使用 D: 以外的代號。 如需相關指示，請參閱 [變更 Windows 暫存磁碟的磁碟機代號](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>如何變更暫存磁碟的磁碟機代號？
您可以透過移動分頁檔並重新指派磁碟機代號來變更磁碟機代號，但必須確定會以特定的順序執行這些步驟。 有关说明，请参阅[更改 Windows 临时磁盘的驱动器号](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>我是否可以將現有的 VM 新增到可用性設定組？
沒有。 如果您希望 VM 屬於可用性設定組的一部分，就必須在設定組當中建立 VM。 目前不支援在建立 VM 之後，再將其加入可用性設定組。

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>我是否可以將虛擬機器上傳到 Azure？
是。 如需指示，請參閱[移轉內部部署 VM 至 Azure](on-prem-to-azure.md)。

## <a name="can-i-resize-the-os-disk"></a>我是否可以調整 OS 磁碟機的大小？
是。 如需指示，請參閱 [如何擴充 Azure 資源群組之虛擬機器的 OS 磁碟機](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>我是否可以複製或再製現有的 Azure VM？
是。 您可以使用受控映像來建立虛擬機器映像，然後使用此映像建置多個新 VM。 如需指示，請參閱[建立 VM 的自訂映像](tutorial-custom-images.md)。

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>为什么在 Azure Resource Manager 中看不到加拿大中部和加拿大东部区域？

針對現有 Azure 訂用帳戶所建立的虛擬機器，不會自動註冊加拿大中部和加拿大東部這兩個新的區域。 當虛擬機器透過 Azure 入口網站使用 Azure Resource Manager 部署到任何其他區域時，就會自動完成註冊。 将虚拟机部署到其他任何 Azure 区域后，新区域可供后续虚拟机使用。

## <a name="does-azure-support-linux-vms"></a>Azure 是否支援 Linux VM？
是。 若要快速建立 Linux VM 以試用，請參閱 [使用入口網站在 Azure 上建立 Linux VM](../linux/quick-create-portal.md)。

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>我可以在建立 VM 之後將 NIC 新增至此 VM 嗎？
是的，目前可行。 您必須先停止解除配置 VM。 然後您可以新增或移除 NIC (除非它是 VM 上的最後一個 NIC)。 

## <a name="are-there-any-computer-name-requirements"></a>是否有任何電腦名稱需求？
是。 電腦名稱的長度最多可以有 15 個字元。 如需命名資源相關詳細資訊，請參閱[命名慣例規則與限制](/azure/architecture/best-practices/naming-conventions#compute)。

## <a name="are-there-any-resource-group-name-requirements"></a>是否有任何資源群組名稱需求？
是。 資源群組名稱長度最多可以有 90 個字元。 如需資源群組相關詳細資訊，請參閱[命名慣例規則與限制](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions)。

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>建立 VM 時的使用者名稱需求為何？

使用者名稱長度最多為 20 個字元，而且不能以句號 (".") 結尾。 

不允許下列使用者名稱︰

| | | | |
|-----------------|-----------|--------------------|----------|
| `administrator` | `admin`   | `user`             | `user1`  |
| `test`          | `user2`   | `test1`            | `user3`  |
| `admin1`        | `1`       | `123`              | `a`      |
| `actuser`       | `adm`     | `admin2`           | `aspnet` |
| `backup`        | `console` | `david`            | `guest`  |
| `john`          | `owner`   | `root`             | `server` |
| `sql`           | `support` | `support_388945a0` | `sys`    |
| `test2`         | `test3`   | `user4`            | `user5`  |


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>建立 VM 時的密碼需求為何？

有各種不同的密碼長度要求，視您所使用的工具而定：
 - 入口網站-12-72 個字元之間
 - PowerShell-介於 8 到 123 個字元之間
 - CLI-12-123 之間

* 包含小寫字元
* 包含大小字元
* 具有数字
* 包含特殊字元 (Regex match [\W_])

不允許下列密碼︰

<table>
    <tr>
        <td>abc@123</td>
        <td>iloveyou!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$$word</td>
        <td>pass@word1</td>
        <td>Password!</td>
        <td>Password1</td>
        <td>Password22</td>
    </tr>
</table>
