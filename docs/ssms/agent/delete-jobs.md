---
description: 删除作业
title: 删除作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- delete jobs
ms.assetid: bffb915e-bc84-4417-aa35-183243ca3312
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: c494163b12b3b5a0b374ac5143b5da3d45ad5d08
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97423624"
---
# <a name="delete-jobs"></a>删除作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

作业是一系列由 SQL Server 代理按顺序执行的指定操作。 默认情况下，执行完成时不删除作业。 无论该作业是否成功，你都可以删除一个或多个 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。 还可以配置 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理以在作业成功、失败或完成时自动将其删除。  
  
默认情况下， **sysadmin** 固定服务器角色的成员可以执行 [sp_delete_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md) 系统存储过程或删除某一作业。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
**sysadmin** 固定服务器角色成员可以通过执行 **sp_delete_job** 删除任何作业。 非 **sysadmin** 固定服务器角色成员的用户只能删除该用户所拥有的作业。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|说明|主题|  
|-|-|   
|介绍如何删除一个或多个 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|[删除一个或多个作业](../../ssms/agent/delete-one-or-more-jobs.md)|  
|介绍如何配置 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，以在作业成功、失败或完成时自动将其删除。|[自动删除作业](../../ssms/agent/automatically-delete-a-job.md)|  
