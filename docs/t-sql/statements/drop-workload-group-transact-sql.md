---
description: DROP WORKLOAD GROUP (Transact-SQL)
title: DROP WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_WORKLOAD_GROUP_TSQL
- DROP WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DROP WORKLOAD GROUP statement
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 60b18502d8ac5a39f7e429bc7eac90a0abe39313
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496852"
---
# <a name="drop-workload-group-transact-sql"></a>DROP WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=sqlallproducts-allversions"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 托管实例](drop-workload-group-transact-sql.md?view=azuresqldb-mi-current)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](drop-workload-group-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [DROP WORKLOAD GROUP](../../includes/drop-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](drop-workload-group-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](drop-workload-group-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
:::row-end:::

&nbsp;

##  <a name="sql-server-and-sql-managed-instance"></a>SQL Server 和 SQL 托管实例

[!INCLUDE [DROP WORKLOAD GROUP](../../includes/drop-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](drop-workload-group-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        [SQL 托管实例](drop-workload-group-transact-sql.md?view=azuresqldb-mi-current)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics 

删除工作负荷组。  语句完成后，设置生效。

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

## <a name="syntax"></a>语法

```syntaxsql
DROP WORKLOAD GROUP group_name  
```

## <a name="arguments"></a>参数

group_name  
现有的用户定义工作负荷组的名称。

## <a name="remarks"></a>备注

如果存在用于工作负荷组的分类器，则不能删除工作负荷组。  在删除工作负荷组之前，先删除分类器。  如果有活动请求正在使用要删除的工作负荷组中的资源，则删除工作负载语句将在这些请求之后被阻止。

## <a name="examples"></a>示例

使用以下代码示例来确定在删除工作负荷组之前需要删除的分类器。

```sql
SELECT c.name as classifier_name
      ,'DROP WORKLOAD CLASSIFIER '+c.name as drop_command
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_groups g
    ON c.group_name = g.name
  WHERE g.name = 'wgXYZ' --change the filter to the workload being dropped
```

## <a name="permissions"></a>权限

需要 CONTROL DATABASE 权限

## <a name="see-also"></a>另请参阅

- [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)
- [ALTER 工作负荷组 (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [快速入门：使用 T-SQL 配置工作负荷隔离](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
