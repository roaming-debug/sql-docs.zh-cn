---
title: 示例：使用 OPENXML | Microsoft Docs
description: 查看有关如何使用 OPENXML 创建 XML 文档的行集视图的示例。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ColPattern [XML in SQL Server]
- XML [SQL Server], mapping data
- OPENXML statement, about OPENXML statement
- overflow in XML document [SQL Server]
- mapping XML data [SQL Server]
- combining attribute-centric and element centric mapping
- unconsumed data
- attribute-centric mapping
- column patterns [XML in SQL Server]
- XML [SQL Server], overflow handling
- row patterns [XML in SQL Server]
- rowpattern [XML in SQL Server]
- flags parameter
- element-centric mapping [SQL Server]
- edge tables
ms.assetid: 689297f3-adb0-4d8d-bf62-cfda26210164
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 560e3b6ff5b9beeed4dcf93f831a51b941b9d74e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126310"
---
# <a name="examples-using-openxml"></a>示例：使用 OPENXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题中的示例说明如何使用 OPENXML 创建 XML 文档的行集视图。 有关 OPENXML 语法的信息，请参阅 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)。 这些示例说明了 OPENXML 的各个方面，但不包括在 OPENXML 中指定元属性。 有关如何在 OPENXML 中指定元属性的详细信息，请参阅 [在 OPENXML 中指定元属性](../../relational-databases/xml/specify-metaproperties-in-openxml.md)。  
  
## <a name="examples"></a>示例  
 在检索数据时， *rowpattern* 可用于在确定行的 XML 文档中标识节点。 此外， *rowpattern* 是用实现 MSXML XPath 所采用的 XPath 模式语言表示的。 例如，如果模式以元素或属性结束，则为 *rowpattern* 选择的每个元素或属性节点创建一行。  
  
 *flags* 值提供默认映射。 如果 *SchemaDeclaration* 中没有指定 *ColPattern*，则假定使用 *flags* 所指定的映射。 如果在 *SchemaDeclaration* 中指定了 *ColPattern* ，则忽略 *flags* 值。 指定的 *ColPattern* 决定了映射是以属性为中心还是以元素为中心，还决定了在处理溢出数据和未用完数据时的行为。  
  
### <a name="a-executing-a-simple-select-statement-with-openxml"></a>A. 使用 OPENXML 执行简单的 SELECT 语句  
 此示例中的 XML 文档由 <`Customer`>、<`Order`> 和 <`OrderDetail`> 元素组成。 OPENXML 语句从 XML 文档中检索两列行集（ **CustomerID** 和 **ContactName**）中的客户信息。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   *rowpattern* (/ROOT/Customer) 标识要处理的 <`Customer`> 节点。  
  
-   *flags* 参数值设置为 **1** ，表示以属性为中心的映射。 因此，XML 属性映射到 *SchemaDeclaration* 中所定义的行集中的列。  
  
-   在 WITH 子句的 *SchemaDeclaration* 中所指定的 *ColName* 值与相应的 XML 属性名称相匹配。 因此，在 *SchemaDeclaration* 中不指定 *ColPattern* 参数。  
  
 SELECT 语句随后将检索 OPENXML 所提供的行集中的所有列。  
  
```  
DECLARE @DocHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @DocHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@DocHandle, '/ROOT/Customer',1)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @DocHandle  
```  
  
 结果如下：  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 由于 <`Customer`> 元素没有任何子元素，因而如果在 *flags* 设置为 **2** 时（表示以元素为中心的映射）执行上述 SELECT 语句，则两个客户的 **CustomerID** 和 **ContactName** 值将返回 NULL 值。  
  
 \@xmlDocument 也可以是 xml 类型或 (n)varchar(max) 类型。  
  
 如果 XML 文档中的 <`CustomerID`> 和 <`ContactName`> 是子元素，则以元素为中心的映射将检索值。  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer>  
   <CustomerID>VINET</CustomerID>  
   <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer>     
   <CustomerID>LILAS</CustomerID>  
   <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT    *  
