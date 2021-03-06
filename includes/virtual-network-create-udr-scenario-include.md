---
title: 包含檔案
description: 包含檔案
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 40b81904daabfdad7e45571d8ab86cf32cac8964
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2019
ms.locfileid: "67173908"
---
## <a name="scenario"></a>案例
為了更清楚說明如何建立 UDR，此文件會使用下列案例：

![影像說明](./media/virtual-network-create-udr-scenario-include/figure1.png)

在此案例中，您將針對「前端子網路」  建立一個 UDR，並針對「後端子網路」  建立另一個 UDR ，如下所述： 

* **UDR-FrontEnd**。 前端 UDR 會套用到「前端」  子網路且包含一個路由：    
  * **RouteToBackend**。 這個路由會將流向後端子網路的所有流量傳送到 **FW1** 虛擬機器。
* **UDR-BackEnd**。 後端 UDR 會套用到「後端」  子網路，且包含一個路由：    
  * **RouteToFrontend**。 這個路由會將流向前端子網路的所有流量傳送到 **FW1** 虛擬機器。

這些路由的組合可確保從某一個子網路流向另一個子網路的所有流量將會路由至 **FW1** 虛擬機器，此虛擬機器是用來作為虛擬設備。 您也需要針對該 **FW1** VM 開啟 IP 轉送，以確保它可以接收目的地為其他 VM 的流量。

