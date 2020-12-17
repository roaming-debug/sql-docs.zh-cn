---
description: 管理计划
title: 管理计划
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.manageschedules.f1
ms.assetid: f56c0736-dccc-41d2-afcf-71344aff143a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8fe612c119493fb87229819a001c7058389a0e62
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482248"
---
# <a name="manage-schedules"></a>管理计划
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

允许查看和更改 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划的属性。  
  
## <a name="options"></a>选项  
**可用计划**  
列出可用于此用户的计划。 请注意，作业和计划的所有者必须相同。 因此，此列表仅包括该作业所有者所拥有的计划。  
  
**Name**  
显示计划的名称。  
  
**Enabled**  
选择此选项可以启用该计划。  
  
**说明**  
描述计划运行作业的条件。  
  
**计划中的作业**  
列出附加到计划的作业编号。 单击编号可以查看相应作业的属性。  
  
**新建**  
单击此按钮可创建新计划。  
  
**删除**  
单击此按钮可删除所选计划。  
  
**属性**  
单击此按钮可更改所选计划的属性。  
  
## <a name="see-also"></a>另请参阅  
[创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