FROM      OPENXML (@XmlDocumentHandle, '/ROOT/Customer',2)  
           WITH (CustomerID  varchar(10),  
                 ContactName varchar(20))  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 结果如下：  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 请注意， **sp_xml_preparedocument** 返回的文档句柄在批处理持续时间（而非会话持续时间）内有效。  
  
### <a name="b-specifying-colpattern-for-mapping-between-rowset-columns-and-the-xml-attributes-and-elements"></a>B. 为行集列与 XML 属性及元素之间的映射指定 ColPattern  
 此示例说明如何在可选的 *ColPattern* 参数中指定 XPath 模式，以提供行集列和 XML 属性以及元素之间的映射。  
  
 此示例中的 XML 文档由 <`Customer`>、<`Order`> 和 <`OrderDetail`> 元素组成。 OPENXML 语句从该 XML 文档中检索客户和订单信息作为行集（**CustomerID**、 **OrderDate**、 **ProdID** 和 **Qty**）。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   *rowpattern* (/ROOT/Customer/Order/OrderDetail) 标识要处理的 <`OrderDetail`> 节点。  
  
 为了举例说明，将 *flags* 参数值设置为 **2** ，表示以元素为中心的映射。 但是， *ColPattern* 中指定的映射覆盖了此映射。 即 *ColPattern* 中指定的 XPath 模式将行集中的列映射到属性。 这将产生以属性为中心的映射。  
  
 在 WITH 子句内的 *SchemaDeclaration* 中，也可以用 *ColName* 和 *ColType* 参数指定 *ColPattern* 。 可选的 *ColPattern* 是指定的 XPath 模式，表示以下内容：  
  
-   行集中的 **OrderID**、 **CustomerID** 和 **OrderDate** 列映射到 *rowpattern* 所标识节点的父节点的属性，同时，*rowpattern* 还标识 <`OrderDetail`> 节点。 因此，**CustomerID** 和 **OrderDate** 列映射到 <`Order`> 元素的 **CustomerID** 和 **OrderDate** 属性。  
  
-   行集中的 **ProdID** 和 **Qty** 列映射到 **rowpattern** 所标识节点的 **ProductID** 和 *Quantity* 属性。  
  
 SELECT 语句随后将检索 OPENXML 所提供的行集中的所有列。  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT stmt using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@XmlDocumentHandle, '/ROOT/Customer/Order/OrderDetail',2)  
WITH (OrderID     int         '../@OrderID',  
      CustomerID  varchar(10) '../@CustomerID',  
      OrderDate   datetime    '../@OrderDate',  
      ProdID      int         '@ProductID',  
      Qty         int         '@Quantity')  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 结果如下：  
  
```  
OrderID CustomerID        OrderDate          ProdID    Qty  
-------------------------------------------------------------  
10248    VINET     1996-07-04 00:00:00.000     11       12  
10248    VINET     1996-07-04 00:00:00.000     42       10  
10283    LILAS     1996-08-16 00:00:00.000     72        3  
```  
  
 也可以对已指定为 *ColPattern* 的 XPath 模式重新指定，以将 XML 元素映射到行集列。 这将产生以元素为中心的映射。 在下例中，XML 文档 <`CustomerID`> 和 <`OrderDate`> 是 <`Orders`> 元素的子元素。 由于 *ColPattern* 覆盖 *flags* 参数中所指定的映射，所以在 OPENXML 中没有指定 *flags* 参数。  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order EmployeeID="5" >  
      <OrderID>10248</OrderID>  
      <CustomerID>VINET</CustomerID>  
      <OrderDate>1996-07-04T00:00:00</OrderDate>  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order  EmployeeID="3" >  
      <OrderID>10283</OrderID>  
      <CustomerID>LILAS</CustomerID>  
      <OrderDate>1996-08-16T00:00:00</OrderDate>  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail')  
WITH (CustomerID  varchar(10)   '../CustomerID',  
      OrderDate   datetime      '../OrderDate',  
      ProdID      int           '@ProductID',  
      Qty         int           '@Quantity')  
