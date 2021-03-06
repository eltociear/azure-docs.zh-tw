---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/10/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 50ce8530aca40eed07741f35be1a57bbd7cc1868
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133611"
---
以較高的權限開啟 PowerShell 主控台。

如果您是以本機執行 Azure PowerShell，請連線到您的 Azure 帳戶。 *Connect-AzAccount* Cmdlet 會提示您輸入認證。 驗證後即會下載您的帳戶設定供 Azure PowerShell 使用。 如果您改為使用 Azure Cloud Shell，就不需要執行*disconnect-azaccount*。 Azure Cloud Shell 會自動連接到您的 Azure 帳戶。

```azurepowershell
Connect-AzAccount
```

如果您有多個訂用帳戶，請取得 Azure 訂用帳戶的清單。

```azurepowershell-interactive
Get-AzSubscription
```

指定您要使用的訂用帳戶。

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```