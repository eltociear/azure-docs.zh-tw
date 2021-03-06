---
title: 診斷圖形效能問題遠端桌面-Azure
description: 本文說明如何在遠端桌面通訊協定會話中使用 RemoteFX 圖形計數器，以診斷 Windows 虛擬桌面中圖形的效能問題。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/23/2019
ms.author: helohr
ms.openlocfilehash: c41a433ee19969546e1db2aa583c72ed166b7ebf
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607467"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>診斷遠端桌面的圖形效能問題

若要診斷遠端會話的經驗品質問題，請在效能監視器的 [RemoteFX 圖形] 區段底下提供計數器。 本文可協助您在使用這些計數器的遠端桌面通訊協定（RDP）會話期間，找出並修正與圖形相關的效能瓶頸。

## <a name="find-your-remote-session-name"></a>尋找您的遠端會話名稱

您將需要遠端會話名稱，以識別圖形效能計數器。 遵循本節中的指示來識別每個計數器的實例。

1. 從遠端會話開啟 Windows 命令提示字元。
2. 執行**qwinsta**命令並尋找您的會話名稱。
    - 如果您的會話裝載于多會話的虛擬機器（VM）中：每個計數器的實例尾碼會與您會話名稱的尾碼相同，例如「rdp-tcp 37」。
    - 如果您的會話裝載于支援虛擬圖形處理器（vGPU）的 VM 中：每個計數器的實例會儲存在伺服器上，而不是存放在您的 VM 中。 您的計數器實例包含 VM 名稱，而不是會話名稱中的數位，例如 "Win8 Enterprise VM"。

>[!NOTE]
> 雖然計數器的名稱中有 RemoteFX，但它們也包含 vGPU 案例中的遠端桌面圖形。

## <a name="access-performance-counters"></a>存取效能計數器

在您決定遠端會話名稱之後，請遵循這些指示來收集遠端會話的 RemoteFX 圖形效能計數器。

1. 選取 [**啟動**] > [系統**管理工具**] [ > **效能監視器**]。
2. 在 [**效能監視器**] 對話方塊中，展開 [**監視工具**]，選取 [**效能監視器**]，然後選取 [**新增**]。
3. 在 [**新增計數器**] 對話方塊的 [**可用的計數器**] 清單中，展開 RemoteFX 圖形的區段。
4. 選取要監視的計數器。
5. 在 [**所選取物件的實例**] 清單中，選取要監視所選取計數器的特定實例，然後選取 [**新增**]。 若要選取所有可用的計數器實例，請選取 [**所有實例**]。
6. 新增計數器之後，請選取 **[確定]** 。

選取的效能計數器會出現在 [效能監視器] 畫面上。

>[!NOTE]
>主機上的每個使用中會話都有它自己的每個效能計數器實例。

## <a name="diagnose-issues"></a>診斷問題

圖形相關的效能問題通常分為四個類別：

- 低畫面播放速率
- 隨機停止
- 高輸入延遲
- 畫面格品質不佳

### <a name="addressing-low-frame-rate-random-stalls-and-high-input-latency"></a>解決低畫面播放速率、隨機停止和高輸入延遲

首先，請檢查 [輸出框架/秒] 計數器。 它會測量用戶端可用的畫面格數目。 如果這個值小於輸入畫面/秒計數器，則會略過框架。 若要識別瓶頸，請使用略過的/第二個計數器的畫面。

有三種類型的框架略過/第二個計數器：

- 略過的框架/秒（伺服器資源不足）
- 略過的框架/秒（網路資源不足）
- 略過的框架/秒（用戶端資源不足）

任何略過的框架/第二個計數器的值很高，表示問題與計數器追蹤的資源相關。 例如，如果用戶端未以相同的速率解碼和呈現畫面格，伺服器會提供框架，則略過的框架/秒（用戶端資源不足）計數器會很高。

如果輸出畫面/秒計數器與輸入畫面/秒計數器相符，但您仍發現異常的延遲或停止，則平均編碼時間可能是原因。 編碼是一種同步處理常式，會在單一會話（vGPU）案例中的伺服器上，以及在多會話案例中的 VM 上執行。 平均編碼時間應低於33毫秒。 如果平均編碼時間低於33毫秒，但您仍有效能問題，則您使用的應用程式或作業系統可能會發生問題。

如需診斷應用程式相關問題的詳細資訊，請參閱[使用者輸入延遲效能計數器](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters)。

因為 RDP 支援33毫秒的平均編碼時間，所以支援最多每秒30個畫面的輸入畫面播放速率。 請注意，33毫秒是支援的畫面播放速率上限。 在許多情況下，使用者所遇到的畫面播放速率會較低，視來源提供給 RDP 的頻率而定。 例如，觀看影片的工作需要30個框架/秒的完整輸入畫面播放速率，但較少的運算密集工作（例如不常編輯檔）會導致輸入畫面/秒的值變得較低，而且使用者的不會降低體驗品質。

### <a name="addressing-poor-frame-quality"></a>解決不佳的框架品質

使用 [畫面格品質] 計數器來診斷畫面格品質問題。 此計數器會以來源框架品質的百分比表示輸出框架的品質。 品質損失可能是因為 RemoteFX，或可能是圖形來源的固有。 如果 RemoteFX 導致品質遺失，問題可能是缺少網路或伺服器資源來傳送更高精確度的內容。

## <a name="mitigation"></a>緩和

如果伺服器資源造成瓶頸，請嘗試下列其中一種方法來改善效能：

- 減少每部主機的會話數目。
- 增加伺服器上的記憶體和計算資源。
- 捨棄連接的解析。

如果網路資源造成瓶頸，請嘗試下列其中一種方法來改善每個會話的網路可用性：

- 減少每部主機的會話數目。
- 使用較高頻寬的網路。
- 捨棄連接的解析。

如果用戶端資源造成瓶頸，請嘗試下列其中一種方法來改善效能：

- 安裝最新的遠端桌面用戶端。
- 增加用戶端電腦上的記憶體和計算資源。

> [!NOTE]
> 我們目前不支援 [來源框架/秒] 計數器。 目前，[來源框架/秒] 計數器一律會顯示0。

## <a name="next-steps"></a>後續步驟

- 若要建立 GPU 優化的 Azure 虛擬機器，請參閱[設定 Windows 虛擬桌面環境的圖形處理單元（GPU）加速](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu)。
- 如需疑難排解和擴大追蹤的總覽，請參閱[疑難排解總覽、意見反應和支援](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview)。
- 若要深入瞭解此服務，請參閱[Windows 桌面環境](https://docs.microsoft.com/azure/virtual-desktop/environment-setup)。
