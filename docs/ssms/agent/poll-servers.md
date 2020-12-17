---
description: 轮询服务器
title: 轮询服务器
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- target servers [SQL Server], polling interval
- polling master servers [SQL Server]
- server polling [SQL Server]
- master servers [SQL Server], polling
- polling interval [SQL Server]
ms.assetid: 96f5fd43-3edd-4418-9dd0-4d34e618890e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: f28cb7829046b27ad0579ce546373834d6199d02
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477008"
---
# <a name="poll-servers"></a>轮询服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例 Instance/docs.microsoft.com/azure/sql-database/sql-database-managed-instance) 目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

实现多服务器管理后，目标服务器将定期联系主服务器以上载有关已执行的作业的信息，并下载新的作业。 联系主服务器的过程称为服务器轮询，该过程按定期的轮询间隔发生。  
  
## <a name="polling-intervals"></a>轮询间隔  
轮询间隔（默认情况下为一分钟）控制目标服务器连接到主服务器以下载指令并上载作业执行结果的频率。  
  
当目标服务器轮询主服务器时，它从 **msdb** 数据库的 **sysdownloadlist** 表中读取分配给目标服务器的操作。 这些操作控制多服务器作业和目标服务器行为的不同方面。 操作的示例包括删除作业、插入作业、启动作业和更新目标服务器的轮询间隔。  
  
将操作发布到 **sysdownloadlist** 表中有下面两种方式：  
  
-   使用 **sp_post_msx_operation** 存储过程显式发布。  
  
-   使用其他作业存储过程隐式发布。  
  
如果使用作业存储过程修改多服务器作业计划或作业步骤，或者使用 SQL 分布式管理对象 (SQL-DMO) 控制多服务器作业，请在修改多服务器作业步骤或计划后发出下列命令：  
  
```  
EXECUTE msdb.dbo.sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
发出此命令将保持目标服务器与当前作业定义同步。  
  
如果使用以下项，则无需显式发布操作：  
  
-   Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 控制多服务器作业。  
  
-   不修改作业计划或作业步骤的作业存储过程。  
  
**强制目标服务器轮询主服务器**  
  
-   [SQL Server Management Studio](../../ssms/agent/force-a-target-server-to-poll-the-master-server.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
[管理事件](../../ssms/agent/manage-events.md)  
