---
title: 快速入門：建立 iOS 應用程式
description: 在本快速入門中，您將了解如何使用 Spatial Anchors 建置 iOS 應用程式。
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 277a669484201a060a2bb5455d6154165bbb8e84
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465159"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>快速入門：使用 Swift 或 Objective-C 建立搭配 Azure Spatial Anchors 的 iOS 應用程式

本快速入門將說明如何透過 Swift 或 Objective-C 使用 [Azure Spatial Anchors](../overview.md) 建立 iOS 應用程式。 Azure Spatial Anchors 是一款跨平台開發人員服務，可讓您使用在一段時間之後仍跨裝置保持其位置的物件，建立混合實境體驗。 當您完成時，您將會有可儲存和回收空間錨點的 ARKit iOS 應用程式。

您將學習如何：

> [!div class="checklist"]
> * 建立 Spatial Anchors 帳戶
> * 設定 Spatial Anchors 帳戶識別碼和帳戶金鑰
> * 在 iOS 裝置上部署和執行

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisites

若要完成本快速入門，請確定您具備︰

- 由開發人員啟用且已安裝最新版本 <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> 和 <a href="https://cocoapods.org" target="_blank">CocoaPods</a> 的 macOS 機器。
- 透過 HomeBrew 安裝的 Git。 在終端機的單一行輸入以下命令：`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`。 然後執行 `brew install git` 和 `brew install git-lfs`。
- 由開發人員啟用且<a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">與 ARKit 相容</a>的 iOS 裝置。

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>開啟範例專案

使用終端機執行下列動作。

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

使用 CocoaPods 安裝所需的 Pod：

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

瀏覽至 `iOS/Swift/`。

```bash
cd ./iOS/Swift/
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

瀏覽至 `iOS/Objective-C/`。

```bash
cd ./iOS/Objective-C/
```

---

執行 `pod install --repo-update` 以安裝適用於專案的 CocoaPods。

現在，在 Xcode 中開啟 `.xcworkspace`。

> [!NOTE]
> 如果您在升級至 macOS Catalina (10.15) 之後遇到 CocoaPod 問題，請參閱[這裡](#cocoapods-issues-on-macos-catalina-1015)的疑難排解步驟。

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>設定帳戶識別碼和金鑰

下一個步驟是將應用程式設定為使用您的帳戶識別碼和帳戶金鑰。 [設定空間錨點資源](#create-a-spatial-anchors-resource)時，將它們複製到文字編輯器中。

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

開啟 `iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift`。

找出 `spatialAnchorsAccountKey` 欄位，並將 `Set me` 取代為帳戶金鑰。

找出 `spatialAnchorsAccountId` 欄位，並將 `Set me` 取代為帳戶識別碼。

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

開啟 `iOS/Objective-C/SampleObjC/BaseViewController.m`。

找出 `SpatialAnchorsAccountKey` 欄位，並將 `Set me` 取代為帳戶金鑰。

找出 `SpatialAnchorsAccountId` 欄位，並將 `Set me` 取代為帳戶識別碼。

---

## <a name="deploy-the-app-to-your-ios-device"></a>將應用程式部署到您的 iOS 裝置

將 iOS 裝置連接至 Mac，並將 [作用中配置]  設定為您的 iOS 裝置。

![選取裝置](./media/get-started-ios/select-device.png)

選取 [建置然後執行目前的配置]  。

![部署並執行](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> 如果您看到 `library not found for -lPods-SampleObjC` 錯誤，您可能開到 `.xcodeproj` 檔案，而不是 `.xcworkspace`。 開啟 `.xcworkspace`，然後再試一次。

在 Xcode 中，按下 [停止]  即可停止應用程式。

## <a name="troubleshooting"></a>疑難排解

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>macOS Catalina (10.15) 上的 CocoaPods 問題

如果您最近更新至 macOS Catalina(10.15) 並且事先安裝 CocoaPods，則 CocoaPods 可能處於中斷狀態，無法正確設定您的 Pod 和 `.xcworkspace` 專案檔案。 若要解決此問題，您必須執行下列命令來重新安裝 CocoaPods：

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [教學課程：跨裝置共用 Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)
