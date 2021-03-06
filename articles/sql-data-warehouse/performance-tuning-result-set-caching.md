---
title: 使用結果集快取進行效能微調
description: Azure SQL 資料倉儲的結果集快取功能總覽
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.custom: seo-lt-2019
ms.openlocfilehash: 461320b9c3ed48176fb60fe695704c582edcd552
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692949"
---
# <a name="performance-tuning-with-result-set-caching"></a>使用結果集快取進行效能微調  
啟用結果集快取時，Azure SQL 資料倉儲會自動快取使用者資料庫中的查詢結果，以供重複使用。  這可讓後續的查詢執行直接從持續性快取取得結果，因此不需要重新計算。   結果集快取可改善查詢效能並減少計算資源使用量。  此外，使用快取結果集的查詢不會使用任何平行存取插槽，因此不會計算現有的並行限制。 基於安全性，如果使用者與建立快取結果的使用者具有相同的資料存取權限，則只能存取快取的結果。  

## <a name="key-commands"></a>主要命令
[開啟/關閉使用者資料庫的結果集快取](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azure-sqldw-latest)

[開啟/關閉會話的結果集快取](https://docs.microsoft.com/sql/t-sql/statements/set-result-set-caching-transact-sql?view=azure-sqldw-latest)

[檢查快取結果集的大小](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?view=azure-sqldw-latest)  

[清除快取](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?view=azure-sqldw-latest)

## <a name="whats-not-cached"></a>未快取的內容  

一旦開啟資料庫的結果集快取，就會快取所有查詢的結果，直到快取填滿為止（這些查詢除外）：
- 使用不具決定性的函數（例如 DateTime）進行查詢。 Now （）
- 使用使用者定義函數的查詢
- 使用已啟用資料列層級安全性或資料行層級安全性之資料表的查詢
- 傳回資料列大小大於64KB 的查詢

> [!IMPORTANT]
> 建立結果集快取，以及從快取取出資料的作業，會發生在資料倉儲實例的控制節點上。 開啟結果集快取時，執行傳回大型結果集的查詢（例如，> 1 百萬個數據列）可能會導致控制節點上的 CPU 使用率過高，並使實例上的整體查詢回應變慢。  這些查詢通常會在資料探索或 ETL 作業期間使用。 若要避免不足負荷過重控制節點並造成效能問題，使用者應該先關閉資料庫的結果集快取，再執行這些類型的查詢。  

針對查詢的結果集快取作業所花費的時間執行此查詢：

```sql
SELECT step_index, operation_type, location_type, status, total_elapsed_time, command 
FROM sys.dm_pdw_request_steps 
WHERE request_id  = <'request_id'>; 
```

以下是使用已停用結果集快取所執行之查詢的範例輸出。

![查詢-使用-rsc-已停用](media/performance-tuning-result-set-caching/query-steps-with-rsc-disabled.png)

以下是在啟用結果集快取的情況下執行之查詢的範例輸出。

![查詢-具有-rsc-已啟用的步驟](media/performance-tuning-result-set-caching/query-steps-with-rsc-enabled.png)

## <a name="when-cached-results-are-used"></a>使用快取的結果時

如果符合下列所有需求，則會針對查詢重複使用快取的結果集：
- 執行查詢的使用者可以存取查詢中參考的所有資料表。
- 新查詢與產生結果集快取的前一個查詢之間有完全相符的情況。
- 從產生快取結果集的資料表中，沒有任何資料或架構變更。

執行此命令，以檢查查詢是否以結果快取點擊或遺漏的方式執行。 如果發生快取命中，result_cache_hit 會傳回1。

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests 
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>管理快取的結果 

結果集快取的大小上限為每個資料庫 1 TB。  當基礎查詢資料變更時，快取的結果會自動失效。  

快取收回是依照此排程自動進行 Azure SQL 資料倉儲管理： 
- 如果結果集尚未使用或已失效，則每隔48小時。 
- 當結果集快取接近大小上限時。

使用者可以使用下列其中一個選項，手動清空整個結果集快取： 
- 關閉資料庫的結果集快取功能 
- 連接到資料庫時執行 DBCC DROPRESULTSETCACHE

暫停資料庫不會清空快取的結果集。  

## <a name="next-steps"></a>後續步驟
如需更多開發秘訣，請參閱 [SQL 資料倉儲開發概觀](sql-data-warehouse-overview-develop.md)。 
