---
description: 设置索引选项
title: 设置索引选项 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1803a13e13f75bbb3cf38cbcd841fa7e2dae1508
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478188"
---
# <a name="set-index-options"></a>设置索引选项

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改索引的属性。

 **本文内容**

- **开始之前：**

   [限制和局限](#Restrictions)

   [安全性](#Security)

- **修改索引的属性，使用：**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

- 以下选项通过使用 ALTER INDEX 语句中的 SET 子句，立即应用于索引：ALLOW_PAGE_LOCKS、ALLOW_ROW_LOCKS、OPTIMIZE_FOR_SEQUENTIAL_KEY、IGNORE_DUP_KEY 和 STATISTICS_NORECOMPUTE。
- 使用 ALTER INDEX REBUILD 或 CREATE INDEX WITH DROP_EXISTING 重新生成索引时，可以设置以下选项：PAD_INDEX、FILLFACTOR、SORT_IN_TEMPDB、IGNORE_DUP_KEY、STATISTICS_NORECOMPUTE、ONLINE、ALLOW_ROW_LOCKS、ALLOW_PAGE_LOCKS、MAXDOP 和 DROP_EXISTING（仅 CREATE INDEX）。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 权限

要求对表或视图具有 ALTER 权限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>在表设计器中修改索引的属性

1. 在对象资源管理器中，单击加号以便展开包含您要修改索引属性的表的数据库。
2. 单击加号以便展开 **“表”** 文件夹。
3. 右键单击你要修改索引属性的表，然后选择“设计”。
4. 在“表设计器”菜单上，单击“索引/键”。
5. 选择要修改的索引。 其属性将显示在主网格中。
6. 更改任意属性的设置以自定义索引。
7. 单击“关闭”  。
8. 在“文件”菜单上，选择“保存”以保存 _table_name_。

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>在对象资源管理器中修改索引的属性

1. 在对象资源管理器中，单击加号以便展开包含您要修改索引属性的表的数据库。
2. 单击加号以便展开 **“表”** 文件夹。
3. 单击加号以展开您要修改索引属性的表。
4. 单击加号以便展开 **“索引”** 文件夹。
5. 右键单击要修改其属性的索引，然后选择“属性”。
6. 在 **“选择页”** 下，选择 **“选项”** 。
7. 更改任意属性的设置以自定义索引。
8. 若要添加、删除或更改索引列的位置，请从“索引属性 - index_name”对话框中选择“常规”页 。 有关详细信息，请参阅 [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md)

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>查看表中所有索引的属性

下面的示例展示了 AdventureWorks 数据库中表的所有索引的属性。

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>设置索引的属性

下面的示例设置 AdventureWorks 数据库中索引的属性。

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

有关详细信息，请参阅 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)。
