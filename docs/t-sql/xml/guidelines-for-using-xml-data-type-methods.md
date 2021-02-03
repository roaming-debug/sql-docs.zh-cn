---
description: xml 数据类型方法的使用准则
title: xml 数据类型方法的使用准则
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: 1a483aa1-42de-4c88-a4b8-c518def3d496
author: MightyPen
ms.author: genemi
ms.openlocfilehash: adf76774e56841978d64661704f91cd6fd04d3fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193137"
---
# <a name="guidelines-for-using-xml-data-type-methods"></a>xml 数据类型方法的使用准则

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍 xml 数据类型方法的使用指南。

## <a name="the-print-statement"></a>PRINT 语句

xml 数据类型方法不能用于 PRINT 语句，如下面的示例所示。 xml 数据类型方法视为子查询来处理，而 PRINT 语句中不允许使用子查询。 因此，下面的示例将返回一个错误：

```sql
DECLARE @x XML
SET @x = '<root>Hello</root>'
PRINT @x.value('/root[1]', 'varchar(20)') -- will not work because this is treated as a subquery (select top 1 col from table)
```

一种解决方案是先将 value() 方法的结果分配给一个 xml 类型的变量，然后在查询中指定该变量。

```sql
DECLARE @x XML
DECLARE @c VARCHAR(max)
SET @x = '<root>Hello</root>'
SET @c = @x.value('/root[1]', 'VARCHAR(11)')
PRINT @c
```

## <a name="the-group-by-clause"></a>GROUP BY 子句

xml 数据类型方法在内部视为子查询来处理。 因为 GROUP BY 需要一个标量并且不允许使用聚合和子查询，所以在 GROUP BY 子句中不能指定 xml 数据类型方法。 另一种解决方案是调用用户定义函数，然后在其内部使用 XML 方法。

## <a name="reporting-errors"></a>报告错误

报告错误时，xml 数据类型方法会引发一个如下格式的错误：

```
Msg errorNumber, Level levelNumber, State stateNumber:
XQuery [database.table.method]: description_of_error
```

例如：

```
Msg 2396, Level 16, State 1:
XQuery [xmldb_test.xmlcol.query()]: Attribute may not appear outside of an element
```

## <a name="singleton-checks"></a>单一性检查

如果编译器无法确定在运行时能否确保单一性，则具有单一性要求的位置步骤、函数参数和运算符将返回错误。 此问题经常出现在非类型化数据上。 例如，查找属性时就需要使用单一的父元素。 通过一个用来选择单个父节点的序号即可满足此要求。 而在计算 node()-value() 组合以提取属性值时可能不需要指定序号规范。 如下例所示。

### <a name="example-known-singleton"></a>示例：已知单一实例

在此示例中，nodes() 方法为每个 `<book>` 元素生成一个单独的行。 对 `<book>` 节点进行计算的 value() 方法提取 `@genre` 值，并且是单一属性。

```sql
SELECT nref.value('@genre', 'VARCHAR(max)') LastName
FROM T CROSS APPLY xCol.nodes('//book') AS R(nref)
```

XML 架构用于对类型化的 XML 进行类型检查。 如果将某个节点指定为 XML 架构中单一的节点，则编译器将使用该信息，并且不会发生任何错误。 否则，需要使用一个用来选择单个节点的序号。 具体而言，使用 descendant-or-self 轴 (//) 轴（例如在 `/book//title` 中）会丢失 `<title>` 元素的单一性基数推理，即使 XML 架构指定其如此。 因此，应将其重写为 `(/book//title)[1]`。

对于类型检查，务必注意 `//first-name[1]` 和 `(//first-name)[1]` 之间的差异。 前者返回一组 `<first-name>` 节点，其中每个节点都是其同级节点中最左侧的 `<first-name>` 节点。 后者返回 XML 实例中按文档顺序排列的第一个单一的 `<first-name>` 节点。

### <a name="example-using-value"></a>示例：使用 value()

下面对非类型化 XML 列的查询导致发生静态的编译错误。这是因为 value() 希望将一个单一节点作为第一个参数，而编译器无法确定在运行时是否将仅有一个 `<last-name>` 节点：

```sql
SELECT xCol.value('//author/last-name', 'NVARCHAR(50)') LastName
FROM T
```

可以考虑下面的解决办法：

```sql
SELECT xCol.value('//author/last-name[1]', 'NVARCHAR(50)') LastName
FROM T
```

但是，该解决办法不解决错误，因为在每个 XML 实例中可能会有多个 `<author>` 节点。 采用下面的重写代码可以解决问题：

```sql
SELECT xCol.value('(//author/last-name/text())[1]', 'NVARCHAR(50)') LastName
FROM T
```

此查询返回每个 XML 实例中第一个 `<last-name>` 元素的值。

## <a name="see-also"></a>另请参阅

- [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)
