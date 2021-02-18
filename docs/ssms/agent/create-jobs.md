---
description: 创建作业
title: 创建作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: 465fb7fc-7622-4252-a178-ea51691c935b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4a79b08a37c6fb21f8a1e6c1b3f16466c15d135b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342722"
---
# <a name="create-jobs"></a>创建作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

作业是一系列由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理按顺序执行的指定操作。 一个作业可以执行各种类型的活动，包括运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本、命令提示符应用程序、Microsoft ActiveX 脚本、Integration Services 包、Analysis Services 命令和查询或复制任务。 作业可以运行重复或可计划的任务，然后它们可以通过生成警报来自动通知用户作业状态，从而极大地简化了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理。  
  
若要创建作业，用户必须是某个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色或 **sysadmin** 固定服务器角色的成员。 作业只能由其所有者或 **sysadmin** 角色的成员进行编辑。 **sysadmin** 角色的成员可以将作业所有权分配给其他用户，并且他们可以运行所有人的作业。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的固定数据库角色的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
可以编写作业，使其运行在企业内的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地实例或多个实例上。 必须设置至少一台主服务器以及一台或多台目标服务器，才能在多台服务器上运行作业。 有关主服务器和目标服务器的详细信息，请参阅 [企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在作业历史记录中记录作业和作业步骤信息。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|说明|主题|  
|-|-|  
|介绍如何创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|[创建作业](../../ssms/agent/create-a-job.md)|  
|介绍如何将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的所有权重新指派给另一用户。|[将作业所有权授予其他人](../../ssms/agent/give-others-ownership-of-a-job.md)|  
|介绍如何设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业历史记录日志。|[Set Up the Job History Log](../../ssms/agent/set-up-the-job-history-log.md)|  
  
## <a name="see-also"></a>另请参阅  
[管理作业步骤](../../ssms/agent/manage-job-steps.md)  
[企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[运行作业](../../ssms/agent/run-jobs.md)  
[查看或修改作业](../../ssms/agent/view-or-modify-jobs.md)  
