---
title: 教學課程：Apache Kafka Producer 和 Consumer API - Azure HDInsight
description: 了解如何搭配 HDInsight 上的 Kafka 使用 Apache Kafka Producer 和 Consumer API。 在本教學課程中，您將了解如何從 Java 應用程式將這些 API 用於 HDInsight 上的 Kafka。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 10/08/2019
ms.openlocfilehash: 65fc3259b0bc5fce61ccd1ceb8df30f1bba49b19
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77161710"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>教學課程：使用 Apache Kafka Producer 和 Consumer API

了解如何搭配 HDInsight 上的 Kafka 使用 Apache Kafka Producer 和 Consumer API。

Kafka Producer API 可讓應用程式將資料流傳送至 Kafka 叢集。 Kafka Consumer API 可讓應用程式從叢集讀取資料流。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * Prerequisites
> * 了解程式碼
> * 建置並部署應用程式
> * 在叢集上執行應用程式

如需 API 的詳細資訊，請參閱 [Producer API](https://kafka.apache.org/documentation/#producerapi) 和 [Consumer API](https://kafka.apache.org/documentation/#consumerapi) 的 Apache 說明文件。

## <a name="prerequisites"></a>Prerequisites

* HDInsight 3.6 上的 Apache Kafka。 若要深入了解如何建立 HDInsight 上的 Apache Kafka 叢集，請參閱[開始使用 HDInsight 上的 Apache Kafka](apache-kafka-get-started.md)。

* [Java Developer Kit (JDK) 第 8 版](https://aka.ms/azure-jdks)或同等版本，例如 OpenJDK。

* 根據 Apache 正確[安裝](https://maven.apache.org/install.html)的 [Apache Maven](https://maven.apache.org/download.cgi)。  Maven 是適用於 Java 專案的專案建置系統。

* SSH 用戶端。 如需詳細資訊，請參閱[使用 SSH 連線至 HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md)。

## <a name="understand-the-code"></a>了解程式碼

範例應用程式位於 [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) 的 `Producer-Consumer` 子目錄中。 該應用程式主要包含四個檔案：

* `pom.xml`:此檔案會定義專案相依性、Java 版本和封裝方法。
* `Producer.java`:此檔案會使用 Producer API 將隨機句子傳送至 Kafka。
* `Consumer.java`:此檔案會使用 Consumer API 從 Kafka 讀取資料並將其發送至 STDOUT。
* `Run.java`:用來執行產生者和取用者程式碼的命令列介面。

### <a name="pomxml"></a>Pom.xml

在 `pom.xml` 檔案中務必要了解的事項包括：

* 相依性：此專案依存於 `kafka-clients` 套件所提供的 Producer 和 Consumer API。 下列 XML 程式碼會定義此相依性：

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>${kafka.version}</version>
    </dependency>
    ```

    `${kafka.version}` 項目會在 `pom.xml` 的 `<properties>..</properties>` 區段中進行宣告，並設定為 HDInsight 叢集的 Kafka 版本。

* 外掛程式：Maven 外掛程式可提供多種功能。 在此專案中，會使用下列外掛程式：

    * `maven-compiler-plugin`:用來將專案所使用的 Java 版本設為 8。 這是HDInsight 3.6 所使用的 Java 版本。
    * `maven-shade-plugin`:用來產生包含此應用程式以及任何相依性的 uber jar。 它也可用來設定應用程式的進入點，如此您即可直接執行 Jar 檔案，而不需要指定主要類別。

### <a name="producerjava"></a>Producer.java

產生者會與 Kafka 訊息代理程式主機 (背景工作節點) 通訊，並將資料傳送至 Kafka 主題。 下列程式碼片段取自 [GitHub 存放庫](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started)中的 [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) 檔案，可說明如何設定產生者屬性：

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

取用者會與 Kafka 代理程式主機 (背景工作節點) 通訊，以迴圈方式讀取記錄。 來自於 [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) 檔案的下列程式碼片段會設定取用者屬性：

```java
KafkaConsumer<String, String> consumer;
// Configure the consumer
Properties properties = new Properties();
// Point it to the brokers
properties.setProperty("bootstrap.servers", brokers);
// Set the consumer group (all consumers must belong to a group).
properties.setProperty("group.id", groupId);
// Set how to serialize key/value pairs
properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
// When a group is first created, it has no offset stored to start reading from. This tells it to start
// with the earliest record in the stream.
properties.setProperty("auto.offset.reset","earliest");

consumer = new KafkaConsumer<>(properties);
```

在此程式碼中，取用者會設定為從主題的開頭處讀取 (`auto.offset.reset` 設為 `earliest`)。

### <a name="runjava"></a>Run.java

[Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) 檔案會提供可執行產生者或取用者程式碼的命令列介面。 您必須提供 Kafka 代理程式主機資訊作為參數。 您可以選擇性地包含取用者程序所使用的群組識別碼值。 如果您使用相同的群組識別碼建立多個取用者執行個體，這些執行個體將會以負載平衡的方式讀取主題。

## <a name="build-and-deploy-the-example"></a>建置並部署範例

1. 從 [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) 下載並解壓縮範例。

2. 將目前目錄設定為 `hdinsight-kafka-java-get-started\Producer-Consumer` 目錄的位置並使用下列命令︰

    ```cmd
    mvn clean package
    ```

    此命令會建立名為 `target` 的目錄，其中包含名為 `kafka-producer-consumer-1.0-SNAPSHOT.jar` 的檔案。

3. 將 `sshuser` 取代為叢集的 SSH 使用者，並將 `CLUSTERNAME` 取代為叢集的名稱。 輸入下列命令，將 `kafka-producer-consumer-1.0-SNAPSHOT.jar` 檔案複製到 HDInsight 叢集。 出現提示時，請輸入 SSH 使用者的密碼。

    ```cmd
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```

## <a id="run"></a> 執行範例

1. 將 `sshuser` 取代為叢集的 SSH 使用者，並將 `CLUSTERNAME` 取代為叢集的名稱。 輸入下列命令，開啟與叢集的 SSH 連線。 出現提示時，請輸入 SSH 使用者帳戶的密碼。

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. 安裝 [jq](https://stedolan.github.io/jq/)，這是命令列 JSON 處理器。 從開啟的 SSH 連線，輸入下列命令來安裝 `jq`：

    ```bash
    sudo apt -y install jq
    ```

1. 設定密碼變數。 請將 `PASSWORD` 取代為叢集登入密碼，然後輸入下列命令：

    ```bash
    export password='PASSWORD'
    ```

1. 擷取正確大小寫的叢集名稱。 視叢集的建立方式而定，叢集名稱的實際大小寫可能與您預期的不同。 此命令會取得實際的大小寫，然後將其儲存在變數中。 輸入下列命令：

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    ```
    > [!Note]  
    > 如果您是從叢集外部執行此程序，則應以不同的程序儲存叢集名稱。 請從 Azure 入口網站取得小寫的叢集名稱。 然後，在下列命令中，以叢集名稱取代 `<clustername>`，並執行命令：`export clusterName='<clustername>'`。  

1. 若要取得 Kafka 訊息代理程式主機，請使用下列命令：

    ```bash
    export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
    ```

    > [!Note]  
    > 此命令需要 Ambari 存取權。 如果您的叢集位於 NSG 後方，請從可存取 Ambari 的機器執行此命令。

1. 輸入下列命令，建立 Kafka 主題 `myTest`：

    ```bash
    java -jar kafka-producer-consumer.jar create myTest $KAFKABROKERS
    ```

1. 若要執行產生器，並且將資料寫入至主題，請使用下列命令：

    ```bash
    java -jar kafka-producer-consumer.jar producer myTest $KAFKABROKERS
    ```

1. 產生器完成後，請使用下列命令從主題讀取︰

    ```bash
    java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS
    ```

    已讀取的記錄以及記錄計數隨即顯示。

1. 使用 __Ctrl + C__ 來結束取用者。

### <a name="multiple-consumers"></a>多個取用者

Kafka 取用者會在讀取記錄時使用取用者群組。 多個取用者使用相同群組會導致從主題讀取負載平衡。 群組中的每個取用者都會收到一部分的記錄。

取用者應用程式接受作為群組識別碼的參數。 例如，下列命令會使用 `myGroup` 的群組識別碼啟動取用者：

```bash
java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup
```

使用 __Ctrl + C__ 來結束取用者。

若要查看此程序的運作情況，請使用下列命令︰

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; split-window -h 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; attach
```

此命令會使用 `tmux` 將終端機分割成兩個資料行。 取用者會在每個資料行中啟動 (使用相同的群組識別碼值)。 取用者完成讀取後，請留意到每個取用者僅讀取了記錄的一部分。 請使用 __Ctrl + C__ 兩次來結束 `tmux`。

透過主題的資料分割處理相同群組內的用戶端取用。 在此程式碼範例中，稍早建立的 `test` 主題有八個分割區。 如果您啟動八個取用者，則每個取用者都會從主題的單一分割區讀取記錄。

> [!IMPORTANT]  
> 一個取用者群組中的取用者執行個體不得超過資料分割。 在此範例中，一個取用者群組可以包含最多 8 個取用者，因為這是主題中的資料分割數目。 或者，您可以有多個取用者群組，其各有不超過 8 個取用者。

Kafka 中儲存的記錄會依照其在資料分割內接收的順序儲存。 若要達到依序傳遞「資料分割內」  的記錄，請建立取用者群組，其中的取用者執行個體數目與資料分割數目相符。 若要達到依序傳遞「主題內」  的記錄，請建立只有一個取用者執行個體的取用者群組。

## <a name="clean-up-resources"></a>清除資源

若要清除本教學課程所建立的資源，您可以刪除資源群組。 刪除資源群組也會刪除相關聯的 HDInsight 叢集，以及與資源群組相關聯的任何其他資源。

若要使用 Azure 入口網站移除資源群組：

1. 在 Azure 入口網站中展開左側功能表，以開啟服務的功能表，然後選擇 [資源群組]  以顯示資源群組的清單。
2. 找出要刪除的資源群組，然後以滑鼠右鍵按一下清單右側的 [更多]  按鈕 (...)。
3. 選取 [刪除資源群組]  ，並加以確認。

## <a name="next-steps"></a>後續步驟

在本文件中，您會了解如何使用 Apache Kafka Producer 和 Consumer API 搭配 HDInsight 上的 Apache Kafka。 使用下列各項來深入了解 Kafka 的使用方式︰

> [!div class="nextstepaction"]
> [分析 Apache Kafka 記錄](apache-kafka-log-analytics-operations-management.md)