EXEC sp_xml_removedocument @docHandle  
```  
  
### <a name="c-combining-attribute-centric-and-element-centric-mapping"></a>C. 组合以属性为中心的映射和以元素为中心的映射  
 在此示例中， *flags* 参数设置为 **3** ，表示将应用以属性为中心的映射和以元素为中心的映射。 在这种情况下，首先应用以属性为中心的映射，然后对所有未处理的列应用以元素为中心的映射。  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument =N'<ROOT>  
<Customer CustomerID="VINET"  >  
     <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" >   
     <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer',3)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @docHandle  
```  
  
 下面是查询结果。  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 以属性为中心的映射应用于 **CustomerID**。 在 <`Customer`> 元素中不存在 **ContactName** 属性。 因此，应用以元素为中心的映射。  
  
### <a name="d-specifying-the-text-xpath-function-as-colpattern"></a>D. 指定 text() XPath 函数作为 ColPattern  
 此示例中的 XML 文档由 <`Customer`> 和 <`Order`> 元素组成。 OPENXML 语句从 <`Order`> 元素、*rowpattern* 所标识节点的父节点 ID 和元素内容的叶值字符串中检索由 **oid** 属性组成的行集。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   *rowpattern* (/root/Customer/Order) 标识要处理的 <`Order`> 节点。  
  
-   *flags* 参数值设置为 **1** ，表示以属性为中心的映射。 因此，XML 属性映射到 *SchemaDeclaration* 中定义的行集列。  
  
-   在 WITH 子句的 *SchemaDeclaration* 中， **oid** 和 **amount** 行集列名与相应的 XML 属性名称相匹配。 因此，没有指定 *ColPattern* 参数。 对于行集中的 **comment** 列，XPath 函数 **text()** 将被指定为 *ColPattern*。 这将覆盖在 *flags* 参数中指定的以属性为中心的映射，而且列将包含元素内容的叶值字符串。  
  
 SELECT 语句随后将检索 OPENXML 所提供的行集中的所有列。  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied  
      </Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
            <Urgency>Important</Urgency>  
            Happy Customer.  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH (oid     char(5),   
           amount  float,   
           comment ntext 'text()')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 结果如下：  
  
```  
oid   amount        comment  
----- -----------   -----------------------------  
O1    3.5           NULL  
O2    13.4          Customer was very satisfied  
O3    100.0         Happy Customer.  
O4    10000.0       NULL  
```  
  
### <a name="e-specifying-tablename-in-the-with-clause"></a>E. 在 WITH 子句中指定 TableName  
 此示例在 WITH 子句中指定 *TableName* ，而不指定 *SchemaDeclaration*。 当表具有想要的结构而不具备列模式（ *ColPattern* 参数）时，这非常有用。  
  
 此示例中的 XML 文档由 <`Customer`> 和 <`Order`> 元素组成。 OPENXML 语句从 XML 文档中检索三列行集（**oid**、 **date** 和 **amount**）中的订单信息。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   *rowpattern* (/root/Customer/Order) 标识要处理的 <`Order`> 节点。  
  
-   在 WITH 子句中没有 *SchemaDeclaration* 。 而是指定了一个表名。 因此，表架构将用作行集架构。  
  
-   *flags* 参数值设置为 **1** ，表示以属性为中心的映射。 因此， *rowpattern* 所标识的元素属性将映射到同名的行集列。  
  
 SELECT 语句随后将检索 OPENXML 所提供的行集中的所有列。  
  
```  
-- Create a test table. This table schema is used by OPENXML as the  
-- rowset schema.  
CREATE TABLE T1(oid char(5), date datetime, amount float)  
GO  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
-- Sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH T1  
EXEC sp_xml_removedocument @docHandle  
```  
  
 结果如下：  
  
```  
oid   date                        amount  
----- --------------------------- ----------  
O1    1996-01-20 00:00:00.000     3.5  
O2    1997-04-30 00:00:00.000     13.4  
O3    1999-07-14 00:00:00.000     100.0  
O4    1996-01-20 00:00:00.000     10000.0  
```  
  
