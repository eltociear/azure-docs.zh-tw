---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2020
ms.locfileid: "75893980"
---
建立 Azure Machine Learning 工作區或工作區所使用的資源時，您可能會收到類似下列訊息的錯誤：

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

大部分的資源提供者會自動註冊，但不是全部。 如果您收到此訊息，則需要註冊所述的提供者。

如需註冊資源提供者的詳細資訊，請參閱[解決資源提供者註冊的錯誤](../articles/azure-resource-manager/templates/error-register-resource-provider.md)。