---
description: nodes() 方法（xml 数据类型）
title: nodes() 方法（xml 数据类型）
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nodes() method
- nodes method
ms.assetid: 7267fe1b-2e34-4213-8bbf-1c953822446c
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 71b636999583da9a957e72c0d8f3a5e0b9fe98bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207515"
---
# <a name="nodes-method-xml-data-type"></a>nodes() 方法（xml 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如果要将 xml 数据类型实例拆分为关系数据，则 nodes() 方法非常有用 。 它允许您标识将映射到新行的节点。  
  
每个 xml 数据类型实例都有隐式提供的上下文节点。 对于在列或变量中存储的 XML 实例来说，此节点是文档节点。 文档节点是位于每个 xml 数据类型实例顶部的隐式节点。  
  
nodes() 方法的结果是一个包含原始 XML 实例的逻辑副本的行集。 在这些逻辑副本中，每个行示例的上下文节点都被设置成由查询表达式标识的节点之一。 这样，后续的查询可以浏览与这些上下文节点相关的节点。  
  
您可以从行集中检索多个值。 例如，可以将 value() 方法应用于 nodes() 所返回的行集，从原始 XML 实例中检索多个值 。 当 value() 方法应用于 XML 实例时，它仅返回一个值。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
nodes (XQuery) as Table(Column)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*XQuery*  
字符串文字，即一个 XQuery 表达式。 如果查询表达式构造节点，这些已构造的节点将在结果行集中显示。 如果查询表达式生成一个空序列，则行集也为空。 如果查询表达式静态生成一个包含原子值而不是节点的序列，将产生静态错误。  
  
Table(Column)   
结果行集的表名称和列名称。  
  
## <a name="remarks"></a>备注  
例如，假设有下表：  
  
```sql
T (ProductModelID INT, Instructions XML)  
```  
  
表中存储了以下生产说明文档。 仅显示一个片段。 注意，文档中有三个生产位置。  
  
```
<root>  
  <Location LocationID="10"...>  
     <step>...</step>  
     <step>...</step>  
      ...  
  </Location>  
  <Location LocationID="20" ...>  
       ...  
  </Location>  
  <Location LocationID="30" ...>  
       ...  
  </Location>  
</root>  
```  
  
带有查询表达式 `nodes()` 的 `/root/Location` 方法调用将返回一个行集，其中包含三行（每行都有一个原始 XML 文档的逻辑副本），并且上下文项设置为 `<Location>` 节点之一：  
  
```
Product  
ModelID      Instructions  
----------------------------------  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
```  
  
然后，可以使用 xml 数据类型方法查询此行集。 以下查询为每个生成的行提取上下文项的子树：  
  
```sql
SELECT T2.Loc.query('.')  
FROM T  
CROSS APPLY Instructions.nodes('/root/Location') AS T2(Loc)   
```  
  
下面是结果：  
  
```
ProductModelID  Instructions  
----------------------------------  
1        <Location LocationID="10" ... />  
1        <Location LocationID="20" ... />  
1        <Location LocationID="30" .../>  
```  
  
返回的行集已对类型信息进行了维护。 可以将 xml 数据类型方法（例如 query()、value()、exist() 和 nodes()）应用于 nodes() 方法的结果     。 但不能将 modify() 方法用于修改 XML 实例。  
  
另外，行集中的上下文节点无法具体化。 即，无法在 SELECT 语句中使用此节点。 但是，可以在 IS NULL 和 COUNT(*) 中使用它。  
  
使用 nodes() 方法的情况与使用 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md) 的情况相同，会提供 XML 的行集视图。 但是，在包含 XML 文档的若干行的表中使用 nodes() 方法时，无需使用游标。  
  
由 nodes() 方法返回的行集是未命名的行集。 因此，必须通过使用别名来显式命名。  
  
nodes() 函数不能直接应用于用户定义函数的结果。 若要将 nodes() 函数用于标量用户定义的函数结果，您可以执行以下任一操作：
 
- 将用户定义函数的结果分配给一个变量
- 使用派生表将列别名分配非用户定义的函数返回值，然后使用 `CROSS APPLY` 从别名中进行选择。  
  
以下示例显示了使用 `CROSS APPLY` 从用户定义函数结果中进行选择的一种方式。  
  
```sql
USE AdventureWorks;  
GO  
  
CREATE FUNCTION XTest()  
RETURNS XML  
AS  
BEGIN  
RETURN '<document/>';  
END;  
GO  
  
SELECT A2.B.query('.')  
FROM  
(SELECT dbo.XTest()) AS A1(X)   
CROSS APPLY X.nodes('.') A2(B);  
GO  
  
DROP FUNCTION XTest;  
GO  
```  
  
## <a name="examples"></a>示例  
  
