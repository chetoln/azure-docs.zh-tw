---
ms.openlocfilehash: aa2e6d80620f0a4cf5063919a6de53e4de20f706
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301440"
---
1. 將安裝程式複製到您想要保護之伺服器上的本機資料夾 (例如 /tmp)。 在終端機中執行下列命令：
     ```
     cd /tmp;
     tar -xvzf Microsoft-ASR_UA*release.tar.gz
     ```
2. 若要安裝行動服務，請執行下列命令：

     ```
     sudo ./install -d <Install Location> -r MS -v VmWare -q
     ```
3. 安裝完成後，行動服務必須向組態伺服器註冊。 執行下列命令來向組態伺服器註冊行動服務：

     ```
     /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
     ```

#### <a name="mobility-service-installer-command-line"></a>行動服務安裝程式命令列

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|參數|類型|描述|可能的值|
|-|-|-|-|
|-r |強制|指定應該安裝行動服務 (MS) 還是應該安裝主要目標 (MT)。|MS <br /> MT|
|-d |選用|行動服務的安裝位置。|/usr/local/ASR|
|-v|強制|指定要安裝行動服務的平台。 <br /> <br />- **VMware**:使用此值，如果您在上執行的 VM 上安裝行動服務*VMware vSphere ESXi 主機*， *HYPER-V 主機*，並*實體伺服器*。 <br /> - **Azure**:如果您在 Azure IaaS VM 上安裝代理程式，請使用此值。| VMware <br /> Azure|
|-q|選用|指定要以無訊息模式執行安裝程式。| N/A|

#### <a name="mobility-service-configuration-command-line"></a>行動服務設定命令列

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|參數|類型|描述|可能的值|
|-|-|-|-|
|-i |強制|設定伺服器的 IP|任何有效的 IP 位址|
|-P |強制|儲存連線複雜密碼之檔案的完整檔案路徑|任何有效的資料夾|
<!--Update_Description: wording update-->
<!--ms.date: 03/05/2018-->