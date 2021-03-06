---
title: 調整查詢和索引工作負載的容量
titleSuffix: Azure Cognitive Search
description: 調整 Azure 認知搜尋中的磁碟分割和複本電腦資源，其中每個資源都是以可計費的搜尋單位計價。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 349587063c528fef1cbdb09d84e61e82443d45d1
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906723"
---
# <a name="scale-up-partitions-and-replicas-to-add-capacity-for-query-and-index-workloads-in-azure-cognitive-search"></a>相應增加資料分割和複本，以在 Azure 認知搜尋中新增查詢和索引工作負載的容量

在您[選擇定價層](search-sku-tier.md)和[佈建搜尋服務](search-create-service-portal.md)之後，下一個步驟是選擇性地增加服務所使用的複本或分割區數目。 每一層都提供固定的計費單位數目。 本文說明如何配置這些單位以達到最佳的組態，讓您在查詢執行、編制索引和儲存體等需求之間取得平衡。

當您在[基本層](https://aka.ms/azuresearchbasic)或其中一個[標準或儲存體優化層](search-limits-quotas-capacity.md)中設定服務時，可以使用資源設定。 對於這些層的服務，購買容量就是增加「搜尋單位」(SU)，每個分割區和複本會計為一個 SU。 

使用較少 SU，帳單費用也會相應降低。 只要服務處於已設定的狀態，就會持續計費。 如果您暫時不使用某項服務，避免計費的唯一方法就是刪除該服務，然後當您需要該服務時再予以重建。

> [!Note]
> 刪除服務會刪除它上面的一切。 Azure 認知搜尋內沒有任何機制可備份和還原持續性搜尋資料。 若要將現有的索引重新部署在新的服務上，您應該執行原先用來建立和載入此索引的程式。 

## <a name="terminology-replicas-and-partitions"></a>術語：複本和分割區
複本和分割區是回到搜尋服務的主要資源。

| 資源 | 定義 |
|----------|------------|
|*分割數* | 為讀寫作業 (例如，在重建或重新整理索引時) 提供索引儲存體和 I/O。|
|*複本* | 搜尋服務的執行個體，主要用來讓查詢作業達到負載平衡。 每個複本一律會裝載一份索引。 如果您有 12 個複本，服務上就會分別載入 12 份索引。|

> [!NOTE]
> 沒有任何方法可直接操作或管理哪些索引會在複本上執行。 每個複本上各有一份索引是服務架構的一部分。
>


## <a name="how-to-allocate-replicas-and-partitions"></a>如何配置複本和分割區
在「Azure 認知搜尋」中，一開始會配置一項服務，其中包含一個資料分割和一個複本的最低層級資源。 如果階層支援，您便可以增量調整運算資源，方法是在您需要較多的儲存體和 I/O 時新增分割區，或新增更多複本來因應較大的查詢磁碟區或提供較佳的效能。 單一服務必須具有足夠的資源，才能處理所有工作負載 (編製索引和查詢)。 您無法將多個服務之間的工作負載再加以細分。

若要增加或變更複本和分割區的配置，建議使用 Azure 入口網站。 入口網站會對允許的組合強制執行限制，而這會維持在最大限制之下。 如果您需要以腳本為基礎或以程式碼為基礎的布建方法， [Azure PowerShell](search-manage-powershell.md)或[管理 REST API](https://docs.microsoft.com/rest/api/searchmanagement/services)為替代解決方案。

一般而言，搜尋應用程式需要的複本數量會多於分割區數量，特別是在服務作業偏向查詢工作負載的情況下。 [高可用性](#HA) 一節將會說明原因。

1. 登入 [Azure 入口網站](https://portal.azure.com/)，然後選取搜尋服務。

2. 在 [**設定**] 中，開啟 [**調整**] 頁面以修改複本和分割區。 

   下列螢幕擷取畫面顯示以一個複本和分割區布建的標準服務。 底部的公式表示正在使用的搜尋單位數目（1）。 如果單價為 $100 （不是真正的價格），執行這項服務的每月成本會平均為 $100。

   ![顯示目前值的縮放頁面](media/search-capacity-planning/1-initial-values.png "顯示目前值的縮放頁面")

3. 使用滑杆來增加或減少分割區的數目。 底部的公式表示正在使用多少個搜尋單位。

   這個範例會將容量加倍，其中每個都有兩個複本和分割區。 請注意搜尋單位元數目;這現在是四個，因為計費公式是複本乘以分割區（2 x 2）。 比起執行服務的成本翻倍，使容量加倍。 如果搜尋單位成本為 $100，則新的每月帳單現在會是 $400。

   如需每個層級的目前每個單位成本，請造訪[定價頁面](https://azure.microsoft.com/pricing/details/search/)。

   ![新增複本和分割區](media/search-capacity-planning/2-add-2-each.png "新增複本和分割區")

3. 按一下 [**儲存**] 以確認變更。

   ![確認變更規模和計費](media/search-capacity-planning/3-save-confirm.png "確認變更規模和計費")

   容量變更需要數小時才能完成。 您無法在進程啟動後取消，而且不會即時監視複本和分割區的調整。 不過，在進行變更時，仍會顯示下列訊息。

   ![入口網站中的狀態訊息](media/search-capacity-planning/4-updating.png "入口網站中的狀態訊息")


> [!NOTE]
> 服務在佈建之後，即無法升級到較高的 SKU。 您必須在新層中建立搜尋服務，然後重新載入您的索引。 如需有關服務布建的說明，請參閱[在入口網站中建立 Azure 認知搜尋服務](search-create-service-portal.md)。
>
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>資料分割與複本組合

「基本」服務可以有不多不少 1 個分割區及最多 3 個複本，上限為 3 個 SU。 唯一可調整的資源是複本。 您至少需要 2 個複本，才能在查詢上達到高可用性。

所有標準和儲存體優化的搜尋服務都可以採用下列複本和分割區組合，受限於 36-SU 限制。 

|   | **1 個資料分割** | **2 個資料分割** | **3 個資料分割** | **4 個資料分割** | **6 個資料分割** | **12 個資料分割** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 個複本** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 個複本** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 個複本** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 個複本** |4 SU |8 SU |12 SU |16 SU |24 SU |N/A |
| **5 個複本** |5 SU |10 SU |15 SU |20 SU |30 SU |N/A |
| **6 個複本** |6 SU |12 SU |18 SU |24 SU |36 SU |N/A |
| **12 個複本** |12 SU |24 SU |36 SU |N/A |N/A |N/A |

SU、定價和容量會在 Azure 網站上詳細說明。 如需詳細資訊，請參閱[定價詳細資料](https://azure.microsoft.com/pricing/details/search/)。

> [!NOTE]
> 複本數和資料分割數可整除 12 (明確來說就是 1、2、3、4、6、12)。 這是因為 Azure 認知搜尋會將每個索引預先分割成12個分區，以便將其分散到所有資料分割的相同部分。 例如，如果您的服務有三個資料分割，而您建立了索引，則每個資料分割將會包含 4 個該索引的分區。 Azure 認知搜尋如何分區索引是一個執行詳細資料，在未來的版本中可能有所變更。 雖然現在分區數為 12，但您不應預期未來該數字永遠都會是 12。
>


<a id="HA"></a>

## <a name="high-availability"></a>高可用性
由於相應增加非常快速簡單，我們通常建議您從一個資料分割和一或兩個複本開始，然後於查詢量增長時再相應增加。 查詢工作負載主要是在複本上執行。 如果您需要更多的輸送量或高可用性，可能會需要額外的複本。

針對高可用性的一般建議為：

* 針對唯讀工作負載 (查詢)，需有 2 個複本才能達到高可用性

* 針對讀寫工作負載 (查詢再加上新增、更新或刪除個別文件時的索引編製)，需有 3 個或更多個複本才能達到高可用性

Azure 認知搜尋的服務等級協定（SLA）是以查詢作業和包含新增、更新或刪除檔的索引更新為目標。

基本層最多一個分割區和三個複本。 如果想要具備立即回應檢索和查詢輸送量需求波動的彈性，請考慮標準層的其中一個。  如果您發現儲存體需求的成長速度比查詢輸送量快，請考慮其中一個儲存體優化層級。

### <a name="index-availability-during-a-rebuild"></a>索引在重建期間的可用性

Azure 認知搜尋的高可用性適用于不需要重建索引的查詢和索引更新。 如果您刪除欄位、變更資料類型，或是重新命名欄位，您必須重建索引。 若要重建索引，您必須刪除索引、重新建立索引，並重新載入資料。

> [!NOTE]
> 您可以將新欄位新增至 Azure 認知搜尋索引，而不需重建索引。 所有已在索引中之文件的新欄位值將為 null。

重建索引時，將會有一段時間會將資料加入至新的索引。 如果您想要在這段期間繼續使用舊索引，您必須在相同的服務上有一個具有不同名稱的舊索引複本，或在不同的服務上有相同名稱的索引複本。，然後在您的程式碼中提供重新導向或容錯移轉邏輯。

## <a name="disaster-recovery"></a>災害復原
目前沒有任何內建的機制可進行災害復原。 因此新增資料分割或複本並不是達成災害復原目標的正確選擇。 最常見的方法是在其他區域中設定第二個搜尋服務，來新增服務等級的備援。 與索引重建期間的可用性一樣，重新導向或容錯移轉邏輯必須來自您的程式碼。

## <a name="increase-query-performance-with-replicas"></a>利用複本提高查詢效能
查詢延遲是一項指標，代表需要額外的複本。 一般而言，改善查詢效能的第一步是新增更多這項資源。 當您新增複本時，額外的幾份索引會上線以支援較大的查詢工作負載，以及讓多個複本上的要求達到負載平衡。

我們無法提供固定的每秒查詢數目 (QPS) 預估值：查詢效能取決於查詢和競爭工作負載的複雜性。 雖然新增複本會明顯獲得更好的效能，但是最終結果並不會完全地呈線性關係：新增 3 個複本並不保證有 3 倍的輸送量。

如需估計工作負載之 QPS 的指引，請參閱[Azure 認知搜尋效能和優化考慮](search-performance-optimization.md)。

## <a name="increase-indexing-performance-with-partitions"></a>使用資料分割提高編製索引的效能
要求近乎即時資料重新整理的搜尋應用程式，按比例需要比複本更多的資料分割數。 新增資料分割可將讀取/寫入作業分配到更大量的計算資源。 它也提供更多磁碟空間來儲存額外的索引和文件。

索引越大，查詢所需的時間就越長。 這麼一來，您可能會發現每個資料分割中每個累加式的增加在複本中都需要有較小但按比例的增加。 要將查詢執行速度提高到何種程度，需將您的查詢和查詢磁碟區複雜性納入考量。


## <a name="next-steps"></a>後續步驟

[選擇 Azure 認知搜尋的定價層](search-sku-tier.md)
