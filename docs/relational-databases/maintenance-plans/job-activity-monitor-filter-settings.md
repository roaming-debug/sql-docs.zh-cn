---
description: 作业活动监视器（筛选设置）
title: 作业活动监视器（筛选设置）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.jobactivitymon.filter.f1
ms.assetid: 89cb0055-5262-447f-8464-7203d4caba78
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f7533366a28517fcc2fdb8ca5c2e4ad0c0dd01c5
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88424049"
---
# <a name="job-activity-monitor-filter-settings"></a>作业活动监视器（筛选设置）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此页可以减少作业活动监视器中可见的行数。 在一个或多个可用框中输入条件，以便仅显示符合指定值的行。 其中某些框（如“状态”或“阻塞类型”）通过下拉列表提供有限数量的可能值。 其他框（如 **“应用程序”** ）则允许以逗号分隔的列表形式输入不限数量的任意值。 使用工具栏图标，可以按类别或按字母顺序对可用的框进行排序。 单击各个条件可以显示该条件的简短说明。  
  
 若要筛选作业活动监视器，请提供所需数量的筛选条件，单击 **“应用筛选器”**，再单击 **“确定”**。  
  
## <a name="all-jobs"></a>所有作业  
 在筛选作业活动监视器时，可以使用这组筛选条件。  
  
 **名称**  
 按名称筛选作业。  
  
 **下次运行时间**  
 按下次计划运行的日期筛选。  
  
 **可运行**  
 查看可以运行的作业或不能运行的作业。 选择 **“是”** 将只查看可以运行的作业，选择 **“否”** 将只查看不能运行的作业，或选择 **“全部”** 将同时查看可以运行和不能运行的作业。  
  
 **上次运行时间**  
 按上次运行的日期筛选。  
  
 **上次运行结果**  
 按作业上次运行的状态筛选作业。  
  
 **已启用**  
 仅查看启用或未启用的作业。  
  
 **类别**  
 按作业类别筛选作业。  
  
 **计划**  
 查看所有计划或未计划的作业。  
  
 **Status**  
 按状态筛选作业。  
  
## <a name="description-area"></a>说明区  
 **说明框**  
 选定条件后，此未命名框将提供对所选条件的简短说明。  
  
 **“应用筛选器”**  
 若要应用筛选器，请单击“应用筛选器”，再单击“确定” 。 若要保留“筛选器设置”对话框中的筛选器设置，但不予以应用，请取消选中“应用筛选器”，再单击“确定”，以显示所有行  。  
  
 **Clear**  
 将筛选器设置恢复为默认设置。  
  
## <a name="see-also"></a>另请参阅  
 [监视作业活动](../../ssms/agent/monitor-job-activity.md)  
  
  
