---
title: 使用 Java 從 Azure 事件中樞傳送或接收事件 (最新)
description: 本文提供的逐步解說說明如何建立 Java 應用程式，以使用最新的 azure-messaging-eventhubs 套件，從 Azure 事件中樞傳送/接收事件。
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: d9d22374868f3befd659918c532f339d49ba1642
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2020
ms.locfileid: "77032046"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>使用 Java 將事件傳送至 Azure 事件中樞或從中接收事件 (azure-messaging-eventhubs)
本快速入門說明如何建立 Java 應用程式，以將事件傳送至 Azure 事件中樞或從中接收事件。 

Azure 事件中樞是巨量資料串流平台和事件擷取服務，每秒可接收和處理數百萬個事件。 事件中樞可以處理及儲存分散式軟體和裝置所產生的事件、資料或遙測。 傳送至事件中樞的資料可以透過任何即時分析提供者或批次/儲存體配接器來轉換和儲存。 如需事件中樞的詳細概觀，請參閱[事件中樞概觀](event-hubs-about.md)和[事件中樞功能](event-hubs-features.md)。

> [!IMPORTANT]
> 本快速入門使用新的 **azure-messaging-eventhubs** 套件。 如需使用舊版 **azure-eventhubs** 和 **azure-eventhubs-eph** 套件的快速入門，請參閱[這篇文章](event-hubs-java-get-started-send.md)。 


## <a name="prerequisites"></a>Prerequisites

若要完成本教學課程，您需要下列必要條件：

- 使用中的 Azure 帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。
- Java 開發環境。 本教學課程使用 [Eclipse](https://www.eclipse.org/)。 需要含第 8 版或更新版本的 Java 開發套件 (JDK)。 
- **建立事件中樞命名空間和事件中樞**。 第一個步驟是使用 [Azure 入口網站](https://portal.azure.com)來建立「事件中樞」類型的命名空間，然後取得您應用程式與「事件中樞」進行通訊所需的管理認證。 若要建立命名空間和事件中樞，請依照[這篇文章](event-hubs-create.md)中的程序操作。 然後，依照以下文章中的指示，取得事件中樞的存取金鑰值：[取得連接字串](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)。 您可以在您於本教學課程稍後撰寫的程式碼中，使用此存取金鑰。 預設的金鑰名稱是：**RootManageSharedAccessKey**。

## <a name="send-events"></a>傳送事件 
本節說明如何建立可將事件傳送至事件中樞的 Java 應用程式。 

### <a name="add-reference-to-azure-event-hubs-library"></a>加入 Azure 事件中樞程式庫的參考

[Maven 中央儲存機制](https://search.maven.org/search?q=a:azure-messaging-eventhubs)中的 Maven 專案可使用事件中樞的 Java 用戶端程式庫。 您可以在 Maven 專案檔中用下列相依性宣告來參照此程式庫：

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>撰寫程式碼以將訊息傳送到事件中樞

針對下列範例，在您最喜愛的 Java 開發環境中，先為主控台/殼層應用程式建立新的 Maven 專案。 新增名為 `SimpleSend` 的類別，並在此類別中新增下列程式碼：

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
    public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>連接字串和事件中樞
此程式碼會使用事件中樞命名空間的連接字串，以及事件中樞名稱來建置事件中樞用戶端。 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>建立事件中樞生產者用戶端 
此程式碼會建立生產者用戶端物件，其用來產生事件/將事件傳送到事件中樞。 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducer();
```

### <a name="prepare-a-batch-of-events"></a>準備事件批次
此程式碼會準備一批事件。 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>將這批事件傳送到事件中樞
此程式碼會將您在上一個步驟中準備的事件批次傳送到事件中樞。 下列程式碼會封鎖傳送作業。 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>關閉並清除
此程式碼會關閉生產者。 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>用以傳送事件的完整程式碼
以下是可將事件傳送至事件中樞的完整程式碼。 

```java
import com.azure.messaging.eventhubs.*;

public class Sender {
    public static void main(String[] args) {
        final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        final String eventHubName = "EVENT HUB NAME";

        // create a producer using the namespace connection string and event hub name
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducer();

        // prepare a batch of events to send to the event hub    
        EventDataBatch batch = producer.createBatch();
        batch.tryAdd(new EventData("First event"));
        batch.tryAdd(new EventData("Second event"));
        batch.tryAdd(new EventData("Third event"));
        batch.tryAdd(new EventData("Fourth event"));
        batch.tryAdd(new EventData("Fifth event"));

        // send the batch of events to the event hub
        producer.send(batch);

        // close the producer
        producer.close();
    }
}
```

建置程式，並確定沒有任何錯誤。 執行接收者程式之後，您將會執行此程式。 

## <a name="receive-events"></a>接收事件
本教學課程中的程式碼是根據 [GitHub 上的 EventProcessorClient 程式碼](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java)，您可以檢查該程式碼以查看完整的運作中應用程式。

### <a name="create-a-java-project"></a>建立 Java 專案

適用於事件中樞的 Java 用戶端程式庫可以在來自 [Maven 中央儲存機制](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)的 Maven 專案中使用，而且可在您的 Maven 專案檔內使用下列相依性宣告來參考： 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. 使用下列程式碼，建立名為 `Receiver`的新類別。 使用您建立事件中樞和儲存體帳戶時所用的值，取代預留位置：
   
   ```java
    import com.azure.messaging.eventhubs.*;
    import com.azure.messaging.eventhubs.models.ErrorContext;
    import com.azure.messaging.eventhubs.models.EventContext;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;

    public class Receiver {

        private static final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        private static final String eventHubName = "EVENT HUB NAME";
    
        public static void main(String[] args) throws Exception {

            // function to process events
            Consumer<EventContext> processEvent = eventContext  -> {
                System.out.print("Received event: ");
                // print the body of the event
                System.out.println(eventContext.getEventData().getBodyAsString());
                eventContext.updateCheckpoint();
            };

            // function to process errors
            Consumer<ErrorContext> processError = errorContext -> {
                // print the error message
                System.out.println(errorContext.getThrowable().getMessage());
            };

            EventProcessorBuilder eventProcessorBuilder = new EventProcessorBuilder()
                .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                .connectionString(connectionString, eventHubName)
                .processEvent(processEvent)
                .processError(processError)
                .checkpointStore(new InMemoryCheckpointStore());
        
            EventProcessorClient eventProcessorClient = eventProcessorClientBuilder.buildEventProcessorClient();
            System.out.println("Starting event processor");
            eventProcessorClient.start();

            System.out.println("Press enter to stop.");
            System.in.read();

            System.out.println("Stopping event processor");
            eventProcessor.stop();
            System.out.println("Event processor stopped.");
    
            System.out.println("Exiting process");
        }
    }
    ```
    
2. 從 [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) 下載 **InMemoryCheckpointStore.java** 檔案，然後將其新增到您的專案。 
3. 建置程式，並確定沒有任何錯誤。 

## <a name="run-the-applications"></a>執行應用程式
1. 先執行**接收者**應用程式。
1. 然後，執行**傳送者**應用程式。 
1. 在**接收者**應用程式視窗中，確認您看到傳送者應用程式所發佈的事件。
1. 按下接收者應用程式視窗中 **ENTER** 以停止應用程式。 

## <a name="next-steps"></a>後續步驟
查看 [GitHub 上的 Java SDK 範例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)

