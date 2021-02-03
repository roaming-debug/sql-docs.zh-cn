---
description: exist() 方法（xml 数据类型）
title: exist() 方法（xml 数据类型）
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- exist() method
- exist method
ms.assetid: a55b75e0-0a17-4787-a525-9b095410f7af
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b29a0143b2fca6bbcafaa94acd60e096a121480d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206889"
---
# <a name="exist-method-xml-data-type"></a>exist() 方法（xml 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回位，表示下列条件之一：  
  
-   1，表示 True（如果查询中的 XQuery 表达式返回一个非空结果）。 即，它至少返回一个 XML 节点。  
  
-   0，表示 False（如果它返回一个空结果）。  
  
-   NULL（如果执行查询的 xml 数据类型实例包含 NULL）。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
exist (XQuery)   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 XQuery  
 是一个 XQuery 表达式，字符串文字。  
  
## <a name="remarks"></a>备注  
  
> [!NOTE]  
>  对于返回非空结果的 XQuery 表达式，exist() 方法返回 1。 如果在 exist() 方法中指定 true() 或 false() 函数，exist() 方法将返回 1，因为函数 true() 和 false() 将分别返回布尔值 True 和 False     。 也就是说，它们将返回非空结果。 因此，exist() 将返回 1 (True)，如以下示例所示：  
  
```sql
DECLARE @x XML;  
SET @x='';  
SELECT @x.exist('true()');   
```  
  
## <a name="examples"></a>示例  
 以下示例演示如何指定 exist() 方法。  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-variable"></a>示例：对 xml 类型变量指定 exist() 方法  
 以下示例中，@x 是一个 xml 类型变量（非类型化的 xml），@f 是一个整数类型变量，用于存储 exist() 方法返回的值 。 如果 XML 实例中存储的日期值为 `2002-01-01`，exist() 方法返回 True (1)。  
  
```sql  
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<root Somedate = "2002-01-01Z"/>';  
SET @f = @x.exist('/root[(@Somedate cast as xs:date?) eq xs:date("2002-01-01Z")]');  
SELECT @f;  
```  
  
 用 exist() 方法比较日期时，请注意下列事项：  
  
-   代码 `cast as xs:date?` 用于将值强制转换为 xs:date 类型，以进行比较。  
  
-   \@Somedate 属性的值是非类型化的。 进行比较时，此值将隐式强制转换为比较右侧的类型（xs:date 类型）。  
  
-   可使用 xs:date() 构造函数，而非 cast as xs:date() 。 有关详细信息，请参阅[构造函数 (XQuery)](../../xquery/constructor-functions-xquery.md)。  
  
 下面的示例与上一示例类似，不同之处在于它具有 <`Somedate`> 元素。  
  
```sql
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<Somedate>2002-01-01Z</Somedate>';  
SET @f = @x.exist('/Somedate[(text()[1] cast as xs:date ?) = xs:date("2002-01-01Z") ]')  
SELECT @f;  
```  
  
 请注意上述查询的以下方面：  
  
-   text() 方法返回一个文本节点，其中包含非类型化的值 `2002-01-01`。 （XQuery 类型为 xdt:untypedAtomic。）必须将此类型化的值从 x 显式强制转换为 xsd:date，因为本示例中不支持隐式转换 。  
  
### <a name="example-specifying-the-exist-method-against-a-typed-xml-variable"></a>示例：对类型化的 xml 变量指定 exist() 方法  
 以下示例说明了如何对 xml 类型变量使用 exist() 方法 。 它是类型化的 XML 变量，因为它指定了架构命名空间集合名称 `ManuInstructionsSchemaCollection`。  
  
 在该示例中，首先将生产说明文档分配给此变量，然后使用 exist() 方法查看文档中是否包含 LocationID 属性值为 50 的 <`Location`> 元素 。  
  
 如果生产说明文档包含具有 `LocationID=50` 的 <`Location`> 元素，对 @x 变量指定的 exist() 方法返回 1 (True)。 否则，该方法返回 0 (False)。  
  
```sql
DECLARE @x XML (Production.ManuInstructionsSchemaCollection);  
SELECT @x=Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=67;  
--SELECT @x  
DECLARE @f INT;  
SET @f = @x.exist(' declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /AWMI:root/AWMI:Location[@LocationID=50]  
');  
SELECT @f;  
```  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-column"></a>示例：对 xml 类型列指定 exist() 方法  
 下面的查询检索的是目录说明中不包括规范（<`Specifications`> 元素）的产品型号 ID：  
  
```sql
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
 请注意上述查询的以下方面：  
  
-   WHERE 子句只选择 ProductDescription 表中符合根据 CatalogDescription xml 类型列指定的条件的那些行 。  
  
-   如果 XML 不包括任何 <`Specifications`> 元素，则 WHERE 子句中的 exist() 方法返回 1 (True)。 请注意 [not() 函数 (XQuery)](../../xquery/functions-on-boolean-values-not-function.md) 的使用方法。  
  
-   [sql:column() 函数 (XQuery)](../../xquery/xquery-extension-functions-sql-column.md) 用于从非 XML 列中引入值。  
  
-   此查询返回一个空的行集。  
  
 此查询指定 xml 数据类型的 query() 和 exist() 方法，并且这两种方法在查询 prolog 中声明了相同的命名空间 。 在本例中，可能需要使用 WITH XMLNAMESPACES 来声明前缀并在查询中使用它。  
  
```sql
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
