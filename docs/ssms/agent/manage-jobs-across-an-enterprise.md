---
description: 管理整个企业内的作业
title: 管理整个企业内的作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- multiserver job management [SQL Server]
- jobs [SQL Server Agent], modifying
- modifying jobs
- SQL Server Agent jobs, modifying
- target servers [SQL Server], job changes
ms.assetid: 4fe7f6c6-f89b-4430-979c-4994a5dcf7a6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 62e4eb1574d48ecd40af46ba081f6443bdafb000
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482222"
---
# <a name="manage-jobs-across-an-enterprise"></a>管理整个企业内的作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

如果在 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 以外对多服务器作业定义进行了更改，则必须将更改发布到下载列表中，以便目标服务器可以再次下载更新后的作业。 为了确保目标服务器具有当前的作业定义，在更新多服务器作业后，请发布一条 INSERT 指令，如下所示：  
  
```  
EXECUTE sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
若要通知目标服务器多服务器作业已被修改，必须在使用以下任一过程之后调用前一个命令：  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_update_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)  
  
-   [sp_delete_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)  
  
-   [管理事件](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_detach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
    > [!NOTE]  
    > 调用 **sp_update_job** 或 **sp_delete_job** 后不需要调用 **sp_post_msx_operation**，因为这些存储过程会自动向下载列表发布所需的更改。  
  
以下是管理整个企业内的作业要执行的常规任务：  
  
**检查目标服务器的状态**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)  
  
-   [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)  
  
**更改作业的目标服务器**  
  
-   [SQL Server Management Studio](../../ssms/agent/modify-the-target-servers-for-a-job.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
-   [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)  
  
**更改目标服务器的位置**  
  
-   [SQL Server Management Studio](../../ssms/agent/specify-a-target-server-s-location-sql-server-management-studio.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md)  
  
-   [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)  
  
**同步目标服务器的时钟**  
  
-   [SQL Server Management Studio](../../ssms/agent/synchronize-target-server-clocks-sql-server-management-studio.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)  
  
**强制目标服务器轮询主服务器**  
  
-   [SQL Server Management Studio](../../ssms/agent/force-a-target-server-to-poll-the-master-server.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
[管理事件](../../ssms/agent/manage-events.md)  
