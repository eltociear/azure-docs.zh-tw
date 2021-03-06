---
title: Azure 虛擬機器擴展集實例的終止通知
description: 瞭解如何啟用 Azure 虛擬機器擴展集實例的終止通知
author: shandilvarun
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 08/27/2019
ms.author: vashan
ms.openlocfilehash: a1b1e07fa0622ae25d8086ec65827816ec52a5ce
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271744"
---
# <a name="terminate-notification-for-azure-virtual-machine-scale-set-instances-preview"></a>Azure 虛擬機器擴展集實例的終止通知（預覽）
擴展集實例可以選擇接收實例終止通知，並將預先定義的延遲時間設定為終止作業。 終止通知會透過 Azure Metadata Service- [Scheduled Events](../virtual-machines/windows/scheduled-events.md)傳送，這會提供影響力作業的通知和延遲，例如重新開機和重新部署。 預覽解決方案會將另一個事件–終止–新增至 Scheduled Events 清單，而終止事件的相關延遲將取決於使用者在其擴展集模型設定中所指定的延遲限制。

註冊至功能之後，擴展集實例不需要等到指定的超時時間過期，就會刪除實例。 收到終止通知之後，實例可以選擇在終止超時時間到期之前隨時刪除。

> [!IMPORTANT]
> 擴展集實例的終止通知目前處於公開預覽狀態。 使用以下所述的公開預覽功能時，不需要任何加入宣告程式。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="enable-terminate-notifications"></a>啟用終止通知
有多種方式可在您的擴展集實例上啟用終止通知，如下列範例所述。

### <a name="rest-api"></a>REST API

下列範例會在擴展集模型上啟用終止通知。

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "virtualMachineProfile": {
            "scheduledEventsProfile": {
                "terminateNotificationProfile": {
                    "notBeforeTimeout":"PT5M",
                    "enable":true
                }
            }
        }
    }        
}

```

上述區塊會針對擴展集內所有實例上的任何終止作業，指定5分鐘的超時延遲（如*PT5M*所示）。 欄位*notBeforeTimeout*可以採用 ISO 8601 格式的5到15分鐘之間的任何值。 您可以修改 [ *terminateNotificationProfile* ] 底下所述的*notBeforeTimeout*屬性，以變更終止作業的預設超時。

在擴展集模型上啟用*scheduledEventsProfile*並設定*notBeforeTimeout*之後，請將個別的實例更新為[最新的模型](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)，以反映變更。

> [!NOTE]
>只有在使用 API 版本2019-03-01 和更新版本時，才能啟用擴展集實例上的終止通知

### <a name="azure-powershell"></a>Azure PowerShell
建立新的擴展集時，您可以使用[get-azvmss](/powershell/module/az.compute/new-azvmss) Cmdlet，在擴展集上啟用終止通知。

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -TerminateScheduledEvents
```

上述範例會建立新的擴展集，並啟用具有5分鐘預設超時時間的終止通知。 建立新的擴展集時，參數*TerminateScheduledEvents*不需要值。 若要變更 timeout 值，請透過*TerminateScheduledEventNotBeforeTimeoutInMinutes*參數指定所需的時間。

使用[get-azvmss](/powershell/module/az.compute/update-azvmss) Cmdlet，在現有的擴展集上啟用終止通知。

```azurepowershell-interactive
Update-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -TerminateScheduledEvents $true
  -TerminateScheduledEventNotBeforeTimeoutInMinutes 15
```
上述範例會在現有的擴展集上啟用終止通知，並設定終止事件的15分鐘超時。

在擴展集模型上啟用排程事件並設定超時之後，請將個別實例更新為[最新的模型](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)，以反映變更。

## <a name="get-terminate-notifications"></a>取得終止通知

終止通知會透過[Scheduled Events](../virtual-machines/windows/scheduled-events.md)（也就是 Azure Metadata Service）傳遞。 如果您是使用可由 VM 內存取的 REST 端點來執行虛擬機器，Azure 中繼資料服務會公開這類相關資訊。 此資訊可透過無法路由傳送的 IP 取得，因此不會在 VM 之外公開。

當您第一次提出事件要求時，會為您的擴展集啟用 Scheduled Events。 您可以預期第一次呼叫最多兩分鐘的延遲回應。 定期查詢端點，以偵測即將進行的維護事件和持續維護活動的狀態。

如果擴展集實例未提出要求24小時，您的擴展集就會停用 Scheduled Events。

### <a name="endpoint-discovery"></a>端點探索
針對已啟用 VNET 的 Vm，可從靜態不可路由傳送的 IP 169.254.169.254 取得 Metadata Service。

