---
title: 使用匯入 Microsoft Excel 的 Azure 資料總管 Kusto 查詢將資料視覺化
description: 在本文中，您將瞭解如何將 Azure 資料總管 Kusto 查詢匯入至 Microsoft Excel。
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 4999000e2084922b43b8085034f545d4b5c644a9
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849083"
---
# <a name="visualize-data-using-an-azure-data-explorer-kusto-query-imported-into-microsoft-excel"></a>使用匯入 Microsoft Excel 的 Azure 資料總管 Kusto 查詢將資料視覺化

Azure 資料總管提供兩個選項來連接到 Excel 中的資料：使用原生連接器，或從 Azure 資料總管匯入查詢。 本文說明如何將查詢從 Azure 資料總管匯入至 Excel，以將資料視覺化。 新增 Kusto 查詢做為 Excel 資料來源，以對資料執行其他計算或視覺效果。

## <a name="prerequisites"></a>必要條件

* 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費 Azure 帳戶](https://azure.microsoft.com/free/)。
* 屬於 Azure Active directory 成員的組織電子郵件帳戶，因此您可以[連線到 azure 資料總管](https://dataexplorer.azure.com/clusters/help/databases/Samples)說明叢集 
<br>或</br>
* 建立[測試叢集和資料庫](create-cluster-database-portal.md)，並登入[AZURE 資料總管 Web UI 應用程式](https://dataexplorer.azure.com/)。

## <a name="define-kusto-query-as-an-excel-data-source"></a>將 Kusto 查詢定義為 Excel 資料來源

1. 在[Azure 資料總管 WEB UI](https://dataexplorer.azure.com/clusters/help/databases/Samples)中，執行查詢並檢查結果。

1. 選取 [**共用**] 索引標籤，然後選取 [**查詢] 以 Power BI**。

    ![Power BI 的 Web UI 查詢](media/excel-blank-query/web-ui-query-to-powerbi.png)

1. 視窗隨即出現，並顯示下列通知：

    ![將查詢匯出至剪貼簿](media/excel-blank-query/query-exported-to-clipboard.png)

1. 開啟 **Microsoft Excel**。

1. 在 [**資料**] 索引標籤中，選取 [**從其他來源** **取得資料** > ] > **空白查詢**。

    ![取得資料並選取空白查詢](media/excel-blank-query/get-data-blank-query.png)

1. [ **Power Query 編輯器**] 視窗隨即開啟。 在視窗中，選取 [**進階編輯器**]。

    ![Power query 編輯器視窗](media/excel-blank-query/power-query-editor.png)

1. 在 [**進階編輯器**] 視窗中，貼上您匯出至剪貼簿的查詢，然後選取 [**完成**]。

    ![Advanced editor 查詢](media/excel-blank-query/advanced-editor-query.png)    

1. 若要進行驗證，請選取 [**編輯認證**]。

    ![編輯認證](media/excel-blank-query/edit-credentials.png)

1. 選取 [**組織帳戶**] 並登**入**。 完成登入程式，然後選取 [連線 **]** 。

    ![完成登入](media/excel-blank-query/complete-sign-in.png)

    重複上述步驟來新增更多查詢。 您可以將查詢重新命名為更有意義的名稱。

1. 選取 [**關閉 & 載入**] 按鈕，將您的資料放入 Excel 中。

    ![選取 [關閉並載入]](media/excel-blank-query/close-and-load.png)

1. 您的資料現在是在 Excel 中。 選取 [**重新**整理] 按鈕以重新整理查詢。

    ![在 excel 中查看資料](media/excel-blank-query/data-in-excel.png)

## <a name="next-steps"></a>後續步驟

[使用適用于 Excel 的 Azure 資料總管連接器將資料視覺化](excel-connector.md)