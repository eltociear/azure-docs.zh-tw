---
title: 模式新增精確度-LUIS
titleSuffix: Azure Cognitive Services
description: 新增模式範本，以改善 Language Understanding （LUIS）應用程式中的預測精確度。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 21afb12bf2464218119ebf52ebd980745e3d731d
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311711"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>如何新增模式以改善預測精確度
LUIS 應用程式收到端點語句之後，請使用[模式](luis-concept-patterns.md)來改善以單字順序和字組選擇顯示模式之語句的預測精確度。 模式會使用特定的[語法](luis-concept-patterns.md#pattern-syntax)來指出的位置：[實體](luis-concept-entity-types.md)、實體[角色](luis-concept-roles.md)和選擇性文字。

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

> [!CAUTION]
> 模式只包含機器學習的實體父系，而非子元件。

## <a name="adding-example-utterances-as-pattern"></a>將範例語句新增為模式

如果您想要加入實體的模式，_最簡單_的方式是從 [意圖詳細資料] 頁面建立模式。 這可確保您的語法符合範例語句。

1. 在[預覽 LUIS 入口網站](https://preview.luis.ai)中，從 [**我的應用程式**] 頁面選取應用程式。
1. 在 [**意圖**清單] 頁面上，選取您想要建立語句的範本範例語句的意圖名稱。
1. 在 [意圖詳細資料] 頁面上，選取您想要作為範本語句之範例語句的資料列，然後從內容工具列選取 [ **+ 新增為模式**]。

    > [!div class="mx-imgBorder"]
    > ![在 [意圖詳細資料] 頁面上選取 [範例語句] 做為範本模式的螢幕擷取畫面。](./media/luis-how-to-model-intent-pattern/add-example-utterances-as-pattern-template-utterance-from-intent-detail-page.png)

1. 在快顯方塊中，選取 [**確認模式**] 頁面上的 [**完成**]。 您不需要定義實體的子元件、條件約束或描述項。 您只需要列出機器學習的實體。

    > [!div class="mx-imgBorder"]
    > ![在 [意圖詳細資料] 頁面上確認範例語句為範本模式的螢幕擷取畫面。](./media/luis-how-to-model-intent-pattern/confirm-patterns-from-example-utterance-intent-detail-page.png)

1. 如果您需要編輯範本（例如，選取 [文字] 做為選擇性）和 [`[]` （方形）] 括弧，您必須從 [**模式**] 頁面進行這種編輯。

1. 在導覽列中，選取 [**定型**]，以新的模式訓練應用程式。

## <a name="add-template-utterance-using-correct-syntax"></a>使用正確的語法新增範本語句

1. 在 [我的應用程式]頁面上選取您的應用程式名稱加以開啟，然後在 [改善應用程式效能] 之下，選取左面板中的 [模式]。

    > [!div class="mx-imgBorder"]
    > ![模式清單的螢幕擷取畫面](./media/luis-how-to-model-intent-pattern/patterns-1.png)

1. 選取模式的正確意圖。

1. 在範本文字方塊中，輸入範本語句並選取 Enter。 當您想要輸入實體名稱時，請使用正確的模式實體語法。 實體語法的開頭為 `{`。 實體清單隨即顯示。 選取正確的實體。

    > [!div class="mx-imgBorder"]
    > ![模式的實體螢幕擷取畫面](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    如果您的實體包含[角色](luis-concept-roles.md)，請在機構名稱後面指定具有單一冒號的角色 `:`，例如 `{Location:Origin}`。 實體的角色清單會顯示在清單中。 選取角色，然後選取 Enter。

    > [!div class="mx-imgBorder"]
    > ![具有角色](./media/luis-how-to-model-intent-pattern/patterns-4.png) 的實體螢幕擷取畫面

    在您選取正確的實體之後，完成模式輸入，然後選取 Enter。 當您輸入好模式時，請[訓練](luis-how-to-train.md)您的應用程式。

    > [!div class="mx-imgBorder"]
    > ![輸入模式的螢幕擷取畫面，其中包含兩種類型的實體](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>在利用模式變更模型後訓練您的應用程式
在您新增、編輯、移除或重新指派模式之後，[訓練](luis-how-to-train.md)並[發佈](luis-how-to-publish-app.md)您的應用程式，您的變更才會影響端點查詢。

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>使用內容相關的工具列

模式清單上方的內容相關工具列可讓您：

* 搜尋模式
* 編輯模式
* 將個別模式重新指派至不同的意圖
* 將數個模式重新指派至不同的意圖
* Delete-a-單一模式
* 刪除數個模式
* 依實體篩選模式清單
* 篩選模式-依意圖列出
* 移除實體或意圖篩選條件
* 在意圖或實體頁面上從現有的語句新增模式

## <a name="next-steps"></a>後續步驟

* 瞭解如何使用模式[建立模式](luis-tutorial-pattern.md)。任何和角色都有教學課程。
* 了解如何[訓練](luis-how-to-train.md)您的應用程式。
