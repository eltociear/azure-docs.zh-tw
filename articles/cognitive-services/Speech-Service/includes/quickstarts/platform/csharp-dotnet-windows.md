---
title: 快速入門：適用於 .NET Framework (Windows) 的語音 SDK 平台設定 - 語音服務
titleSuffix: Azure Cognitive Services
description: 使用本指南來設定您的平台，以在適用於 Windows 的 .NET Framework 下搭配使用 C# 與語音服務 SDK。
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: db00398efa638ac945bc188ad0c44f904059e689
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467512"
---
本指南說明如何安裝適用於 .NET Framework (Windows) 的[語音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md)。

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Prerequisites

本快速入門需要：

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>建立 Visual Studio 專案並安裝語音 SDK

您需要安裝 [語音 SDK NuGet 套件](https://aka.ms/csspeech/nuget)，以便在程式碼中加以參考。 若要這麼做，您可能必須先建立 **helloworld** 專案。 如果您已經有 **.NET 桌面開發**工作負載的專案，您可以使用該專案，並跳至 [使用 NuGet 套件管理員來安裝語音 SDK](#use-nuget-package-manager-to-install-the-speech-sdk)。

### <a name="create-helloworld-project"></a>建立 helloworld 專案

1. 開啟 Visual Studio 2019。

1. 在 [開始] 視窗中，選取 [建立新專案]  。 

1. 在 [建立新專案]  視窗中，選擇 [主控台應用程式 (.NET Framework)]  ，然後選取 [下一步]  。

1. 在 [設定您的新專案]  視窗中，在 [專案名稱]  中輸入 *helloworld*、在 [位置]  中選擇或建立目錄路徑，然後選取 [建立]  。

1. 從 [Visual Studio] 功能表列，選取 [工具]   > [取得工具和功能]  ，以開啟 Visual Studio 安裝程式並顯示 [修改]  對話方塊。

1. 請檢查 [.NET 桌面開發]  工作負載是否可用。 如果尚未安裝此工作負載，請選取旁邊的核取方塊，然後選取 [修改]  以開始安裝。 下載及安裝可能需要幾分鐘的時間。

   如果 [.NET 桌面開發]  旁邊的核取方塊已經選取，請選取 [關閉]  來結束對話方塊。

   ![啟用 .NET 桌面開發](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. 關閉 Visual Studio 安裝程式。

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>使用 NuGet 套件管理員來安裝語音 SDK

1. 在 [方案總管] 中，以滑鼠右鍵按一下 **helloworld** 專案，然後選取 [管理 NuGet 套件]  以顯示 NuGet 套件管理員。

   ![NuGet 套件管理員](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. 在右上角找到 [套件來源]  下拉式方塊，並確定已選取 **`nuget.org`** 。

1. 選取左上角的 [瀏覽]  。

1. 在搜尋方塊中，鍵入 *Microsoft.CognitiveServices.Speech*，然後選取 **Enter**。

1. 從搜尋結果中，選取 [Microsoft.CognitiveServices.Speech]  套件，然後選取 [安裝]  以安裝最新的穩定版本。

   ![安裝 Microsoft.CognitiveServices.Speech NuGet 套件](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. 接受所有合約和授權，即可開始安裝。

   安裝套件之後，[套件管理員主控台]  視窗中會出現確認訊息。

您現在可以繼續進行下列[後續步驟](#next-steps)。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [windows](../quickstart-list.md)]