### <a name="f-obtaining-the-result-in-an-edge-table-format"></a>F. 获得边缘表格式的结果  
 在下例中，在 OPENXML 语句中未指定 WITH 子句。 因此，OPENXML 所生成的行集具有边缘表格式。 SELECT 语句将返回边缘表中的所有列。  
  
 示例中的示例 XML 文档由 <`Customer`>、<`Order`> 和 <`OrderDetail`> 元素组成。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   *rowpattern* (/ROOT/Customer) 标识要处理的 <`Customer`> 节点。  
  
-   不带 WITH 子句。 因此，OPENXML 将以边缘表格式返回行集。  
  
 继而，SELECT 语句将检索边缘表中的所有列。  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
SET @xmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer')  
  
EXEC sp_xml_removedocument @docHandle  
```  
  
 结果作为边缘表返回。 您可以对边缘表编写查询以获得信息。 例如：  
  
-   下面的查询返回文档中的 **Customer** 节点数。 因为未指定 WITH 子句，所以 OPENXML 将返回边缘表。 SELECT 语句将查询此边缘表。  
  
    ```  
    SELECT count(*)  
    FROM OPENXML(@docHandle, '/')  
    WHERE localname = 'Customer'  
    ```  
  
-   以下查询返回元素类型的 XML 节点的本地名称。  
  
    ```  
    SELECT distinct localname   
    FROM OPENXML(@docHandle, '/')   
    WHERE nodetype = 1   
    ORDER BY localname  
    ```  
  
### <a name="g-specifying-rowpattern-ending-with-an-attribute"></a>G. 指定以属性结束的 rowpattern  
 此示例中的 XML 文档由 <`Customer`>、<`Order`> 和 <`OrderDetail`> 元素组成。 OPENXML 语句从 XML 文档中检索三列行集（**ProductID**、 **Quantity** 和 **OrderID**）中的订单详细信息。  
  
 首先调用 **sp_xml_preparedocument** 存储过程以获得文档句柄。 此文档句柄传递给 OPENXML。  
  
 OPENXML 语句说明了以下信息：  
  
-   rowpattern (/ROOT/Customer/Order/OrderDetail/\@ProductID) 以 XML 属性 ProductID 结尾。 在所得到的行集中，为在 XML 文档中选定的每个属性节点都创建一行。  
  
-   在下例中未指定 *flags* 参数。 相反，由 *ColPattern* 参数指定映射。  
  
 在 WITH 子句的 *SchemaDeclaration* 中，还可以用 *ColName* 和 *ColType* 参数指定 *ColPattern* 。 可选的 *ColPattern* 是指定的 XPath 模式，用以表示以下内容：  
  
-   在行集中为 **ProdID** 列指定为 *ColPattern* 的 XPath 模式 ( **.** ) 将标识上下文节点（当前节点）。 按照指定的 *rowpattern*，它是 <`OrderDetail`> 元素的 **ProductID** 属性。  
  
-   为行集中的 Qty 列指定的 ColPattern（即 ../\@Quantity）标识上下文节点 \<ProductID> 的父节点 <`OrderDetail`> 的 Quantity 属性。  
  
-   同样，为行集中的 OID 列指定的 ColPattern（即 ../../\@OrderID）标识上下文节点的父节点的父级 <`Order`> 的 OrderID 属性。 该父节点是 <`OrderDetail`>，上下文节点是 <`ProductID`>。  
  
 SELECT 语句随后将检索 OPENXML 所提供的行集中的所有列。  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--Sample XML document  
SET @xmlDocument =N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail/@ProductID')  
       WITH ( ProdID  int '.',  
              Qty     int '../@Quantity',  
              OID     int '../../@OrderID')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 结果如下：  
  
```  
ProdID      Qty         OID  
----------- ----------- -------   
11          12          10248  
42          10          10248  
72          3           10283  
```  
  
### <a name="h-specifying-an-xml-document-that-has-multiple-text-nodes"></a>H. 指定具有多个文本节点的 XML 文档  
 如果在 XML 文档中有多个文本节点，则包含 *ColPattern*, **text()** 的 SELECT 语句只返回第一个文本节点，而不是返回所有节点。 例如：  
  
```  
DECLARE @h int  
EXEC sp_xml_preparedocument @h OUTPUT,  
         N'<root xmlns:a="urn:1">  
           <a:Elem abar="asdf">  
             T<a>a</a>U  
           </a:Elem>  
         </root>',  
         '<ns xmlns:b="urn:1" />'  
  
