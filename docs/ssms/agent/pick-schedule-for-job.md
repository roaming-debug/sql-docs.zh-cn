---
description: 为作业选取计划
title: 为作业选取计划
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.pickscheduleforjob.f1
helpviewer_keywords:
- Pick Schedule for Job dialog box
ms.assetid: 6de2025d-c25c-47b9-9a25-18c294935c15
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 860d949cf17c41daee42a3261785e5b067b32ce0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474418"
---
# <a name="pick-schedule-for-job"></a>为作业选取计划
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 之间的差异](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此对话框，可以为 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业选择现有计划。  
  
## <a name="options"></a>选项  
**可用计划**  
列出此作业的可用计划。 因为作业和计划必须具有相同的所有者，所以，此列表只包括该作业的所有者所拥有的计划。  
  
**Name**  
显示计划的名称。  
  
**Enabled**  
如果计划处于启用状态，则选中该选项。  
  
**说明**  
描述计划运行作业的条件。  
  
**计划中的作业**  
列出附加到计划的作业编号。 单击编号可以查看相应作业的属性。  
  
**属性**  
启动“作业计划属性”对话框，可以在该对话框中查看该计划的有关信息。  
  
## <a name="see-also"></a>另请参阅  
[创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
