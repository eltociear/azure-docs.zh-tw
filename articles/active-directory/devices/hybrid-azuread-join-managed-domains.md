---
title: 設定適用於受控網域的混合式 Azure Active Directory Join | Microsoft Docs
description: 了解如何設定適用於受控網域的混合式 Azure Active Directory Join。
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17bfbc29f38230dc2533c9ccc63cdee4fc776717
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2020
ms.locfileid: "76512103"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>教學課程：設定適用於受控網域的混合式 Azure Active Directory Join

就像是貴組織中的使用者，裝置是您想要保護的核心身分識別。 您可以使用裝置的身分識別，隨時隨地保護您的資源。 您可以使用下列其中一種方法，將裝置身分識別導入 Azure Active Directory (Azure AD) 中進行管理，以達到此目標：

- Azure AD Join
- 混合式 Azure AD Join
- Azure AD 註冊

將您的裝置導入 Azure AD 中，您將可透過跨雲端和內部部署資源的單一登入 (SSO)，將使用者的生產力最大化。 同時您可以利用[條件式存取](../active-directory-conditional-access-azure-portal.md)來保護對雲端和內部部署資源的存取。

在本教學課程中，您會了解如何為受控環境中已加入 Active Directory 網域的電腦裝置設定混合式 Azure AD Join。 

受控環境可使用[無縫單一登入](../hybrid/how-to-connect-sso.md)透過[密碼雜湊同步 (PHS)](../hybrid/whatis-phs.md) 或[傳遞驗證 (PTA)](../hybrid/how-to-connect-pta.md) 進行部署。 在這些案例中，您不需要設定同盟伺服器以進行驗證。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * 設定混合式 Azure AD Join
> * 啟用舊版 Windows 裝置
> * 確認加入的裝置
> * 疑難排解

## <a name="prerequisites"></a>Prerequisites

本教學課程假設您已熟悉以下文章：

