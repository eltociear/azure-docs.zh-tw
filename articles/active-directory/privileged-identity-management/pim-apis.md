---
title: 適用於 PIM 的 Microsoft Graph API (預覽) - Azure AD | Microsoft Docs
description: 針對如何使用適用於 Azure AD Privileged Identity Management (PIM) 的 Microsoft Graph API (預覽) 提供相關資訊。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 01/02/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6da6bffbc54bfa6e9c39ddace665eb7cfec58614
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2020
ms.locfileid: "75638658"
---
# <a name="microsoft-graph-apis-for-privileged-identity-management-preview"></a>適用於 Privileged Identity Management 的 Microsoft Graph API (預覽)

您可以使用適用於 Azure Active Directory 的 [Microsoft Graph API](https://developer.microsoft.com/graph/docs/concepts/overview) 來執行所有 Privileged Identity Management 工作。 本文將說明使用適用於 Privileged Identity Management 的 Microsoft Graph AP 時，應了解的重要概念。

如需有關 Microsoft Graph API 的詳細資訊，請參閱 [Azure AD Privileged Identity Management API 參考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)。

> [!IMPORTANT]
> Microsoft Graph 中 /beta 版本底下的 API 皆處於預覽階段，可能會有所變更。 不支援在生產應用程式中使用這些 API。
>
> 雖然所有 PIM 作業都可透過圖形 API 命令來支援，但我們仍使用不同的系統來核准啟用要求。 核准的圖形 API 目前正在開發，將在未來的幾個月內發行。

## <a name="required-permissions"></a>所需的權限

若要呼叫適用於 Privileged Identity Management 的 Microsoft Graph API，您必須擁有下列**一個或多個**權限：

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>設定權限

應用程式若要呼叫適用於 Privileged Identity Management 的 Microsoft Graph API，則一定要有必要權限。 指定必要權限的最簡單方式是使用 [Azure AD 同意架構](../develop/consent-framework.md)。

### <a name="set-permissions-in-graph-explorer"></a>在 Graph 總管中的設定權限

如果您使用 Graph 總管來測試呼叫，您可以在工具中指定權限。

1. 以全域管理員的身分登入 [Graph 總管](https://developer.microsoft.com/graph/graph-explorer)。

1. 按一下 [修改權限]  。

    ![Graph 總管 - 修改權限](./media/pim-apis/graph-explorer.png)

1. 在您想要包含的權限旁選取核取方塊。 Graph 總管中尚無法使用 `PrivilegedAccess.ReadWrite.AzureAD`。

    ![Graph 總管 - 修改權限](./media/pim-apis/graph-explorer-modify-permissions.png)

1. 按一下 [修改權限]  以套用權限變更。

## <a name="next-steps"></a>後續步驟

- [Azure AD Privileged Identity Management API 參考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
