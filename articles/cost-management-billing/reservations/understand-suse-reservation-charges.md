---
title: 軟體方案折扣 -Azure | Microsoft Docs
description: 了解軟體方案折扣如何套用至虛擬機器上的軟體。
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2019
ms.author: banders
ms.openlocfilehash: f3d0c123fcbd16f3a4125ad74f0502863b6ce6d1
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2020
ms.locfileid: "75995256"
---
# <a name="azure-software-plan-discount"></a>Azure 軟體方案折扣

SUSE 和 RedHat 的 Azure 軟體方案是已部署的 VM 所適用的保留。 軟體方案折扣適用於符合保留的已部署 VM 對軟體的使用。

當您關閉一個 VM 後，折扣會自動套用至另一個相符的 VM (如果有的話)。 軟體方案涵蓋在 VM 上執行軟體的成本。 其他費用 (例如計算、儲存體和網路) 會個別收費。

若要購買適當的方案，您必須了解您的 VM 使用量，以及這些 VM 上的 vCPU 數目。 請參考下列各節的內容，根據您的使用量資料找出所要購買的方案。

## <a name="how-reservation-discount-is-applied"></a>保留折扣的套用方式

保留折扣採「不用則作廢」  的原則。 因此，如果您有任何一小時沒有相符的資源，就會失去該小時的保留數量。 您無法遞轉未使用的保留時數。

當您關閉資源時，保留折扣會自動套用至指定範圍中另一個相符的資源。 如果在指定的範圍內找不到相符的資源，則會「失去」  保留時數。

## <a name="review-redhat-vm-usage-before-you-buy"></a>購買前請先檢閱 RedHat VM 使用量

請從您的使用量資料取得產品名稱，並購買具有相同類型和大小的 RedHat 方案。

例如，如果您的使用量資料中包含 **Red Hat Enterprise Linux - 1-4 vCPU VM 授權**產品，則您應購買適用於 **1-4 vCPU VM** 的 **Red Hat Enterprise Linux**。

<!--ADD RHEL SCREENSHOT -->

## <a name="review-suse-vm-usage-before-you-buy"></a>購買前請先檢閱 SUSE VM 使用量

請從您的使用量資料取得產品名稱，並購買具有相同類型和大小的 SUSE 方案。

例如，如果您的使用量資料中包含 **SUSE Linux Enterprise Server Priority - 2-4 vCPU VM 支援**產品，則您應購買適用於 **2-4 vCPU** 的 **SUSE Linux Enterprise Server Priority**。

![選取要購買何種產品的範例](./media/understand-suse-reservation-charges/select-suse-linux-enterprise-server-priority-2-4-vcpu.png)

## <a name="discount-applies-to-different-vm-sizes-for-suse-plans"></a>折扣適用於 SUSE 方案的不同 VM 大小

如同保留的 VM 執行個體，購買 SUSE 方案時具有執行個體大小彈性。 這表示您的折扣甚至適用於當您部署具有不同 vCPU 計數的 VM 時。 折扣適用於軟體方案內的各種 VM 大小。

折扣金額取決於下列表格中所列的比例。 此比例會比較該群組中每個計量的相對使用量。 此比例取決於 VM vCPU。 使用比例值來計算有多少 VM 執行個體取得 SUSE Linux 方案折扣。

例如，如果您針對具有 3 或 4 個 vCPU 的 VM，購買適用於 SUSE Linux Enterprise Server for HPC Priority 的方案，則該保留的比例為 2。 此折扣涵蓋以下的 SUSE 軟體成本：

- 2 部已部署的 VM (具有 1 或 2 個 vCPU)、
- 1 部已部署的 VM (具有 3 或 4 個 vCPU)、
- 或 0.77 或大約 77% 的 VM (具有 5 個以上 vCPU)。

5 個以上 vCPU 的比例為 2.6。 因此，對於具有 5 個以上 vCPU 的 VM，其 SUSE 保留只涵蓋軟體成本的一部分 (大約是 77%)。

下表會顯示您可以購買保留的軟體方案、其相關聯的使用量計量，以及每個計量的比例。

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server for HPC Priority

Azure 入口網站市集名稱：

- SLES 12 SP3 for HPC (Priority)

|SUSE VM | 計量識別碼| 比例| VM 大小範例|
| -------| ------------------------| --- |--- |
|SLES for HPC (1-2 個 vCPU)|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SLES for HPC (3-4 個 vCPU)|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SLES for HPC (5 個以上 vCPU)|4edcd5a5-8510-49a8-a9fc-c9721f501913|2.6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>SUSE Linux Enterprise Server for HPC Standard

Azure 入口網站市集名稱：

- SLES 12 SP3 for HPC

