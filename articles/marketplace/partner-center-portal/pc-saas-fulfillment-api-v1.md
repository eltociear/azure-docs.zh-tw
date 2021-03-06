---
title: SaaS 履行 Api v1 |Azure Marketplace
description: 說明如何使用相關聯的履行 v1 Api，在 Azure Marketplace 上建立和管理 SaaS 供應專案。
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ROBOTS: NOINDEX
ms.openlocfilehash: f56e9b4f6c3db6fb47452c7478f5a27445955e87
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715378"
---
# <a name="saas-fulfillment-apis-version-1-deprecated"></a>SaaS 履行 Api 第1版（已淘汰）

本文說明如何使用 API 建立 SaaS 供應項目。 如果您已選取 [透過 Azure 銷售]，則必須要有由 REST 方法和端點組成的 Api，才能允許您的 SaaS 供應專案的訂閱。  

> [!WARNING]
> 此初始版本的 SaaS 履行 API 已淘汰;請改用[SaaS 履行 API V2](./pc-saas-fulfillment-api-v2.md)。  此 API 的初始版本目前僅供維護現有的發行者。 

下列 API 可協助您將 SaaS 服務和 Azure 整合：

-   解決
-   訂閱
-   轉換
-   取消訂閱


## <a name="api-methods-and-endpoints"></a>API 方法和端點

下列區段說明適用於啟用 SaaS 供應項目之訂用帳戶的 API 方法和端點。


### <a name="marketplace-api-endpoint-and-api-version"></a>Marketplace API 端點和 API 版本

Azure Marketplace API 的端點為 `https://marketplaceapi.microsoft.com`。

目前的 API 版本為 `api-version=2017-04-15`。


### <a name="resolve-subscription"></a>解析訂用帳戶

解析端點上的 POST 動作可讓使用者解析持續性資源識別碼的 Marketplace 權杖。  資源識別碼是 SAAS 訂用帳戶的唯一識別碼。 

當使用者重新導向至 ISV 的網站時，URL 會在查詢參數中包含一個權杖。 ISV 必須使用此權杖，並提出要求，以解決此問題。 回應包含唯一的 SAAS 訂用帳戶識別碼、名稱、供應項目識別碼和資源方案。 此權杖僅在一小時內有效。

*要求*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **參數名稱** |     **說明**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  要用於此要求的作業版本。   |
|  |  |


*標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | 否           | 用於追蹤用戶端要求的特殊字串值，最好是全域唯一識別碼 (GUID)。 如果未提供此值，則回應標頭中會產生並提供一個。  |
| x-ms-correlationid | 否           | 用於用戶端作業的特殊字串值。 此欄位會將用戶端作業的所有事件與伺服器端上的事件相互關聯。 如果未提供此值，則回應標頭中會產生並提供一個。 |
| Content-Type       | 是          | `application/json`                                        |
| 授權      | 是          | JSON Web 權杖 (JWT) 持有人權杖。                    |
| x-ms-marketplace-token| 是| 當使用者從 Azure 重新導向至 SaaS ISV 的網站時，URL 中的權杖查詢參數。 **注意：** 此權杖的有效時間為1小時。 此外，URL 在使用瀏覽器的權杖值之前，會先將其解碼。|
|  |  |  |
  

*回應主體*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **參數名稱** | **Data type** | **說明**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | SaaS 訂用帳戶的識別碼。          |
| subscriptionName| String| 使用者訂閱 SaaS 服務時在 Azure 中設定的 SaaS 訂用帳戶名稱。|
| OfferId            | String        | 使用者訂閱的供應項目識別碼。 |
| planId             | String        | 使用者訂閱的方案識別碼。  |
|  |  |  |


*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | 成功解析權杖。                                                            |
| 400                  | `BadRequest`         | 要求的標頭遺失或指定的 API 版本不正確。 無法解析權杖，因為其格式不正確或已過期 (此權杖在產生後只有 1 小時的效力)。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                                 |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                                |
| 503                  | `ServiceUnavailable` | 服務已關閉，請稍後重試。                                        |
|  |  |  |


*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此識別碼會用於任何對帳。 |
| Retry-after        | 否           | 此值只針對 429 回應設定。                                                                   |
|  |  |  |


### <a name="subscribe"></a>訂閱

訂閱端點可讓使用者針對指定方案開始訂閱 SaaS 服務，並在商務系統中啟用計費功能。

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | 透過解析 API 解析權杖之後取得的 SaaS 訂用帳戶唯一識別碼。                              |
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