SELECT * FROM openxml(@h, '/root/b:Elem')  
      WITH (Col1 varchar(20) 'text()')  
EXEC sp_xml_removedocument @h  
```  
  
 SELECT 语句返回 **T** 作为结果，而不是返回 **TaU**。  
  
### <a name="i-specifying-the-xml-data-type-in-the-with-clause"></a>I. 在 WITH 子句中指定 xml 数据类型  
 在 WITH 子句中，映射到 **xml** 数据类型列的列模式（不论是类型化的还是非类型化的）必须返回一个空序列，或者元素、处理指令、文本节点和注释的序列。 数据将转换为 **xml** 数据类型。  
  
 在下例中，WITH 子句中的表架构声明包括 **xml** 类型列。  
  
```  
DECLARE @h int  
DECLARE @x xml  
set @x = '<Root>  
  <row id="1"><lname>Duffy</lname>  
   <Address>  
            <Street>111 Maple</Street>  
            <City>Seattle</City>  
   </Address>  
  </row>  
  <row id="2"><lname>Wang</lname>  
   <Address>  
            <Street>222 Pine</Street>  
            <City>Bothell</City>  
   </Address>  
  </row>  
</Root>'  
  
EXEC sp_xml_preparedocument @h output, @x  
SELECT *  
FROM   OPENXML (@h, '/Root/row', 10)  
      WITH (id int '@id',  
  
            lname    varchar(30),  
            xmlname  xml 'lname',  
            OverFlow xml '@mp:xmltext')  
EXEC sp_xml_removedocument @h  
```  
  
 具体而言，是将 xml 类型变量 (\@x) 传递给 sp_xml_preparedocument() 函数。  
  
 结果如下：  
  
```  
id  lname   xmlname                   OverFlow  
--- ------- ------------------------------ -------------------------------  
1   Duffy   <lname>Duffy</lname>  <row><Address>  
                                   <Street>111 Maple</Street>  
                                   <City>Seattle</City>  
                                  </Address></row>  
2   Wang    <lname>Wang</lname>   <row><Address>  
                                    <Street>222 Pine</Street>  
                                    <City>Bothell</City>  
                                   </Address></row>  