此預覽的最新 Scheduled Events 版本的完整端點為：
> 'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 '

### <a name="query-response"></a>查詢回應
回應包含排定的事件陣列。 空白陣列表示目前沒有任何排定的事件。

在有排定事件的情況下，回應會包含事件陣列。 對於「終止」事件，回應看起來會像這樣：
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Terminate",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled",
            "NotBefore": {timeInUTC},
        }
    ]
}
```
DocumentIncarnation 是 ETag，透過它很容易就能檢查自從上次查詢之後，事件裝載是否已變更。

如需上述每個欄位的詳細資訊，請參閱[Windows](../virtual-machines/windows/scheduled-events.md#event-properties)和[Linux](../virtual-machines/linux/scheduled-events.md#event-properties)的 Scheduled Events 檔。

### <a name="respond-to-events"></a>回應事件
一旦學會即將到來的事件並完成正常關機的邏輯之後，您可以使用 EventId 對中繼資料服務進行 POST 呼叫，以核准未完成的事件。 POST 呼叫會向 Azure 指出它可以繼續執行 VM 刪除。

以下是 POST 要求主體中預期的 json。 要求應該包含 StartRequests 的清單。 每個 StartRequest 都會包含您想要加速之事件的 EventId：
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

請確定擴展集中的每個 VM 只會核准與該 VM 相關的 EventID。 VM 可以[透過實例中繼資料](virtual-machine-scale-sets-instance-ids.md#instance-metadata-vm-name)取得自己的 vm 名稱。 這個名稱的格式為 "{scale-set-name} _ {instance-id}"，並會顯示在上述查詢回應的 [資源] 區段中。

您也可以參考使用[PowerShell](../virtual-machines/windows/scheduled-events.md#powershell-sample)和[Python](../virtual-machines/linux/scheduled-events.md#python-sample)查詢和回應事件的範例腳本。

## <a name="tips-and-best-practices"></a>祕訣和最佳作法
-   僅在「刪除」作業上終止通知–如果您的擴展集已啟用*scheduledEventsProfile* ，則所有刪除作業（手動刪除或自動調整起始的相應縮小）都會產生終止事件。 其他作業（如重新開機、重新安裝映射、重新部署和停止/解除配置）不會產生終止事件。 無法啟用低優先順序 Vm 的終止通知。
-   不需要等待超時時間–您可以在收到事件之後以及事件的*NotBefore*時間到期之前，隨時啟動終止作業。
-   在超時時強制刪除-預覽在產生事件之後，不會提供擴充超時值的任何功能。 當超時時間過期時，將會處理暫止的終止事件，而且 VM 將會被刪除。
-   可修改的超時值–您可以在刪除實例之前隨時修改 timeout 值，方法是修改擴展集模型上的*notBeforeTimeout*屬性，並將 VM 實例更新為最新的模型。
-   核准所有暫止的刪除–如果 VM_1 上有未核准的暫止刪除，而且您已核准 VM_2 上的另一個終止事件，則 VM_2 不會刪除，直到 VM_1 的終止事件已核准，或其超時時間已過。 一旦您核准 VM_1 的終止事件之後，VM_1 和 VM_2 都會被刪除。
-   核准所有同時刪除–延伸上述範例，如果 VM_1 且 VM_2 具有相同的*NotBefore*時間，則必須核准這兩個終止事件，或在超時時間到期之前，都不會刪除 VM。

## <a name="troubleshoot"></a>疑難排解
### <a name="failure-to-enable-scheduledeventsprofile"></a>無法啟用 scheduledEventsProfile
如果您收到「BadRequest」錯誤，並出現錯誤訊息，指出「在 ' VirtualMachineProfile ' 類型的物件上找不到成員 ' scheduledEventsProfile '」，請檢查用於擴展集作業的 API 版本。 此預覽需要計算 API 版本**2019-03-01**或更高版本。

### <a name="failure-to-get-terminate-events"></a>無法取得終止事件
如果您未透過 Scheduled Events 取得任何**終止**事件，請檢查用來取得事件的 API 版本。 終止事件需要 Metadata Service API **2019-01-01**或更高版本。
>'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 '

### <a name="getting-terminate-event-with-incorrect-notbefore-time"></a>以不正確的 NotBefore 時間取得終止事件  
在擴展集模型上啟用*scheduledEventsProfile*並設定*notBeforeTimeout*之後，請將個別的實例更新為[最新的模型](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)，以反映變更。

## <a name="next-steps"></a>後續步驟
了解如何在虛擬機器擴展集上[部署您的應用程式](virtual-machine-scale-sets-deploy-app.md)。