|  **標頭索引鍵**        | **必要** |  **說明**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   否         | 用於追蹤用戶端要求的特殊字串值，最好是全域唯一識別碼 (GUID)。 如果未提供此值，則系統會產生一個並提供於回應標頭中。 |
| x-ms-correlationid     |   否         | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則系統會產生一個並提供於回應標頭中。 |
| If-Match/If-None-Match |   否         |   驗證程式 Etag 值強度。                                                          |
| Content-Type           |   是        |    `application/json`                                                                   |
|  授權         |   是        |    JSON Web 權杖 (JWT) 持有人權杖。                                               |
| x-ms-marketplace-session-mode| 否 | 可在訂閱 SaaS 供應項目時啟用測試模式的標記。 若有設定，該訂閱將無須支付費用。 這對於 ISV 測試方式非常實用。 請將它設定為 **' dryrun '**|
|  |  |  |

*本文*

``` json
{
    "lanId": "",
}
```

| **元素名稱** | **Data type** | **說明**                      |
|------------------|---------------|--------------------------------------|
| planId           | (必要) 字串        | SaaS 服務使用者正在訂閱的方案識別碼。  |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | 針對指定方案收到的 SaaS 訂用帳戶啟用。                   |
| 400                  | `BadRequest`         | 必要標頭遺失或 JSON 主體的格式不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                   |
| 404                  | `NotFound`           | 找不到具有該指定識別碼的訂用帳戶                                  |
| 409                  | `Conflict`           | 訂用帳戶上有其他作業正在進行中。                     |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                  |
| 503                  | `ServiceUnavailable` | 服務已關閉，請稍後重試。                          |
|  |  |  |

針對202回應，請在「作業-位置」標頭上追蹤要求操作的狀態。 驗證作業與其他 Marketplace API 相同。

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 是          | 用戶端可用來檢查狀態的間隔。                                                       |
| Operation-Location | 是          | 用於取得作業狀態的資源連結。                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>變更方案端點

變更端點可讓使用者將其目前訂閱的方案轉換成新的方案。

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS 訂用帳戶識別碼。                              |
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

| **標頭索引鍵**          | **必要** | **說明**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | 否           | 用於追蹤用戶端要求的特殊字串值。 建議的 GUID。 如果未提供此值，則系統會產生一個並提供於回應標頭中。   |
| x-ms-correlationid      | 否           | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則系統會產生一個並提供於回應標頭中。 |
| If-Match/If-None-Match | 否           | 驗證程式 Etag 值強度。                              |
| Content-Type            | 是          | `application/json`                                        |
| 授權           | 是          | JSON Web 權杖 (JWT) 持有人權杖。                    |
|  |  |  |

*本文*

```json
{
    "planId": ""
}
```

|  **元素名稱** |  **Data type**  | **說明**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (必要) 字串         | SaaS 服務使用者正在訂閱的方案識別碼。          |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | 針對指定方案收到的 SaaS 訂用帳戶啟用。                   |
| 400                  | `BadRequest`         | 必要標頭遺失或 JSON 主體的格式不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                   |
| 404                  | `NotFound`           | 找不到具有該指定識別碼的訂用帳戶                                  |
| 409                  | `Conflict`           | 訂用帳戶上有其他作業正在進行中。                     |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                  |
| 503                  | `ServiceUnavailable` | 服務已關閉，請稍後重試。                          |
|  |  |  |

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 是          | 用戶端可用來檢查狀態的間隔。                                                       |
| Operation-Location | 是          | 用於取得作業狀態的資源連結。                                                        |
|  |  |  |

### <a name="delete-subscription"></a>刪除訂用帳戶

訂閱端點上的刪除動作，可讓使用者刪除具有指定識別碼的訂用帳戶。

*要求*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS 訂用帳戶識別碼。                              |
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | 否           | 用於追蹤用戶端要求的特殊字串值。 建議的 GUID。 如果未提供此值，則回應標頭中會產生並提供一個。                                                           |
| x-ms-correlationid | 否           | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則系統會產生一個並提供於回應標頭中。 |
| 授權      | 是          | JSON Web 權杖 (JWT) 持有人權杖。                    |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | 針對指定方案收到的 SaaS 訂用帳戶啟用。                   |
| 400                  | `BadRequest`         | 必要標頭遺失或 JSON 主體的格式不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                   |
| 404                  | `NotFound`           | 找不到具有該指定識別碼的訂用帳戶                                  |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                  |
| 503                  | `ServiceUnavailable` | 服務已暫時關閉。 請稍後重試。                          |
|  |  |  |

