---
title: Azure Data Factory 中的 Wait 活動
description: Wait (等待) 活動會讓管線執行暫停一段指定的時間。
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/12/2018
ms.openlocfilehash: f9dd53fded06eec169219d00993620a0f2aa2bf0
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678249"
---
# <a name="execute-wait-activity-in-azure-data-factory"></a>執行 Azure Data Factory 中的 wait 活動
在管線中使用 Wait (等待) 活動時，管線便會等待一段指定的時間，然後再繼續執行後續的活動。 

## <a name="syntax"></a>語法

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>類型屬性

屬性 | 說明 | 允許的值 | 必要
-------- | ----------- | -------------- | --------
名稱 | `Wait` 活動的名稱。 | 字串 | 是
類型 | 必須設為 **Wait**。 | 字串 | 是
waitTimeInSeconds | 管線繼續進行處理之前所等待的秒數。 | Integer | 是

## <a name="example"></a>範例

> [!NOTE]
> 本節提供 JSON 定義和 PowerShell 命令範例，以供執行管線。 針對使用 Azure PowerShell 和 JSON 定義來建立 Data Factory 管線，如需包含逐步指示的逐步解說，請參閱[教學課程：使用 Azure PowerShell 來建立資料處理站](quickstart-create-data-factory-powershell.md)。

### <a name="pipeline-with-wait-activity"></a>具有 Wait 活動的管線
在此範例中，管線有兩個活動：**Until** 與 **Wait**。 Wait (等待) 活動設定為等待一秒鐘。 管線是在迴圈中執行 Wait (等待) 活動，每次執行之間會等待一秒鐘。 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
    }
}

```

## <a name="next-steps"></a>後續步驟
請參閱 Data Factory 支援的其他控制流程活動： 

- [If 條件活動](control-flow-if-condition-activity.md)
- [執行管線活動](control-flow-execute-pipeline-activity.md)
- [For Each 活動](control-flow-for-each-activity.md)
- [取得中繼資料活動](control-flow-get-metadata-activity.md)
- [查閱活動](control-flow-lookup-activity.md)
- [Web 活動](control-flow-web-activity.md)
- [Until 活動](control-flow-until-activity.md)
