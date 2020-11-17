---
title: FOR XML (SQL Server)
description: 了解在 SQL 查询中用于将结果作为 XML 进行检索的 FOR XML 子句。
ms.prod: sql
ms.prod_service: database-engine
ms.technology: xml
ms.topic: conceptual
f1_keywords:
- FOR_XML_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FOR XML clause, about FOR XML clause
- PATH FOR XML mode, construction
- EXPLICIT FOR XML mode
- RAW FOR XML mode
- retrieving XML data
- XML [SQL Server], FOR XML clause
- AUTO FOR XML mode
- XML [SQL Server], construction
ms.assetid: 2b6b5c61-c5bd-49d2-8c0c-b7cf15857906
author: RothJa
ms.author: jroth
ms.reviewer: ''
ms.custom: fresh2019may
ms.date: 04/03/2020
ms.openlocfilehash: 974da804e79a6c571b7543caec230984296f6a8e
ms.sourcegitcommit: 2bf83972036bdbe6a039fb2d1fc7b5f9ca9589d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674145"
---
# <a name="for-xml-sql-server"></a>FOR XML (SQL Server)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

SELECT 查询将结果作为行集返回。 （可选操作）您可以通过在 SQL 查询中指定 FOR XML 子句，从而将该查询的正式结果作为 XML 来检索。 FOR XML 子句可以用在顶级查询和子查询中。 顶级 FOR XML 子句只能用在 SELECT 语句中。 而在子查询中，FOR XML 可以用在 INSERT、UPDATE 和 DELETE 语句中。 FOR XML 还可以用在赋值语句中。

在 FOR XML 子句中，指定以下模式之一：

- RAW
- AUTO
- EXPLICIT
- PATH

RAW 模式将为 SELECT 语句所返回行集中的每行生成一个 \<row> 元素。 可以通过编写嵌套 FOR XML 查询来生成 XML 层次结构。

AUTO 模式将基于指定 SELECT 语句的方式来使用试探性方法在 XML 结果中生成嵌套。 您对生成的 XML 的形状具有最低限度的控制能力。 除了 AUTO 模式的试探性方法生成的 XML 形状之外，还可以编写 FOR XML 查询来生成 XML 层次结构。

EXPLICIT 模式允许对 XML 的形状进行更多控制。 您可以随意混合属性和元素来确定 XML 的形状。 由于执行查询而生成的结果行集需要具有特定的格式。 此行集格式随后将映射为 XML 形状。 使用 EXPLICIT 模式能够随意混合属性和元素、创建包装和嵌套的复杂属性、创建用空格分隔的值（例如 OrderID 属性可能具有一列排序顺序 ID 值）以及混合内容。

但是，编写 EXPLICIT 模式的查询会比较麻烦。 可以使用某些新的 FOR XML 功能（例如编写嵌套 FOR XML RAW/AUTO/PATH 模式查询和 TYPE 指令），而不使用 EXPLICIT 模式来生成层次结构。 嵌套 FOR XML 查询可以生成使用 EXPLICIT 模式可生成的任何 XML。 有关详细信息，请参阅 [使用嵌套的 FOR XML 查询](../../relational-databases/xml/use-nested-for-xml-queries.md) 和 [FOR XML 查询中的 TYPE 指令](../../relational-databases/xml/type-directive-in-for-xml-queries.md)。

PATH 模式与嵌套 FOR XML 查询功能一起以较简单的方式提供了 EXPLICIT 模式的灵活性。

仅当执行设置了这些模式的查询时，这些模式才有效。 它们不会影响以后执行的任何查询的结果。

对于任何与 FOR BROWSE 子句一起使用的选择语句，FOR XML 均无效。

## <a name="example"></a>示例

下面的 `SELECT` 语句将从 `Sales.Customer` 数据库的 `Sales.SalesOrderHeader` 和 `AdventureWorks2012` 表中检索信息。 此查询在 `AUTO` 子句中指定了 `FOR XML` 模式：

```sql
USE AdventureWorks2012
GO
SELECT Cust.CustomerID,
       OrderHeader.CustomerID,
       OrderHeader.SalesOrderID,
       OrderHeader.Status
FROM Sales.Customer Cust 
INNER JOIN Sales.SalesOrderHeader OrderHeader
ON Cust.CustomerID = OrderHeader.CustomerID
FOR XML AUTO;
```

## <a name="the-for-xml-clause-and-server-names"></a>FOR XML 子句和服务器名称

如果带 FOR XML 子句的 SELECT 语句在查询中指定了一个由四部分组成的名称，则在本地计算机上执行查询时，所得 XML 文档中将不返回该服务器名称。 但在网络服务器上执行该查询时，此服务器名称将作为由四部分组成的名称返回。

例如，请考虑下面的查询：

```sql
SELECT TOP 1 LastName
  FROM ServerName.AdventureWorks2012.Person.Person
  FOR XML AUTO;
```

&nbsp;

**本地服务器**：&nbsp; 如果 `ServerName` 是本地服务器，此查询返回以下文本：

```xml
<AdventureWorks2012.Person.Person LastName="Achong" />  
```

&nbsp;

**网络服务器**：&nbsp; 如果 `ServerName` 是网络服务器，此查询返回以下文本：

```xml
<ServerName.AdventureWorks2012.Person.Person LastName="Achong" />
```

&nbsp;

**避免歧义**：&nbsp; 通过指定如下别名，可以避免这种潜在歧义：

```sql
SELECT TOP 1 LastName
  FROM ServerName.AdventureWorks2012.Person.Person x
  FOR XML AUTO;
```

现在，无法消除歧义的查询返回以下文本：

```xml
<x LastName="Achong"/>
```

## <a name="see-also"></a>另请参阅

[FOR XML 子句的基本语法](../../relational-databases/xml/basic-syntax-of-the-for-xml-clause.md)  
[将 RAW 模式与 FOR XML 一起使用](../../relational-databases/xml/use-raw-mode-with-for-xml.md)  
[将 AUTO 模式与 FOR XML 一起使用](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
[将 EXPLICIT 模式与 FOR XML 一起使用](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
[将 PATH 模式与 FOR XML 一起使用](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
[OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
[使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)
