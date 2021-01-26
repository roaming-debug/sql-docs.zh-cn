---
title: 配置“并行的开销阈值”服务器配置选项 | Microsoft Docs
description: 了解并行的开销阈值选项。 了解其值如何影响 SQL Server 是否运行查询的并行计划，以及如何设置它。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- cost threshold for parallelism option
ms.assetid: dad21bee-fe28-41f6-9d2f-e6ababfaf9db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ea4681fd02065129c4b565d0e9dbb744a321f9ae
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783295"
---
# <a name="configure-the-cost-threshold-for-parallelism-server-configuration-option"></a>配置并行的开销阈值服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明了如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] “并行的开销阈值” [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **cost threshold for parallelism** 选项指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建和运行并行查询计划的阈值。 仅当运行同一查询的串行计划的估计开销高于在“并行的开销阈值”中设置的值时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 才创建和运行该查询的并行计划。 成本指的是在特定硬件配置中运行串行计划估计需要花费的成本，而不是时间单位。 **“并行的开销阈值”** 选项可设置为 0 到 32767 之间的任何值。 默认值为 5。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要配置并行的开销阈值选项，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置并行的开销阈值选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   成本是指成本的抽象单位，而不是预估时间的单位。 只能为对称多处理器设置 **cost threshold for parallelism** 。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将忽略 **或** 的值：  
  
    -   计算机只有一个逻辑处理器。  
  
    -   由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 相关性掩码 **配置选项的原因，只有一个逻辑处理器可供** 使用。  
  
    -   **“最大并行度”** 选项设置为 1。  
  
逻辑处理器是处理器硬件的基本单元，可让操作系统调度任务或执行线程上下文。 每个逻辑处理器一次只执行一个线程上下文。 处理器内核是提供解码和执行指令的能力的电路。 一个处理器内核可能包含一个或多个逻辑处理器。 以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询可用于获取系统的 CPU 信息。  
  
```sql  
SELECT (cpu_count / hyperthread_ratio) AS PhysicalCPUs,   
cpu_count AS logicalCPUs   
FROM sys.dm_os_sys_info  
```  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   在某些情况下，即使查询的开销计划小于当前 **“并行的开销阈值”** 的值，也有可能选择并行计划。 出现这种情况，是因为使用并行还是串行计划是根据先前优化过程所提供的开销估计确定的。 有关详细信息，请参阅[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing)。  

-   虽然对于大多数系统而言，默认值 5 已足够，但可能需要不同的值。 如果需要，可以使用高值和低值执行应用程序测试，以优化应用程序性能。
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>配置并行的开销阈值选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“高级”** 节点。  
  
3.  在“并行”下，将“并行的开销阈值”选项更改为所需值。 键入或选择一个值（介于 0 到 32767 之间）。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>配置并行的开销阈值选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `cost threshold for parallelism` 选项的值设置为 `10`。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'cost threshold for parallelism', 10 ;  
GO  
RECONFIGURE  
GO  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-cost-threshold-for-parallelism-option"></a><a name="FollowUp"></a> 跟进：在配置并行的开销阈值选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)   
 [查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)   
 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [affinity mask 服务器配置选项](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
