---
title: 寄回 Azure 資料箱的教學課程 | Microsoft Docs
description: 了解如何將您的 Azure 資料箱寄給 Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 517940ab4a3e004d99faf6ca2bedb43c93dba8c5
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514182"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>教學課程：送回 Azure 資料箱，並確認資料上傳至 Azure

::: zone-end

::: zone target="chromeless"

# <a name="return-data-box-and-verify-data-upload-to-azure"></a>送回資料箱，並確認資料上傳至 Azure

::: zone-end

::: zone target="docs"

本教學課程將說明如何送回 Azure 資料箱，並確認資料已上傳至 Azure。

在本教學課程中，您將了解如下所列的主題：

> [!div class="checklist"]
> * Prerequisites
> * 準備寄送
> * 將資料箱寄送給 Microsoft
> * 確認資料上傳至 Azure
> * 清除資料箱的資料

## <a name="prerequisites"></a>Prerequisites

在您開始前，請確定：

- 您已完成[教學課程：將資料複製到 Azure 資料箱並確認](data-box-deploy-copy-data.md)。 
- 複製作業已完成。 如果複製作業正在進行，則無法執行寄送準備。

## <a name="prepare-to-ship"></a>準備寄送

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

資料複製完成之後，您就可以準備寄送裝置。 裝置到達 Azure 資料中心時，資料就會自動上傳到 Azure。

## <a name="prepare-to-ship"></a>準備寄送

在您準備寄送之前，請確定複製作業已完成。

1. 前往本機 Web UI 中的 [準備寄送]  頁面，並開始準備寄送。 
2. 從本機 Web UI 關閉裝置。 從裝置移除纜線。 

後續步驟則取決於您退回裝置的地點。

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>寄回資料箱

確保裝置的資料副本完整且 [準備寄送]  執行成功。 根據您寄送裝置的區域，程序會有所不同。

::: zone-end

## <a name="in-us-canada-europetabin-us-canada-europe"></a>[美國、加拿大、歐洲](#tab/in-us-canada-europe)

如果在美國、加拿大或歐洲退回裝置，請執行下列步驟。

1. 確定裝置的電源已關閉，然後移除纜線。 
2. 纏繞裝置隨附的電源線，並將其安全地放在裝置背後。
3. 請確定出貨標籤已顯示在電子筆墨顯示器上，並安排貨運業者取貨。 如果標籤損毀、遺失或未顯示在電子筆墨顯示器上，請與 Microsoft 支援服務連絡。 若支援人員提出建議，則您可以前往 Azure 入口網站中的 [概觀] > [下載出貨標籤]  。 下載出貨標籤並貼在裝置上。 
4. 如果要送回裝置，請安排由 UPS 取貨。 若要排定取貨時間：

    - 致電給當地的 UPS (國家/地區特定的免付費電話號碼)。
    - 在您的電話中提供反向出貨追蹤號碼，如 E-ink 顯示或您列印出的標籤中所示。
    - 若未提供追蹤號碼，UPS 將在取貨期間要求您支付額外的費用。

    除了排定取貨時間，您也可以在最接近的托運地點托運該資料箱。
4. 一旦貨運業者收取資料箱並進行掃描，入口網站的訂單狀態會更新為 [已取貨]  。 此外，也會顯示追蹤識別碼。

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>確認資料上傳至 Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>清除資料箱的資料
 
完成上傳到 Azure 之後，資料箱會根據 [NIST SP 800-88 修訂 1 指導方針](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi)清除磁碟中的資料。

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]



::: zone-end


## <a name="in-australiatabin-australia"></a>[澳大利亞](#tab/in-australia)

澳洲的 Azure 資料中心有額外的安全性通知。 所有送達的貨物都必須有預先通知。 在澳洲寄送可採取下列步驟。


1. 保留用來寄送裝置以供退貨的原始外盒。
2. 確保裝置的資料副本完整且 [準備寄送]  執行成功。
3. 關閉裝置電源並移除纜線。
4. 纏繞裝置隨附的電源線，並將其安全地放在裝置背後。
5. 透過 [DHL 連結](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference)線上預約取貨服務。

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>確認資料上傳至 Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>清除資料箱的資料
 
完成上傳到 Azure 之後，資料箱會根據 [NIST SP 800-88 修訂 1 指導方針](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi)清除磁碟中的資料。

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-japantabin-japan"></a>[日本](#tab/in-japan) 

1. 保留用來寄送裝置以供退貨的原始外盒。
2. 關閉裝置電源並移除纜線。
3. 纏繞裝置隨附的電源線，並將其安全地放在裝置背後。
4. 在理貨單上寫下貴公司名稱和地址資訊，作為您的寄件者資訊。
5. 使用下列電子郵件範本傳送電子郵件給 Quantium Solutions。

    - 如果日本郵局運費到付託運單未隨附或遺失，請在這封電子郵件註明。 Quantium Solutions (Japan) 會要求日本郵局在取貨時提供理貨單。
    - 如果您有多個訂單，請透過電子郵件確保每件都會順利取貨。

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. 在預約取貨時間後，接收來自 Quantium Solutions 的電子郵件確認。 電子郵件確認也會包含運費到付理貨單的資訊。

如有需要，您可以透過下列資訊連絡 Quantium Solutions 支援人員 (日文)： 

- 電子郵件：Customerservice.JP@quantiumsolutions.com 
- 電話：03-5755-0150 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>確認資料上傳至 Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>清除資料箱的資料
 
完成上傳到 Azure 之後，資料箱會根據 [NIST SP 800-88 修訂 1 指導方針](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi)清除磁碟中的資料。

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end



