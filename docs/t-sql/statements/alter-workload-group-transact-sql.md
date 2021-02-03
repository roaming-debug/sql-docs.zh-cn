---
description: ALTER WORKLOAD GROUP (Transact-SQL)
title: ALTER WORKLOAD GROUP (Transact-SQL)
ms.custom: ''
ms.date: 05/05/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_WORKLOAD_GROUP_TSQL
- ALTER WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER WORKLOAD GROUP statement
ms.assetid: 957addce-feb0-4e54-893e-5faca3cd184c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 8f387c9ab6d67f11211107b4e7c44d38d23702cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199381"
---
# <a name="alter-workload-group-transact-sql"></a>ALTER WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

更改现有工作负荷组。

若要详细了解 `ALTER WORKLOAD GROUP` 在具有正在运行和已排队的请求的系统中的行为，请参阅下面的 `ALTER WORKLOAD GROUP` 行为部分。 

对 [CREATE WORKLOAD GROUP](create-workload-group-transact-sql.md) 实施的限制同样适用于 `ALTER WORKLOAD GROUP`。  在修改参数之前，请查询 [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md) 以确保值在可接受的范围内。

## <a name="syntax"></a>语法

```syntaxsql
ALTER WORKLOAD GROUP group_name
 WITH
 ([       MIN_PERCENTAGE_RESOURCE = value ]
  [ [ , ] CAP_PERCENTAGE_RESOURCE = value ]
  [ [ , ] REQUEST_MIN_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ] 
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]
  ```

## <a name="arguments"></a>参数

group_name  
正在更改现有的用户定义工作负荷组的名称。  group_name 不可更改。 

MIN_PERCENTAGE_RESOURCE = value  
value 为 0 到 100 之间的整数。  更改 min_percentage_resource 时，所有工作负荷组的 min_percentage_resource 的总和不能超过 100。  更改 min_percentage_resource 时，需要在完成该命令前在工作负荷组中完成所有正在运行的查询。  有关更多详细信息，请参阅本文档中的“ALTER WORKLOAD GROUP 行为”一节。

CAP_PERCENTAGE_RESOURCE = value  
value 为 1 到 100 之间的整数。  cap_percentage_resource 的值必须大于 min_percentage_resource。  更改 cap_percentage_resource 时，需要在完成该命令前在工作负荷组中完成所有正在运行的查询。  有关更多详细信息，请参阅本文档中的“ALTER WORKLOAD GROUP 行为”一节。 

REQUEST_MIN_RESOURCE_GRANT_PERCENT = value  
value 为十进制数，范围介于 0.75 和 100.00 之间。  request_min_resource_grant_percent 的值必须是 min_percentage_resource 的因数，并且必须小于 cap_percentage_resource。 
  
REQUEST_MAX_RESOURCE_GRANT_PERCENT = value  
value 为十进制数，必须大于 request_min_resource_grant_percent。

IMPORTANCE = { LOW \|  BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
更改工作负荷组中某个请求的默认重要性。

QUERY_EXECUTION_TIMEOUT_SEC = value  
更改查询在取消之前可以执行的最长时间（以秒为单位）。 value 必须为 0 或一个正整数。 value 的默认设置为 0，也就是说无限制。   

## <a name="permissions"></a>权限

需要 CONTROL DATABASE 权限

## <a name="example"></a>示例

以下示例检查 wgDataLoads 目录视图中的值并更改这些值。

```sql
SELECT *
FROM sys.workload_management_workload_groups  
WHERE [name] = 'wgDataLoads'

ALTER WORKLOAD GROUP wgDataLoads WITH
( MIN_PERCENTAGE_RESOURCE            = 40
 ,CAP_PERCENTAGE_RESOURCE            = 80
 ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 10 )
 ```

## <a name="alter-workload-group-behavior"></a>ALTER WORKLOAD GROUP 行为

在任意时间点，系统中都有 3 种类型的请求
- 尚未分类的请求。
- 已分类且正在等待对象锁定或系统资源的请求。
- 已分类且正在运行的请求。

根据要更改的工作负荷组的属性，设置生效的时间将有所不同。

重要性或 query_execution_timeout - 对于重要性和 query_execution_timeout 属性，未分类的请求选取新的配置值。  正在等待且正在运行的请求将通过旧配置执行。  无论工作负荷组中是否存在正在运行的查询，`ALTER WORKLOAD GROUP` 请求都会立即执行。

Request_min_resource_grant_percent 或 request_max_resource_grant_percent - 对于 request_min_resource_grant_percent 和 request_max_resource_grant_percent，正在运行的请求将通过旧配置执行。  正在等待的请求和未分类的请求选取新的配置值。  无论工作负荷组中是否存在正在运行的查询，`ALTER WORKLOAD GROUP` 请求都会立即执行。

Min_percentage_resource 或 cap_percentage_resource - 对于 min_percentage_resource 和 cap_percentage_resource，正在运行的请求将通过旧配置执行。  正在等待的请求和未分类的请求选取新的配置值。 

更改 min_percentage_resource 和 cap_percentage_resource 需要用尽要更改的工作负荷组中正在运行的请求。  当减少 min_percentage_resource 时，释放的资源将返回到共享池，这样，来自其他工作负荷组的请求就可以利用它。  相反，增加 min_percentage_resource 会等到只利用共享池中所需资源的请求完成。  更改工作负荷组操作可以优先访问共享资源，而不是在共享池上等待执行的其他请求。  如果 min_percentage_resource 的总和超过 100%，ALTER WORKLOAD GROUP 请求会立即失败。 

锁定行为 - 更改工作负荷组要求对所有工作负荷组进行全局锁定。  更改工作负荷组的请求排在已提交的创建或删除工作负荷组请求后面。  如果立即提交一批更改语句，它们将按照提交的顺序进行处理。  

## <a name="see-also"></a>另请参阅

- [CREATE WORKLOAD GROUP (Transact-SQL)](create-workload-group-transact-sql.md)
- [DROP 工作负荷组 (Transact-SQL)](drop-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [快速入门：使用 T-SQL 配置工作负荷隔离](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