```  
  
 请注意结果中的以下内容：  
  
-   对于 **varchar(30)** 类型的 **lname** 列，从对应的 <`lname`> 元素检索它的值。  
  
-   对于 **xml** 类型的 **xmlname** 列，返回相同的名称元素作为它的值。  
  
-   标志设置为 10。 10 表示 2 + 8，其中 2 表示以元素为中心的映射，8 表示应该仅将未用完的 XML 数据添加到 WITH 子句中定义的 OverFlow 列。 如果将标志设置为 2，则整个 XML 文档将复制到 WITH 子句中指定的 OverFlow 列。  
  
-   如果 WITH 子句中的列是类型化的 XML 列并且 XML 实例不符合架构，将返回错误。  
  
### <a name="j-retrieving-individual-values-from-multivalued-attributes"></a>J. 从多值属性中检索单值  
 XML 文档会含有多值属性。 例如， **IDREFS** 属性可以是多值属性。 在 XML 文档内，多值属性值被指定为一个字符串，并用空格分隔值。 在以下 XML 文档中，\<Student> 元素的 attends 属性与 \<Class> 的 attendedBy 属性都是多值属性。 从多值 XML 属性中检索单值并将每个值存储到数据库中的不同行中，这要求额外的工作。 下例显示了此过程。  
  
 此示例 XML 文档由以下元素组成：  
  
-   \<Student>  
  
     **id** （学生 ID）、 **name** 和 **attends** 属性。 **attends** 属性是多值属性。  
  
-   \<Class>  
  
     **id** （班级 ID）、 **name** 和 **attendedBy** 属性。 **attendedBy** 属性是多值属性。  
  
 \<Student> 中的 attends 属性和 \<Class> 中的 attendedBy 属性表示 Student 表与 Class 表之间的 m:n 关系。 一个学生可在很多班上课，而一个班也可有很多学生。  
  
 假设希望拆分此文档，并将它保存到下列数据库中：  
  
-   将 \<Student> 数据保存到 Students 表中。  
  
-   将 \<Class> 数据保存到 Courses 表中。  
  
-   将 Student 与 Class 之间的 **m:n** 关系数据保存到 CourseAttendence 表中。 提取这些值需要做额外的工作。 若要检索该信息并将其存储到表中，请使用下列存储过程：  
  
    -   **Insert_Idrefs_Values**  
  
         将课程 ID 和学生 ID 的值插入 CourseAttendence 表中。  
  
    -   **Extract_idrefs_values**  
  
         从每个 \<Course> 元素中提取单个学生 ID。 用边缘表检索这些值。  
  
 以下是具体步骤：  
  
```  
-- Create these tables:  
DROP TABLE CourseAttendance  
DROP TABLE Students  
DROP TABLE Courses  
GO  
CREATE TABLE Students(  
                id   varchar(5) primary key,  
                name varchar(30)  
                )  
GO  
CREATE TABLE Courses(  
               id       varchar(5) primary key,  
               name     varchar(30),  
               taughtBy varchar(5)  
)  
GO  
CREATE TABLE CourseAttendance(  
             id         varchar(5) references Courses(id),  
             attendedBy varchar(5) references Students(id),  
             constraint CourseAttendance_PK primary key (id, attendedBy)  
)  
go  
-- Create these stored procedures:  
DROP PROCEDURE f_idrefs  
GO  
CREATE PROCEDURE f_idrefs  
    @t      varchar(500),  
    @idtab  varchar(50),  
    @id     varchar(5)  
