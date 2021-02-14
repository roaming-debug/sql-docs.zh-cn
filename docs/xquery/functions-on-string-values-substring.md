---
title: substring 函数 (XQuery) |Microsoft Docs
description: 了解返回源字符串的指定部分的 XQuery 函数 substring () 。
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
author: rothja
ms.author: jroth
ms.openlocfilehash: 7457341a5489485973869e46c2ba721b86352217
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345013"
---
# <a name="functions-on-string-values---substring"></a>基于字符串值的函数 - substring
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回 *$sourceString* 的值的一部分，从 *$startingLoc* 的值指示的位置开始，并继续查找 *$length* 的值所指示的字符数。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>参数  
 *$sourceString*  
 资源字符串。  
  
 *$startingLoc*  
 子字符串在资源字符串中的起点。 如果此值为负数或 0，则只返回那些所在位置大于零的字符。 如果大于 *$sourceString* 的长度，则返回长度为零的字符串。  
  
 *$length*  
 [可选] 要检索的字符数。 如果未指定，它将返回 *$startingLoc* 中指定的位置中的所有字符，直到字符串的末尾。  
  
## <a name="remarks"></a>备注  
 带有三个参数的函数将返回 `$sourceString` 中其位置 `$p` 遵守以下指定的字符串：  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 *$Length* 的值可以大于开始位置之后 *$sourceString* 的值中的字符数。 在这种情况下，子字符串返回 *$sourceString* 结尾的字符。  
  
 字符串中第一个字符位于位置 1。  
  
 如果 *$sourceString* 的值是空序列，则会将其作为长度为零的字符串来处理。 否则，如果 *$startingLoc* 或 *$length* 是空序列，则返回空序列。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 XQuery 函数中代理对的行为依赖于数据库兼容级别，并且在某些情况下，还依赖于函数的默认命名空间 URI。 有关详细信息，请参阅 [SQL Server 2016 中数据库引擎功能的重大更改](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)主题中的 "XQuery 函数是代理感知" 部分。 另请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 和 [排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="implementation-limitations"></a>实现限制  
 SQL Server 要求 *$startingLoc* 和 *$length 参数* 的类型为 xs： decimal 而不是 xs： double。  
  
 SQL Server 允许 *$startingLoc* 和 *$length* 为空序列，因为空序列是一个可能的值，导致动态错误被映射到 () 。  
  
## <a name="examples"></a>示例  
 本主题提供了针对数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 。  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>A. 使用 substring() XQuery 函数来检索部分概要产品型号说明  
 查询检索描述产品型号的文本的前50个字符，<`Summary` 文档中> 元素。  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 请注意上述查询的以下方面：  
  
-   **字符串 ()** 函数返回<> 元素的字符串值 `Summary` 。 使用此函数的原因是，<`Summary`> 元素同时包含文本和子元素 (html 格式设置元素) ，因为你将跳过这些元素并检索所有文本。  
  
-   **Substring ()** 函数检索 **字符串 ()** 检索到的字符串值中的前50个字符。  
  
 下面是部分结果：  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
