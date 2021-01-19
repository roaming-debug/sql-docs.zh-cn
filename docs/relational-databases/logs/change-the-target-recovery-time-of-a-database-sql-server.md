---
title: 更改数据库的目标恢复时间
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中设置或更改 SQL Server 数据库的目标恢复时间。
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 00c7778583545c56b886bfc30c4763bacd1404b3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171559"
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>更改数据库的目标恢复时间 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中设置或更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的目标恢复时间。 默认情况下，目标恢复时间是 60 秒，而且数据库使用间接检查点。 目标恢复时间为此数据库建立恢复时间上限。  
  
> [!NOTE]  
>  如果长时间运行的事务导致过多 UNDO 时间，则可能超过给定数据库的目标恢复时间设置为该数据库指定的上限。  
  
-   **开始之前：** [限制和局限](#Restrictions)、[安全性](#Security)  
  
-   **要更改目标恢复时间，请使用：** [SQL Server Management Studio](#SSMSProcedure) 或 [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限 
  
> [!CAUTION]  
>  为间接检查点配置的数据库上的联机事务工作负荷可能导致性能下降。 间接检查点确保损坏页的数量低于特定阙值，以便在目标恢复时间内完成数据库恢复。 与利用脏页数量的间接检查点相反，“恢复间隔”配置选项使用事务数量来确定恢复时间。 如果在接收大量 DML 操作的数据库上启用了间接检查点，则后台编写器可开始主动将脏缓冲区刷新到磁盘上，确保执行恢复所需的时间位于数据库上设置的目标恢复时间范围内。 这可能造成某些系统上出现额外的 I/O 活动，如果磁盘子系统在 I/O 阙值之上或附近运行，则这会导致性能瓶颈。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **更改目标恢复时间**  
  
1.  在 **“对象资源管理器”** 中，连接到某个 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例，再展开该实例。  
  
2.  展开“数据库”容器，右键单击要更改的数据库，然后单击“属性”命令 。  
  
3.  在 **“数据库属性”** 对话框中，单击 **“选项”** 页。  
  
4.  在“恢复”面板的“目标恢复时间(秒)”字段中，指定要作为此数据库恢复时间上限的秒数 。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **更改目标恢复时间**  
  
1.  连接到数据库所在的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
2.  按如下所示使用以下 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) 语句：  
  
     TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     从 [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]开始，默认值是 1 分钟。 大于 0（较旧版本的默认值）时，指定在发生崩溃的情况下指定数据库的恢复时间上限。  
  
     SECONDS  
     指示 *target_recovery_time* 表示为秒数。  
  
     MINUTES  
     指示 *target_recovery_time* 表示为分钟数。  
  
     以下示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的目标恢复时间设置为 `60` 秒。  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
