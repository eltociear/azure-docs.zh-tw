---
title: Cloud Partner 入口網站 API 參考 |Azure Marketplace
description: Marketplace API 作業的描述、使用先決條件及清單。
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: b6591e1780d03cbfaff70fbd19ec3dfd274fae79
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819639"
---
<a name="cloud-partner-portal-api-reference"></a>Cloud Partner 入口網站 API 參考
==================================

Cloud Partner 入口網站 REST API 允許對工作負載、供應項目及發行設定檔進行以程式設計方式達成的擷取和操作。 這些 API 會使用角色型存取控制 (RBAC) 來在處理期間強制執行正確的權限。

本參考能提供 Cloud Partner 入口網站 REST API 的技術詳細資料。 本文件中的承載範例僅供參考，並會隨新功能的加入而變更。


<a name="prerequisites-and-considerations"></a>先決條件與考量
-------------------------------

在使用 API 之前，您應該檢閱：

- [先決條件](./cloud-partner-portal-api-prerequisites.md)文章以了解如何將服務主體新增至您的帳戶，並取得 Azure Active Directory (Azure AD) 存取權杖以用於驗證。 
- 兩個[並行存取控制](./cloud-partner-portal-api-concurrency-control.md)。
可供呼叫這些 Api 的策略。
- 其他 API [考量](./cloud-partner-portal-api-considerations.md)，例如版本控制和錯誤處理。


<a name="common-tasks"></a>常見工作
------------
本參考會詳細說明用來處理下列常見工作的 API。


### <a name="offers"></a>供應項目

-   [擷取所有供應項目](./cloud-partner-portal-api-retrieve-offers.md)
-   [擷取特定供應項目](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [擷取供應項目狀態](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [建立供應項目](./cloud-partner-portal-api-creating-offer.md)
-   [發佈供應項目](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>作業

-   [擷取作業](./cloud-partner-portal-api-retrieve-operations.md)
-   [取消作業](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>發佈應用程式

-   [啟用](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>其他工作

-   [設定虛擬機器供應項目定價](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>疑難排解

-   [針對驗證錯誤進行疑難排解](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
