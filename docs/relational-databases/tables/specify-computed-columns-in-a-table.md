---
description: 指定表中的计算列
title: 指定表中的计算列 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, define
ms.assetid: 731a4576-09c1-47f0-a8f6-edd0b55679f4
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee1d32ce60064d6ab42b04a9aeddec4a6252d479
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88488584"
---
# <a name="specify-computed-columns-in-a-table"></a>指定表中的计算列

[!INCLUDE[tsql-appliesto-ss-asdb-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

计算列是虚拟列，并非实际存储在表中，除非此列标记为 PERSISTED。 计算列的表达式可以使用其他列中的数据来计算其所属列的值。 可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中为计算列指定一个表达式。

**本主题内容**

- **开始之前：**

   [限制和局限](#Limitations)

   [安全性](#Security)

- **使用以下工具指定计算列：**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前

### <a name="limitations-and-restrictions"></a><a name="Limitations"></a> 限制和局限

- 计算列不能用作 DEFAULT 或 FOREIGN KEY 约束定义，也不能与 NOT NULL 约束定义一起使用。 但是，如果计算列值由具有确定性的表达式定义，并且索引列中允许使用计算结果的数据类型，则可将该列用作索引中的键列，或用作 PRIMARY KEY 或 UNIQUE 约束的一部分。 例如，如果表中包含整数列 a 和 b，则可以对计算列 a + b 创建索引。但不能对计算列 a+DATEPART(dd, GETDATE()) 创建索引，因为在以后的调用中，其值可能发生更改。
- 计算列不能作为 INSERT 或 UPDATE 语句的目标。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 权限

需要对表的 ALTER 权限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-add-a-new-computed-column"></a><a name="NewColumn"></a> 添加新的计算列

1. 在 **“对象资源管理器”** 中，展开要添加新计算列的表。 右键单击“列”，再选择“新建列”。
2. 输入列名并接受默认数据类型 (**nchar**(10))。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 通过将数据类型的优先顺序规则应用到在公式中指定的表达式，来确定计算列的数据类型。 例如，如果公式引用一个类型为 **money** 的列和一个类型为 **int** 的列，则计算列的类型将为 **money** ，因为该数据类型具有较高优先顺序。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。
3. 在 **“列属性”** 选项卡中，展开 **“计算所得的列规范”** 属性。
4. 在“(公式)”子属性中，在右侧的网格单元格中输入此列的表达式。 例如，在 `SalesTotal` 列中，您输入的公式可能是 `SubTotal+TaxAmt+Freight`，它将该值加入到表中每行的这些列中。

   > [!IMPORTANT]
   > 当两个不同数据类型的表达式用公式组合后，数据类型优先级规则指定将优先级较低的数据类型转换为优先级较高的数据类型。 如果此转换不是所支持的隐式转换，则返回错误“`Error validating the formula for column column_name.`”。 使用 CAST 或 CONVERT 函数解决数据类型冲突。 例如，如果类型为 **nvarchar** 的列与类型为 **int** 的列相结合，则整数类型必须转换为 **nvarchar** ，如公式 `('Prod'+CONVERT(nvarchar(23),ProductID))`中所示。 有关详细信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。

5. 从“持久化”子属性的下拉菜单上选择“是”或“否”，以指示该数据是否持久。

6. 在“文件”菜单上，单击“保存表名称” 。

#### <a name="to-add-a-computed-column-definition-to-an-existing-column"></a>将计算列定义添加到现有列中

1. 在“对象资源管理器”中，右键单击该表以及你要对其更改和展开“列”文件夹的列。
2. 右键单击你要为其指定计算列公式的列，然后单击“删除”。 单击“确定”。 
3. 添加一个新列，然后按照前面的步骤添加新计算列以指定新计算列公式。

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

### <a name="to-add-a-computed-column-when-creating-a-table"></a>创建表时添加计算列

下面的示例创建一个表，其中的计算列将 `QtyAvailable` 列的值乘以 `UnitPrice` 列的值。

```sql
CREATE TABLE dbo.Products
   (
      ProductID int IDENTITY (1,1) NOT NULL
      , QtyAvailable smallint
      , UnitPrice money
      , InventoryValue AS QtyAvailable * UnitPrice
    )
;
-- Insert values into the table.
INSERT INTO dbo.Products (QtyAvailable, UnitPrice)
   VALUES (25, 2.00), (10, 1.5)
;
-- Display the rows in the table.
SELECT ProductID, QtyAvailable, UnitPrice, InventoryValue
FROM dbo.Products
;
```

### <a name="to-add-a-new-computed-column-to-an-existing-table"></a>将新计算列定义添加到现有表中

以下示例向在前一个示例中创建的表添加一个新列。

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

（可选）添加 PERSISTED 参数，将计算值以物理方式存储到表中：

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5) PERSISTED
;
```

### <a name="to-change-an-existing-column-to-a-computed-column"></a>将现有列更改为计算列

以下示例修改在前一个示例中添加的列。

```sql
ALTER TABLE dbo.Products DROP COLUMN RetailValue
;
GO
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。
