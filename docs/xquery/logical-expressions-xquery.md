---
title: 逻辑表达式 (XQuery) |Microsoft Docs
description: 了解 XQuery 中支持的逻辑表达式。
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
- logical operators [SQL Server], XQuery
- effective Boolean value [XQuery]
- logical expressions [XQuery]
- EBV
- expressions [XQuery], logical
ms.assetid: de94cd2e-2d48-49fb-9ebd-a2d90c79bf62
author: rothja
ms.author: jroth
ms.openlocfilehash: 44f05374320dc2d5c329ad234e62dd0c420bf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341850"
---
# <a name="logical-expressions-xquery"></a>逻辑表达式 (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery 支持逻辑 **and** 和 **or** 运算符。  
  
```  
expression1 and expression2  
expression1 or expression2  
```  
  
 中的测试表达式 `expression1,``expression2` [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以导致空序列、一个或多个节点的序列或单个布尔值。 根据结果，按照下列方式确定它们的有效布尔值：  
  
-   如果测试表达式的结果是空序列，则表达式的结果为 False。  
  
-   如果测试表达式的结果是单个布尔值，则此值便为表达式的结果。  
  
-   如果测试表达式的结果是包含一个或多个节点的序列，则表达式的结果为 True。  
  
-   否则，将引发静态错误。  
  
 然后，将逻辑 **and** 和 **or** 运算符应用于带有标准逻辑语义的表达式的结果布尔值。  
  
 下面的查询从产品目录中检索特定产品型号的前一小图片（<`Picture`> 元素）。 请注意，对于每一个产品说明文档，目录都可以存储具有不同属性（如大小和角度）的一个或多个产品图片。  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $F in /PD:ProductDescription/PD:Picture[PD:Size="small"   
                                                 and PD:Angle="front"]  
     return   
         $F   
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 结果如下：  
  
```  
<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  <PD:Angle>front</PD:Angle>  
  <PD:Size>small</PD:Size>  
  <PD:ProductPhotoID>31</PD:ProductPhotoID>  
</PD:Picture>  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 表达式](../xquery/xquery-expressions.md)  
  
  
