---
title: 建立管理群組來組織資源-Azure 治理
description: 了解如何使用入口網站、Azure PowerShell 和 Azure CLI 建立 Azure 管理群組來管理多個資源。
ms.date: 12/18/2019
ms.topic: conceptual
ms.openlocfilehash: d9bb2e82404c0188094298f40da3346ee132eec3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75436538"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>建立用於資源組織及管理的管理群組

管理群組是可協助您跨多個訂用帳戶管理容器存取、原則及合規性的容器。 建立這些容器來建置可與 [Azure 原則](../policy/overview.md)和 [Azure 角色型存取控制](../../role-based-access-control/overview.md)搭配使用的高效率階層。 如需有關管理群組的詳細資訊，請參閱[使用 Azure 管理群組來組織資源](overview.md)。

在目錄中建立的第一個管理群組可能需要 15 分鐘的時間才能完成。 第一次在 Azure 中設定目錄的管理群組服務時會執行一些程序。 程序完成時，您會收到通知。 如需詳細資訊，請參閱[管理群組的初始設定](./overview.md#initial-setup-of-management-groups)。 

## <a name="create-a-management-group"></a>建立管理群組

租使用者中的任何 Azure AD 使用者都可以建立管理群組，而不需將管理群組寫入權限指派給該使用者。  這個新的管理群組將是根管理群組的子系，而建立者將會獲得「擁有者」角色指派。 管理群組服務允許此功能，因此不需要根層級的角色指派。 建立根管理群組時，沒有任何使用者可以存取它。  為了避免尋找 Azure AD 全域管理員開始使用管理群組的障礙，我們允許在根層級建立初始管理群組。      

您可以使用入口網站、PowerShell 或 Azure CLI 來建立管理群組。 目前，您無法使用 Resource Manager 範本來建立管理群組。

### <a name="create-in-portal"></a>在入口網站中建立

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 選取 **所有服務** > **管理 + 治理**。

1. 選取**成本管理 + 帳單**

1. 在 成本管理 + 帳單-管理群組 頁面上，選取 **管理群組**

1. 選取 [ **+ 新增管理群組**]。

   ![使用管理群組的頁面](./media/main.png)

1. 填寫管理群組識別碼欄位。

   - [管理群組識別碼] 是用來在此管理群組上提交命令的目錄唯一識別碼。 此識別碼在建立後即無法編輯，因為整個 Azure 系統會使用它來識別此群組。 系統會使用識別碼 Azure Active Directory 識別碼，自動建立[根管理群組](overview.md#root-management-group-for-each-directory)。 針對所有其他管理群組，請指派唯一的識別碼。
   - [顯示名稱] 欄位是顯示在 Azure 入口網站內的名稱。 在建立管理群組時，不同的顯示名稱是選擇性欄位，並且可以隨時進行變更。  

   ![用於建立新管理群組的選項窗格](./media/create_context_menu.png)  

1. 選取 [儲存]。

### <a name="create-in-powershell"></a>在 PowerShell 中建立

針對 PowerShell，請使用[AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) Cmdlet 來建立新的管理群組。

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** 是要建立的唯一識別碼。 其他命令會使用此識別碼來參考此群組，而且稍後無法變更。

如果您想要管理群組在 Azure 入口網站中顯示不同的名稱，請新增**DisplayName**參數。 例如，若要以 Contoso 的組名和顯示名稱 "Contoso Group" 建立管理群組，請使用下列 Cmdlet：

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

在上述範例中，會在根管理群組下建立新的管理群組。 若要指定不同的管理群組做為父系，請使用**ParentId**參數。

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>在 Azure CLI 中建立

針對 Azure CLI，請使用[az account management-group create](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create)命令來建立新的管理群組。

```azurecli-interactive
az account management-group create --name Contoso
```

**名稱**是要建立的唯一識別碼。 其他命令會使用此識別碼來參考此群組，而且稍後無法變更。

如果您想要管理群組在 Azure 入口網站中顯示不同的名稱，請新增**顯示名稱**參數。 例如，若要以 Contoso 的組名和顯示名稱 "Contoso Group" 建立管理群組，請使用下列命令：

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

在上述範例中，會在根管理群組下建立新的管理群組。 若要指定不同的管理群組做為父系，請使用**parent**參數並提供父群組的名稱。

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>後續步驟

若要深入了解管理群組，請參閱：

- [建立管理群組以組織 Azure 資源](create.md)
- [如何變更、刪除或管理您的管理群組](manage.md)
- [檢閱 Azure PowerShell 資源模組中的管理群組](/powershell/module/az.resources#resources)
- [檢閱 REST API 中的管理群組](/rest/api/resources/managementgroups)
- [檢閱 Azure CLI 中的管理群組](/cli/azure/account/management-group)
