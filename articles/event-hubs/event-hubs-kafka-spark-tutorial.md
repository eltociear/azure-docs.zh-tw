---
title: 與 Apache Spark 應用程式連線 - Azure 事件中樞 | Microsoft Docs
description: 本文提供有關如何將 Apache Spark 與「適用於 Kafka 的 Azure 事件中樞」搭配使用的資訊。
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: tutorial
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 5bd25604844ce0521b6bac2d637a2d48a6ef089b
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555729"
---
# <a name="connect-your-apache-spark-application-with-kafka-enabled-azure-event-hubs"></a>連線 Apache Spark 應用程式與已啟用 Kafka 的 Azure 事件中樞
本教學課程會引導您將 Spark 應用程式連線至已啟用 Kafka 的事件中樞，以進行即時串流。 這項整合可讓您無須變更通訊協定用戶端或執行您自己的 Kafka 或 Zookeeper 叢集，即可進行串流。 本教學課程需要 Apache Spark v2.4+ 和 Apache Kafka v2.0+。

> [!NOTE]
> 您可在 [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark/) 上取得此範例

在本教學課程中，您會了解如何：
> [!div class="checklist"]
> * 建立事件中樞命名空間
> * 複製範例專案
> * 執行 Spark
> * 讀取適用於 Kafka 的事件中樞
> * 寫入適用於 Kafka 的事件中樞

## <a name="prerequisites"></a>必要條件

開始本教學課程之前，請確定您具有：
-   Azure 訂用帳戶。 如果您沒有訂用帳戶，請[建立免費帳戶](https://azure.microsoft.com/free/)。
-   [Apache Spark v2.4](https://spark.apache.org/downloads.html)
-   [Apache Kafka v2.0]( https://kafka.apache.org/20/documentation.html)
-   [Git](https://www.git-scm.com/downloads)

> [!NOTE]
> 自 Spark v2.4 起，Spark-Kafka 配接器已更新為支援 Kafka v2.0。 在舊版的 Spark 中，配接器可支援 Kafka v0.10 和更新版本，但須依賴 Kafka v0.10 API。 由於適用於 Kafka 的事件中樞不支援 Kafka v0.10，v2.4 之前的 Spark 版本隨附的 Spark-Kafka 配接器將不受「適用於 Kafka 的事件中樞」生態系統支援。


## <a name="create-an-event-hubs-namespace"></a>建立事件中樞命名空間
您需要事件中樞命名空間，才能從任何事件中樞服務傳送和接收。 如需有關取得事件中樞 Kafka 端點的指示，請參閱[建立啟用 Kafka 的事件中樞](event-hubs-create.md)。 請取得事件中樞連接字串和完整網域名稱 (FQDN) 以供稍後使用。 如需相關指示，請參閱[取得事件中樞連接字串](event-hubs-get-connection-string.md)。 

## <a name="clone-the-example-project"></a>複製範例專案
請複製 Azure 事件中樞存放庫，並瀏覽至 `tutorials/spark` 子資料夾：

```bash
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/spark
```

## <a name="read-from-event-hubs-for-kafka"></a>讀取適用於 Kafka 的事件中樞
透過一些組態變更，您即可開始讀取適用於 Kafka 的事件中樞。 使用您的命名空間詳細資料更新 **BOOTSTRAP_SERVERS** 和 **EH_SASL**，您即可開始使用事件中樞進行串流處理，如同使用 Kafka 一般。 如需完整的範例程式碼，請參閱 GitHub 上的 sparkConsumer.scala 檔案。 

```scala
//Read from your Event Hub!
val df = spark.readStream
    .format("kafka")
    .option("subscribe", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("kafka.request.timeout.ms", "60000")
    .option("kafka.session.timeout.ms", "30000")
    .option("kafka.group.id", GROUP_ID)
    .option("failOnDataLoss", "false")
    .load()

//Use dataframe like normal (in this example, write to console)
val df_write = df.writeStream
    .outputMode("append")
    .format("console")
    .start()
```

## <a name="write-to-event-hubs-for-kafka"></a>寫入適用於 Kafka 的事件中樞
您也可以用寫入 Kafka 的相同方式來寫入事件中樞。 別忘了使用您事件中樞命名空間中的資訊來更新組態，以變更 **BOOTSTRAP_SERVERS** 和 **EH_SASL**。  如需完整的範例程式碼，請參閱 GitHub 上的 sparkProducer.scala 檔案。 

```scala
df = /**Dataframe**/

//Write to your Event Hub!
df.writeStream
    .format("kafka")
    .option("topic", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("checkpointLocation", "./checkpoint")
    .start()
```



## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何使用 Spark-Kafka 連接器和適用於 Kafka 的事件中樞進行串流處理。 您執行了下列步驟： 

> [!div class="checklist"]
> * 建立事件中樞命名空間
> * 複製範例專案
> * 執行 Spark
> * 讀取適用於 Kafka 的事件中樞
> * 寫入適用於 Kafka 的事件中樞

若要深入了解事件中樞和適用於 Kafka 的事件中樞，請參閱下列主題：  

- [了解事件中樞](event-hubs-what-is-event-hubs.md)
- [適用於 Apache Kafka 的事件中樞](event-hubs-for-kafka-ecosystem-overview.md)
- [如何建立已啟用 Kafka 的事件中樞](event-hubs-create-kafka-enabled.md)
- [從您的 Kafka 應用程式串流到事件中樞](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [在已啟用 Kafka 的事件中樞中鏡射 Kafka 訊息代理程式](event-hubs-kafka-mirror-maker-tutorial.md)
- [將 Apache Flink 連線到已啟用 Kafka 的事件中樞](event-hubs-kafka-flink-tutorial.md)
- [整合 Kafka Connect 與已啟用 Kafka 的事件中樞](event-hubs-kafka-connect-tutorial.md)
- [將 Akka Streams 連線到已啟用 Kafka 的事件中樞](event-hubs-kafka-akka-streams-tutorial.md)
- [在 GitHub 上探索範例](https://github.com/Azure/azure-event-hubs-for-kafka) \(英文\)
