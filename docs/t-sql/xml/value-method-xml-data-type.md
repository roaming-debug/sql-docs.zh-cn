---
description: value() 方法（xml 数据类型）
title: value() 方法（xml 数据类型）
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- value method
- value() method
ms.assetid: 298a7361-dc9a-4902-9b1e-49a093cd831d
author: MightyPen
ms.author: genemi
ms.openlocfilehash: dce7148340136615b79d4bf4e4ec9dd15627c88e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181728"
---
# <a name="value-method-xml-data-type"></a>value() 方法（xml 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对 XML 执行 XQuery，并返回 SQL 类型的值。 此方法将返回标量值。  
  
 通常，可以使用此方法从 xml 类型列、参数或变量内存储的 XML 实例中提取值。 这样，您就可以指定将 XML 数据与非 XML 列中的数据进行合并或比较的 SELECT 查询。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
value (XQuery, SQLType)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *XQuery*  
 从 XML 实例内部检索数据的 XQuery 表达式（一种字符串文字）。 XQuery 必须最多返回一个值。 否则，将返回错误。  
  
 SQLType  
 要返回的首选 SQL 类型（一种字符串文字）。 此方法的返回类型与 SQLType 参数匹配。 SQLType 不能为 xml 数据类型、公共语言运行时 (CLR) 用户定义类型、image、text、ntext 或 sql_variant 数据类型    。 SQLType 可以是用户定义数据类型 SQL。  
  
 value() 方法隐式使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] CONVERT 运算符并尝试将 XQuery 表达式（序列化字符串表示形式）的结果从 XSD 类型转换为由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 转换指定的相应 SQL 类型。 有关 CONVERT 的类型强制转换规则的详细信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。  
  
> [!NOTE]  
>  由于性能原因，不在谓词中使用 value() 方法与关系值进行比较，而改用具有 sql:column() 的 exist()  。 如下面的示例 D 所示。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-value-method-against-an-xml-type-variable"></a>A. 对 xml 类型的变量使用 value() 方法  
 在下面的示例中，XML 实例存储在 `xml` 类型的变量中。 `value()` 方法从 XML 中检索 `ProductID` 属性值。 然后将该值分配给 `int` 变量。  
  
```sql
DECLARE @myDoc XML  
DECLARE @ProdID INT  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
  
SET @ProdID =  @myDoc.value('(/Root/ProductDescription/@ProductID)[1]', 'int' )  
SELECT @ProdID  
```  
  
 返回值 1 作为结果。  
  
 即使 XML 实例中仅有一个 `ProductID` 属性，静态类型化规则也会要求显式指定路径表达式返回 1。 因此，在路径表达式结尾处指定了另一个 `[1]`。 有关静态类型化的详细信息，请参阅 [XQuery 与静态类型化](../../xquery/xquery-and-static-typing.md)。  
  
### <a name="b-using-the-value-method-to-retrieve-a-value-from-an-xml-type-column"></a>B. 使用 value() 方法从 xml 类型的列中检索值  
 根据 `AdventureWorks` 数据库中的 xml 类型列 (`CatalogDescription`) 指定以下查询。 查询从列中存储的每个 XML 实例中检索 `ProductModelID` 属性值。  
  
```sql
SELECT CatalogDescription.value('             
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";             
       (/PD:ProductDescription/@ProductModelID)[1]', 'int') AS Result             
FROM Production.ProductModel             
WHERE CatalogDescription IS NOT NULL             
ORDER BY Result DESC             
```  
  
 请注意上述查询的以下方面：  
  
-   `namespace` 关键字用于定义命名空间前缀。  
  
-   对于每个静态类型化要求，都在 `[1]` 方法的路径表达式结尾处添加 `value()` 以显式指示路径表达式返回 1。  
  
 下面是部分结果：  
  
```  
-----------  
35           
34           
...  
```  
  
### <a name="c-using-the-value-and-exist-methods-to-retrieve-values-from-an-xml-type-column"></a>C. 使用 value() 和 exist() 方法从 xml 类型的列中检索值  
 以下示例显示同时使用 xml 数据类型的 `value()` 方法和 [exist() 方法](../../t-sql/xml/exist-method-xml-data-type.md)。 `value()` 方法用于从 XML 中检索 `ProductModelID` 属性值。 `exist()` 子句中的 `WHERE` 方法用于从表中筛选行。  
  
 此查询将从把保修信息（<`Warranty`> 元素）作为功能之一的 XML 实例中检索产品型号 ID。 `WHERE` 子句中的条件使用 `exist()` 方法仅检索满足该条件的行。  
  
```sql
SELECT CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           (/PD:ProductDescription/@ProductModelID)[1] ', 'int') AS Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
 请注意上述查询的以下方面：  
  
-   `CatalogDescription` 列是类型化的 XML 列。 这意味着它具有与其相关的架构集合。 在 [XQuery Prolog](../../xquery/modules-and-prologs-xquery-prolog.md) 中，命名空间声明用于定义以后会在查询主体中使用的前缀。  
  
-   如果 `exist()` 方法返回 `1` (True)，则指示 XML 实例包括 <`Warranty`> 子元素作为功能之一。  
  
-   然后，`value()` 子句中的 `SELECT` 方法将 `ProductModelID` 属性值作为整数进行检索。  
  
 下面是部分结果：  
  
```  
Result       
-----------  
19           
23           
...  
```  
  
### <a name="d-using-the-exist-method-instead-of-the-value-method"></a>D. 使用 exist() 方法而不使用 value() 方法  
 由于性能原因，不在谓词中使用 `value()` 方法与关系值进行比较，而改用具有 `exist()` 的 `sql:column()`。 例如：  
  
```sql
CREATE TABLE T (c1 INT, c2 VARCHAR(10), c3 XML)  
GO  
  
SELECT c1, c2, c3   
FROM T  
WHERE c3.value( '/root[1]/@a', 'integer') = c1  
GO  
```  
  
 可以写入以下语句：  
  
```sql
SELECT c1, c2, c3   
FROM T  
WHERE c3.exist( '/root[@a=sql:column("c1")]') = 1  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
