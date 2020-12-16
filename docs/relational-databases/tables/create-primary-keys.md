---
description: 创建主键
title: 创建主键 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- primary keys [SQL Server], creating
ms.assetid: 85c623ca-4656-4d70-a9db-ee4d897cd214
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: edc3d1c699000e72dd963fc7b115edb72904329a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484499"
---
# <a name="create-primary-keys"></a>创建主键


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中定义主键。 创建主键会自动创建相应的唯一群集索引、聚集索引或非聚集索引（如果这样指定）。

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

- 一个表只能包含一个 PRIMARY KEY 约束。

- 在 PRIMARY KEY 约束中定义的所有列都必须定义为 NOT NULL。 如果没有指定为 Null 性，则加入 PRIMARY KEY 约束的所有列的为 Null 性都将设置为 NOT NULL。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 权限

使用主键创建新表需要在数据库中具有 CREATE TABLE 权限，并对在其中创建表的架构具有 ALTER 权限。

在某一现有表中创建主键需要对该表具有 ALTER 权限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-create-a-primary-key"></a>创建主键

1. 在对象资源管理器中，右键单击要为其添加唯一约束的表，然后单击“设计”。
2. 在 **“表设计器”** 中，单击要定义为主键的数据库列的行选择器。 若要选择多个列，请在单击其他列的行选择器时按住 Ctrl 键。
3. 右键单击该列的行选择器，然后选择“设置主键”。

> [!CAUTION]
> 若要重新定义主键，则必须首先删除与现有主键之间的任何关系，然后才能创建新主键。 此时，将显示一条消息警告您：作为该过程的一部分，将自动删除现有关系。

主键列由其行选择器中的主键符号标识。

如果主键由多个列组成，则其中一个列将允许重复值，但是主键中所有列的值的各种组合必须是唯一的。

如果定义复合键，则主键中列的顺序将与表中显示的列顺序相匹配。 不过，您可以在创建主键之后更改列的顺序。 有关详细信息，请参阅 [修改主键](../../relational-databases/tables/modify-primary-keys.md)。

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

### <a name="to-create-a-primary-key-in-an-existing-table"></a>在现有表中创建主键

下面的示例对 AdventureWorks 数据库中的 `TransactionID` 列创建主键。

```sql
ALTER TABLE Production.TransactionHistoryArchive
   ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID);
```

### <a name="to-create-a-primary-key-in-a-new-table"></a>在新表中创建主键

下面的示例创建一个表，并对 AdventureWorks 数据库中的 `TransactionID` 列定义主键。

```sql
CREATE TABLE Production.TransactionHistoryArchive1
   (
      TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
   )
;
```

### <a name="to-create-a-primary-key-with-clustered-index-in-a-new-table"></a>在新表中创建具有聚集索引的主键

下面的示例创建一个表，并对 AdventureWorks 数据库中的 `CustomerID` 列和 `TransactionID` 分别定义主键和聚集索引。

```sql
-- Create table to add the clustered index
CREATE TABLE Production.TransactionHistoryArchive1
   (
      CustomerID uniqueidentifier DEFAULT NEWSEQUENTIALID()
      , TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive1_CustomerID PRIMARY KEY NONCLUSTERED (CustomerID)
   )
;

-- Now add the clustered index
CREATE CLUSTERED INDEX CIX_TransactionID ON Production.TransactionHistoryArchive1 (TransactionID);
```

## <a name="see-also"></a>另请参阅

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 
- [table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)
