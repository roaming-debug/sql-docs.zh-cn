---
description: query() 方法（xml 数据类型）
title: query() 方法（xml 数据类型）
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query method
- query() method
ms.assetid: f48f6f7b-219f-463a-bf36-bc10f21afaeb
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 19dd6786f9ee59e4185b68c1ab49a9f5c9924a9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200231"
---
# <a name="query-method-xml-data-type"></a>query() 方法（xml 数据类型）
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

对 xml 数据类型的实例指定 XQuery。 结果为 xml 类型。 该方法返回非类型化的 XML 实例。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
query ('XQuery')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
XQuery  
字符串，在 XML 实例中查询 XML 节点（如元素和属性）的 XQuery 表达式。  
  
## <a name="examples"></a>示例  
本节提供使用 xml 数据类型的 query() 方法的示例。  
  
### <a name="a-using-the-query-method-against-an-xml-type-variable"></a>A. 对 xml 类型的变量使用 query() 方法  
以下示例声明了 xml 类型的变量 \@myDoc 并将 XML 实例分配给它 。 然后使用 query() 方法对文档指定 XQuery。  
  
该查询检索 <`ProductDescription`> 元素的 <`Features`> 子元素：  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc.query('/Root/ProductDescription/Features')  
```  
  
以下输出显示了结果：  
  
```  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>        
```  
  
### <a name="b-using-the-query-method-against-an-xml-type-column"></a>B. 对 XML 类型列使用 query() 方法  
在以下示例中，使用 query() 方法对 AdventureWorks 数据库中 xml 类型的 CatalogDescription 列指定 XQuery   ：  
  
```sql
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />  
') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
请注意上述查询中的以下项：  
  
-   CatalogDescription 列是类型化 xml 列，这意味着它有关联的架构集合。 在 [XQuery Prolog](../../xquery/modules-and-prologs-xquery-prolog.md) 中，namespace 关键字定义稍后用于查询主体的前缀。  
  
-   query() 方法构造 XML，即包含 ProductModelID 属性的 <`Product`> 元素，其中 ProductModelID 属性值是从数据库中检索的  。 有关 XML 构造的详细信息，请参阅 [XML 构造 (XQuery)](../../xquery/xml-construction-xquery.md)。  
  
-   WHERE 子句中的 [exist() 方法（XML 数据类型）](../../t-sql/xml/exist-method-xml-data-type.md)仅查找在 XML 中包含 <`Warranty`> 元素的行。 同样，namespace 关键字定义两个命名空间前缀。  
  
以下输出显示了部分结果：  
  
```  
<Product ProductModelID="19"/>   
<Product ProductModelID="23"/>   
...  
```  
  
请注意，query() 方法和 exist() 方法都声明 PD 前缀。 在这些情况下，可以使用 WITH XMLNAMESPACES 首先定义前缀，然后在查询中使用它。  
  
```sql
WITH XMLNAMESPACES 
(  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM
)  
SELECT CatalogDescription.query('<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />')
       AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/PD:ProductDescription/PD:Features/WM:Warranty ') = 1;
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