針對202回應，請在「作業-位置」標頭上追蹤要求操作的狀態。 驗證作業與其他 Marketplace API 相同。

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 是          | 用戶端可用來檢查狀態的間隔。                                                       |
| Operation-Location | 是          | 用於取得作業狀態的資源連結。                                                        |
|   |  |  |

### <a name="get-operation-status"></a>取得作業狀態

此端點可讓使用者追蹤已觸發的非同步作業狀態 (訂閱/取消訂閱/變更方案)。

*要求*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/ *{operationId}* ?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | 已觸發作業的唯一識別碼。                |
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 否           | 用於追蹤用戶端要求的特殊字串值。 建議的 GUID。 如果未提供此值，則回應標頭中會產生並提供一個。   |
| x-ms-correlationid | 否           | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則回應標頭中會產生並提供一個。  |
| 授權      | 是          | JSON Web 權杖 (JWT) 持有人權杖。                    |
|  |  |  | 

*回應主體*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **參數名稱** | **Data type** | **說明**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | 作業的識別碼。                                                                      |
| status             | 例舉          | 下列其中之一的作業狀態：`In Progress`、 `Succeeded` 或 `Failed`。          |
| resourceLocation   | String        | 已建立或修改之訂用帳戶的連結。 此連結可協助用戶端取得更新的狀態後續作業。 此值並未針對 `Unsubscribe` 作業進行設定。 |
| created            | Datetime      | 作業建立時間 (UTC)。                                                           |
| lastModified       | Datetime      | 作業上次更新時間 (UTC)。                                                      |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | 已成功解析 get 要求，且主體包含回應。    |
| 400                  | `BadRequest`         | 要求的標頭遺失或指定的 API 版本不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                      |
| 404                  | `NotFound`           | 找不到具有指定識別碼的訂用帳戶。                                     |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                     |
| 503                  | `ServiceUnavailable` | 服務已關閉，請稍後重試。                             |
|  |  |  |

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 是          | 用戶端可用來檢查狀態的間隔。                                                       |
|  |  |  |

### <a name="get-subscription"></a>取得訂用帳戶

訂閱端點上的「get」動作可讓使用者取出具有指定資源識別碼的訂用帳戶。

*要求*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS 訂用帳戶識別碼。                              |
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 否           | 用於追蹤用戶端要求的特殊字串值，最好是全域唯一識別碼 (GUID)。 如果未提供此值，則回應標頭中會產生並提供一個。                                                           |
| x-ms-correlationid | 否           | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則回應標頭中會產生並提供一個。 |
| 授權      | 是          | JSON Web 權杖 (JWT) 持有人權杖。                                                                    |
|  |  |  |

*回應主體*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **參數名稱**     | **Data type** | **說明**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure 中的 SaaS 訂用帳戶資源識別碼。    |
| offerId                | String        | 使用者訂閱的供應項目識別碼。         |
| planId                 | String        | 使用者訂閱的方案識別碼。          |
| saasSubscriptionName   | String        | SaaS 訂用帳戶名稱。                |
| saasSubscriptionStatus | 例舉          | 作業狀態。  下列其中之一：  <br/> - `Subscribed`：訂用帳戶作用中。  <br/> - `Pending`：使用者建立了該資源，但它不是由 ISV 所啟動。   <br/> - `Unsubscribed`：使用者已取消訂閱。   <br/> - `Suspended`：使用者已暫止訂用帳戶。   <br/> - `Deactivated`：Azure 訂用帳戶已暫時停用。  |
| created                | Datetime      | 建立訂用帳戶的時間戳記值 (UTC)。 |
| lastModified           | Datetime      | 已修改之訂用帳戶的時間戳記值 (UTC)。 |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | 已成功解析 get 要求，且主體包含回應。    |
| 400                  | `BadRequest`         | 要求的標頭遺失或指定的 API 版本不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                      |
| 404                  | `NotFound`           | 找不到具有該指定識別碼的訂用帳戶                                     |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                     |
| 503                  | `ServiceUnavailable` | 服務已關閉，請稍後重試。                             |
|  |  |  |

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 否           | 用戶端可用來檢查狀態的間隔。                                                       |
| etag               | 是          | 用於取得作業狀態的資源連結。                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>取得訂用帳戶

