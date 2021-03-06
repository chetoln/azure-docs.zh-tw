---
title: 使用多個節點集區中 Azure Kubernetes Service (AKS)
description: 了解如何建立和管理 Azure Kubernetes Service (AKS) 叢集的多個節點集區
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/29/2019
ms.author: iainfou
ms.openlocfilehash: 1c24bbb9433e4164d4b2f6ce1ac7bd726cc36356
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65506896"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>預覽-建立和管理 Azure Kubernetes Service (AKS) 叢集的多個節點集區

Azure Kubernetes Service (AKS) 中，相同的設定節點會分組放入*節點的集區*。 這些節點的集區包含執行應用程式的基礎 Vm。 當您建立 AKS 叢集，這會建立定義節點和其大小 (SKU) 的初始數目*預設節點集區*。 若要支援具有不同的計算或儲存體需求的應用程式，您可以建立其他的節點集區。 比方說，Gpu 提供大量運算應用程式，或存取高效能 SSD 儲存體中使用這些額外的節點集區。

這篇文章會示範如何建立和管理 AKS 叢集中的多個節點集區。 此功能目前為預覽狀態。

> [!IMPORTANT]
> AKS 预览功能是自助服务和可以选择加入的功能。 提供预览是为了从我们的社区收集反馈和 bug。 但是，Azure 技术支持部门不为其提供支持。 如果你创建一个群集，或者将这些功能添加到现有群集，则除非该功能不再为预览版并升级为公开发布版 (GA)，否则该群集不会获得支持。
>
> 如果遇到预览版功能的问题，请[在 AKS GitHub 存储库中提交问题][aks-github]，并在 Bug 标题中填写预览版功能的名称。

## <a name="before-you-begin"></a>開始之前

您需要 Azure CLI 2.0.61 版或更新版本安裝並設定。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI][install-azure-cli]。

### <a name="install-aks-preview-cli-extension"></a>安裝 aks-preview CLI 擴充功能

CLI 命令來建立和管理多個節點的集區共有*aks 預覽*CLI 擴充功能。 安裝*aks 預覽*Azure CLI 擴充功能使用[az 延伸模組加入][ az-extension-add]命令，如下列範例所示：

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> 如果您先前已安裝*aks 預覽*延伸模組，安裝任何可用更新使用`az extension update --name aks-preview`命令。

### <a name="register-multiple-node-pool-feature-provider"></a>註冊多個節點集區的功能提供者

若要建立的 AKS 叢集，可以使用多個節點的集區，請先啟用您的訂用帳戶上的兩個功能旗標。 多節點的集區的叢集使用的虛擬機器擴展集 (VMSS)，來管理的部署和 Kubernetes 節點的組態。 註冊*MultiAgentpoolPreview*並*VMSSPreview*功能旗標使用[az 功能註冊][ az-feature-register]命令中所示下列範例：

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> 任何您已成功註冊之後，您建立的 AKS 叢集*MultiAgentpoolPreview*使用此預覽叢集體驗。 若要繼續建立一般、 完全支援的叢集，請勿啟用生產訂用帳戶上的預覽功能。 使用個別的測試或開發 Azure 訂用帳戶進行測試預覽功能。

狀態需要幾分鐘的時間才會顯示「已註冊」。 您可以使用 [az feature list][az-feature-list] 命令檢查註冊狀態：

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

準備就緒時，使用 [az provider register][az-provider-register] 命令重新整理 *Microsoft.ContainerService* 資源提供者的註冊：

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>限制

當您建立和管理支援多個節點的集區的 AKS 叢集時，就會套用下列限制：

* 多個節點的集區僅適用於您已成功註冊之後建立的叢集*MultiAgentpoolPreview*並*VMSSPreview*訂用帳戶的功能。 您無法加入或使用現有的 AKS 叢集建立之前已成功註冊這些功能的節點集區。
* 您無法刪除第一個節點集區。
* 無法使用的 HTTP 應用程式路由附加元件。

雖然這項功能處於預覽狀態，其他的限制如下：

* AKS 叢集中可以有最多八個節點的集區。
* AKS 叢集中可以有最多 400 個節點的跨這八個節點的集區。

## <a name="create-an-aks-cluster"></a>建立 AKS 叢集

