---
title: sql： column () 函数 (XQuery) |Microsoft Docs
description: 了解如何使用 XQuery 函数 sql： column () 将 XML 内的非 XML 关系数据绑定在一起，并将关系数据和 XML 数据组合在一起。
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
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d2f550e3516594a269257786e109df9c84a1082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341872"
---
# <a name="xquery-extension-functions---sqlcolumn"></a>XQuery 扩展函数 - sql:column()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  如主题在 [Xml 内部绑定关系数据](../t-sql/xml/binding-relational-data-inside-xml-data.md)中所述，当使用 [Xml 数据类型方法](../t-sql/xml/xml-data-type-methods.md)在 XQuery 内公开关系值时，可以使用 **sql： column ( ()** 函数。  
  
 例如， [查询 () 方法 (xml 数据类型)](../t-sql/xml/query-method-xml-data-type.md) 用于对存储在 **xml** 类型的变量或列中的 xml 实例指定查询。 有时，您可能还希望查询使用其他非 XML 列中的值同时引入关系数据和 XML 数据。 为此，请使用 **sql： column ()** 函数。  
  
 SQL 值将映射到相应的 XQuery 值，其类型将为 XQuery 基类型，等效于相应的 SQL 类型。  
  
## <a name="syntax"></a>语法  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>备注  
 请注意，对在 XQuery 内的 **sql： column ()** 函数中指定的列的引用引用正在处理的行中的列。  
  
 在中 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，只能在 xml-DML insert 语句的源表达式的上下文中引用 **xml** 实例; 否则，不能引用类型为 **xml** 或 CLR 用户定义类型的列。  
  
 联接操作中不支持 **sql： column ()** 函数。 可改用 APPLY 操作。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>A. 使用 sql:column() 检索 XML 中的关系值  
 在构造 XML 时，下面的示例说明了如何从非 XML 关系列中检索值以绑定 XML 数据和关系数据。  
  
 该查询将构造如下形式的 XML 内容：  
  
```xml
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 请注意构造的 XML 中的下列内容：  
  
-   **ProductID**、 **ProductName** 和 **ProductPrice** 属性值是从 **Product** 表中获取的。  
  
-   从 **ProductModel** 表中检索 **ProductModelID** 属性值。  
  
-   为了使查询更有趣， **ProductModelName** 属性值是从 **Xml 类型** 的 **CatalogDescription** 列中获取的。 由于未存储所有产品型号的 XML 产品型号目录信息，因此将使用 `if` 语句检索该值（如果存在）。  
  
    ```sql
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 请注意上述查询的以下方面：  
  
-   由于从两个不同的表检索值，因此 FROM 子句指定两个表。 WHERE 子句中的条件用于筛选结果，并只检索产品型号具有目录说明的产品。  
  
-   [XQuery Prolog](../xquery/modules-and-prologs-xquery-prolog.md)中的 **namespace** 关键字用于定义查询主体中使用的 XML 命名空间前缀 "pd"。 请注意，表别名（“P”和“PM”）是在查询本身的 FROM 子句中定义的。  
  
-   **Sql： column ()** 函数用于将非 xml 值引入到 xml 中。  
  
 下面是部分结果：  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 下面的查询构造了包含产品特定信息的 XML。 此信息包括 ProductID、ProductName、ProductPrice 以及属于特定产品型号 (ProductModelID=19) 的所有产品的 ProductModelName（如果有）。 然后，将 XML 分配给 @x **xml** 类型的变量。  
  
```sql
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server XQuery 扩展函数]()   
 [类型化的 XML 与非类型化的 XML 的比较](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [创建 XML 数据的实例](../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
