---
title: 字符串长度函数 (XQuery) |Microsoft Docs
description: 了解如何使用 XQuery 函数字符串长度 () 。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- string-length function
- fn:string-length function
ms.assetid: 7cd69c8b-cf2c-478c-b9a3-e0e14e1aa8aa
author: rothja
ms.author: jroth
ms.openlocfilehash: 588e3a4dc19d502da07d23971aea7b987e49a8ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344993"
---
# <a name="functions-on-string-values---string-length"></a>基于字符串值的函数 - string-length
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回字符串的长度（以字符为单位）。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:string-length() as xs:integer  
fn:string-length($arg as xs:string?) as xs:integer  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 要计算其长度的源字符串。  
  
## <a name="remarks"></a>备注  
 如果 *$arg* 的值是空序列，则返回 **xs： integer** 值0。  
  
 XQuery 函数中代理对的行为依赖于数据库兼容级别。 如果该兼容级别为 110 或更高，则每个代理对都作为单个字符计数。 对于更低的兼容级别，它们会作为两个字符计数。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 和 [排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。  
  
 如果该值包含由两个代理项字符表示的 4 字节 Unicode 字符，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将单独对代理项字符计数。  
  
 不带参数的 **字符串长度 ()** 只能在谓词内使用。 例如，下面的查询将返回 <`ROOT`> 元素：  
  
```  
DECLARE @x xml;  
SET @x='<ROOT>Hello</ROOT>';  
SELECT @x.query('/ROOT[string-length()=5]');  
```  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 XQuery 函数中代理对的行为依赖于数据库兼容级别，并且在某些情况下，还依赖于函数的默认命名空间 URI。 有关详细信息，请参阅 [SQL Server 2016 中数据库引擎功能的重大更改](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)主题中的 "XQuery 函数是代理感知" 部分。 另请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 和 [排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="examples"></a>示例  
 本主题提供了针对 AdventureWorks 数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-string-length-xquery-function-to-retrieve-products-with-long-summary-descriptions"></a>A. 使用 string-length() XQuery 函数检索带有较长摘要说明的产品  
 对于摘要说明超过50个字符的产品，下面的查询将检索产品 ID、摘要说明的长度以及摘要本身、<`Summary`> 元素。  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT CatalogDescription.query('  
      <Prod ProductID= "{ /pd:ProductDescription[1]/@ProductModelID }" >  
       <LongSummary SummaryLength =   
           "{string-length(string( (/pd:ProductDescription/pd:Summary)[1] )) }" >  
           { string( (/pd:ProductDescription/pd:Summary)[1] ) }  
       </LongSummary>  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('string-length( string( (/pd:ProductDescription/pd:Summary)[1]))', 'decimal') > 200;  
```  
  
 请注意上述查询的以下方面：  
  
-   WHERE 子句的条件只检索在 XML 文档中存储的摘要说明大于 200 个字符的行。 它使用 [ () 方法 (XML 数据类型) 的值 ](../t-sql/xml/value-method-xml-data-type.md)。  
  
-   SELECT 子句仅构造您想要的 XML。 它使用 [query () 方法 (xml 数据类型) ](../t-sql/xml/query-method-xml-data-type.md) 来构造 xml，并指定必要的 XQuery 表达式来检索 xml 文档中的数据。  
  
 下面是部分结果：  
  
```  
Result  
-------------------  
<Prod ProductID="19">  
      <LongSummary SummaryLength="214">Our top-of-the-line competition   
             mountain bike. Performance-enhancing options include the  
             innovative HL Frame, super-smooth front suspension, and   
             traction for all terrain.  
      </LongSummary>  
</Prod>  
...  
```  
  
### <a name="b-using-the-string-length-xquery-function-to-retrieve-products-whose-warranty-descriptions-are-short"></a>B. 使用 string-length() XQuery 函数检索保修说明较短的产品  
 对于其保修说明长度少于20个字符的产品，下面的查询将检索包含产品 ID、长度、保修说明和 <`Warranty`> 元素本身的 XML。  
  
 保修是厂商为产品提供的服务之一。 在 `Warranty` <> 元素之后> 子元素的可选 <`Features` 。  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for   $ProdDesc in /pd:ProductDescription,  
            $pf in $ProdDesc/pd:Features/wm:Warranty  
      where string-length( string(($pf/wm:Description)[1]) ) < 20  
      return   
          <Prod >  
             { $ProdDesc/@ProductModelID }  
             <ShortFeature FeatureDescLength =   
                             "{string-length( string(($pf/wm:Description)[1]) ) }" >  
                 { $pf }  
             </ShortFeature>  
          </Prod>  
     ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription')=1;  
```  
  
 请注意上述查询的以下方面：  
  
-   **pd** 和 **wm** 是在此查询中使用的命名空间前缀。 它们标识正在查询的文档中使用的相同命名空间。  
  
-   XQuery 指定嵌套 FOR 循环。 需要外部 FOR 循环，因为要检索 <> 元素的 **ProductModelID** 属性 `ProductDescription` 。 内部 FOR 循环也是必需的，因为您只需检索那些保修功能说明少于 20 个字符的产品。  
  
 下面是部分结果：  
  
```  
Result  
-------------------------  
<Prod ProductModelID="19">  
  <ShortFeature FeatureDescLength="15">  
    <wm:Warranty   
       xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
      <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
      <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
   </ShortFeature>  
</Prod>  
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
