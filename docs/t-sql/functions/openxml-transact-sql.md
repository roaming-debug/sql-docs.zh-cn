---
description: OPENXML (Transact-SQL)
title: OPENXML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPENXML_TSQL
- OPENXML
dev_langs:
- TSQL
helpviewer_keywords:
- OPENXML statement
- rowsets [SQL Server], XML documents
- XML [SQL Server], rowset views
ms.assetid: 8088b114-7d01-435a-8e0d-b81abacc86d6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: ee78ea030b50ee6ab90c5df616f2bd49f163ded2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91115893"
---
# <a name="openxml-transact-sql"></a>OPENXML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  OPENXML 通过 XML 文档提供行集视图。 由于 OPENXML 是行集提供程序，因此可在会出现行集提供程序（如表、视图或 OPENROWSET 函数）的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用 OPENXML。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OPENXML( idoc int [ in] , rowpattern nvarchar [ in ] , [ flags byte [ in ] ] )   
[ WITH ( SchemaDeclaration | TableName ) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *idoc*  
 XML 文档的内部表式形式的文档句柄。 通过调用 **sp_xml_preparedocument** 创建 XML 文档的内部表式形式。  
  
 *rowpattern*  
 XPath 模式，用于标识要作为行处理的节点。 节点来自在 idoc 参数中传递句柄的 XML 文档。
  
 *flag*  
 指明在 XML 数据和关系行集间使用的映射，以及如何填充溢出列。 flag 为可选输入参数，可以是下列值之一。  
  
|字节值|描述|  
|----------------|-----------------|  
|**0**|默认为“以属性为中心”的映射。|  
|**1**|使用“以属性为中心”的映射。 可以与 XML_ELEMENTS 一起使用。 在这种情况下，“以属性为中心”映射先应用。 接下来，对任何剩余列应用“以元素为中心”映射。|  
|**2**|使用“以元素为中心”的映射。 可以与 XML_ATTRIBUTES 一起使用。 在这种情况下，“以属性为中心”映射先应用。 接下来，对任何剩余列应用“以元素为中心”映射。|  
|**8**|可与 XML_ATTRIBUTES 或 XML_ELEMENTS 组合使用（逻辑或）。 在检索上下文中，此标志指明不得将已用数据复制到溢出属性 \@mp:xmltext。|  
  
 _SchemaDeclaration_  
 是窗体的架构定义：_ColName_*ColType* [_ColPattern_ | _MetaProperty_] [**,**_ColNameColType_ [_ColPattern_ | _MetaProperty_]...]  
  
 _ColName_  
 行集中的列名。  
  
 *ColType*  
 行集中列的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 如果列类型不同于属性的基础 xml 数据类型，则将发生类型强制。  
  
 *ColPattern*  
 可选的通用 XPath 模式，它说明应如何将 XML 节点映射到列。 如果未指定 ColPattern，发生的是默认映射（由 flags 指定的“以属性为中心”或“以元素为中心”映射）。  
  
 指定为 ColPattern 的 XPath 模式用于，指定重写或增强由 flags 指明的默认映射的映射的特殊性质（对于“以属性为中心”和“以元素为中心”映射）。  
  
 指定为 ColPattern 的通用 XPath 模式也支持元属性。  
  
 *MetaProperty*  
 由 OPENXML 提供的元属性之一。 如果指定 MetaProperty，则该列包含元属性提供的信息。 使用元属性可以提取有关 XML 节点的信息（如相对位置和命名空间信息）。 这些元属性提供了比文本表示形式更详细的信息。  
  
 *TableName*  
 如果具有所需架构的表已经存在且不要求列模式，则为给定的表名（而不是 SchemaDeclaration）。  
  
## <a name="remarks"></a>注解  
 通过使用 SchemaDeclaration 或指定一个现有 TableName，WITH 子句提供一种行集格式（根据需要还可提供其他映射信息）。 如果未指定可选的 WITH 子句，结果以 edge 表格式返回。 边缘表在单个表中表示 XML 文档的细密结构（例如，元素/属性名、文档层次结构、命名空间、处理说明等）。  
  
 下表介绍了边缘表的结构。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**bigint**|文档节点的唯一 ID。<br /><br /> 根元素具有的 ID 值为 0。 保留负的 ID 值。|  
|**parentid**|**bigint**|标识节点的父节点。 此 ID 标识的父节点不一定是父元素，而是取决于此 ID 所标识节点的子节点的 NodeType。 例如，如果节点是文本节点，则其父节点可能是属性节点。<br /><br />  如果节点位于 XML 文档的顶层，则其 ParentID 为 NULL。|  
|**nodetype**|**int**|标识节点类型。 一个对应于 XML DOM 节点类型编号的整数。<br /><br /> 节点类型包括：<br /><br /> 1 = 元素节点<br /><br /> 2 = 属性节点<br /><br /> 3 = 文本节点|  
|**localname**|**nvarchar**|给出元素或属性的本地名称。 如果 DOM 对象没有名称，则为 NULL。|  
|**prefix**|**nvarchar**|节点名称的命名空间前缀。|  
|**namespaceuri**|**nvarchar**|节点的命名空间 URI。 如果值为 NULL，则命名空间不存在。|  
|**datatype**|**nvarchar**|元素或属性行的实际数据类型，否则为 NULL。 数据类型是从内联 DTD 中或从内联架构中推断得出。|  
|**prev**|**bigint**|前一个同级元素的 XML ID。 如果前面没有同级元素，则为 NULL。|  
|**text**|**ntext**|包含文本窗体中的属性值或元素内容（或者，如果 edge 表项不需要值，则为 NULL）。|  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-a-simple-select-statement-with-openxml"></a>A. 使用带 OPENXML 的简单 SELECT 语句  
 以下示例使用 `sp_xml_preparedocument` 创建 XML 图像的内部表示形式。 然后对 XML 文档的内部表示形式执行使用 `SELECT` 行集提供程序的 `OPENXML` 语句。  
  
 flag 值设为 `1`。 此值表示“以属性为中心”映射。 因此，XML 属性映射到行集中的列。 指定为 `/ROOT/Customer` 的 rowpattern 标识要处理的 `<Customers>` 节点。  
  
 未指定可选的 ColPattern（列模式）参数，因为列名与 XML 属性名匹配。  
  
 `OPENXML` 行集提供程序创建了一个双列行集（`CustomerID` 和 `ContactName`），`SELECT` 语句从该行集中检索必要的列（在本例中检索所有的列）。  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;  
-- Execute a SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customer',1)  
            WITH (CustomerID  VARCHAR(10),  
                  ContactName VARCHAR(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 如果将 flags 设置为 `2`（表示“以元素为中心”映射），并执行相同的 `SELECT` 语句，那么 XML 文档中针对两个客户的 `CustomerID` 和 `ContactName` 的值都返回为 NULL，因为 XML 文档中没有任何元素名为 `CustomerID` 或 `ContactName`。  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName  
---------- -----------  
NULL       NULL  
NULL       NULL  
```  
  
### <a name="b-specifying-colpattern-for-mapping-between-columns-and-the-xml-attributes"></a>B. 为列和 XML 属性之间的映射指定 ColPattern  
 下面的查询从 XML 文档返回客户 ID、订单日期、产品 ID 和数量属性。 rowpattern 标识 `<OrderDetails>` 元素。 `ProductID` 和 `Quantity` 是 `<OrderDetails>` 元素的属性。 而 `OrderID`、`CustomerID` 和 `OrderDate` 是父元素 (`<Orders>`) 的属性。  
  
 下面的映射指定了可选 ColPattern：  
  
-   行集中的 `OrderID`、`CustomerID` 和 `OrderDate` 映射到 XML 文档中的 rowpattern 所标识节点的父节点属性。  
  
-   行集中的 `ProdID` 列映射到 `ProductID` 属性，行集中的 `Qty` 列映射到 rowpattern 中所标识节点的 `Quantity` 属性。  
  
 尽管“以元素为中心”的映射由 flag 参数指定，但 ColPattern 中指定的映射的优先级高于该映射。  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">v  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';   
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT stmt using OPENXML rowset provider  
SELECT *  
FROM   OPENXML (@idoc, '/ROOT/Customer/Order/OrderDetail',2)   
         WITH (OrderID       int         '../@OrderID',
               CustomerID  varchar(10) '../@CustomerID',
               OrderDate   datetime    '../@OrderDate',
               ProdID      int         '@ProductID',
               Qty         int         '@Quantity');
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
OrderID CustomerID           OrderDate                 ProdID    Qty  
------------------------------------------------------------------------  
10248      VINET       1996-07-04 00:00:00.000   11      12  
10248      VINET       1996-07-04 00:00:00.000   42      10  
10283      LILAS       1996-08-16 00:00:00.000   72      3  
```  
  
### <a name="c-obtaining-results-in-an-edge-table-format"></a>C. 获得边缘表格式的结果  
 以下示例中的示例 XML 文档由 `<Customers>`、`<Orders>` 和 `<Order_0020_Details>` 元素组成。 首先调用 sp_xml_preparedocument 以获得文档句柄。 此文档句柄传递给 `OPENXML`。  
  
 在 `OPENXML` 语句中，rowpattern (`/ROOT/Customers`) 标识要处理的 `<Customers>` 节点。 由于未提供 WITH 子句，因此 `OPENXML` 以 edge 表格式返回行集。  
  
 最后，`SELECT` 语句检索“边缘”表中的所有列。  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customers CustomerID="VINET" ContactName="Paul Henriot">  
   <Orders CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <Order_x0020_Details OrderID="10248" ProductID="11" Quantity="12"/>  
      <Order_x0020_Details OrderID="10248" ProductID="42" Quantity="10"/>  
   </Orders>  
</Customers>  
<Customers CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Orders CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <Order_x0020_Details OrderID="10283" ProductID="72" Quantity="3"/>  
   </Orders>  
</Customers>  
</ROOT>';  
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customers')   
EXEC sp_xml_removedocument @idoc;   
```  
  
## <a name="see-also"></a>另请参阅  
 [示例：使用 OPENXML](../../relational-databases/xml/examples-using-openxml.md)  
  
