---
title: 使用 Azure Cosmos 帳戶的帳戶金鑰和連接字串
description: 使用 Azure Cosmos 帳戶的帳戶金鑰和連接字串
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: e766f52d729a4f916eefd2b148d926d929b4f540
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275115"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>透過 Azure CLI 使用 Azure Cosmos 帳戶的帳戶金鑰和連接字串

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

如果您選擇在本機安裝和使用 CLI，本主題會要求您執行 Azure CLI 2.0.73 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli)。

## <a name="sample-script"></a>範例指令碼

此指令碼示範四項作業。

- 列出所有帳戶金鑰
- 列出唯讀帳戶金鑰
- 列出連接字串
- 重新產生帳戶金鑰

> [!NOTE]
> 此範例示範如何使用 SQL (Core) API 帳戶，但帳戶金鑰和連接字串作業在 Cosmos DB 的所有資料庫 API 中都是相同的。

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/keys.sh "Keys and connection string operations for Cosmos DB.")]

## <a name="clean-up-deployment"></a>清除部署

在執行過指令碼範例之後，您可以使用下列命令來移除資源群組和所有與其相關聯的資源。

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| 命令 | 注意 |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 建立用來存放所有資源的資源群組。 |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | 建立 Azure Cosmos DB 帳戶。 |
| [az cosmosdb keys list](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) | 列出 Azure Cosmos DB 帳戶的金鑰。 |
| [az cosmosdb list-read-only-keys](/cli/azure/cosmosdb#az-cosmosdb-list-read-only-keys) | 列出 Azure Cosmos DB 帳戶的唯讀金鑰。 |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | 列出 Azure Cosmos DB 帳戶的連接字串。 |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | 重新產生 Azure Cosmos DB 帳戶的金鑰。 |
| [az group delete](/cli/azure/resource#az-resource-delete) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟

如需 Azure Cosmos DB CLI 的詳細資訊，請參閱 [Azure Cosmos DB CLI 文件](/cli/azure/cosmosdb)。

您可以在 [Azure Cosmos DB CLI GitHub 存放庫](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 指令碼範例。
