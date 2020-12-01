---
description: SET SHOWPLAN_XML (Transact-SQL)
title: SET SHOWPLAN_XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET SHOWPLAN_XML
- SET_SHOWPLAN_XML_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- SET SHOWPLAN_XML statement
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- XML [SQL Server], statement execution information
- SHOWPLAN_XML option
- estimated execution information [SQL Server]
ms.assetid: a467a1b3-10a5-43c4-9085-13d8aed549c9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c9ba02ca79db2e79f14483e632eaa6fa77c3d4a0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88304685"
---
# <a name="set-showplan_xml-transact-sql"></a>SET SHOWPLAN_XML (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回有关如何以定义好的 XML 文档格式执行上述语句的详细信息。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
SET SHOWPLAN_XML { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注

SET SHOWPLAN_XML 的设置是在执行或运行时设置的，而不是在分析时设置的。

如果 SET SHOWPLAN_XML 为 ON，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在不执行语句的情况下返回每个语句的执行计划信息。不会执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 将该选项设置为 ON 以后，将返回有关所有后续 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的执行计划信息，直到将该选项设置为 OFF 为止。 例如，如果在 SET SHOWPLAN_XML 为 ON 时执行 CREATE TABLE 语句，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将从涉及同一个表的后续 SELECT 语句返回错误信息：指定的表不存在。 因此，对此表的后续引用将失败。 如果 SET SHOWPLAN_XML 为 OFF，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将执行语句，但不生成报表。

SET SHOWPLAN_XML 可以为应用程序（如 **sqlcmd** 实用工具）将输出返回为 **nvarchar(max)**，其中 XML 输出随后将由其他工具用来显示和处理查询计划信息。

> [!NOTE]
> 动态管理视图 **sys.dm_exec_query_plan** 将使用 **xml** 数据类型返回与 SET SHOWPLAN XML 相同的信息。 该信息是从 **sys.dm_exec_query_plan** 的 **query_plan** 列返回的。 有关详细信息，请参阅 [sys.dm_exec_query_plan (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)。

不能在存储过程内部指定 SET SHOWPLAN_XML。 该语句必须是批处理中的唯一语句。

SET SHOWPLAN_XML 将返回一组 XML 文档信息。 SET SHOWPLAN_XML ON 语句之后的每个批处理都将在单个文档输出中得到反映。 每个文档都包含批处理中语句的文本，后跟执行步骤的详细信息。 该文档可以显示估计的开销、行数、访问的索引数、执行的运算符的类型、联接次序以及有关执行计划的详细信息。

> [!NOTE]
> 如果在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中选择了“包括实际的执行计划”，则该 SET 选项不会生成 XML 显示计划输出。 请在使用该 SET 选项之前清除“包括实际的执行计划”按钮。

### <a name="location-of-showplan-output"></a>显示计划输出的位置

在安装了 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的本地目录上安装，将复制包含 XML 架构的文档（该架构用于来自 SET SHOWPLAN_XML 的 XML 输出）。 可在包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装文件的驱动器上查找该文档，路径如下所示：

- `\Microsoft SQL Server\130\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd`

在前面的路径中，SQL Server 2016 使用的是节点 `130\`。 数字 130 派生自由 `SELECT @@VERSION` 返回的值的第一个节点，即 13。 SQL Server 2017 的路径将使用 `140\`，这是因为其 @@VERSION 值的第一个节点为 14。 对于 SQL Server 2019，@@VERSION 的第一个值为 15。

显示计划架构也可从[此网站](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)中找到。

## <a name="permissions"></a>权限

必须对执行 SET SHOWPLAN_XML 的语句具有足够的执行权限，必须对包含被引用对象的所有数据库具有 SHOWPLAN 权限。

对于 SELECT、INSERT、UPDATE、DELETE、EXEC stored_procedure 和 EXEC user_defined_function 语句，若要生成显示计划，用户必须   ：

- 具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限。

- 对于包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句引用的对象（如表、视图等）的所有数据库，具有 SHOWPLAN 权限。

对于其他所有语句，如 DDL、USE database_name、SET、DECLARE、动态 SQL 等，只需要具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限  。

## <a name="examples"></a>示例

下面两个语句使用了 SET SHOWPLAN_XML 设置，以显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在查询中分析和优化索引的方法。

第一个查询在 WHERE 子句中使用针对索引列的等于比较运算符 (=)。 第二个查询在 WHERE 子句中使用 LIKE 运算符。 这将强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用聚集索引扫描并查找满足 WHERE 子句条件的数据。 第一个索引查询的 **EstimateRows** 和 **EstimatedTotalSubtreeCost** 属性中的值较小，这表示与非索引查询相比，该查询的处理速度快得多且使用更少的资源。

```sql
USE AdventureWorks2012;
GO
SET SHOWPLAN_XML ON;
GO
-- First query.
SELECT BusinessEntityID
FROM HumanResources.Employee
WHERE NationalIDNumber = '509647174';
GO
-- Second query.
SELECT BusinessEntityID, JobTitle
FROM HumanResources.Employee
WHERE JobTitle LIKE 'Production%';
GO
SET SHOWPLAN_XML OFF;
```

## <a name="see-also"></a>另请参阅

[SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)
