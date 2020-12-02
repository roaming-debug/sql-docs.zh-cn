---
description: 指定列的默认值
title: 指定列的默认值 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], defaults
- default values
ms.assetid: 64514aed-b846-407b-992e-cf813f9a1a91
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 361e98e19788f4d2c6aa921caf71e58552ee53a0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88488551"
---
# <a name="specify-default-values-for-columns"></a>指定列的默认值

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

可使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定将在表列中输入的默认值。 可以通过使用用户界面的对象资源管理器或提交 [!INCLUDE[tsql](../../includes/tsql-md.md)] 设置默认值。

如果没有分配默认值到列，并且用户将该列保留为空白，则：

- 如果设置了允许空值的选项，则将向该列中插入 NULL。

- 如果没有设置允许空值的选项，则该列将保持空白，但在用户为该列提供值之前，他们将无法保存行。

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

在开始之前，请注意以下限制和局限：

- 如果“默认值”字段中的项替换绑定的默认值（以不带圆括号的形式显示），则将提示你解除对默认值的绑定，并将其替换为新的默认值。

- 若要输入文本字符串，请用单引号 (') 将值括起来；不要使用双引号 (")，因为双引号已保留用于带引号的标识符。

- 若要输入数值默认值，请输入数值并且不要用引号将值括起来。

- 若要输入对象/函数，请输入对象/函数的名称并且不要用引号将名称括起来。

### <a name="security-permissions"></a><a name="Security"></a> 安全权限

本文中所述的操作要求对表具有 ALTER 权限。

## <a name="use-ssms-to-specify-a-default"></a><a name="SSMSProcedure"></a> 使用 SSMS 指定默认值

可以使用对象资源管理器指定表列的默认值。

### <a name="object-explorer"></a>“对象资源管理器”

1. 在“对象资源管理器”中，右键单击要更改其小数位数的列所在的表，再单击“设计”。

2. 选择要为其指定默认值的列。

3. 在 **“列属性”** 选项卡中，在 **“默认值或绑定”** 属性中输入新的默认值。

   > [!NOTE]
   > 若要输入数值默认值，请输入该数字。 对于对象或函数，请输入其名称。 对于字母数字默认值，请输入该值，两边用单引号引起来。

4. 在“文件”菜单上，单击“保存表名称” 。

## <a name="use-transact-sql-to-specify-a-default"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL 指定默认值

可通过多种方法使用 SSMS 提交 T-SQL，指定列的默认值。

### <a name="alter-table-t-sql"></a>ALTER TABLE (T-SQL)

1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。

2. 在标准菜单栏上，单击 **“新建查询”** 。

3. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。

   ```sql
   CREATE TABLE dbo.doc_exz (column_a INT, column_b INT); -- Allows nulls.
   GO
   INSERT INTO dbo.doc_exz (column_a) VALUES (7);
   GO
   ALTER TABLE dbo.doc_exz
     ADD CONSTRAINT DF_Doc_Exz_Column_B
     DEFAULT 50 FOR column_b;
   GO
   ```

<!--
The following two T-SQL code examples were offered by 'nycdotnet' (Steve) via public PR 1660, Feb 2019.
-->

### <a name="create-table-t-sql"></a>CREATE TABLE (T-SQL)

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT DEFAULT 50);
```

### <a name="named-constraint-t-sql"></a>命名的 CONSTRAINT (T-SQL)

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT CONSTRAINT DF_Doc_Exz_Column_B DEFAULT 50);
```

有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。
