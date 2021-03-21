---
description: 向表中添加列（数据库引擎）
title: 向表中添加列（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- inserting columns
- columns [SQL Server], adding
- adding columns
ms.assetid: abeb8d52-d562-4e29-9e1e-2923ae874859
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f10eec1fac96247e2d909c6014fcc98c5568d91
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754957"
---
# <a name="add-columns-to-a-table-database-engine"></a>向表中添加列（数据库引擎）

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

本文介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中向表添加新列。

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

 使用 ALTER TABLE 语句向表添加列会自动将这些列添加到该表的末尾。 如果您希望该表中的列采用特定顺序，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。 但请注意，这并非数据库设计的最佳做法。 最佳做法是指定在应用程序级别和查询级别返回列的顺序。 您不应依赖于使用 SELECT * 基于在表中定义列的顺序以预期顺序返回所有列。 请始终按照您希望它们出现的顺序在您的查询和应用程序中按名称指定列。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 权限

需要对表的 ALTER 权限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-insert-columns-into-a-table-with-table-designer"></a>用表设计器向表中插入列

1. 在“对象资源管理器”中，右键单击要为其添加列的表，再选择“设计”。
2. 单击 **“列名”** 列中的第一个空单元。
3. 在该单元中键入列名。 列名是必需设置的值。
4. 按 Tab 键转到 **“数据类型”** 单元格，再从下拉列表中选择数据类型。

   它是必需设置的值，如果你没有作出选择，它将被赋以默认值。

   > [!NOTE]
   >  可以在 **“选项”** 对话框中的 **“数据库工具”** 之下更改默认值。

5. 在 **“列属性”** 选项卡上继续定义任何其他列属性。

    > [!NOTE]
    >  列属性的默认值在您创建新列时添加，但您可以在 **“列属性”** 选项卡中更改这些值。

6. 添加完列后，从“文件”菜单中，选择“保存”_表名称_ 。
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL
  
### <a name="to-insert-columns-into-a-table"></a>向表中插入列  
  
下面的示例将两列添加到表 `dbo.doc_exa`中。

```sql
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL, column_c INT NULL ;
```

#### <a name="for-more-information-see-alter-table-40transact-sql41"></a><a name="FollowUp"></a> 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)
