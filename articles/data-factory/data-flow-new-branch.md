---
title: 對應資料流程中的多個分支
description: 使用多個分支複寫對應資料流程中的資料流程
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 71fb9f1ba9952be0e6b3910dd1079aa6d3c0482d
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834526"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>在對應資料流程中建立新的分支

加入新的分支，針對相同的資料流程執行多組作業和轉換。 當您想要將相同的來源用於多個接收或自我聯結資料時，加入新的分支會很有用。

可以從與其他轉換類似的轉換清單中加入新的分支。 只有在您嘗試分支的轉換後面有現有轉換時，**新分支**才會當做動作使用。

![加入新的分支](media/data-flow/new-branch2.png "加入新的分支")

在下列範例中，資料流程會讀取出租車路線資料。 每日和廠商所匯總的輸出都是必要的。 可以新增分支，而不是建立兩個不同的資料流程，從相同的來源讀取。 如此一來，這兩個匯總就可以當做相同資料流程的一部分來執行。 

![加入新的分支](media/data-flow/new-branch.png "加入新的分支")
