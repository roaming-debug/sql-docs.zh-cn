---
title: " (XQuery) 的本地名称函数 |Microsoft Docs"
description: 了解如何使用 XQuery 函数本地名称 () 返回节点的本地名称部分。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name function
- local-name function
ms.assetid: c901ef5d-89c5-482a-bf64-3eefbcf3098d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9d336db0ac3a6f9c490e3b25a9bee98e1228594f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353297"
---
# <a name="functions-on-nodes---local-name"></a>基于节点的函数 - local-name
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  将 *$arg* 名称的本地部分作为 xs： string 返回，该字符串要么为零长度字符串，要么具有 Xs： NCName 的词法形式。 如果未提供参数，默认值为上下文节点。  
  
## <a name="syntax"></a>语法  
  
```  
fn:local-name() as xs:string  
fn:local-name($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 将检索其本地名称部分的节点名称。  
  
## <a name="remarks"></a>备注  
  
-   在 SQL Server 中，不带参数的 **fn： local name ()** 仅可用于上下文相关的谓词的上下文中。 特别要指出的是，它只能在方括号 (`[ ]`) 内使用。  
  
-   如果提供了参数而参数是空序列，则该函数返回长度为零的字符串。  
  
-   如果目标节点没有名称（因为它是文档节点、注释或文本节点），该函数返回长度为零的字符串。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-retrieve-local-name-of-a-specific-node"></a>A. 检索特定节点的本地名称  
 下面的查询是针对非类型化的 XML 实例指定的。 查询表达式 `local-name(/ROOT[1])` 检索指定节点的本地名称部分。  
  
```  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('local-name(/ROOT[1])')  
-- result = ROOT  
```  
  
 以下查询是针对 ProductModel 表的 Instructions 列（一个类型化的 xml 列）指定的。 表达式 `local-name(/AWMI:root[1]/AWMI:Location[1])` 返回指定节点的本地名称 `Location`。  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     local-name(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
-- result = Location  
```  
  
### <a name="b-using-local-name-without-argument-in-a-predicate"></a>B. 在谓词中使用不带参数的本地名称  
 下面的查询是针对 ProductModel 表的说明列类型化 **xml** 列指定的。 表达式将返回 `root` QName 的本地名称部分为 "Location" <> 元素的所有元素子级。 局部变量中指定了 **本地名称 ()** 函数，但该函数未使用上下文节点。  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
  /AWMI:root//*[local-name() = "Location"]') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 查询返回 `Location` <> 元素的所有 <> 元素子级 `root` 。  
  
## <a name="see-also"></a>另请参阅  
 [节点上的函数](./xquery-functions-against-the-xml-data-type.md)   
 [命名空间 uri 函数 &#40;XQuery&#41;](../xquery/functions-on-nodes-namespace-uri.md)  
  
