---
description: SQL Server 代理表 (Transact-SQL)
title: " (Transact-sql) SQL Server 代理表 |Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Agent, system tables
- system tables [SQL Server], SQL Server Agent
ms.assetid: 6cb39bfd-079e-4be4-9c42-2fa234c65ce1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3cf53dd13bd94c9632c4f3c2464bda38d50e3742
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096133"
---
# <a name="sql-server-agent-tables-transact-sql"></a>SQL Server 代理表 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节中的主题介绍存储 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理所用信息的系统表。 所有表都位于 msdb 数据库的 dbo 架构中。  
  
## <a name="in-this-section"></a>本节内容  
 [dbo.sysalerts](../../relational-databases/system-tables/dbo-sysalerts-transact-sql.md)  
 每个警报在表中各占一行。  
  
 [dbo.syscategories](../../relational-databases/system-tables/dbo-syscategories-transact-sql.md)  
 包含 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 用于组织作业、警报和操作员的类别。  
  
 [dbo.sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
 包含所有目标服务器的下载指令队列。  
  
 [dbo.sysjobactivity](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
 包含有关当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业活动和状态的信息。  
  
 [dbo.sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
 包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行预定作业的信息。  
  
 [dbo.sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
 存储将由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行的各个预定作业的信息。  
  
 [dbo.sysjobschedules](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
 包含代理要执行的作业的计划信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 [dbo.sysjobservers](../../relational-databases/system-tables/dbo-sysjobservers-transact-sql.md)  
 存储特定作业与一个或多个目标服务器的关联或关系。  
  
 [dbo.sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
 包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理要执行的作业中的各个步骤的信息。  
  
 [dbo.sysjobstepslogs](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
 包含有关作业步骤日志的信息。  
  
 [dbo.sysnotifications](../../relational-databases/system-tables/dbo-sysnotifications-transact-sql.md)  
 对每个通知包含一行。  
  
 [dbo.sysoperators](../../relational-databases/system-tables/dbo-sysoperators-transact-sql.md)  
 对每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理操作员包含一行。  
  
 [dbo.sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
 包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户的信息。  
  
 [dbo.sysproxylogin](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)  
 记录与每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 [dbo.sysproxysubsystem](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 记录每个代理帐户所用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理子系统。  
  
 [dbo.sysschedules](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
 包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划的信息。  
  
 [dbo.syssessions](../../relational-databases/system-tables/dbo-syssessions-transact-sql.md)  
 包含每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理会话的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理开始日期。 每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务时，都要创建一个会话。  
  
 [dbo.syssubsystems](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 包含有关所有可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理子系统的信息。  
  
 [dbo.systargetservergroupmembers](../../relational-databases/system-tables/dbo-systargetservergroupmembers-transact-sql.md)  
 记录此多服务器组中当前登记的目标服务器。  
  
 [dbo.systargetservergroups](../../relational-databases/system-tables/dbo-systargetservergroups-transact-sql.md)  
 记录此多服务器环境中当前登记的目标服务器组。  
  
 [dbo.systargetservers](../../relational-databases/system-tables/dbo-systargetservers-transact-sql.md)  
 记录此多服务器操作域中当前登记的目标服务器。  
  
 [dbo.systaskids](../../relational-databases/system-tables/dbo-systaskids-transact-sql.md)  
 包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本中创建的任务到当前版本中的 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 作业的映射。  
  
  