若要開始，請使用單一節點的集區中建立 AKS 叢集。 下列範例會使用[az 群組建立][ az-group-create]命令來建立名為的資源群組*myResourceGroup*中*eastus*區域。 名為 AKS 叢集中*myAKSCluster*則使用建立[az aks 建立][ az-aks-create]命令。 A *-kubernetes 版本*的*1.12.6*用來說明如何更新在下列步驟中的節點集區。 您可以指定任何[支援的 Kubernetes 版本][supported-versions]。

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.12.6
```

建立叢集需要幾分鐘的時間。

備妥叢集時，使用[az aks get-credentials 來取得認證][ az-aks-get-credentials]命令，以用於取得叢集認證`kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>新增節點集區

在上一個步驟中所建立的叢集有單一節點的集區。 讓我們新增第二個的節點集區使用[az aks 節點的集區新增][ az-aks-nodepool-add]命令。 下列範例會建立名為節點集區*mynodepool*執行*3*節點：

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

若要查看節點集區的狀態，請使用[az aks 節點的集區清單][ az-aks-nodepool-list]命令並指定您的資源群組與叢集名稱：

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

下列範例輸出顯示*mynodepool*已成功建立具有三個節點的集區中的節點。 在上一個步驟中，預設值建立 AKS 叢集時*nodepool1*建立的節點計數*1*。

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

> [!TIP]
> 如果沒有*OrchestratorVersion*或是*VmSize*指定當您將加入的節點集區中，AKS 叢集的預設值會根據建立節點。 在此範例中，這是 Kubernetes 版本*1.12.6*和 節點大小*Standard_DS2_v2*。

## <a name="upgrade-a-node-pool"></a>升級的節點集區

在第一個步驟中，建立您的 AKS 叢集時`--kubernetes-version`的*1.12.6*所指定。 讓我們來升級*mynodepool* kubernetes *1.12.7*。 使用[az aks 節點的集區升級][ az-aks-nodepool-upgrade]命令來升級的節點集區中，如下列範例所示：

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.12.7 \
    --no-wait
```

列出您一次使用的節點集區的狀態[az aks 節點的集區清單][ az-aks-nodepool-list]命令。 下列範例顯示*mynodepool*處於*升級*狀態*1.12.7*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

花幾分鐘的時間升級的節點指定的版本。

最佳做法，您應該為相同的 Kubernetes 版本升級為 AKS 叢集中的所有節點集區。 升級個別的節點集區的能力可讓您執行輪流升級和排程維護應用程式執行時間的節點集區之間的 pod。

## <a name="scale-a-node-pool"></a>調整節點集區

為您的應用程式工作負載需求變化，您可能需要調整節點集區中的節點數目。 增加或相應減少，可以調整的節點數目。

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

若要調整節點集區中的節點數目，使用[az aks 節點的集區擴展][ az-aks-nodepool-scale]命令。 下列範例會調整集中的節點數目*mynodepool*要*5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

列出您一次使用的節點集區的狀態[az aks 節點的集區清單][ az-aks-nodepool-list]命令。 下列範例顯示*mynodepool*處於*調整*狀態的新計數*5*節點：

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Scaling              myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

它需要幾分鐘的時間調整規模作業完成。

## <a name="delete-a-node-pool"></a>刪除節點的集區

如果您不再需要集區，您可以將它刪除，並移除基礎的 VM 節點。 若要刪除的節點集區，請使用[az aks 節點的集區刪除][ az-aks-nodepool-delete]命令並指定節點的集區名稱。 下列範例會刪除*mynoodepool*先前步驟中建立：

> [!CAUTION]
> 沒有復原選項，當您刪除節點的集區時，可能會發生資料遺失。 如果 pod 無法排定在其他節點的集區上，這些應用程式都無法使用。 請確定在使用應用程式沒有資料備份或能夠在叢集中其他節點集區上執行時，請勿刪除節點的集區。

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

下列範例輸出[az aks 節點的集區清單][ az-aks-nodepool-list]命令會顯示*mynodepool*處於*刪除*狀態：

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Deleting             myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

花幾分鐘才能刪除節點和節點集區。

## <a name="specify-a-vm-size-for-a-node-pool"></a>指定節點的集區的 VM 大小

在上述範例中建立的節點集區，建立叢集中的節點使用的預設 VM 大小。 更常見的案例是為您建立具有不同的 VM 大小和功能 節點的集區。 例如，您可以建立包含大量的 CPU 或記憶體節點的節點集區或節點集區可提供 GPU 支援。 在下一個步驟中，您 [使用 taints 和 tolerations][#schedule-pods-using-taints-and-tolerations] 告訴 Kubernetes 排程器如何限制對 pod 的存取可在這些節點上執行。

在下列範例中，建立會使用以 GPU 為基礎的節點集區*Standard_NC6* VM 大小。 這些 Vm 是由 NVIDIA Tesla K80 卡提供。 如需可用 VM 大小的資訊，請參閱[在 Azure 中 Linux 虛擬機器的大小][vm-sizes]。

節點集區使用下列方法建立[az aks 節點的集區新增][ az-aks-nodepool-add]命令一次。 這次請指定名稱*gpunodepool*，並使用`--node-vm-size`參數來指定*Standard_NC6*大小：

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

下列範例輸出[az aks 節點的集區清單][ az-aks-nodepool-list]命令會顯示*gpunodepool*是*建立*節點指定*VmSize*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.12.6                 100             Linux     Creating             myResourceGroupPools  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

花費幾分鐘的時間*gpunodepool*已成功建立。

## <a name="schedule-pods-using-taints-and-tolerations"></a>使用 taints 和 tolerations 排程 pod

您現在有兩個節點的集區在您的叢集中預設節點集區一開始建立，並以 GPU 為基礎的節點集區。 使用[kubectl 取得節點][ kubectl-get]命令來檢視您的叢集中的節點。 下列範例輸出顯示每個節點的集區中的一個節點：

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.12.6
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.12.6
```

