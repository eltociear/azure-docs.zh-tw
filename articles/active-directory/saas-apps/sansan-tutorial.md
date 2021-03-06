---
title: 教學課程：Azure Active Directory 與 Sansan 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Sansan 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea824cfd5e80ce3aff06774a2e1525cf97d15f65
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091861"
---
# <a name="tutorial-integrate-sansan-with-azure-active-directory"></a>教學課程：整合 Sansan 與 Azure Active Directory

在本教學課程中，您將了解如何整合 Sansan 與 Azure Active Directory (Azure AD)。 在整合 Sansan 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Sansan。
* 讓使用者使用其 Azure AD 帳戶自動登入 Sansan。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>必要條件

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 SanSan 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。 Sansan 支援 **SP** 起始的 SSO。

## <a name="adding-sansan-from-the-gallery"></a>從資源庫新增 Sansan

若要設定將 Sansan 整合到 Azure AD 中，您需要從資源庫將 Sansan 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增新的應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Sansan**。
1. 從結果面板選取 [Sansan]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

以名為 **Britta Simon** 的測試使用者，設定及測試與 Sansan 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Sansan 中相關使用者之間的連結關聯性。

若要設定及測試與 Sansan 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** ，讓您的使用者能夠使用此功能。
2. **[設定 Sansan](#configure-sansan)** 以在應用程式端設定 SSO 設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** ，以使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** ，讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Sansan 測試使用者](#create-sansan-test-user)** ，讓 Sansan 中對應的 Britta Simon 連結到使用者在 Azure AD 中的代表項目。
6. **[測試 SSO](#test-sso)** ，以驗證組態是否能運作。

### <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Sansan]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [選取單一登入方法]  頁面上，選取 [SAML]  。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  頁面上，輸入下列欄位的值：

    1. 在 [登入 URL]  文字方塊中，使用下列模式輸入任何一個 URL：
    
    | 環境 | URL |
    |:--- |:--- |
    | PC Web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | 原生行動應用程式 |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | 行動瀏覽器設定 |`https://ap.sansan.com/s/saml2/<company name>/acs` |

    2. 在 [識別碼 (實體識別碼)]  文字方塊中，您可以設定多個識別碼值並依據環境選取任何一個值。

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [Sansan 用戶端支援小組](https://www.sansan.com/form/contact)以取得此值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  以下載憑證並將其儲存在電腦上。

   ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Sansan]  區段上，根據您的需求複製適當的 URL。

   ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="configure-sansan"></a>設定 Sansan

若要在 **Sansan** 端設定單一登入，您必須將從 Azure 入口網站下載的 [憑證 (Base64)]  和複製的適當 URL 傳送給 [Sansan 用戶端支援小組](https://www.sansan.com/form/contact)。 他們會進行此設定，讓兩端的 SAML SSO 連線都設定正確。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您會在 Azure 入口網站中建立名稱為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `Britta Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `BrittaSimon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Sansan 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Sansan]  。
1. 在應用程式的概觀頁面，尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊中，從使用者清單選取 **Britta Simon**，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-sansan-test-user"></a>建立 Sansan 測試使用者

在本節中，您要在 Sansan 中建立名為 Britta Simon 的使用者。 Sansan 應用程式必須先在應用程式中佈建該使用者，才能執行 SSO。

> [!NOTE]
> 如果您需要手動建立一個或一批使用者，則必須連絡 [Sansan 支援小組](https://www.sansan.com/form/contact)。

### <a name="test-sso"></a>測試 SSO

當您在存取面板中選取 Sansan 圖格時，應該會自動登入您設定 SSO 的 Sansan。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)