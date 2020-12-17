---
description: 查看或修改作业
title: 查看或修改作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- jobs [SQL Server Agent], viewing
- modifying jobs
- viewing jobs
- SQL Server Agent jobs, viewing
- SQL Server Agent jobs, modifying
- displaying jobs
ms.assetid: 57f649b8-190c-4304-abd7-7ca5297deab7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7c16b835eff545c4623ccf9845a523f2e56bf865
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97422457"
---
# <a name="view-or-modify-jobs"></a>查看或修改作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

您可以查看任何已创建的作业。 运行完一个作业后，还可以查看它的历史记录。 查看作业历史记录使您可以查看作业何时运行、整个作业的状态以及作业中每步作业的状态。 在作业成功完成后，您可以查看该作业过去是否曾失败，还可以查看作业每次运行时创建的输出内容。 **sysadmin** 固定服务器角色的成员可以查看或修改所有人的作业。  
  
> [!NOTE]  
> 作业至少必须运行一次，才会有作业历史记录。 您可以限制作业历史记录日志的总大小以及其中每个作业的大小。  
  
最后，您还可以修改作业以满足新的要求。 可以修改：  
  
-   响应选项  
  
-   计划  
  
-   作业步骤  
  
-   所有权  
  
-   作业类别  
  
-   目标服务器（仅适用于多服务器作业）  
  
为了确保对多服务器作业所做的更改生效，必须将这些更改发布到下载列表中，使目标服务器可以下载更新的作业。 若要确保目标服务器具有最新的作业定义，请在更新完多服务器作业后，按如下所示发布一条 INSERT 指令：  
  
```  
EXECUTE sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
有关详细信息，请参阅 [sp_purge_jobhistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)。  
  
**sysadmin** 固定服务器角色的成员可以查看任何作业的定义或历史记录，还可以修改任何作业。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|说明|主题|  
|-|-|  
|说明如何查看 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|[View a Job](../../ssms/agent/view-a-job.md)|  
|说明如何查看 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业历史记录日志。|[View the Job History](../../ssms/agent/view-the-job-history.md)|  
|说明如何删除 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业历史记录日志的内容。|[清除作业历史记录日志](../../ssms/agent/clear-the-job-history-log.md)|  
|说明如何设置 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业历史记录日志的大小限制。|[调整作业历史记录日志的大小](../../ssms/agent/resize-the-job-history-log.md)|  
|说明如何更改 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的属性。|[修改作业](../../ssms/agent/modify-a-job.md)|  
  
## <a name="see-also"></a>另请参阅  
[sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