AS  
DECLARE @sp int  
DECLARE @att varchar(5)  
SET @sp = 0  
WHILE (LEN(@t) > 0)  
BEGIN   
    SET @sp = CHARINDEX(' ', @t+ ' ')  
    SET @att = LEFT(@t, @sp-1)  
    EXEC('INSERT INTO '+@idtab+' VALUES ('''+@id+''', '''+@att+''')')  
    SET @t = SUBSTRING(@t+ ' ', @sp+1, LEN(@t)+1-@sp)  
END  
Go  
  
DROP PROCEDURE fill_idrefs  
GO  
CREATE PROCEDURE fill_idrefs   
    @xmldoc     int,  
    @xpath      varchar(100),  
    @from       varchar(50),  
    @to         varchar(50),  
    @idtable    varchar(100)  
AS  
DECLARE @t varchar(500)  
DECLARE @id varchar(5)  
  
/* Temporary Edge table */  
SELECT *   
INTO #TempEdge   
FROM OPENXML(@xmldoc, @xpath)  
  
DECLARE fillidrefs_cursor CURSOR FOR  
    SELECT CAST(iv.text AS nvarchar(200)) AS id,  
           CAST(av.text AS nvarchar(4000)) AS refs  
    FROM   #TempEdge c, #TempEdge i,  
           #TempEdge iv, #TempEdge a, #TempEdge av  
    WHERE  c.id = i.parentid  
    AND    UPPER(i.localname) = UPPER(@from)  
    AND    i.id = iv.parentid  
    AND    c.id = a.parentid  
    AND    UPPER(a.localname) = UPPER(@to)  
    AND    a.id = av.parentid  
  
OPEN fillidrefs_cursor  
FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    IF (@@FETCH_STATUS <> -2)  
    BEGIN  
        execute f_idrefs @t, @idtable, @id  
    END  
    FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
END  
CLOSE fillidrefs_cursor  
DEALLOCATE fillidrefs_cursor  
Go  
-- This is the sample document that is shredded and the data is stored in the preceding tables.  
DECLARE @h int  
EXECUTE sp_xml_preparedocument @h OUTPUT, N'<Data>  
  <Student id = "s1" name = "Student1"  attends = "c1 c3 c6"  />  
  <Student id = "s2" name = "Student2"  attends = "c2 c4" />  
  <Student id = "s3" name = "Student3"  attends = "c2 c4 c6" />  
  <Student id = "s4" name = "Student4"  attends = "c1 c3 c5" />  
  <Student id = "s5" name = "Student5"  attends = "c1 c3 c5 c6" />  
  <Student id = "s6" name = "Student6" />  
  
  <Class id = "c1" name = "Intro to Programming"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c2" name = "Databases"   
         attendedBy = "s2 s3" />  
  <Class id = "c3" name = "Operating Systems"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c4" name = "Networks" attendedBy = "s2 s3" />  
  <Class id = "c5" name = "Algorithms and Graphs"   
         attendedBy =  "s4 s5"/>  
  <Class id = "c6" name = "Power and Pragmatism"   
         attendedBy = "s1 s3 s5" />  
</Data>'  
  
INSERT INTO Students SELECT * FROM OPENXML(@h, '//Student') WITH Students  
  
INSERT INTO Courses SELECT * FROM OPENXML(@h, '//Class') WITH Courses  
/* Using the edge table */  
EXECUTE fill_idrefs @h, '//Class', 'id', 'attendedby', 'CourseAttendance'  
  
SELECT * FROM Students  
SELECT * FROM Courses  
SELECT * FROM CourseAttendance  
  
EXECUTE sp_xml_removedocument @h  
```  
  
### <a name="k-retrieving-binary-from-base64-encoded-data-in-xml"></a>K. 从 XML 中的 base64 编码数据中检索二进制数据  
 二进制数据经常包括在使用 base64 编码的 XML 中。 当使用 OPENXML 拆分此 XML 时，将接收到 base64 编码数据。 本示例说明如何将 base64 编码数据转换回二进制。  
  
-   创建包含示例二进制数据的表。  
  
-   使用 FOR XML 查询和 BINARY BASE64 选项来构造将二进制数据按照 base64 进行编码的 XML。  
  
-   使用 OPENXML 拆分 XML。 OPENXML 返回的数据将是 base64 编码的数据。 接下来，调用 .value 函数将其转换回二进制。  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 varbinary(100))  
go  
-- Insert sample binary data  
INSERT T VALUES(1, 0x1234567890)   
go  
 -- Create test XML document that has base64 encoded binary data (use FOR XML query and specify BINARY BASE64 option)  
SELECT * FROM T  
FOR XML AUTO, BINARY BASE64  
go  
-- result  
-- <T Col1="1" Col2="EjRWeJA="/>  
  
-- Now shredd the sample XML using OPENXML.   
-- Call the .value function to convert   
-- the base64 encoded data returned by OPENXML to binary.  
DECLARE @h int ;  
EXEC sp_xml_preparedocument @h OUTPUT, '<T Col1="1" Col2="EjRWeJA="/>' ;  
SELECT Col1,   
CAST('<binary>' + Col2 + '</binary>' AS XML).value('.', 'varbinary(max)') AS BinaryCol   
FROM openxml(@h, '/T')   
WITH (Col1 integer, Col2 varchar(max)) ;  
EXEC sp_xml_removedocument @h ;  
GO  
```  
  
 结果如下： 返回的二进制数据是表 T 的原始二进制数据。  
  
```  
Col1        BinaryCol  
----------- ---------------------  
1           0x1234567890  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_xml_preparedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)   
 [sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)   
 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
  
  
