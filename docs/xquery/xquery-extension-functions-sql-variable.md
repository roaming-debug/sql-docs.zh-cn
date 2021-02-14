---
title: sql：变量 () 函数 (XQuery) |Microsoft Docs
description: 了解如何使用 XQuery 扩展函数 sql： variable () 公开 XQuery 表达式内包含 SQL 关系值的变量。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
author: rothja
ms.author: jroth
ms.openlocfilehash: d879088a98bfa7e826c53e4b90822bc65caff57a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350378"
---
# <a name="xquery-extension-functions---sqlvariable"></a>XQuery 扩展函数 - sql:variable()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  显示在 XQuery 表达式中包含 SQL 关系值的变量。  
  
## <a name="syntax"></a>语法  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>备注  
 如主题在 [Xml 内部绑定关系数据](../t-sql/xml/binding-relational-data-inside-xml-data.md)中所述，当使用 [xml 数据类型方法](../t-sql/xml/xml-data-type-methods.md) 在 XQuery 内公开关系值时，可以使用此函数。  
  
 例如， [query () 方法](../t-sql/xml/query-method-xml-data-type.md) 用于针对 xml 数据类型变量或列中存储 **的 xml 实例** 指定查询。 有时，您可能还希望查询使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 变量或参数中的值同时引入关系数据和 XML 数据。 为此，请使用 **sql： variable** 函数。  
  
 SQL 值将映射到相应的 XQuery 值，并且其类型将是与对应的 SQL 类型等效的 XQuery 基类型。  
  
 只能在 XML-DML insert 语句的源表达式的上下文中引用 **xml** 实例;否则，不能引用类型为 **xml** 或公共语言运行时的值 (CLR) 用户定义类型。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>A. 使用 sql:variable() 函数将 Transact-SQL 变量值放到 XML 中  
 以下示例构造由下列值组成的 XML 实例：  
  
-   非 XML 列中的值 (`ProductID`)。 [Sql： column () 函数](../xquery/xquery-extension-functions-sql-column.md)用于在 XML 中绑定此值。  
  
-   另一个表中非 XML 列中的值 (`ListPrice`)。 同样，`sql:column()` 用于在 XML 中绑定此值。  
  
-   [!INCLUDE[tsql](../includes/tsql-md.md)] 变量中的值 (`DiscountPrice`)。 `sql:variable()` 方法用于将此值绑定到 XML。  
  
-   `ProductModelName`从 **xml** 类型列 () 的值，以使查询更加有趣。  
  
 以下是查询语句：  
  
```sql
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 请注意上述查询的以下方面：  
  
-   `query()` 方法中的 XQuery 构造 XML。  
  
-   `namespace`关键字用于定义[XQuery Prolog](../xquery/modules-and-prologs-xquery-prolog.md)中的命名空间前缀。 执行此操作的原因为 `ProductModelName` 属性值是从 `CatalogDescription xml` 类型列（具有与其关联的架构）中检索的。  
  
 结果如下：  
  
```xml
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server XQuery 扩展函数](./xquery-extension-functions-sql-column.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [创建 XML 数据的实例](../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
