---
title: 在 Azure 通知中樞中設定 Microsoft 推播通知服務 |Microsoft Docs
description: 瞭解如何設定 Azure 通知中樞的 Microsoft 推播通知服務設定。
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 0d7bf5410e05bb74a215fb7a95c704673b764b93
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212484"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-for-a-notification-hub-in-the-azure-portal"></a>在 Azure 入口網站中設定通知中樞的 Microsoft 推播通知服務（MPNS）設定
本文說明如何使用 Azure 入口網站來設定 Azure 通知中樞的 Microsoft 推播通知服務（MPNS）設定。 

## <a name="prerequisites"></a>必要條件
如果您尚未建立通知中樞，立即建立一個。 如需詳細資訊，請參閱[在 Azure 入口網站中建立 Azure 通知中樞](create-notification-hub-portal.md)。 

## <a name="configure-microsoft-push-notification-service-mpns"></a>設定 Microsoft 推播通知服務（MPNS）

下列程式提供設定通知中樞之 Microsoft 推播通知服務（MPNS）設定的步驟： 

1. 在 Azure 入口網站的 [**通知中樞**] 頁面上，選取左側功能表上的 **[Windows Phone （MPNS）** ]。
1. 啟用未經驗證或已驗證的推播通知：

   a. 若要啟用未經驗證的推播通知，請選取 [啟用未經驗證的推播通知]  >  [儲存]。

      ![顯示如何啟用未經驗證的推播通知的螢幕擷取畫面](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. 啟用已驗證推播通知：
      * 在工具列上選取 [上傳憑證]。
      * 選取檔案圖示，然後選取憑證檔案。
      * 輸入憑證的密碼。
      * 選取 [確定]。
      * 在 [Windows Phone (MPNS)] 頁面上，選取 [儲存]。

## <a name="next-steps"></a>後續步驟
如需教學課程，其中包含使用 Azure 通知中樞和 Microsoft 推播通知服務（MPNS）將通知推送至 Windows Phone 裝置的逐步指示，請參閱[使用通知中樞將通知推送至 Windows Phone 應用程式](notification-hubs-windows-mobile-push-notifications-mpns.md)。

