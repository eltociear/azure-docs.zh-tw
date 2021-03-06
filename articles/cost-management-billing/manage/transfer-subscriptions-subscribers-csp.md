---
title: 在訂閱者與 CSP 之間轉移 Azure 訂用帳戶
description: 了解如何在訂閱者與 CSP 之間轉移 Azure 訂用帳戶。
services: billing
author: bandersmsft
ms.reviewer: dhgandhi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: banders
ms.openlocfilehash: 08aeac9a8cd21e6f13bab0ee10ba4d212d96c5b0
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2020
ms.locfileid: "76775409"
---
# <a name="transfer-azure-subscriptions-between-subscribers-and-csps"></a>在訂閱者與 CSP 之間轉移 Azure 訂用帳戶

本文概述在雲端解決方案提供者 (CSP) 合作夥伴及其客戶之間轉移 Azure 訂用帳戶的步驟。

## <a name="transfer-ea-subscriptions"></a>轉移 EA 訂用帳戶

已認證為 [Azure Expert 受控服務提供者 (MSP) 的](https://partner.microsoft.com/membership/azure-expert-msp)可以要求為具有直接 Enterprise 合約 (EA) 的客戶轉移 Azure 訂用帳戶。 只有已接受 Microsoft 客戶合約 (MCA) 並購買 Azure 方案的客戶才能轉移訂用帳戶。

當要求獲得核准後，CSP 就可以為其客戶提供合併的發票。 若要深入了解 CSP 的訂用帳戶轉移，請參閱[取得您 MPA 帳戶的 Azure 訂用帳戶帳單擁有權](mpa-request-ownership.md)。

## <a name="other-subscription-transfers-to-a-csp-partner"></a>其他訂用帳戶轉移至 CSP 合作夥伴

若要將任何其他 Azure 訂用帳戶轉移至 CSP 合作夥伴，訂閱者必須將資源從來源訂用帳戶移至 CSP 訂用帳戶。 使用下列指導方針在訂用帳戶之間移動資源。

1. 請與您的 CSP 合作夥伴合作，以建立目標 Azure CSP 訂用帳戶。
1. 請確定來源和目標 CSP 訂用帳戶位於相同的 Azure Active Directory (Azure AD) 租用戶中。  
    您無法變更 Azure CSP 訂用帳戶的 Azure AD 租用戶。 相反地，您必須將來源訂用帳戶新增至 CSP Azure AD 租用戶，或與之建立關聯。 如需詳細資訊，請參閱[將 Azure 訂用帳戶關聯或新增至您的 Azure Active Directory 租用戶](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)。
    > [!IMPORTANT]
    > - 當您將訂用帳戶與不同的 Azure AD 目錄建立關聯時，已使用[角色型存取控制 (RBAC)](../../role-based-access-control/role-assignments-portal.md)指派角色的使用者會失去其存取權。 傳統訂用帳戶管理員 (包括服務管理員和共同管理員) 也會失去存取權。 
    > - 當訂用帳戶與不同目錄建立關聯時，也會從訂用帳戶移除原則指派。
1. 您用來進行轉移的使用者帳戶必須擁有這兩個訂用帳戶的 [RBAC](add-change-subscription-administrator.md) 擁有者存取權。
1. 開始之前，請先[確認](/rest/api/resources/resources/validatemoveresources)所有 Azure 資源都可以從來源訂用帳戶移至目的地訂用帳戶。  
    某些 Azure 資源無法在訂用帳戶之間移動。 若要檢視可移動的 Azure 資源完整清單，請參閱[資源的移動作業支援](../../azure-resource-manager/management/move-support-resources.md)。
    > [!IMPORTANT]
    >  - Azure CSP 僅支援 Azure Resource Manager 資源。 如果來源訂用帳戶中的 Azure 資源是使用 Azure 傳統部署模型建立的，則必須先將其遷移至 [Azure Resource Manager](https://docs.microsoft.com/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-asm-to-arm)，才能進行移轉。 您必須是合作夥伴才能看到網頁。

1. 確認所有來源訂用帳戶服務都可使用 Azure Resource Manager 模型。 然後，使用 [Azure 資源移動](../../azure-resource-manager/management/move-resource-group-and-subscription.md)，將資源從來源訂用帳戶轉移至目的地訂用帳戶。
    > [!IMPORTANT]
    >  - 在訂用帳戶之間移動 Azure 資源可能會導致服務停機 (取決於訂用帳戶中的資源)。

## <a name="all-subscription-transfers-from-a-csp-partner"></a>從 CSP 合作夥伴轉移至所有訂用帳戶

若要將 CSP 合作夥伴中的任何其他訂用帳戶轉移至任何其他 Azure 供應項目，訂閱者必須在來源 CSP 訂用帳戶和目標訂用帳戶之間移動資源。

1. 建立目標 Azure 訂用帳戶。
1. 請確定來源和目標訂用帳戶位於相同的 Azure Active Directory (Azure AD) 租用戶中。 如需變更 Azure AD 租用戶的詳細資訊，請參閱[將 Azure 訂用帳戶關聯或新增至您的 Azure Active Directory 租用戶](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)。

    > [!IMPORTANT]
    >  - 當您將訂用帳戶與不同的目錄建立關聯時，已使用 [RBAC](../../role-based-access-control/role-assignments-portal.md)指派角色的使用者會失去其存取權。 傳統訂用帳戶管理員 (包括服務管理員和共同管理員) 也會失去存取權。
    >  - 當訂用帳戶與不同目錄建立關聯時，也會從訂用帳戶移除原則指派。

1. 您用來進行轉移的使用者帳戶必須擁有這兩個訂用帳戶的 [RBAC](add-change-subscription-administrator.md) 擁有者存取權。
1. 開始之前，請先[確認](/rest/api/resources/resources/validatemoveresources)所有 Azure 資源都可以從來源訂用帳戶移至目的地訂用帳戶。
    > [!IMPORTANT]
    >  - 某些 Azure 資源無法在訂用帳戶之間移動。 若要檢視可移動的 Azure 資源完整清單，請參閱[資源的移動作業支援](../../azure-resource-manager/management/move-support-resources.md)。

1. 使用 [Azure 資源移動](../../azure-resource-manager/management/move-resource-group-and-subscription.md)，將資源從來源訂用帳戶轉移至目的地訂用帳戶。
    > [!IMPORTANT]
    >  - 在訂用帳戶之間移動 Azure 資源可能會導致服務停機 (取決於訂用帳戶中的資源)。

## <a name="next-steps"></a>後續步驟
- [取得您 MPA 帳戶的 Azure 訂用帳戶帳單擁有權](mpa-request-ownership.md)。
- 了解如何[使用 Azure 計費管理帳戶和訂用帳戶](index.yml)。
