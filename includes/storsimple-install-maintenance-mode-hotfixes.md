---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2019
ms.locfileid: "67174009"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>透過 Windows PowerShell for StorSimple 安裝維護模式 Hotfix
> [!IMPORTANT]
> 在維護模式中，您需要先在某一個控制站上套用 Hotfix，然後在另一個控制站上套用 Hotfix。
> 
> 

1. 使裝置處於維護模式。 如需如何進入維護模式的相關指示，請參閱[步驟 2：進入維護模式](../articles/storsimple/storsimple-update-device.md#step2)。
2. 若要套用 Hotfix，請輸入：
   
     `Start-HcsHotfix` 
3. 出現提示時，請提供包含 Hotfix 檔案的網路共用資料夾所在路徑。
4. 系統將提示您進行確認。 輸入 **Y** 繼續安裝 Hotfix。
5. 在某一個控制站上套用 Hotfix 之後，請登入另一個控制站。 使用您為前一個控制站所做的方式來套用 Hotfix。
6. 套用 Hotfix 之後，結束維護模式。 如需相關指示，請參閱[步驟 4：結束維護模式](../articles/storsimple/storsimple-update-device.md#step4)。

