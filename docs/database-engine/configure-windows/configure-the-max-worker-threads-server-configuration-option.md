---
title: 配置“最大工作线程数”服务器配置选项 | Microsoft Docs
description: 了解如何使用“最大工作线程数”选项配置 SQL Server 可用于处理某些请求的工作线程数。
ms.custom: contperf-fy20q4
ms.date: 04/14/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5291b502ec70be8b86c6806e14404664d0b0831
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347000"
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>配置 max worker threads 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何使用  或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中配置 max worker threads [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 “最大工作线程数”选项配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 范围内可用于处理查询请求、登录、注销和类似应用程序请求的工作线程数。


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用操作系统的本机线程服务来确保满足以下条件：

- 一个或多个线程同时支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的每个网络。

- 一个线程处理数据库检查点。

- 一个线程池处理所有用户。

**max worker threads** 的默认值为 0。 这使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在启动时自动配置工作线程数。 默认设置对于大多数系统为最佳设置。 不过，根据您的系统配置，有时将 **max worker threads** 设置为特定值会提高性能。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果实际的查询请求数超过了“最大工作线程数”中设置的值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 就会将工作线程集中到池中，这样下一个可用的工作线程就可以处理请求了。 仅将工作线程分配给活动请求，并在处理请求后释放该工作线程。 即使发出请求的用户会话/连接保持打开状态，也会发生这种情况。 

-   “最大工作线程数”服务器配置选项不限制引擎中可能生成的所有线程。 LazyWriter、Checkpoint、日志编写器、Service Broker、锁管理器或其他任务所需的系统线程在此限制之外生成。 可用性组使用“最大工作线程数限制”范围内的一些工作线程，但也使用系统线程（请参阅[可用性组的线程使用情况](../availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md#ThreadUsage)）。如果超过了配置的线程数，下列查询提供有关已生成附加线程的系统任务的信息。  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。 如果怀疑存在性能问题，可能不是由于工作线程不可用。 原因很可能与占用工作线程且未释放它们的活动有关。 例如长时间运行的查询或导致长时间等待的查询的系统瓶颈（I/O、阻塞、闩锁等待、网络等待）。 在更改最大工作线程设置之前，最好找到导致性能问题的根本原因。 有关评估性能的详细信息，请参阅[监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)。
  
-   当服务器上连接有大量客户端时，线程池有助于优化性能。 一般情况下，会为每个查询请求创建一个单独的操作系统线程。 但是，当到服务器的连接达到数以百计时，为每个查询请求使用一个线程会占用大量的系统资源。 **max worker threads** 选项使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以为更大数量的查询请求创建一个工作线程池，这将提高性能。  
  
-   下表显示了根据 CPU、计算机体系结构和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的各种组合自动配置的最大工作线程数（当值设置为 0 时），计算公式如下：默认最大工作器数 + ((逻辑 CPU 数* - 4) * 每 CPU 工作器数)** 。  
  
    |CPU 数|32 位计算机（不高于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]）|64 位计算机（不高于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1）|64 位计算机（自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起）|   
    |------------|------------|------------|------------|  
    |\<= 4|256|512|512|   
    |8|288|576|576|   
    |16|352|704|704|   
    |32|480|960|960|   
    |64|736|1472|1472|   
    |128|1248|2496|4480|   
    |256|2272|4544|8576|   
    
    在不高于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 的版本中，“每 CPU 工作器数”只取决于体系结构（32 位还是 64 位）：
    
    |CPU 数|32 位计算机<sup>1</sup>|64 位计算机|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4|256 +（（逻辑 CPU 位数 - 4）* 8）|512<sup>2</sup> + ((逻辑 CPU 数 - 4) * 16)|   
    
    自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，“每 CPU 工作器数”取决于体系结构和处理器数（介于 4 和 64 之间还是大于 64）：
    
    |CPU 数|32 位计算机<sup>1</sup>|64 位计算机|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4 和 \<= 64|256 +（（逻辑 CPU 位数 - 4）* 8）|512<sup>2</sup> + ((逻辑 CPU 数 - 4) * 16)|   
    |\> 64|256 + ((逻辑 CPU 位数 - 4) * 32)|512<sup>2</sup> + ((逻辑 CPU 数 - 4) * 32)|   
  
    <sup>1</sup>自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 起，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不能再安装在 32 位操作系统上。 为了帮助运行 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更低版本的客户，我们列出了 32 位计算机值。 建议对 32 位计算机上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用最大工作线程数 1,024。
    
    <sup>2</sup>自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，对于内存小于 2GB 的计算机，“默认最大工作器数”值除以 2。
  
    > [!TIP]  
    > 有关使用 64 个以上的 CPU 的建议，请参考 [在具有超过 64 个 CPU 的计算机上运行 SQL Server 的最佳做法](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus)。  
  
-   如果所有工作线程因为长时间运行的查询而处于活动状态， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能停止响应，直到一个工作线程完成并变成可用。 虽然这不是缺点，但有时用户可能并不希望如此。 如果进程显示为停止响应并且不再处理新查询，则将使用专用管理员连接 (DAC) 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并关闭此进程。 为避免此种情况发生，请增大最大工作线程数。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要使用两个参数执行 sp_configure 来更改配置选项或运行 `RECONFIGURE` 语句，用户必须拥有 `ALTER SETTINGS` 服务器级别权限。 `ALTER SETTINGS` 权限由 sysadmin 和 serveradmin 固定服务器角色隐式拥有。  
  
##  <a name="using-ssmanstudiofull"></a><a name="SSMSProcedure"></a> 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>配置 max worker threads 选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“处理器”** 节点。  
  
3.  在“最大工作线程数”框中，键入或选择一个介于 128 到 32767 之间的值。  
  
> [!TIP]
> 使用 **max worker threads** 选项配置可用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的工作线程数。 **max worker threads** 的默认设置适用于大多数系统。 不过，根据您的系统配置，有时将 **max worker threads** 设置为较小的值会提高性能。
> 有关详细信息，请参阅本页的[建议](#Recommendations)。
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>配置 max worker threads 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `max worker threads` 选项的值配置为 `900`。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="follow-up-after-you-configure-the-max-worker-threads-option"></a><a name="FollowUp"></a> 跟进：在配置“最大工作线程数”选项后  
 执行 [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) 后，此更改将立即生效，而无需重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
## <a name="see-also"></a>另请参阅  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [用于数据库管理员的诊断连接](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)
