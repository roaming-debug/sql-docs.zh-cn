---
title: 将组策略用于可用性组运行状况
description: 了解如何查看 AlwaysOn 仪表板用来提供可用性组运行状况信息的组系统策略。
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 26bf8f71-c2b8-45ef-b3a3-372b96c9e6e3
author: rothja
ms.author: jroth
ms.openlocfilehash: f2f6357e434e4a8d4d6385de10cac58c7bd6f2d0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641421"
---
# <a name="evaluate-health-of-the-always-on-availability-group-using-group-policies"></a>使用组策略评估 AlwaysOn 可用性组的运行状况
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  AlwaysOn 仪表板使用 AlwaysOn 可用性组系统策略向用户提供有关可用性组运行状况的信息。 它们对可用性组运行问题的初始故障排除非常有用。 可扩展这些策略并将其用于自定义 Always On 仪表板，也可立即运行这些策略以报告所需运行状况信息。  
  
 有 14 条针对可用性组的系统策略。 有关每条策略的详细信息，请参阅[针对 Always On 可用性组运行问题的 Always On 策略 (SQL Server)](always-on-policies-for-operational-issues-always-on-availability.md)。  
  
## <a name="view-or-evaluate-availability-groups-system-policies"></a>查看或评估可用性组系统策略  
 要在 SQL Server Management Studio (SSMS) 中查看或运行可用性组系统策略，请执行以下操作：  
  
1.  在对象资源管理器中，依次展开“管理”、“策略管理”、“策略”和“系统策略”      。  
  
2.  右键单击其中一条策略并单击“评估”  。 如果要评估所选策略，则已完成。 可单击“目标详细信息”框中的“查看”，查看评估结果的详细信息   。  
  
3.  要查看所有可用性组系统策略，请单击“选择页”窗格中的“策略选择”   。  
  
## <a name="next-steps"></a>后续步骤  
 [Always On 运行状况模型，第 2 部分：扩展运行状况模型](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)。   
  
