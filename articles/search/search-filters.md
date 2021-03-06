---
title: 篩選搜尋結果
titleSuffix: Azure Cognitive Search
description: 依使用者安全性身分識別、語言、地理位置或數值進行篩選，以減少 Azure 認知搜尋（Microsoft Azure 上託管的雲端搜尋服務）中查詢的搜尋結果。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f4ce3cd0db20f76aa6169f15254cf36ee64151a5
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406753"
---
# <a name="filters-in-azure-cognitive-search"></a>Azure 認知搜尋中的篩選 

*篩選*條件提供選取 Azure 認知搜尋查詢中使用之檔的準則。 未篩選的搜尋會包含索引中的所有文件。 篩選條件會將搜尋查詢的範圍設定為文件子集。 例如，篩選條件可以限制只對具有特定品牌或色彩且超過特定閾值之價格點的產品進行全文檢索搜尋。

某些搜尋體驗會強制實行篩選條件需求以作為實作的一部分，但是，當您想要使用「以值為基礎」的準則 (針對 "Simon & Schuster" 所發行的類別 "non-fiction"，將搜尋範圍設定為產品類型 "books") 來限制搜尋時，隨時都可使用篩選條件。

反之，如果您的目標是在特定資料「結構」上進行搜尋 (將搜尋範圍設定為客戶評論欄位)，則有替代方法，如下所述。

## <a name="when-to-use-a-filter"></a>使用篩選條件的時機

篩選條件是數個搜尋體驗的基礎，包括 "find near me"、多面向導覽和安全性篩選，以便只顯示允許使用者查看的文件。 如果您實作這其中任一個體驗，則篩選條件是必要的。 它是搜尋查詢附加的篩選條件，這類查詢可提供地理位置座標、使用者所選取的 Facet 類別或要求者的安全性識別碼。

範例案例包含下列項目：

1. 使用篩選條件，根據索引中的資料值來為索引配量。 假設有一個具有城市、房屋類型及便利設施的結構描述，您可能會建立一個篩選條件，明確選取滿足您準則 (位於西雅圖、公寓、濱水區) 的文件。 

   具備相同輸入的全文檢索搜尋通常會產生類似的結果，但篩選條件更為精確，因為它需要針對索引中的內容取得與篩選條件完全相符的項目。 

2. 如果搜尋體驗伴隨著篩選條件需求，請使用篩選條件：

   * [多面向導覽](search-faceted-navigation.md)會使用篩選條件，傳回使用者所選取的 Facet 類別。
   * 地理搜尋會使用篩選條件，在 "find near me" 應用程式中傳遞目前位置的座標。 
   * 安全性篩選條件會傳遞安全性識別碼作為篩選條件準則，索引中的相符項目則會作為用以存取文件權限的 Proxy。

3. 如果您想要數值欄位上的搜尋準則，請使用篩選條件。 

   數值欄位在文件中是可擷取且可出現在搜尋結果中，但不可單獨搜尋它們 (受限於全文檢索搜尋)。 如果您需要以數值資料為基礎的選取準則，請使用篩選條件。

### <a name="alternative-methods-for-reducing-scope"></a>縮減範圍的替代方法

如果您希望在搜尋結果中產生縮減效果，則篩選條件並非唯一的選擇。 視您的目標而定，這些替代選項可能更適合：

 + `searchFields` 查詢參數會將搜尋限制為特定欄位。 例如，如果您的索引分別提供適用於英文和西班牙文描述的欄位，您可以使用 searchFields，將目標設定為要用來進行全文檢索搜尋的欄位。 

+ `$select` 參數可用來指定要包含在結果集中的欄位，先有效地調整回應，然後將它傳送至呼叫應用程式。 此參數不會縮小查詢範圍或減少檔集合，但如果您的目標是較小的回應，則此參數是要考慮的選項。 

