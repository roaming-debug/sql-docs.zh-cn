---
description: 'sys.sp_cleanup_temporal_history (Transact-sql) '
title: sys.sp_cleanup_temporal_history |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 6eff30b4-b261-4f1f-b93c-1f69d754298d
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.openlocfilehash: e4afeb9f30040cf576a35b1b822bf5292752c148
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97427145"
---
# <a name="syssp_cleanup_temporal_history-transact-sql"></a>sys.sp_cleanup_temporal_history (Transact-sql) 

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

删除临时历史记录表中的所有行，这些行与单个事务中的配置 HISTORY_RETENTION 时间段匹配。

## <a name="syntax"></a>语法

```
sp_cleanup_temporal_history [@schema_name = ] schema_name, [@table_name = ] table_name [, [@row_count=] @row_count_var [OUTPUT]]
```

## <a name="arguments"></a>自变量

*\@table_name*

为其调用保留清除的临时表的名称。

*schema_name*

当前时态表所属的架构的名称

*row_count_var* [输出]

返回已删除的行数的输出参数。 如果历史记录表中包含聚集列存储索引，则此参数将始终返回0。

## <a name="remarks"></a>备注

此存储过程只能与指定了有限保留期的临时表一起使用。
仅当需要立即清除历史记录表中的所有过期行时，才使用此存储过程。 您应该知道，它会对数据库日志和 i/o 子系统产生重大影响，因为它会删除同一事务中所有符合条件的行。

始终建议依赖于清理的内部后台任务，以删除过期的行，对常规工作负荷和数据库的影响最小。

## <a name="permissions"></a>权限

需要 db_owner 权限。

## <a name="example"></a>示例

```sql
declare @rowcnt int
EXEC sys.sp_cleanup_temporal_history 'dbo', 'Department', @rowcnt output
select @rowcnt
```

## <a name="next-steps"></a>后续步骤

[时态表保留策略](/azure/sql-database/sql-database-temporal-tables-retention-policy)