|SUSE VM | 計量識別碼 | 比例|VM 大小範例|
| ------- | --- | ------------------------| --- |
|SLES for HPC (1-2 個 vCPU) |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SLES for HPC (3-4 個 vCPU)|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1.92308|D4s_v3|
|SLES for HPC (5 個以上 vCPU) |907a85de-024f-4dd6-969c-347d47a1bdff|2.92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-priority"></a>SUSE Linux Enterprise Server for SAP Priority

Azure 入口網站市集名稱：

- SLES for SAP 15 (Priority)
- SLES for SAP 12 SP3 (Priority)
- SLES for SAP 12 SP2 (Priority)

|SUSE VM | 計量識別碼 | 比例|VM 大小範例|
| ------- |------------------------| --- | --- |
|SLES for SAP Priority (1-2 個 vCPU)|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SLES for SAP Priority (3-4 個 vCPU) |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SLES for SAP Priority (5 個以上 vCPU) |18ae79cd-dfce-48c9-897b-ebd3053c6058|2.41176|D8s_v3|

### <a name="suse-linux-enterprise-server-priority"></a>SUSE Linux Enterprise Server Priority

Azure 入口網站市集名稱：

- SLES 15 (PRIORITY)
- SLES 12 SP3 (Priority)
- SLES 11 SP4 (Priority)

|SUSE VM | 計量識別碼 | 比例|VM 大小範例|
| ------- |------------------------| --- |--- |
|SLES 1 vCPU|462cd632-ec6b-4663-b79f-39715f4e8b38|1|B1ms|
|SLES 2-4 個 vCPU |924bee71-5eb8-424f-83ed-a58823c33908|2|D4s_v3|
|SLES 2-4 個 vCPU |60b3ae9d-e77a-46b2-9cdf-92fa87407969|2|D4s_v3|
|SLES 6 個 vCPU |e8862232-6131-4dbe-bde4-e2ae383afc6f|3||
|SLES 8 個 vCPU |e11331a8-fd32-4e71-b60e-4de2a818c67a|3.2|D8s_v3|
|SLES 12 個核心 vCPU |a5afd00d-d3ef-4bcd-8b42-f158b2799782|3.2||
|SLES 16 個 vCPU |bb21066f-fe46-46d3-8006-b326b1663e52|3.2| D16s_v3|
|SLES 20 個 vCPU |c5228804-1de6-4bd4-a61c-501d9003acc8|3.2| |
|SLES 24 個核心 vCPU |-005d-4075-ac11-822ccde9e8f6|3.2| ND24s|
|SLES 32 個 vCPU |180c1a0a-b0a5-4de3-a032-f92925a4bf90|3.2| D32s_v3|
|SLES 40 個核心 vCPU |a161d3d3-0592-4956-9b64-6829678b6506|3.2||
|SLES 64 個 vCPU |7f5a36ed-d5b5-4732-b6bb-837dbf0fb9d8|3.2| D64s_v3|
|SLES 72 個核心 vCPU |93329a72-24d7-4faa-93d9-203f367ed334|3.2|F72s_v2|
|SLES 96 個核心 vCPU |2018c3a8-ff13-41f8-b64d-9558c5206547|3.2||
|SLES 128 個核心 vCPU |ac27e4d7-44b5-4fee-bc1a-78ac5b4abaf7|3.2| M128ms|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server Standard

Azure 入口網站市集名稱：

- SLES 15
- SLES 15 (Standard)
- SLES 12 SP3 (Standard)

|SUSE VM | 計量識別碼 | 比例|VM 大小範例|
| ------- |------------------------| --- |--- |
|SLES 1-2 個核心 vCPU |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SLES 3-4 個核心 vCPU |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1.92308|D4s_v3|
|SLES 5 個以上 vCPU |7b349b65-d906-42e5-833f-b2af38513468|2.30769| D8s_v3|

## <a name="need-help-contact-us"></a>需要協助嗎？ 與我們連絡

如有問題或需要協助，請[建立支援要求](https://go.microsoft.com/fwlink/?linkid=2083458)。

## <a name="next-steps"></a>後續步驟

若要深入了解保留，請參閱下列文章：

- [什麼是 Azure 保留項目？](save-compute-costs-reservations.md)
- [使用 Azure Reservations 預付 SUSE 軟體方案](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [預付具有 Azure 保留 VM 執行個體的虛擬機器](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [管理 Azure 保留項目](manage-reserved-vm-instance.md)
- [了解隨用隨付訂用帳戶的保留使用量](understand-reserved-instance-usage.md)
- [了解 Enterprise 註冊的保留項目使用量](understand-reserved-instance-usage-ea.md)
