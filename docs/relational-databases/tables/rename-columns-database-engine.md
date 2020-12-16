---
description: 重命名列（数据库引擎）
title: 重命名列（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], names
- renaming columns
- column names [SQL Server]
ms.assetid: 7c71ec9f-0180-4398-b32a-4bfb7592e75d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 70df620a283ac7f49c8ec854884a25843adc3e1c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464508"
---
# <a name="rename-columns-database-engine"></a>重命名列（数据库引擎）


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 重命名 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的表列。

**本主题内容**

- **开始之前：**

   [限制和局限](#Restrictions)

   [安全性](#Security)

- **若要重命名列，请使用：**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

重命名列将不会自动重命名对该列的引用。 您必须手动修改引用已重命名列的任何对象。 例如，如果您重命名表列，并且触发器中引用了该列，则必须修改触发器以反映新的列名。 请使用 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 在重命名对象之前列出对象的依赖关系。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 权限

需要对对象的 ALTER 权限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-rename-a-column-using-object-explorer"></a>使用对象资源管理器重命名列

1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。
2. 在“对象资源管理器”中，右键单击要重命名其中的列的表，再选择“重命名”。
3. 键入新的列名称。

### <a name="to-rename-a-column-using-table-designer"></a>使用表设计器重命名列

1. 在“对象资源管理器”中，右键单击要为其重命名列的表，再选择“设计”。
2. 在 **“列名”** 下，选择要更改的名称，并键入新名称。
3. 在“文件”菜单上，单击“保存表名称” 。

> [!NOTE]
> 您也可以在 **“列属性”** 选项卡中更改列名。选择要更改名称的列，并为 **“名称”** 键入新值。

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

**重命名列**

### <a name="to-rename-a-column"></a>重命名列

下面的示例将 AdventureWorks 数据库中表 `Sales.SalesTerritory` 内的 `TerritoryID` 列重命名为 `TerrID`。

```sql
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';
```

有关详细信息，请参阅 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)。
