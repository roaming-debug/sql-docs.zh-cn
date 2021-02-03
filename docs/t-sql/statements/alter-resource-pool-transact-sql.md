---
description: ALTER RESOURCE POOL (Transact-SQL)
title: ALTER RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_RESOURCE_POOL_TSQL
- ALTER RESOURCE POOL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE POOL
ms.assetid: 9c1c4cfb-0e3b-4f01-bf57-3fce94c7d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0f33cb191bee4b8856983f946a768c5abdd38726
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204051"
---
# <a name="alter-resource-pool-transact-sql"></a>ALTER RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中现有资源调控器的资源池配置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER RESOURCE POOL { pool_name | "default" }  
[WITH  
    ( [ MIN_CPU_PERCENT = value ]  
    [ [ , ] MAX_CPU_PERCENT = value ]   
    [ [ , ] CAP_CPU_PERCENT = value ]   
    [ [ , ] AFFINITY {
                        SCHEDULER = AUTO 
                      | ( <scheduler_range_spec> ) 
                      | NUMANODE = ( <NUMA_node_range_spec> )
                      }]   
    [ [ , ] MIN_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]   
    [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
    [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
)]   
[;]  
  
<scheduler_range_spec> ::=  
{SCHED_ID | SCHED_ID TO SCHED_ID}[,...n]  
  
<NUMA_node_range_spec> ::=  
{NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID}[,...n]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 { pool_name | "default" }  
 是现有用户定义资源池或安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时创建的默认资源池的名称。  
  
 与 ALTER RESOURCE POOL 一起使用时，"default" 必须用引号 ("") 引起来或用方括号 ([]) 括起来，以免与系统保留字 DEFAULT 冲突。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
> [!NOTE]  
>  预定义工作负荷组和资源池都使用小写名称，例如“default”。 对于使用区分大小写排序规则的服务器，应当注意这一点。 使用不区分大小写排序规则的服务器（例如 SQL_Latin1_General_CP1_CI_AS）会将“default”和“Default”视为相同。  
  
 MIN_CPU_PERCENT = value  
 指定在出现 CPU 争用时资源池中的所有请求保证能接收的平均 CPU 带宽。 value 为整数且默认设置为 0。 value 的允许范围是 0 到 100。  
  
 MAX_CPU_PERCENT =value  
 指定在存在 CPU 争用时资源池中的所有请求将接收的最大平均 CPU 带宽。 value 为整数且默认设置为 100。 value 的允许范围是 1 到 100。  
  
 CAP_CPU_PERCENT =value  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 为资源池中的请求指定目标最大 CPU 容量。 value 为整数且默认设置为 100。 value 的允许范围是 1 到 100。  
  
> [!NOTE]  
>  由于 CPU 治理的统计特性，你可能会注意到偶尔会出现超过 CAP_CPU_PERCENT 中指定值的峰值。  
  
 AFFINITY {SCHEDULER = AUTO | (Scheduler_range_spec) | NUMANODE = (NUMA_node_range_spec)}  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 将资源池附加到特定的计划程序。 默认值为 AUTO。  
  
 AFFINITY SCHEDULER = (Scheduler_range_spec) 将资源池映射到由给定 ID 标识的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计划。 这些 ID 映射到 [sys.dm_os_schedulers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md) 的 scheduler_id 列中的值。  
  
 当您使用 AFFINITY NAMANODE = (NUMA_node_range_spec) 时，资源池关联到映射到物理 CPU 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计划程序，而这些 CPU 与给定的一个 NUMA 节点或一系列节点相对应。 您可以使用下面的 Transact-SQL 查询发现物理 NUMA 配置与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计划程序 ID 之间的映射。  
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc 
   ON osn.node_id = sc.parent_node_id 
      AND sc.scheduler_id < 1048576;  
```  
  
 MIN_MEMORY_PERCENT =value  
 指定为此资源池保留的、不能与其他资源池共享的最小内存量。 value 为整数且默认设置为 0。 value 的允许范围是 0 到 100。  
  
 MAX_MEMORY_PERCENT =value  
 指定此资源池中的请求可使用的总服务器内存量。 value 为整数且默认设置为 100。 value 的允许范围是 1 到 100。  
  
 MIN_IOPS_PER_VOLUME =value  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
 指定为资源池保留的每个磁盘卷每秒的最小 I/O 操作数 (IOPS)。 value 的允许范围是 0 到 2^31-1 (2,147,483,647)。 指定 0 表示池没有最小值阈值。  
  
 MAX_IOPS_PER_VOLUME =value  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
 指定可用于该资源池的每个磁盘卷每秒的最大 I/O 操作数 (IOPS)。 value 的允许范围是 0 到 2^31-1 (2,147,483,647)。 指定 0 表示为池设置无限制的阈值。 默认值为 0。  
  
 如果池的 MAX_IOPS_PER_VOLUME 设置为 0，则该池根本不受管控，可以采用系统中的所有 IOPS，即使其他池设置了 MIN_IOPS_PER_VOLUME 也是如此。 对于这种情况，我们建议您在希望管控此池的 IO 时将此池的 MAX_IOPS_PER_VOLUME 值设置为较高的数字（例如，最大值 2^31-1）。  
  
## <a name="remarks"></a>注解  
 MAX_CPU_PERCENT 和 MAX_MEMORY_PERCENT 必须分别大于或等于 MIN_CPU_PERCENT 和 MIN_MEMORY_PERCENT。  
  
 如果可以，MAX_CPU_PERCENT 可使用高于 MAX_CPU_PERCENT 值的 CPU 容量。 虽然可能有高于 CAP_CPU_PERCENT 的周期峰值，但即使有额外的 CPU 容量可用，工作负载也不应延时超过 CAP_CPU_PERCENT。  
  
 每个关联组件（计划程序或 NUMA 节点）的总 CPU 百分比不应超过 100%。  
  
 建议您在熟悉资源调控器状态之后再执行 DDL 语句。 有关详细信息，请参阅 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)。  
  
 在更改计划影响到设置时，只有在执行 DBCC FREEPROCCACHE (pool_name) 后，新设置才会在之前已缓存的计划中生效，其中 pool_name 是 Resource Governor 资源池的名称。  
  
-   如果将 AFFINITY 从多个计划程序更改为单个计划程序，则不需要执行 DBCC FREEPROCCACHE，因为并行计划可以在串行模式中运行。 但是，它可能不如编译为串行计划的计划那么有效。  
  
-   如果将 AFFINITY 从单计划程序更改为多个计划程序，则不需要执行 DBCC FREEPROCCACHE。 但串行计划不能并行运行，因此清除相应的缓存将允许使用并行编译新计划。  
  
> [!CAUTION]  
>  从多个工作负载组关联的资源池中清除缓存计划将影响用户定义资源池由 pool_name 标识的所有工作负载  。  
  
## <a name="permissions"></a>权限  
 需要 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
 下面的示例保留了 `default` 池的所有默认资源池设置，只有 `MAX_CPU_PERCENT` 除外，该设置更改为 `25`。  
  
```sql  
ALTER RESOURCE POOL "default"  
WITH  
     ( MAX_CPU_PERCENT = 25);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 在以下示例中，`CAP_CPU_PERCENT` 将硬性上限设置为 80%，并且 `AFFINITY SCHEDULER` 设置为单个值 8 以及 12 到 16 这个范围。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
```sql  
ALTER RESOURCE POOL Pool25  
WITH(   
     MIN_CPU_PERCENT = 5,  
     MAX_CPU_PERCENT = 10,       
     CAP_CPU_PERCENT = 80,  
     AFFINITY SCHEDULER = (8, 12 TO 16),   
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
);  
  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