- [什麼是裝置身分識別？](overview.md)
- [如何規劃混合式 Azure AD Join 實作](hybrid-azuread-join-plan.md)
- [如何進行混合式 Azure AD 聯結的受控驗證](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD 不支援受控網域中的智慧卡或憑證。

若要設定本文中的案例，您必須安裝[最新版的 Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 或更新版本)。

確認 Azure AD Connect 已將您要加入混合式 Azure AD 之裝置的電腦物件同步處理至 Azure AD。 如果電腦物件屬於特定組織單位 (OU)，則您也必須在 Azure AD Connect 中設定 OU 進行同步處理。 若要深入了解如何使用 Azure AD Connect 同步處理電腦物件，請參閱[使用 Azure AD Connect 設定篩選](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering)。

從 1.1.819.0 版開始，Azure AD Connect 即包含一個精靈，您可用來設定混合式 Azure AD Join。 此精靈可大幅簡化設定程序。 精靈會設定裝置註冊的服務連接點 (SCP)。

本文中的設定步驟是以在 Azure AD Connect 中使用精靈為基礎。

要使用混合式 Azure AD Join，您的裝置必須能夠從組織的網路中存取下列 Microsoft 資源：  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (如果您使用或計劃使用無縫 SSO)

如果組織需要透過輸出 Proxy 存取網際網路，Microsoft 建議[實作 Web Proxy 自動探索 (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) \(英文\)，讓 Windows 10 電腦能向 Azure AD 進行裝置註冊。 如果您在設定和管理 WPAD 方面遇到問題，請參閱[針對自動偵測進行疑難排解](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10))。 

如果您未使用 WPAD 且必須在電腦上設定 Proxy 設定，您可以從 Windows 10 1709 開始執行操作。 如需詳細資訊，請參閱[使用群組原則物件 (GPO) 設定 WinHTTP 設定](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/)。

> [!NOTE]
> 如果您使用 WinHTTP 設定在電腦上設定 Proxy 設定，則任何無法連線到所設定 Proxy 的電腦將無法連線到網際網路。

如果您的組織需要透過已驗證的輸出 Proxy 存取網際網路，您就必須確定 Windows 10 電腦可以成功向輸出 Proxy 進行驗證。 因為 Windows 10 電腦會使用電腦內容來執行裝置註冊，所以必須使用電腦內容來設定輸出 Proxy 驗證。 請向您的輸出 Proxy 提供者洽詢相關設定需求。

若要確認裝置是否能夠在系統帳戶下存取上述 Microsoft 資源，您可以使用[測試裝置註冊連線能力](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0)指令碼。

## <a name="configure-hybrid-azure-ad-join"></a>設定混合式 Azure AD Join

若要使用 Azure AD Connect 設定混合式 Azure AD Join，您需要：

- Azure AD 租用戶的全域管理員認證
- 每個樹系的企業系統管理員認證

**若要使用 Azure AD Connect 設定混合式 Azure AD Join：**

1. 啟動 Azure AD Connect，然後選取 [設定]  。

   ![歡迎使用](./media/hybrid-azuread-join-managed-domains/11.png)

1. 在 [其他工作]  頁面上選取 [設定裝置選項]  ，然後選取 [下一步]  。

   ![其他工作](./media/hybrid-azuread-join-managed-domains/12.png)

1. 在 [概觀]  頁面上，選取 [下一步]  。

   ![概觀](./media/hybrid-azuread-join-managed-domains/13.png)

1. 在 [連線到 Azure AD]  頁面上，輸入 Azure AD 租用戶的全域管理員認證。  

   ![連接至 Azure AD](./media/hybrid-azuread-join-managed-domains/14.png)

1. 在 [裝置選項]  頁面上選取 [設定混合式 Azure AD Join]  ，然後選取 [下一步]  。

   ![裝置選項](./media/hybrid-azuread-join-managed-domains/15.png)

1. 在 [SCP]  頁面上，針對要以 Azure AD Connect 設定 SCP 的每個樹系完成下列步驟，然後選取 [下一步]  ：

   ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

   1. 選取樹系。
   1. 選取驗證服務。
   1. 選取 [新增]  ，並輸入企業系統管理員認證。

1. 在 [裝置作業系統]  頁面上，選取 Active Directory 環境中的裝置所使用的作業系統，然後選取 [下一步]  。

   ![裝置作業系統](./media/hybrid-azuread-join-managed-domains/17.png)

1. 在 [準備設定]  頁面上，選取 [設定]  。

   ![準備設定](./media/hybrid-azuread-join-managed-domains/19.png)

1. 在 [設定完成]  頁面上，選取 [結束]  。

   ![組態完成](./media/hybrid-azuread-join-managed-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>啟用舊版 Windows 裝置

如果有些已加入網域的裝置是舊版 Windows 裝置，您需要︰

- 設定裝置註冊的近端內部網路設定
- 設定無縫 SSO
- 為舊版 Windows 電腦安裝 Microsoft Workplace Join

> [!NOTE]
> Windows 7 支援已於 2020 年 1 月 14 日終止。 如需詳細資訊，請參閱 [Windows 7 的支援已終止](https://support.microsoft.com/en-us/help/4057281/windows-7-support-ended-on-january-14-2020)。

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>設定裝置註冊的近端內部網路設定

若要順利完成舊版 Windows 裝置的混合式 Azure AD Join，並避免在裝置向 Azure AD 進行驗證時出現憑證提示，您可以將原則推送到已加入網域的裝置，以在 Internet Explorer 中將下列 URL 新增至近端內部網路區域︰

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

您也必須在使用者的本機內部網路區域中啟用 [允許透過指令碼更新狀態列]  。

### <a name="configure-seamless-sso"></a>設定無縫 SSO

若要在使用 [PHS](../hybrid/whatis-phs.md) 或 [PTA](../hybrid/how-to-connect-pta.md) 作為 Azure AD 雲端驗證方法的受控網域中，順利完成舊版 Windows 裝置的混合式 Azure AD Join，您也必須[設定無縫 SSO](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature)。

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>為舊版 Windows 電腦安裝 Microsoft Workplace Join

若要註冊舊版 Windows 裝置，組織必須安裝[適用於非 Windows 10 電腦的 Microsoft Workplace Join](https://www.microsoft.com/download/details.aspx?id=53554)。 適用於非 Windows 10 電腦的 Microsoft Workplace Join 可在 Microsoft 下載中心取得。

您可以使用軟體發佈系統 (例如  [Microsoft Endpoint Configuration Manager](https://docs.microsoft.com/configmgr/)) 來部署此套件。 此套件支援使用 `quiet` 參數的標準無訊息安裝選項。 組態管理員的目前分支會提供勝過舊版的好處，例如能夠追蹤已完成的註冊。

安裝程式會在系統上建立排定的工作，此工作是在使用者內容中執行。 此工作會在使用者登入 Windows 時觸發。 此工作會在使用 Azure AD 進行驗證之後，利用使用者認證以無訊息方式向 Azure AD 加入裝置。

## <a name="verify-the-registration"></a>確認註冊

若要確認 Azure 租用戶的裝置註冊狀態，您可以在 [Azure Active Directory PowerShell 模組](/powershell/azure/install-msonlinev1?view=azureadps-2.0)中使用 **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** Cmdlet。

使用 **Get-MSolDevice** Cmdlet 來檢查服務詳細資料時：

- 必須要有**裝置識別碼**與 Windows 用戶端上的識別碼相符的物件存在。
- **DeviceTrustType** 的值必須是 [已加入網域]  。 此設定相當於 Azure AD 入口網站中的 [裝置]  頁面上所呈現的 [已加入混合式 Azure AD]  狀態。
- 在條件式存取中使用的裝置，[Enabled]  的值必須是 [True]  ，而 [DeviceTrustLevel]  必須是 [Managed]  。

**若要查看服務詳細資料**：

1. 以系統管理員身分開啟 Windows PowerShell。
1. 輸入 `Connect-MsolService` 以連線至您的 Azure 租用戶。  
1. 輸入 `get-msoldevice -deviceId <deviceId>`。
1. 確認 [已啟用]  設為 [True]  。

## <a name="troubleshoot-your-implementation"></a>對您的實作進行疑難排解

如果您為已加入網域的 Windows 裝置完成混合式 Azure AD Join 時遇到問題，請參閱：

- [針對現行 Windows 裝置的混合式 Azure AD Join 進行疑難排解](troubleshoot-hybrid-join-windows-current.md)
- [針對舊版 Windows 裝置的混合式 Azure AD Join 進行疑難排解](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>後續步驟

深入了解如何[使用 Azure 入口網站來管理裝置身分識別](device-management-azure-portal.md)。
