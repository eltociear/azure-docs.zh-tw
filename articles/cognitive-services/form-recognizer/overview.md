---
title: 什麼是表單辨識器？
titleSuffix: Azure Cognitive Services
description: Microsoft 認知服務表單辨識器可讓您識別和擷取表單文件中的索引鍵/值組和資料表資料。
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 91ea2b68828ac54d4128a90550e9c60e065b719d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75379418"
---
# <a name="what-is-form-recognizer"></a>什麼是表單辨識器？

Azure 表單辨識器是一項認知服務，其使用機器學習技術來識別和擷取表單文件中的文字、索引鍵/值組和資料表資料。 其會從表單中擷取文件，並輸出在原始檔中包含關聯性的結構化資料。 您可以快速取得針對特定內容量身打造的精確結果，而不需要大量手動操作或廣泛的資料科學專業知識。 表單辨識器是由自訂模型、預建的收據模型和版面配置 API 所組成。 您可以使用 REST API 呼叫表單辨識器模型，以降低複雜度並將其整合至工作流程或應用程式。

表單辨識器是由下列服務所組成：
* **自訂模型** - 從表單擷取索引鍵/值組和資料表資料。 這些模型會使用您自己的資料進行定型，因此會針對您的表單量身打造。
* **預建的收據模型** - 使用預建的模型，從美國銷售收據中擷取資料。
* **版面配置 API** - 從文件中擷取文字和資料表結構，以及其週框方塊座標。

<!-- add diagram -->

## <a name="custom-models"></a>自訂模型

表單辨識器自訂模型可根據您自己的資料來定型，而您只需要五個輸入表單範例即可開始。 已定型的模型可以輸出在原始表單文件中包含關聯性的結構化資料。 定型模型後，您就可以測試及重新定型模型，並於最終根據您的需求用此模型來從更多的表單可靠地擷取資料。

當您將自訂模型定型時，可以選擇下列選項：使用標記資料和不使用標記資料進行定型。

### <a name="train-without-labels"></a>不使用標籤進行定型

根據預設，表單辨識器會使用不受監督的學習來了解表單中欄位與項目之間的版面配置和關聯性。 當您提交輸入表單時，演算法會依類型將表單群集、探索目前有哪些索引鍵和資料表，並將值與索引鍵以及將項目與資料表相關聯。 這不需要手動資料標記或大量編碼和維護，因此建議您先嘗試此方法。

### <a name="train-with-labels"></a>使用標籤進行定型

當您使用標記資料進行定型時，此模型會使用您提供的標記表單，進行受監督式學習來擷取感興趣的值。 這會造就表現更佳的模型，並可產生可搭配複雜表單或含有值 (但不含索引鍵) 的表單使用的模型。