Kubernetes 排程器可以使用污點和容差來限制可以在節點上執行的工作負載。

* **污點**會套用至節點，該節點指示僅可以在其上排程特定的 pod。
* 然後**容差**會套用至容器，允許它們*容許*節點的污點。

如需有關如何使用進階 Kubernetes 排程功能，請參閱[AKS 中的進階排程器功能的最佳做法][taints-tolerations]

在此範例中，適用於誤導您以 GPU 為基礎的節點使用[kubectl 誤導節點][ kubectl-taint]命令。 指定您從先前的輸出以 GPU 為基礎的節點名稱`kubectl get nodes`命令。 套用誤導*索引鍵 / 值*，然後排程選項。 下列範例會使用*sku = gpu*配對，然後定義 pod 無權*NoSchedule*能力：

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

下列基本範例 YAML 資訊清單使用 toleration 允許 Kubernetes 排程器以 GPU 為基礎的節點上執行的 NGINX pod。 若要對 MNIST 資料集執行 Tensorflow 作業更合適，但需要大量時間的範例，請參閱[針對計算密集型工作負載在 AKS 上使用 Gpu][gpu-cluster]。

建立名為 `gpu-toleration.yaml` 的檔案，然後將下列範例 YAML 複製進來：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Pod 使用排程`kubectl apply -f gpu-toleration.yaml`命令：

```console
kubectl apply -f gpu-toleration.yaml
```

需要幾秒鐘的時間排程 pod 和提取 NGINX 映像。 使用[kubectl 描述 pod] [ kubectl-describe]命令來檢視 pod 的狀態。 下列扼要的範例輸出中會顯示*sku = gpu:NoSchedule* toleration 會套用。 在 [事件] 區段中，排程器已指派至的 pod *aks gpunodepool-28993262 vmss000000* GPU 為基礎的節點：

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

可以排定在節點上的已套用這個誤導的 pod *gpunodepool*。 會在排定的任何其他 pod *nodepool1*節點集區。 如果您建立其他的節點集區時，您可以使用其他 taints，以限制哪些 pod tolerations 可以排程在這些節點資源上。

## <a name="clean-up-resources"></a>清除資源

在本文中，您會建立 AKS 叢集，其中包含以 GPU 為基礎的節點。 若要減少不必要的成本，您可能想要刪除*gpunodepool*，或整個 AKS 叢集。

若要刪除以 GPU 為基礎的節點集區，請使用[az aks 節點的集區刪除][ az-aks-nodepool-delete]命令，在下列範例所示：

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

若要刪除叢集本身，請使用[az 群組刪除][ az-group-delete]命令來刪除 AKS 資源群組：

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>後續步驟

在這篇文章中，您學會如何建立和管理 AKS 叢集中的多個節點集區。 如需如何控制跨節點集區的 pod 的詳細資訊，請參閱[AKS 中的進階排程器功能的最佳做法][operator-best-practices-advanced-scheduler]。

<!-- EXTERNAL LINKS -->
[aks-github]: https://github.com/azure/aks/issues]
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