### <a name="using-nodes-method-against-a-variable-of-xml-type"></a>针对 xml 类型的变量使用 nodes() 方法  
在以下示例中，XML 文档具有一个 <`Root`> 顶级元素和三个 <`row`> 子元素。 查询使用 `nodes()` 方法设置单独的上下文节点，每个 <`row`> 元素一个上下文节点。 `nodes()` 方法返回包含三行的行集。 每行都有一个原始 XML 的逻辑副本，其中每个上下文节点都标识原始文档中的一个不同的 <`row`> 元素。  
  
然后，查询会从每行返回上下文节点：  
  
```sql
DECLARE @x XML   
SET @x='<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>'  
SELECT T.c.query('.') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
在以下示例结果中，查询方法返回上下文项及其内容：  
  
```
<row id="1"><name>Larry</name><oflw>some text</oflw></row>  
<row id="2"><name>moe</name></row>  
<row id="3"/>  
```  
  
对上下文节点应用父级取值函数将返回所有三行的 <`Root`> 元素。  
  
```sql
SELECT T.c.query('..') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
下面是结果：  
  
```
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
```  
  
### <a name="specifying-the-nodes-method-against-a-column-of-xml-type"></a>针对 xml 类型的列指定 nodes() 方法  
此示例中使用了自行车生产说明，该说明存储在 ProductModel 表的 Instructions xml 类型列中 。  
  
在以下示例中，已对 `ProductModel` 表中 xml 类型的 `Instructions` 列指定 `nodes()` 方法。  
  
`nodes()` 方法通过指定 `/MI:root/MI:Location` 路径，将 <`Location`> 元素设置为上下文节点。 结果行集包括原始文档的逻辑副本，每个副本对应文档中的一个 <`Location`> 节点，上下文节点设置为 <`Location`> 元素。 因此，`nodes()` 函数提供一组 <`Location`> 上下文节点。  
  
`query()` 方法对此行集请求 `self::node`，然后在每行中返回 `<Location>` 元素。  
  
在此示例中，查询将特定产品样式的生产说明文档中将每一个 <`Location`> 元素都设置为上下文节点。 可以使用这些上下文节点来按照这些方式来检索值：  
  
- 查找每个 <`Location`> 中的位置 ID  
  
- 在每个 <`Location`> 中检索生产步骤（<`step`> 子元素）  
  
在 `'.'` 方法中，此查询返回上下文项，其中指定了 `self::node()` 的缩写语法 `query()`。  
  
注意以下事项：
  
- 将 `nodes()` 方法应用于 Instructions 列，并且返回行集 `T (C)`。 此行集包含原始生产说明文档的逻辑副本，并且以 `/root/Location` 作为上下文项。  
  
- CROSS APPLY 将 `nodes()` 应用于 `Instructions` 表中的每行，并且仅返回生成结果集的行。  
  
    ```sql  
    SELECT C.query('.') as result  
    FROM Production.ProductModel  
    CROSS APPLY Instructions.nodes('  
    declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /MI:root/MI:Location') as T(C)  
    WHERE ProductModelID=7  
    ```  
  
  下面是部分结果：  
  
    ```
    <MI:Location LocationID="10"  ...>  
       <MI:step ... />  
          ...  
    </MI:Location>  
    <MI:Location LocationID="20"  ... >  
        <MI:step ... />  
          ...  
    </MI:Location>  
    ...  
    ```  
  
### <a name="applying-nodes-to-the-rowset-returned-by-another-nodes-method"></a>将 nodes() 应用于另一 nodes() 方法返回的行集  
以下代码查询 XML 文档，以获得 `Instructions` 表的 `ProductModel` 列中的生产说明。 此查询返回包含产品样式 ID、生产位置和生产步骤的行集。  
  
注意以下事项：  
  
- 将 `nodes()` 方法应用于 `Instructions` 列，并且返回 `T1 (Locations)` 行集。 此行集包含原始生产说明文档的逻辑副本，并且 `/root/Location` 元素作为项上下文。  
  
- 将 `nodes()` 应用于 `T1 (Locations)` 行集，并且返回 `T2 (steps)` 行集。 此行集包含原始生产说明文档的逻辑副本，并且 `/root/Location/step` 元素作为项上下文。  
  
```sql
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
./MI:step ') AS T2(steps)         
WHERE ProductModelID=7         
GO         
```  
  
下面是结果：  
  
```
ProductModelID LocID Step         
----------------------------         
7      10   <step ... />         
7      10   <step ... />         
...         
7      20   <step ... />         
7      20   <step ... />         
7      20   <step ... />         
...         
```  
  
该查询声明 `MI` 前缀两次。 替代方法为，使用 `WITH XMLNAMESPACES` 声明前缀一次，便可将其用于查询：  
  
```sql
WITH XMLNAMESPACES (  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS MI)  
  
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
./MI:step ') as T2(steps)         
WHERE ProductModelID=7         
GO    
```  
  
## <a name="see-also"></a>另请参阅  
[使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
[创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
[xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)  
  
  