表單辨識器會使用[版面配置 API](#layout-api) 來學習所列印和手寫文字元素的預期大小和位置。 然後，其會使用使用者指定的標籤來學習文件中的索引鍵/值關聯。 建議您使用相同類型的五個手動標記表單，以便在定型新模型時開始使用，並視需要新增更多標記資料，以改善模型精確度。

## <a name="prebuilt-receipt-model"></a>預先建置的回條模型

表單辨識器也包含可供讀取美國英文銷售收據的模型，&mdash;餐廳、加油站、零售等所使用的類型 ([範例收據](./media/contoso-receipt-small.png))。 此模型會擷取索引鍵資訊，例如交易的時間和日期、商家資訊、稅金和總計的數量等等。 此外，預先建置的收據模型已定型為用來辨識及傳回收據上所有文字。

## <a name="layout-api"></a>版面配置 API

表單辨識器也可以使用高畫質的光學字元辨識 (OCR)，擷取文字和資料表結構 (與文字相關聯的資料列和資料行編號)。 

## <a name="where-do-i-start"></a>我該從哪裡開始？

**步驟 1：** 要求存取：

表單辨識器是以有限存取預覽版的形式提供。 若要存取此預覽服務，請先填寫並提交[表單辨識器存取要求](https://aka.ms/FormRecognizerRequestAccess)表單。 該表單會要求您提供您個人、您的公司，以及您會在何種案例中使用表單辨識器的相關資訊。

**步驟 2：** 在 Azure 入口網站中建立表單辨識器資源：

當您獲得使用表單辨識器的存取權時，您將會收到「歡迎使用」電子郵件，內含數個連結和資源。 請使用該訊息中的 [Azure 入口網站] 連結開啟 Azure 入口網站，並建立表單辨識器資源。

**步驟 3：** 從表單中擷取資料：

* 自訂 - 將模型定型至您的表單
  * 不使用標籤進行定型
    * [快速入門：搭配使用 REST API 與 cURL 來將表單辨識器模型定型並擷取表單資料](quickstarts/curl-train-extract.md)
    * [快速入門：搭配使用 REST API 與 Python 來將表單辨識器模型定型並擷取表單資料](quickstarts/python-train-extract.md)
  * 使用標籤進行定型 
    * [使用範例標記工具，以標籤定型表單辨識器模型](quickstarts/label-tool.md)
    * [使用 REST API 和 Python 以標籤定型表單辨識器模型](quickstarts/python-labeled-data.md) 
* 預建的收據 - 從美國銷售收據中擷取資料
  * [快速入門：使用 cURL 擷取收據資料](quickstarts/curl-receipts.md)
  * [快速入門：使用 Python 擷取收據資料](quickstarts/python-receipts.md)
* 版面配置 - 從表單中擷取文字和資料表結構
  * [快速入門：使用 Python 擷取版面配置資料](quickstarts/python-layout.md)

當您學習技術時，我們建議您使用免費的服務。 請記住，免費的頁數限制為每個月 500 頁。

**步驟4：** 檢閱 REST API：

您將使用下列 API 來定型模型及擷取表單中的結構化資料。

|名稱 |描述 |
|---|---|
| **定型自訂模型**| 使用相同類型的 5 個表單來定型新模型，以分析您的表單。 將 _useLabelFile_ 參數設定為 `true`，以手動標記的資料進行定型。 |
| **分析表單** |分析以資料流形式傳入的一份文件，以使用自訂模型從表單擷取文字、索引鍵/值組和資料表。  |
| **分析收據** |分析單一收據文件，以擷取索引鍵資訊和其他收據文字。|
| **分析版面配置** |分析表單的版面配置以擷取文字和資料表結構。|

若要了解更多，請瀏覽 [REST API 參考文件](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)。 如果您熟悉舊版的 API，請參閱[新功能](./whats-new.md)一文，以了解最近的變更。

## <a name="input-requirements"></a>輸入需求
### <a name="custom-model"></a>自訂模型

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>預先建置的回條模型

接收模型的輸入需求會略有不同。

* 格式必須是 JPEG、PNG、BMP、PDF (文字或掃描) 或 TIFF。
* 檔案大小必須小於 20 MB。
* 影像維度必須介於 50 x 50 像素和 10000 x 10000 像素之間。 
* PDF 維度最多必須是 17 x 17 英寸，對應 Legal 或 A3 紙張大小 (或更小尺寸)。
* 針對 PDF 和 TIFF，只有前 200 個頁面會進行處理 (若使用免費層的訂用帳戶，只會處理前兩個頁面)。

## <a name="data-privacy-and-security"></a>資料隱私權和安全性

此服務是依據[線上服務條款](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)以 Azure 服務的[預覽版](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)形式來提供。 和所有認知服務一樣，使用表單辨識器服務的開發人員應該要了解 Microsoft 對於客戶資料的政策。 請參閱 Microsoft 信任中心上的[認知服務頁面](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices)，以進行深入了解。

## <a name="next-steps"></a>後續步驟

請完成[快速入門](quickstarts/curl-train-extract.md)來開始使用[表單辨識器 API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)。
