---
description: ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
title: ALTER EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- ALTER_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL RESOURCE POOL statement
ms.assetid: 634c327d-971b-49ba-b8a2-e243a04040db
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: de8fe4503436963094dde524c7fb65a677e91826
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464138"
---
# <a name="alter-external-resource-pool-transact-sql"></a>ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

更改一个 Resource Governor 外部池，该池用于指定外部进程可使用的资源。 

::: moniker range="=sql-server-2016"
对于 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 中的[!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]，外部池控制 `rterm.exe`、`BxlServer.exe` 以及它们生成的其他进程。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
对于 [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]，外部池控制 `rterm.exe`、`python.exe`、`BxlServer.exe`，以及它们生成的其他进程。
::: moniker-end

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

## <a name="syntax"></a>语法
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```syntaxsql
ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end
::: moniker range="=sql-server-2016||=sql-server-2017"
 ```syntaxsql

ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] AFFINITY CPU =
            {
                AUTO
              | ( <cpu_range_spec> )
              | NUMANODE = (( <NUMA_node_id> )
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end 

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

{ pool_name | "default" }  
现有用户定义外部资源池或安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时创建的默认外部资源池的名称。
与 `ALTER EXTERNAL RESOURCE POOL` 一起使用时，"default" 必须用引号 ("") 引起来或用方括号 ([]) 括起来，以免与系统保留字 `DEFAULT` 冲突。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =value  
指定在存在 CPU 争用时外部资源池中的所有请求可以接收的最大平均 CPU 带宽。 value 是一个整数。 value 的允许范围是 1 到 100。

MAX_MEMORY_PERCENT =value  
指定此外部资源池中的请求可使用的总服务器内存量。 value 是一个整数。 value 的允许范围是 1 到 100。

MAX_PROCESSES =value  
指定外部资源池允许的最大进程数。 指定 0 以便为池设置无限阈值，此阈值之后仅受计算机资源约束。
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =value  
指定在存在 CPU 争用时外部资源池中的所有请求可以接收的最大平均 CPU 带宽。 value 是一个整数。 value 的允许范围是 1 到 100。

AFFINITY {CPU = AUTO | ( \<CPU_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}  
将外部资源池附加到特定的 CPU。

AFFINITY CPU = ( \<CPU_range_spec> ) 将外部资源池映射到由给定 CPU_ID 标识的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CPU 。 使用 AFFINITY NUMANODE = ( \<NUMA_node_range_spec> ) 时，会将外部资源池关联到对应于给定 NUMA 节点或节点范围的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 物理 CPU 。

MAX_MEMORY_PERCENT =value  
指定此外部资源池中的请求可使用的总服务器内存量。 value 是一个整数。 value 的允许范围是 1 到 100。

MAX_PROCESSES =value  
指定外部资源池允许的最大进程数。 指定 0 以便为池设置无限阈值，此阈值之后仅受计算机资源约束。
::: moniker-end
## <a name="remarks"></a>备注

执行 [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md) 语句时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]实现资源池。

有关资源池的常规信息，请参阅 [Resource Governor 资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)、[sys.resource_governor_external_resource_pools (Transact-SQL)](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) 和 [sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)。  

有关使用外部资源池管理机器学习作业的特定信息，请参阅 [SQL Server 中机器学习的资源调控](../../machine-learning/administration/resource-governor.md)。
## <a name="permissions"></a>权限

需要 `CONTROL SERVER` 权限。

## <a name="examples"></a>示例

以下语句更改外部池，将 CPU 使用率限制为 50% 并将最大内存限制为计算机可用内存的 25%。
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>另请参阅

+ [SQL Server 中机器学习的资源调控](../../machine-learning/administration/resource-governor.md)
+ [“已启用外部脚本”服务器配置选项](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [ALTER RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md) 
