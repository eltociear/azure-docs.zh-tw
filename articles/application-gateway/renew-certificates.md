---
title: 更新 Azure 應用程式閘道憑證
description: 了解如何更新與應用程式閘道接聽程式相關聯的憑證。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: c7c27f00b9f8b4fdcd8f735f842edb8f66803c6a
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278007"
---
# <a name="renew-application-gateway-certificates"></a>更新應用程式閘道憑證

如果您為應用程式閘道設定了 SSL 加密，您在某些時候將必須更新憑證。

您可以使用 Azure 入口網站、Azure PowerShell 或 Azure CLI 來更新與接聽程式相關聯的憑證：

## <a name="azure-portal"></a>Azure Portal

若要從入口網站更新接聽程式憑證，請瀏覽至您的應用程式閘道接聽程式。 按一下有憑證需要更新的接聽程式，然後按一下 [更新或編輯選取的憑證]。

![更新憑證](media/renew-certificate/ssl-cert.png)

上傳新的 PFX 憑證並為其命名，輸入密碼，然後按一下 [儲存]。

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

若要使用 Azure PowerShell 更新您的憑證，請使用下列指令碼：

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>後續步驟

若要了解如何設定與「Azure 應用程式閘道」搭配運作的「SSL 卸載」，請參閱 [設定 SSL 卸載](application-gateway-ssl-portal.md)