如需其中任一個參數的詳細資訊，請參閱[搜尋文件 > 要求 > 查詢參數](https://docs.microsoft.com/rest/api/searchservice/search-documents#request) \(英文\)。


## <a name="how-filters-are-executed"></a>如何執行篩選

在查詢時，篩選剖析器會接受準則做為輸入，將運算式轉換成以樹狀結構表示的不可部分完成的布林運算式，然後在索引的可篩選欄位上評估篩選樹狀結構。

篩選會與搜尋一併進行，以符合要在檔抓取和相關性評分的下游處理中包含哪些檔。 當與搜尋字串配對時，篩選準則會有效減少後續搜尋作業的重新叫用組。 單獨使用時 (例如，當查詢字串空白時，其中 `search=*`)，篩選條件準則會是唯一的輸入。 

## <a name="defining-filters"></a>定義篩選器
篩選器是 OData 運算式，使用[Azure 認知搜尋中支援的 Odata V4 語法子集](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)進行明確表述。 

您可以針對每個**搜尋**作業指定一個篩選準則，但篩選準則本身可以包含多個欄位、多個準則，而且如果您使用**ismatch**函數，多個全文檢索搜尋運算式。 在多部分的篩選條件運算式中，您可以依任何順序指定述詞（受限於運算子優先順序的規則）。 如果您嘗試以特定順序重新整理述詞，則不會有任何顯著的效能改善。

篩選條件運算式的其中一個限制是要求的大小上限。 整個要求 (包含篩選條件) 針對 POST 最多可有 16 MB，針對 GET 則是 8 KB。 您的篩選條件運算式中的子句數目也有限制。 最佳經驗法則是，如果您有數百個子句，則必須承擔達到限制的風險。 我們建議以這類不會產生無大小限制之篩選條件的方式來設計您的應用程式。

下列範例會呈現數個 API 中的典型篩選條件定義。

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?api-version=2019-05-06&search=*&$filter=Rooms/any(room: room/BaseRate lt 150.0)&$select=HotelId, HotelName, Rooms/Description, Rooms/BaseRate

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "Rooms/any(room: room/BaseRate lt 150.0)",
    "select": "HotelId, HotelName, Rooms/Description, Rooms/BaseRate"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(room: room/BaseRate lt 150.0)",
            Select = new[] { "HotelId", "HotelName", "Rooms/Description" ,"Rooms/BaseRate"}
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-usage-patterns"></a>篩選使用模式

下列範例說明篩選準則案例的數種使用模式。 如需更多概念，請參閱 [OData 運算式語法 > 範例](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples) \(英文\) 。

+ 獨立的 **$filter** (不含查詢字串)，在篩選條件運算式能夠完全限定為感興趣的文件時很有用。 沒有查詢字串，就沒有語彙或語言分析、沒有計分且沒有排名。 請注意，搜尋字串只是一個星號，這表示「符合所有檔」。

   ```
   search=*&$filter=Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Honolulu'
   ```

+ 查詢字串和 **$filter** 的組合，其中的篩選條件會建立子集，而查詢字串會提供字詞輸入，以便在篩選的子集上進行全文檢索搜尋。 新增詞彙（走距離電影院）會在結果中引進搜尋分數，其中最符合詞彙的檔會排名較高。 使用含有查詢字串的篩選是最常見的使用模式。

   ```
  search=walking distance theaters&$filter=Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Seattle'&$count=true
   ```

+ 複合式查詢 (以 "or" 分隔)，每個均有自己的篩選條件準則 (例如，'dog' 中的 'beagles' 或 'cat' 中的 'siamese')。 與 `or` 結合的運算式會個別進行評估，並包含符合回應中傳回之每個運算式的檔聯集。 此使用模式是透過 `search.ismatchscoring` 函式來達成。 您也可以使用非計分版本，`search.ismatch`。

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'Description') or Category eq 'Luxury'&$count=true
   ```

  您也可以使用 `and` 而不是 `or`，透過 `search.ismatchscoring` 結合全文檢索搜尋，但這在功能上等同于在搜尋要求中使用 `search` 和 `$filter` 參數。 例如，下列兩個查詢會產生相同的結果：

  ```
  $filter=search.ismatchscoring('pool') and Rating ge 4

  search=pool&$filter=Rating ge 4
  ```

繼續閱讀下列文章，以取得特定使用案例的完整指導方針：

+ [Facet 篩選條件](search-filters-facets.md)
+ [語言篩選條件](search-filters-language.md)
+ [安全性調整](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>進行篩選的欄位需求

在 REST API 中，簡單欄位的可篩選預設為 [*開啟*]。 可篩選的欄位會增加索引大小；請務必針對您不打算在篩選條件中實際使用的欄位設定 `"filterable": false`。 如需適用於欄位定義之設定的詳細資訊，請參閱[建立索引](https://docs.microsoft.com/rest/api/searchservice/create-index) \(英文\)。

在 .NET SDK 中，可篩選預設為 *off*。 您可以將對應[欄位](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field?view=azure-dotnet)物件的[IsFilterable 屬性](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet)設定為 `true`，以篩選欄位。 您也可以使用[IsFilterable 屬性](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute)，以宣告方式執行這項操作。 在下列範例中，屬性是在對應至索引定義之模型類別的 `BaseRate` 屬性上設定。

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>將現有欄位設為可篩選

您無法修改現有的欄位，使其可進行篩選。 相反地，您需要加入新的欄位，或重建索引。 如需重建索引或重新填入欄位的詳細資訊，請參閱[如何重建 Azure 認知搜尋索引](search-howto-reindex.md)。

## <a name="text-filter-fundamentals"></a>文字篩選條件基本概念

文字篩選準則會比對字串欄位與您在篩選準則中提供的常值字串。 不同于全文檢索搜尋，文字篩選不會有任何詞法分析或斷詞，因此比較僅適用于完全相符的專案。 例如，假設欄位*f*包含 "sunny day"，`$filter=f eq 'Sunny'` 不相符，但 `$filter=f eq 'sunny day'` 會。 

文字字串會區分大小寫。 大小寫不符合大寫的單字： `$filter=f eq 'Sunny day'` 將找不到 "sunny day"。

### <a name="approaches-for-filtering-on-text"></a>篩選文字的方法

| 方法 | 描述 | 使用時機 |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | 符合欄位的函式，可針對字串的分隔清單。 | 建議用於[安全性篩選](search-security-trimming-for-azure-search.md)和任何篩選準則，其中有許多原始文字值需要與字串欄位相符。 **Search.in**函數是針對速度而設計的，比使用 `eq` 和 `or`，針對每個字串明確地比較欄位更快。 | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | 此函式可讓您在相同的篩選條件運算式中，混用全文檢索搜尋作業以及純布林值篩選作業。 | 當您想要在一個要求中有多個搜尋篩選器組合時，請使用**ismatch** （或其評分對等的**ismatchscoring**）。 您也可以將它用於 *contains* 篩選條件，以篩選較大字串內的部分字串。 |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | 使用者定義的運算式，由欄位、運算子和值所組成。 | 當您想要在字串欄位與字串值之間尋找完全相符專案時，請使用此專案。 |

## <a name="numeric-filter-fundamentals"></a>數值篩選條件基本概念

數值欄位在全文檢索搜尋的內容中不是 `searchable`。 只有字串才能進行全文檢索搜尋。 例如，如果您輸入 99.99 作為搜尋字詞，則將不會得到價格為 $ 99.99 美元的項目。 相反地，您會看到在文件的字串欄位中有數字 99 的項目。 因此，如果您有數值資料，則很可能會用於篩選條件，包括範圍、Facet、群組等。 

包含數值欄位 (價格、大小、SKU、識別碼) 的文件會在搜尋結果中提供那些值，但前提是已將該欄位標記為 `retrievable`。 此處的重點是全文檢索搜尋本身不適用於數值欄位類型。

## <a name="next-steps"></a>後續步驟

首先，嘗試使用入口網站中的 [搜尋總管]，提交含有 **$filter** 參數的查詢。 [不動產範例索引](search-get-started-portal.md)會在您將下列篩選查詢貼至搜尋列時提供它們的有趣結果：

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

若要使用更多範例，請參閱 [OData 篩選條件運算式語法 > 範例](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples) \(英文\)。

## <a name="see-also"></a>另請參閱

+ [全文檢索搜尋在 Azure 認知搜尋中的運作方式](search-lucene-query-architecture.md)
+ [搜尋文件 REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [簡單查詢語法](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene 查詢語法](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [支援的資料類型](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
