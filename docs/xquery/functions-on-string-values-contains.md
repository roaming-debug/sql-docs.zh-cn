---
title: 包含函数 (XQuery) |Microsoft Docs
description: 了解如何在 XQuery 中使用 contains 函数来确定指定的字符串值是否包含指定的子字符串值。
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
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
author: rothja
ms.author: jroth
ms.openlocfilehash: 18f57d57e1a944a1be66a880231533e6cf34493e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344968"
---
# <a name="functions-on-string-values---contains"></a>基于字符串值的函数 - contains
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回 xs： boolean 类型的值，指示 *$arg 1* 的值是否包含 *$arg 2* 指定的字符串值。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>参数  
 *$arg 1*  
 要测试的字符串值。  
  
 *$arg 2*  
 要查找的子字符串。  
  
## <a name="remarks"></a>备注  
 如果 *$arg 2* 的值是长度为零的字符串，则函数将返回 **True**。 如果 *$arg 1* 的值是长度为零的字符串，而 *$arg 2* 的值不是长度为零的字符串，则该函数返回 **False**。  
  
 如果 *$arg 1* 或 *$arg 2* 的值是空序列，则该参数将被视为长度为零的字符串。  
  
 contains() 函数使用 XQuery 默认的 Unicode 码位排序规则来进行字符串比较。  
  
 为 *$arg 2* 指定的子字符串值必须小于或等于4000个字符。 如果指定的值超过4000个字符，则会出现动态错误条件，而 contains () 函数将返回一个空序列，而不是布尔值 **True** 或 **False**。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会对 XQuery 表达式生成动态错误。  
  
 为了实现不区分大小写的比较，可以使用 [大写](../xquery/functions-on-string-values-upper-case.md) 或小写函数。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 XQuery 函数中代理对的行为依赖于数据库兼容级别，并且在某些情况下，还依赖于函数的默认命名空间 URI。 有关详细信息，请参阅 [SQL Server 2016 中数据库引擎功能的重大更改](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)主题中的 "XQuery 函数是代理感知" 部分。 另请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 和 [排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="examples"></a>示例  
 本主题提供针对 AdventureWorks 数据库中各种 xml 类型列中存储的 XML 实例的 XQuery 示例。  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>A. 使用 contains() XQuery 函数搜索特定的字符串  
 以下查询将查找概要说明中包含单词 Aerodynamic 的产品。 查询返回此类产品的 ProductID 和 <`Summary`> 元素。  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 结果  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
