---
title: 针对 xml 数据类型的 XQuery 函数 |Microsoft Docs
description: 了解支持用于 xml 数据类型的 XQuery 函数。
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
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
author: rothja
ms.author: jroth
ms.openlocfilehash: 82f3eae7098b3ac5e86355aed1e13bdd5c137015
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352810"
---
# <a name="xquery-functions-against-the-xml-data-type"></a>针对 xml 数据类型的 XQuery 函数
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  本主题及其子主题介绍了在针对 **xml** 数据类型指定 XQuery 时可以使用的函数。 有关 W3C 规范，请参阅 [http://www.w3.org/TR/2004/WD-xpath-functions-20040723](https://go.microsoft.com/fwlink/?LinkId=4873) 。  
  
 XQuery 函数属于 http://www.w3.org/2004/07/xpath-functions 命名空间。 W3C 规范使用“fn:”命名空间前缀来说明这些函数。 使用这些函数时，不必显式指定“fn:”命名空间前缀。 由于这个原因，也为了提高可读性，此文档中通常不使用命名空间前缀。  
  
 下表列出了支持 **xml** 数据类型的 XQuery 函数。  
  
|类别|函数名称|  
|--------------|-------------------|  
|[数值上的函数]()|[顶角](../xquery/numeric-values-functions-ceiling.md)|  
||[floor](../xquery/numeric-values-functions-floor.md)|  
||[圆满](../xquery/numeric-values-functions-round.md)|  
|[基于字符串值的 XQuery 函数]()|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contains](../xquery/functions-on-string-values-contains.md)|  
||[substring](../xquery/functions-on-string-values-substring.md)|  
||[&#41;&#40;XQuery 的小写函数 ](../xquery/functions-on-string-values-lower-case.md)|  
||[字符串长度](../xquery/functions-on-string-values-string-length.md)|  
||[&#41;&#40;XQuery 的大写函数 ](../xquery/functions-on-string-values-upper-case.md)|  
|针对布尔值的函数|[not](../xquery/functions-on-boolean-values-not-function.md)|  
|[节点上的函数]()|[数字](../xquery/functions-on-nodes-number.md)|  
||[local-name 函数 (XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[namespace-uri 函数 (XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[上下文函数]()|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[序列上的函数]()|[empty](../xquery/functions-on-sequences-empty.md)|  
||[distinct-values](../xquery/functions-on-sequences-distinct-values.md)|  
||[id 函数 (XQuery)](../xquery/functions-on-sequences-id.md)|  
|[&#40;XQuery 的聚合函数&#41;]()|[计数](../xquery/aggregate-functions-count.md)|  
||[avg](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[sum](../xquery/aggregate-functions-sum.md)|  
|[构造函数 &#40;XQuery&#41;](../xquery/constructor-functions-xquery.md)|[构造函数](../xquery/constructor-functions-xquery.md)|  
|[数据取值函数](../xquery/data-accessor-functions.md)|[string](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[布尔构造函数 &#40;XQuery&#41;]()|[true 函数 (XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[false 函数 (XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[与 QNames &#40;XQuery 相关的函数&#41;](./functions-related-to-qnames-expanded-qname.md)|[expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-name-from-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[namespace-uri-from-QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[SQL Server XQuery 扩展函数](./xquery-extension-functions-sql-column.md)|[sql： column () 函数 (XQuery) ](../xquery/xquery-extension-functions-sql-column.md)|  
||[sql:variable() 函数 (XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>另请参阅  
 [xml 数据类型方法](../t-sql/xml/xml-data-type-methods.md)   
 [XQuery 语言参考 (SQL Server)](../xquery/xquery-language-reference-sql-server.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)  
  