訂用帳戶端點上的「get」動作可讓使用者從 ISV 取出所有供應項目的訂用帳戶。

*要求*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **參數名稱**  | **說明**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | 要用於此要求的作業版本。 |
|  |  |

*標頭*

| **標頭索引鍵**     | **必要** | **說明**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | 否           | 用於追蹤用戶端要求的特殊字串值。 建議的 GUID。 如果未提供此值，則回應標頭中會產生並提供一個。             |
| x-ms-correlationid | 否           | 用於用戶端作業的特殊字串值。 這會將來自用戶端作業的所有事件與伺服器端上的事件相關聯。 如果未提供此值，則回應標頭中會產生並提供一個。 |
| 授權      | 是          | JSON Web 權杖 (JWT) 持有人權杖。                    |
|  |  |  |

*回應主體*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **參數名稱**     | **Data type** | **說明**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure 中的 SaaS 訂用帳戶資源識別碼    |
| offerId                | String        | 使用者訂閱的供應專案識別碼         |
| planId                 | String        | 使用者訂閱的方案識別碼          |
| saasSubscriptionName   | String        | SaaS 訂用帳戶的名稱                |
| saasSubscriptionStatus | 例舉          | 作業狀態。  下列其中之一：  <br/> - `Subscribed`：訂用帳戶作用中。  <br/> - `Pending`：使用者建立了該資源，但它不是由 ISV 所啟動。   <br/> - `Unsubscribed`：使用者已取消訂閱。   <br/> - `Suspended`：使用者已暫止訂用帳戶。   <br/> - `Deactivated`：Azure 訂用帳戶已暫時停用。  |
| created                | Datetime      | 訂用帳戶建立時間戳記值（UTC） |
| lastModified           | Datetime      | 訂用帳戶修改的時間戳記值（UTC） |
|  |  |  |

*回應碼*

| **HTTP 狀態碼** | **錯誤碼**     | **說明**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | 已成功解析 get 要求，且主體包含回應。    |
| 400                  | `BadRequest`         | 要求的標頭遺失或指定的 API 版本不正確。 |
| 403                  | `Forbidden`          | 呼叫者沒有權限執行此作業。                      |
| 404                  | `NotFound`           | 找不到具有該指定識別碼的訂用帳戶                                     |
| 429                  | `RequestThrottleId`  | 服務正忙於處理多個要求，請稍後重試。                     |
| 503                  | `ServiceUnavailable` | 服務已暫時關閉。 請稍後重試。                             |
|  |  |  |

*回應標頭*

| **標頭索引鍵**     | **必要** | **說明**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | 是          | 從用戶端收到的要求識別碼。                                                                   |
| x-ms-correlationid | 是          | 由用戶端傳遞時為相互關聯識別碼，否則此值為伺服器相互關聯識別碼。                   |
| x-ms-activityid    | 是          | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。 |
| Retry-after        | 否           | 用戶端可用來檢查狀態的間隔。                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Webhook

SaaS Webhook 可用來主動通知 SaaS 服務關於變更的訊息。 此 POST API 應未經驗證，且將由 Microsoft 服務所呼叫。 SaaS 服務應該會呼叫作業 API，以在對 Webhook 通知執行動作之前進行驗證和授權。 

*本文*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **參數名稱**     | **Data type** | **說明**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | 已觸發作業的唯一識別碼。                |
| activityId   | String        | 用於追蹤服務要求的特殊字串值。 此值可用於任何核對作業。               |
| subscriptionId                     | String        | Azure 中的 SaaS 訂用帳戶資源識別碼。    |
| offerId                | String        | 使用者訂閱的供應項目識別碼。 僅為「更新」動作提供。        |
| publisherId                | String        | SaaS 供應項目的發行者識別碼         |
| planId                 | String        | 使用者訂閱的方案識別碼。 僅為「更新」動作提供。          |
| 動作                 | String        | 觸發此通知的動作。 可能的值 - 啟動、刪除、暫止、恢復、更新          |
| timeStamp                 | String        | 觸發此通知時的時間戳記值 (採用 UTC)。          |
|  |  |  |


## <a name="next-steps"></a>後續步驟

開發人員也可以使用[CLOUD PARTNER 入口網站 REST api](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview)，以程式設計方式取出和操作工作負載、供應專案和發行者設定檔。
