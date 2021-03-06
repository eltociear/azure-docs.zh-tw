---
title: Azure Functions 的 Event Grid 觸發程序
description: 了解如何在 Azure Functions 中處理 Event Grid 事件。
author: craigshoemaker
ms.topic: reference
ms.date: 09/04/2018
ms.author: cshoe
ms.openlocfilehash: 812875be47cabdd23e6307403bb95d8d6ff174ec
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77167514"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Azure Functions 的 Event Grid 觸發程序

本文說明如何在 Azure Functions 中處理 [Event Grid](../event-grid/overview.md) 事件。 如需如何在 HTTP 端點中處理事件方格訊息的詳細資訊，請參閱[接收事件至 HTTP 端點](../event-grid/receive-events.md)。

Event Grid 是一項 Azure 服務，會傳送 HTTP 要求通知您「發行者」中發生的事件。 發行者是產生事件的服務或資源。 例如，Azure Blob 儲存體帳戶即為發行者，而 [Blob 的上傳或刪除是事件](../storage/blobs/storage-blob-event-overview.md)。 部分 [Azure 服務內建有將事件發佈至 Event Grid 的支援](../event-grid/overview.md#event-sources)。

事件*處理常式*會接收及處理事件。 Azure Functions 是數個[有內建 Event Grid 事件處理支援的 Azure 服務](../event-grid/overview.md#event-handlers)之一。 在本文中，您將了解如何在接收到來自 Event Grid 的事件時使用 Event Grid 觸發程序來叫用函式。

如果您想要的話，可以使用 HTTP 觸發程式來處理事件方格事件;請參閱[將事件接收到 HTTP 端點](../event-grid/receive-events.md)。 目前，當事件是在 [CloudEvents 結構描述](../event-grid/cloudevents-schema.md#azure-functions)中傳遞時，您無法針對 Azure Functions 應用程式使用事件格線觸發程序。 相反地，請使用 HTTP 觸發程序。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>封裝-函數2.x 和更新版本

[Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet 套件 2.x 版中提供 Event Grid 觸發程序。 套件的原始程式碼位於 [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x) GitHub 存放庫中。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>套件 - Functions 1.x

[Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet 套件 1.x 版中提供 Event Grid 觸發程序。 套件的原始程式碼位於 [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master) GitHub 存放庫中。

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>範例

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

如需 HTTP 觸發程式範例，請參閱[將事件接收到 HTTP 端點](../event-grid/receive-events.md)。

### <a name="c-2x-and-higher"></a>C#（2.x 和更新版本）

下列範例顯示繫結至 [ 的 ](functions-dotnet-class-library.md)C# 函式`EventGridEvent`：

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

如需詳細資訊，請參閱套件、[屬性](#attributes)、[設定](#configuration)和[用法](#usage)。

### <a name="version-1x"></a>1\.x 版

下列範例顯示繫結至 [ 的 Functions 1.x ](functions-dotnet-class-library.md)C# 函式`JObject`：

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#文字](#tab/csharp-script)

下列範例示範 function.json 檔案中的觸發程序繫結，以及使用此繫結的 [C# 指令碼函式](functions-reference-csharp.md)。

以下是 *function.json* 檔案中的繫結資料：

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-script-version-2x-and-higher"></a>C#腳本（2.x 版和更新版本）

以下是系結至 `EventGridEvent`的範例：

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

如需詳細資訊，請參閱套件、[屬性](#attributes)、[設定](#configuration)和[用法](#usage)。

### <a name="version-1x"></a>1\.x 版

以下是繫結至 `JObject` 的 Functions 1.x C# 指令碼：

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

下列範例示範的是使用繫結之 function.json 檔案，以及 [JavaScript 函式](functions-reference-node.md)中的觸發程序繫結。

以下是 *function.json* 檔案中的繫結資料：

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

以下是 JavaScript 程式碼：

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

下列範例示範 *function.json* 檔案中的觸發程序繫結，以及使用此繫結的 [Python 函式](functions-reference-python.md)。

以下是 *function.json* 檔案中的繫結資料：

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "event",
      "direction": "in"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

以下是 Python 程式碼：

```python
import json
import logging

import azure.functions as func

def main(event: func.EventGridEvent):

    result = json.dumps({
        'id': event.id,
        'data': event.get_json(),
        'topic': event.topic,
        'subject': event.subject,
        'event_type': event.event_type,
    })

    logging.info('Python EventGrid trigger processed an event: %s', result)
```

# <a name="javatabjava"></a>[Java](#tab/java)

本區段包含下列範例：

* [事件格線觸發程序，字串參數](#event-grid-trigger-string-parameter)
* [事件格線觸發程序，POJO 參數](#event-grid-trigger-pojo-parameter)

下列範例顯示使用系結並印出事件的[JAVA](functions-reference-java.md)觸發程式系結，第一次以 `String` 和第二個 POJO 的方式接收事件。

### <a name="event-grid-trigger-string-parameter"></a>事件方格觸發程式，字串參數

```java
  @FunctionName("eventGridMonitorString")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    String content, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: " + content);      
  }
```

### <a name="event-grid-trigger-pojo-parameter"></a>事件方格觸發程式，POJO 參數

此範例會使用下列 POJO 來代表事件格線事件的最上層屬性：

```java
import java.util.Date;
import java.util.Map;

public class EventSchema {

  public String topic;
  public String subject;
  public String eventType;
  public Date eventTime;
  public String id;
  public String dataVersion;
  public String metadataVersion;
  public Map<String, Object> data;

}
```

抵達時，會將事件的 JSON 承載還原序列化為 ```EventSchema``` POJO，以供函式使用。 這可讓函式以物件導向方式存取事件的屬性。

```java
  @FunctionName("eventGridMonitor")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    EventSchema event, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: ");
      context.getLogger().info("Subject: " + event.subject);
      context.getLogger().info("Time: " + event.eventTime); // automatically converted to Date by the runtime
      context.getLogger().info("Id: " + event.id);
      context.getLogger().info("Data: " + event.data);
  }
```

在 [Java 函式執行階段程式庫](/java/api/overview/azure/functions/runtime)中，對其值來自事件方格的參數使用 `EventGridTrigger` 註釋。 具有這些附註的參數會使得函式在事件抵達時執行。  此註釋可以搭配原生 Java 類型、POJO 或使用 `Optional<T>` 的可為 Null 值使用。

---

## <a name="attributes"></a>屬性

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

在 [C# 類別庫](functions-dotnet-class-library.md)中，使用 [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/TriggerBinding/EventGridTriggerAttribute.cs) 屬性。

以下是方法簽章中的 `EventGridTrigger` 屬性：

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

如需完整範例，請參閱 C# 範例。

# <a name="c-scripttabcsharp-script"></a>[C#文字](#tab/csharp-script)

C#腳本不支援屬性。

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

JavaScript 不支援屬性。

# <a name="pythontabpython"></a>[Python](#tab/python)

Python 不支援屬性。

# <a name="javatabjava"></a>[Java](#tab/java)

[EventGridTrigger](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/EventGridTrigger.java)注釋可讓您藉由提供設定值，以宣告方式設定事件方格系結。 如需詳細資訊，請參閱[範例](#example)[和設定](#configuration)章節。

---

## <a name="configuration"></a>組態

下表說明您在 function.json 檔案中設定的繫結設定屬性。 沒有要在 `EventGridTrigger` 屬性中設定的建構函式參數或屬性。

|function.json 屬性 |描述|
|---------|---------|
| **type** | 必要項目 - 必須設定為 `eventGridTrigger`。 |
| **direction** | 必要項目 - 必須設定為 `in`。 |
| **name** | 必要項目 - 函式程式碼中用於接收事件資料之參數的變數名稱。 |

## <a name="usage"></a>使用量

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

在 Azure Functions 1.x 中，您可以針對事件方格觸發程式使用下列參數類型：

* `JObject`
* `string`

在 Azure Functions 2.x 和更新版本中，您也可以選擇為 Event Grid 觸發程式使用下列參數類型：

* `Microsoft.Azure.EventGrid.Models.EventGridEvent` - 定義所有事件類型通用之欄位的屬性。

> [!NOTE]
> 在 Functions v1 中，如果您嘗試繫結至 `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`，編譯器將會顯示「已淘汰」訊息，並建議您改用 `Microsoft.Azure.EventGrid.Models.EventGridEvent`。 若要使用較新的類型，請參考 [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet 套件，並且為 `EventGridEvent` 類型名稱加上 `Microsoft.Azure.EventGrid.Models` 首碼，加以完整限定。

# <a name="c-scripttabcsharp-script"></a>[C#文字](#tab/csharp-script)

在 Azure Functions 1.x 中，您可以針對事件方格觸發程式使用下列參數類型：

* `JObject`
* `string`

在 Azure Functions 2.x 和更新版本中，您也可以選擇為 Event Grid 觸發程式使用下列參數類型：

* `Microsoft.Azure.EventGrid.Models.EventGridEvent` - 定義所有事件類型通用之欄位的屬性。

> [!NOTE]
> 在 Functions v1 中，如果您嘗試繫結至 `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`，編譯器將會顯示「已淘汰」訊息，並建議您改用 `Microsoft.Azure.EventGrid.Models.EventGridEvent`。 若要使用較新的類型，請參考 [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet 套件，並且為 `EventGridEvent` 類型名稱加上 `Microsoft.Azure.EventGrid.Models` 首碼，加以完整限定。 如需如何在 C# 指令碼函式中參考 NuGet 套件的相關資訊，請參閱[使用 NuGet 套件](functions-reference-csharp.md#using-nuget-packages)

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

您可以透過在*函數 json*檔案的 `name` 屬性中設定的參數，取得事件方格實例。

# <a name="pythontabpython"></a>[Python](#tab/python)

您可以*透過在函*式的 `name` 屬性中設定的參數來使用事件方格實例，其型別為 `func.EventGridEvent`。

# <a name="javatabjava"></a>[Java](#tab/java)

事件方格事件實例可透過與 `EventGridTrigger` 屬性相關聯的參數（類型為 `EventSchema`）來取得。 如需詳細資訊，請參閱[範例](#example)。

---

## <a name="event-schema"></a>結構描述

接收到的 Event Grid 事件資料，會是 HTTP 要求本文中的 JSON 物件。 此 JSON 大致如下列範例所示：

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

範例中顯示一個元素的陣列。 Event Grid 一律會傳送陣列，且在陣列中可能會傳送多個事件。 執行階段分別會為每個陣列元素叫用您的函式一次。

所有事件類型的事件 JSON 資料中會具有相同的最上層屬性，但 `data` 屬性的內容則會隨著事件類型而不同。 此處顯示的是 Blob 儲存體事件的範例。

如需通用屬性和事件特定屬性的說明，請參閱 Event Grid 文件中的[事件屬性](../event-grid/event-schema.md#event-properties)。

`EventGridEvent` 類型只會定義最上層屬性；`Data` 屬性為 `JObject`。

## <a name="create-a-subscription"></a>建立訂用帳戶

若要開始接收 Event Grid HTTP 要求，請建立會指定端點 URL 以叫用函式的 Event Grid 訂用帳戶。

### <a name="azure-portal"></a>Azure 入口網站

對於您在 Azure 入口網站中使用 Event Grid 觸發程序開發的函式，選取**新增 Event Grid 訂用帳戶**。

![在入口網站中建立訂用帳戶](media/functions-bindings-event-grid/portal-sub-create.png)

當您選取此連結時，入口網站會開啟預先填入端點 URL 的**建立事件訂用帳戶**頁面。

![預先填入的端點 URL](media/functions-bindings-event-grid/endpoint-url.png)

如需如何使用 Azure 入口網站建立訂用帳戶的詳細資訊，請參閱 Event Grid 文件中的[建立自訂事件 - Azure 入口網站](../event-grid/custom-event-quickstart-portal.md)。

### <a name="azure-cli"></a>Azure CLI

若要使用 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) 建立訂用帳戶，請使用 [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create) 命令。

此命令需要叫用函式的端點 URL。 下列範例顯示特定版本的 URL 模式：

#### <a name="version-2x-and-higher-runtime"></a>2\.x 版（及更新版本）執行時間

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>1\.x 版執行階段

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

系統金鑰是必須包含在端點 URL 中供 Event Grid 觸發程序使用的授權金鑰。 下一節將說明如何取得系統金鑰。

以下是訂閱 Blob 儲存體帳戶的範例 (以預留位置表示系統金鑰)：

#### <a name="version-2x-and-higher-runtime"></a>2\.x 版（及更新版本）執行時間

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>1\.x 版執行階段

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

如需如何建立訂用帳戶的詳細資訊，請參閱 [Blob 儲存體快速入門](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account)或其他 Event Grid 快速入門。

### <a name="get-the-system-key"></a>取得系統金鑰

您可以使用下列 API (HTTP GET) 來取得系統金鑰：

#### <a name="version-2x-and-higher-runtime"></a>2\.x 版（及更新版本）執行時間

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>1\.x 版執行階段

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

這是管理員 API，因此需要函式應用程式的[主要金鑰](functions-bindings-http-webhook.md#authorization-keys)。 請勿混淆系統金鑰 (用來叫用事件方格觸發程序函式) 與主要金鑰 (用來執行函式應用程式的管理工作)。 當您訂閱 Event Grid 主題時，請務必使用系統金鑰。

以下範例說明提供系統金鑰的回應：

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

您可以從入口網站中的 [函數應用程式設定] 索引標籤取得函數應用程式主要金鑰。

> [!IMPORTANT]
> 主要金鑰為系統管理員提供對函數應用程式的存取權。 請勿與第三方共用此金鑰或將其散發於原生用戶端應用程式。

如需詳細資訊，請參閱 HTTP 觸發程序參考文章中的[授權金鑰](functions-bindings-http-webhook.md#authorization-keys)。

或者，您可以傳送 HTTP PUT 以自行指定金鑰值。

## <a name="local-testing-with-viewer-web-app"></a>使用檢視器 Web 應用程式的本機測試

若要在本機測試 Event Grid 觸發程序，您必須取得從雲端中的原有位置傳遞到本機電腦的 Event Grid HTTP 要求。 其中一種方式是在線上擷取要求，然後手動將其重新傳送至您的本機電腦：

1. [建立檢視器 Web 應用程式](#create-a-viewer-web-app)，以擷取事件訊息。
1. [建立事件格線訂用帳戶](#create-an-event-grid-subscription)，以將事件傳送至檢視器應用程式。
1. [產生要求](#generate-a-request)，並從檢視器應用程式複製要求本文。
1. [手動將要求發佈至](#manually-post-the-request) Event Grid 觸發程序函式的 localhost URL。

完成測試後，您可以藉由更新端點，在生產環境使用相同的訂用帳戶。 使用 [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI 命令。

### <a name="create-a-viewer-web-app"></a>建立檢視器 Web 應用程式

若要簡化擷取事件訊息，您可以部署[預先建置的 Web 應用程式](https://github.com/Azure-Samples/azure-event-grid-viewer)，以顯示事件訊息。 已部署的解決方案包含 App Service 方案、App Service Web 應用程式，以及 GitHub 中的原始程式碼。

選取 [部署至 Azure]，將解決方案部署至您的訂用帳戶。 在 Azure 入口網站中，提供參數的值。

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

部署需要幾分鐘的時間才能完成。 成功部署之後，檢視 Web 應用程式，確定它正在執行。 在網頁瀏覽器中，瀏覽至：`https://<your-site-name>.azurewebsites.net`

您看到網站，但其中尚未發佈任何事件。

![檢視新網站](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>建立事件格線訂用帳戶

建立您想要測試之類型的事件格線訂用帳戶，並從您的 Web 應用程式提供其 URL 作為端點的事件通知。 Web 應用程式的端點必須包含的尾碼 `/api/updates/`。 因此，完整 URL 是 `https://<your-site-name>.azurewebsites.net/api/updates`

如需如何使用 Azure 入口網站建立訂用帳戶的資訊，請參閱事件格線文件中的[建立自訂事件 - Azure 入口網站](../event-grid/custom-event-quickstart-portal.md)。

### <a name="generate-a-request"></a>產生要求

觸發會對您的 Web 應用程式端點產生 HTTP 流量的事件。  例如，如果您已建立 Blob 儲存體訂用帳戶，請上傳或刪除 Blob。 當要求出現在 Web 應用程式上時，請複製要求本文。

您會先收到訂用帳戶驗證要求；請忽略任何驗證要求，並複製事件要求。

![從 Web 應用程式複製要求本文](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>手動發佈要求

在本機執行您的 Event Grid 函式。

使用 [Postman](https://www.getpostman.com/) 或 [curl](https://curl.haxx.se/docs/httpscripting.html) 之類的工具建立 HTTP POST 要求：

* 設定 `Content-Type: application/json` 標頭。
* 設定 `aeg-event-type: Notification` 標頭。
* 將 RequestBin 資料貼到要求本文中。
* 張貼至事件方格觸發程式函式的 URL。
  * 若為2.x 和更高版本，請使用下列模式：

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * 若為1.x，請使用：

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

`functionName` 參數必須是 `FunctionName` 屬性中指定的名稱。

下列螢幕擷取畫面顯示 Postman 中的標頭和要求本文：

![Postman 中的標頭](media/functions-bindings-event-grid/postman2.png)

![Postman 中的要求本文](media/functions-bindings-event-grid/postman.png)

Event Grid 觸發程序函式會執行並顯示類似於下列範例的記錄：

![Event Grid 觸發程序函式記錄的範例](media/functions-bindings-event-grid/eg-output.png)

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [深入了解 Azure Functions 觸發程序和繫結](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [深入了解 Event Grid](../event-grid/overview.md)
