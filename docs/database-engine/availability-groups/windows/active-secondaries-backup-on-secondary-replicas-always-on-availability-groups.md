---
title: 卸载次要可用性组副本的备份
description: 了解卸载 Always On 可用性组次要副本时所支持的不同备份类型。
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 82afe51b-71d1-4d5b-b20a-b57afc002405
author: cawrites
ms.author: chadam
ms.openlocfilehash: 79972c5670370c28404b5cc93d7c3b833551f988
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644303"
---
# <a name="offload-supported-backups-to-secondary-replicas-of-an-availability-group"></a>卸载可用性组次要副本的支持备份
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 活动辅助功能包括支持在次要副本上进行备份。 备份操作可能会给 I/O 和 CPU 带来很大的压力（使用备份压缩）。 将备份负荷转移到已同步或正在同步的辅助副本后，您可以使用承载第一层工作负荷的主副本的服务器实例上的资源。  

> [!NOTE]  
>  在可用性组的主数据库或辅助数据库上不允许 RESTORE 语句。  
  
 
##  <a name="backup-types-supported-on-secondary-replicas"></a><a name="SupportedBuTypes"></a> 辅助副本上支持的备份类型  
  
-   在次要副本上执行 BACKUP DATABASE 时，仅支持数据库、文件或文件组的仅复制完整备份  。 仅复制备份不会影响日志链，也不会清除差异位图。  
  
-   不支持在次要副本上执行差异备份。

-   当前不支持并发备份，例如，在辅助副本上执行完整数据库备份的同时在主副本上执行事务日志备份。 
  
-   **BACKUP LOG** 仅支持常规日志备份（次要副本上的日志备份不支持 COPY_ONLY 选项）。  
  
     对于在任何副本（主副本或辅助副本）上进行的日志备份之间，确保一致的日志链，而与其可用性模式（同步提交或异步提交无关）。  
  
-   若要备份辅助数据库，辅助副本必须能够与主副本进行通信，并且状态必须为 **SYNCHRONIZED** 或 **SYNCHRONIZING**。  

在分布式可用性组中，可以对与活动主要副本相同的可用性组中的次要副本执行备份，或对任何次要可用性组的主要副本执行备份。 无法对次要可用性组中的次要副本执行备份，因为次要副本仅与其可用性组中的主要副本通信。 仅直接与全局主要副本通信的副本才能执行备份操作。

##  <a name="configuring-where-backup-jobs-run"></a><a name="WhereBuJobsRun"></a> 配置运行备份作业的位置  
 在辅助副本上执行备份以减轻主生产服务器的备份工作负荷非常有好处。 但是，对辅助副本执行备份会显著增加用于确定应在何处运行备份作业的过程的复杂性。 要解决这个问题，请按如下所示配置备份作业运行的位置：  
  
1.  配置可用性组以便指定要对其执行备份的可用性副本。 有关详细信息，请参阅 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md) 或 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md) 中的 AUTOMATED_BACKUP_PREFERENCE 和 BACKUP_PRIORITY 参数 。  
  
2.  为承载作为执行备份候选的可用性副本的每个服务器实例上的每个可用性数据库都创建编写了脚本的备份作业。 有关详细信息，请参阅“跟进：配置次要副本备份之后”部分，见[配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **配置辅助副本备份**  
  
-   [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 **确定当前副本是否为首选备份副本**  
  
-   [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **创建备份日志**  
  
-   [使用维护计划向导](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
-   [执行作业](../../../ssms/agent/implement-jobs.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [仅复制备份 (SQL Server)](../../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
