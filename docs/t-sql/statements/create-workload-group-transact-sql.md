---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 51db0074dda0a311dfbea84f51144d8b48ab90ce
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496893"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=sqlallproducts-allversions"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-workload-group-transact-sql.md?view=azuresqldb-mi-current)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-workload-group-transact-sql.md?view=azuresqldb-mi-current)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

创建工作负荷组。 工作负荷组是一组请求的容器，是在系统上配置工作负荷管理的基础。 通过使用工作负荷组，能够为工作负荷隔离保留资源、包含资源、定义每个请求的资源并遵循执行规则。 语句完成后，设置生效。

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

group_name</br>
指定用于标识工作负荷组的名称。 group_name 为 sysname。 最长可为 128 个字符，并且在实例中必须是唯一的。

MIN_PERCENTAGE_RESOURCE = value</br>
指定为此工作负荷组保证的最小资源分配，这些资源不与其他工作负荷组共享。 value 为 0 到 100 之间的整数。 所有工作负荷组的 min_percentage_resource 的总和不能超过 100。 min_percentage_resource 的值不能大于 cap_percentage_resource。 有每个服务级别允许的最小有效值。 有关更多详细信息，请参阅[有效值](#effective-values)。

CAP_PERCENTAGE_RESOURCE = value</br>
指定工作负荷组中所有请求的最大资源利用率。 整数取值范围为 1 到 100。 cap_percentage_resource 的值必须大于 min_percentage_resource。 如果在其他工作负荷组中将 min_percentage_resource 配置为大于零，则 cap_percentage_resource 的有效值会减少。

REQUEST_MIN_RESOURCE_GRANT_PERCENT = value</br>
设置每个请求分配到的最小资源量。 value 是一个必需参数，取值范围为 0.75 到 100.00（十进制）。 request_min_resource_grant_percent 的值必须是0.25 的倍数，必须是 min_percentage_resource 的因数，且小于 cap_percentage_resource。 有每个服务级别允许的最小有效值。 有关更多详细信息，请参阅[有效值](#effective-values)。

例如：

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

将用于资源类的值视为 request_min_resource_grant_percent 的基准。  下表包含用于 Gen2 的资源分配。

|资源类|资源的百分比|
|---|---|
|Smallrc|3%|
|Mediumrc|10%|
|Largerc|22%|
|Xlargerc|70%|
|||

REQUEST_MAX_RESOURCE_GRANT_PERCENT = value</br>         
设置每个请求分配的最小资源量。 value 是一个可选十进制参数，其默认值等于 request_min_resource_grant_percent。 value 必须大于或等于 request_min_resource_grant_percent。 当 request_max_resource_grant_percent 的值大于 request_min_resource_grant_percent 并且系统资源可用时，会向请求分配其他资源。

IMPORTANCE = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }</br>        
指定工作负荷组中某个请求的默认重要性。 重要性为下列值之一，默认值为 NORMAL：

- LOW
- BELOW_NORMAL
- NORMAL（默认值）
- ABOVE_NORMAL
- HIGH

在工作负荷组设置的重要性是工作负荷组中所有请求的默认重要性。 用户还可以在分类器级别设置重要性，这可能会覆盖工作负荷组的重要性设置。 这允许对工作负荷组内请求的重要性进行区分，以便更快地访问非保留资源。 当工作负荷组 min_percentage_resource 的总和小于 100 时，将根据重要性分配非保留资源。

QUERY_EXECUTION_TIMEOUT_SEC = value</br>     
指定查询在取消之前可以执行的最长时间（以秒为单位）。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，查询永不超时。QUERY_EXECUTION_TIMEOUT_SEC 在查询处于运行状态时而不是在查询加入队列时进行计数。

## <a name="remarks"></a>备注

自动创建对应于资源类的工作负荷组，以实现后向兼容性。 不能删除这些系统定义的工作负荷组。 可以创建额外的 8 个用户定义的工作负荷组。

如果使用大于零的 `min_percentage_resource` 创建工作负载组，则 `CREATE WORKLOAD GROUP` 语句将加入队列，直到有足够的资源来创建工作负载组。

## <a name="effective-values"></a>有效值

参数 `min_percentage_resource`、`cap_percentage_resource`、`request_min_resource_grant_percent` 和 `request_max_resource_grant_percent` 具有在当前服务级别和其他工作负载组配置的上下文基础上进行了调整的有效值。

`request_min_resource_grant_percent` 参数具有有效值，因为每个查询所需的最小资源数取决于服务级别。  例如，在最低的 DW100c 服务级别，每个请求至少需要 25% 的资源。  如果将工作负荷组配置为具有 3% 的 `request_min_resource_grant_percent` 和 `request_max_resource_grant_percent`，则在启动实例时，两个参数的有效值将调整为 25%。  如果将实例扩展到 DW1000c，则两个参数的已配置值和有效值均为 3%，因为 3% 是该服务级别支持的最小值。  如果将实例扩展到 DW1000c 以上，则这两个参数的已配置值和有效值将保持在 3%。  有关不同服务级别的有效值的更多详细信息，请参阅下表。

|服务级别|REQUEST_MIN_RESOURCE_GRANT_PERCENT 的最低有效值|最大并行查询|
|---|---|---|
|DW100c|25%|4|
|DW200c|12.5%|8|
|DW300c|8%|12|
|DW400c|6.25%|16|
|DW500c|5%|20|
|DW1000c|3%|32|
|DW1500c|3%|32|
|DW2000c|2%|48|
|DW2500c|2%|48|
|DW3000c|1.5%|64|
|DW5000c|1.5%|64|
|DW6000c|0.75%|128|
|DW7500c|0.75%|128|
|DW10000c|0.75%|128|
|DW15000c|0.75%|128|
|DW30000c|0.75%|128|
||||

`min_percentage_resource` 参数必须大于或等于有效 `request_min_resource_grant_percent`。 如果工作负荷组的 `min_percentage_resource` 被配置为小于有效 `min_percentage_resource`，那么在运行时，该值会被调整为零。 发生这种情况时，为 `min_percentage_resource` 配置的资源可在所有工作负荷组中共享。 例如，工作负荷组 `wgAdHoc` 的 `min_percentage_resource` 为 10%，在 DW1000c 服务级别运行，其有效 `min_percentage_resource` 将为 10%（DW1000c 支持的最低值为 3%）。 DW100c 级别的 `wgAdhoc` 的有效 min_percentage_resource 为 0%。 为 `wgAdhoc` 配置的 10% 将在所有工作负载组之间共享。

`cap_percentage_resource` 参数也具有有效值。 如果工作负载组 `wgAdhoc` 的 `cap_percentage_resource` 配置为 100%，并且创建了另一个工作负载组 `wgDashboards`，其 `min_percentage_resource` 为 25%，则 `wgAdhoc` 的有效 `cap_percentage_resource` 变为 75%。

了解工作负荷组运行时值的最简单方法是查询系统视图 [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)。

## <a name="permissions"></a>权限

需要 `CONTROL DATABASE` 权限

## <a name="see-also"></a>另请参阅

- [DROP 工作负荷组 (Transact-SQL)](drop-workload-group-transact-sql.md)
- [ALTER 工作负荷组 (Transact-SQL)](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [快速入门：使用 T-SQL 配置工作负荷隔离](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
