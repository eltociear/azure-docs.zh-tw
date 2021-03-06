---
title: 具有 VS Code 消費者入門的 Azure Service Fabric
description: 本文是使用 Visual Studio Code 建立 Service Fabric 應用程式的概觀。
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: d7d3182ad00d0ce151c6d327b29584c7e2ff1323
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75457857"
---
# <a name="service-fabric-for-visual-studio-code"></a>適用於 Visual Studio Code 的 Service Fabric

[適用於 VS Code 的 Service Fabric Reliable Services 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services)提供在 Windows、Linux 和 macOS 作業系統上建立及建置 Service Fabric 應用程式，並對其進行偵錯所需的工具。

本文提供擴充功能需求和設定的概觀，以及擴充功能所提供各種命令的使用方式。 

> [!IMPORTANT]
> Service Fabric Java 應用程式可以在 Windows 機器上開發，但是只能部署到 Azure Linux 叢集上。 不支援在 Windows 上針對 Java 應用程式進行偵錯。

## <a name="prerequisites"></a>必要條件

必須在所有環境上安裝下列必要條件。

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Service Fabric SDK](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman 產生器 - 為您的應用程式安裝適當的產生器

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

必須針對 Java 開發安裝下列必要條件：

* [Java SDK](https://aka.ms/azure-jdks) (1.8 版)
* [Gradle](https://gradle.org/install/)
* [適用於 Java VS Code 擴充功能的偵錯工具](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)，需要該工具才能針對 Java 服務進行偵錯。 針對 Java 服務進行偵錯只在 Linux 上受到支援。 您可以藉由在 VS Code 中的 [活動列] 按一下[擴充功能] 圖示並搜尋擴充功能來安裝，或者從 VS Code 市集安裝。

必須針對 .NET Core/C# 開發安裝下列必要條件：

* [.NET Core](https://www.microsoft.com/net/learn/get-started) (版本 2.0.0 或更新版本)
* [適用於 Visual Studio Code 的 C# (由 OmniSharp 驅動) VS Code 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)，需要該擴充功能才能針對 C# 服務進行偵錯。 您可以藉由在 VS Code 中的 [活動列] 按一下[擴充功能] 圖示並搜尋擴充功能來安裝，或者從 VS Code 市集安裝。

## <a name="setup"></a>安裝程式

1. 開啟 VS Code。
2. 按一下 VS Code 左側 [活動列] 上的 [擴充功能] 圖示。 搜尋 "Service Fabric"。 針對 Service Fabric Reliable Services 擴充功能按一下 [安裝]。

## <a name="commands"></a>命令
適用於 VS Code 的 Service Fabric Reliable Services 擴充功能提供許多命令，可以協助開發人員建立及部署 Service Fabric 專案。 您可以按下 `(Ctrl + Shift + p)`、將命令名稱鍵入到輸入列中，然後從提示清單選取想要的命令，從 [命令選擇區] 中呼叫命令。 

* Service Fabric: Create Application 
* Service Fabric: Publish Application 
* Service Fabric: Deploy Application 
* Service Fabric: Remove Application  
* Service Fabric: Build Application 
* Service Fabric: Clean Application 

### <a name="service-fabric-create-application"></a>Service Fabric: Create Application

**Service Fabric: Create Application** 命令會在您目前的工作區中建立全新 Service Fabric 應用程式。 根據開發機器上安裝的是哪個 Yeoman 產生器，您可以建立數種類型的 Service Fabric 應用程式，包括 Java、C#、容器及客體專案。 

1.  選取 **Service Fabric: Add Service** 命令
2.  選取新 Service Fabric 應用程式的類型。 
3.  輸入您想要建立的應用程式名稱
3.  選取您想要新增到 Service Fabric 應用程式中的服務類型。 
4.  遵循提示來為服務命名。 
5.  新的 Service Fabric 應用程式會出現在工作區中。
6.  開啟新的應用程式資料夾，使其變成工作區中的根資料夾。 您可以從這裡繼續執行命令。

### <a name="service-fabric-add-service"></a>Service Fabric: Add Service
**Service Fabric: Add Service** 命令會將新服務新增至現有的 Service Fabric 應用程式中。 服務要新增至其中的應用程式，必須位於工作區的根目錄中。 

1.  選取 **Service Fabric: Add Service** 命令。
2.  選取目前 Service Fabric 應用程式的類型。 
3.  選取您想要新增到 Service Fabric 應用程式中的服務類型。 
4.  遵循提示來為服務命名。 
5.  新的服務會出現在專案目錄中。 

### <a name="service-fabric-publish-application"></a>Service Fabric: Publish Application
**Service Fabric: Publish Application** 命令會在遠端叢集上部署您的 Service Fabric 應用程式。 目標叢集安全與否並無大礙。 若未在 Cloud.json 中設定參數，則應用程式會部署到本機叢集。

1.  第一次建置應用程式時，會在專案目錄中產生 Cloud.json 檔案。
2.  將您想要連線的叢集值輸入到 Cloud.json 檔案中。
3.  選取 **Service Fabric: Publish Application** 命令。
4.  使用 Service Fabric Explorer 檢視目標叢集，確認應用程式是否已安裝。 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: Deploy Application (Localhost)
**Service Fabric: Deploy Application** 命令會在本機叢集上部署您的 Service Fabric 應用程式。 在使用命令之前，請先確定您的本機叢集正在執行。 

1. 選取 **Service Fabric: Deploy Application** 命令
2. 使用 Service Fabric Explorer （HTTP：\//localhost： 19080/Explorer）來查看本機叢集，以確認已安裝應用程式。 這可能需要一些時間，請耐心等候。
3. 您也可以於未在 Cloud.json 檔案中設定參數時使用 **Service Fabric: Publish Application** 命令，以部署到本機叢集。

> [!NOTE]
> 在 Windows 機器上不支援將 Java 應用程式部署到本機叢集。

### <a name="service-fabric-remove-application"></a>Service Fabric: Remove Application
**Service Fabric: Remove Application** 命令會從先前使用 VS Code 擴充功能部署所在的叢集內，移除 Service Fabric 應用程式。 

1.  選取 **Service Fabric: Remove Application** 命令。
2.  使用 Service Fabric Explorer 檢視叢集，確認應用程式是否已移除。 這可能需要一些時間，請耐心等候。

### <a name="service-fabric-build-application"></a>Service Fabric: Build Application
**Service Fabric： Build Application**命令可以建立 JAVA 或C# Service Fabric 應用程式。 

1.  在執行此命令之前，請先確定您是在應用程式根資料夾中。 命令會識別應用程式類型 (C# 或 Java)，並且據以建置您的應用程式。
2.  選取 **Service Fabric: Build Application** 命令。
3.  建置程序的輸出會寫入到整合式終端機。

### <a name="service-fabric-clean-application"></a>Service Fabric: Clean Application
**Service Fabric: Clean Application** 命令會刪除建置所產生的所有 jar 檔案和原生程式庫。 僅適用於 Java 應用程式。 

1.  在執行此命令之前，請先確定您是在應用程式根資料夾中。 
2.  選取 **Service Fabric: Clean Application** 命令。
3.  清除程序的輸出會寫入到整合式終端機。

## <a name="next-steps"></a>後續步驟

* 了解如何[使用 VS Code 開發 C# Service Fabric 應用程式及進行偵錯](./service-fabric-develop-csharp-applications-with-vs-code.md)。
* 了解如何[使用 VS Code 開發 Java Service Fabric 應用程式及進行偵錯](./service-fabric-develop-java-applications-with-vs-code.md)。
