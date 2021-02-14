---
title: last 函数 (XQuery) |Microsoft Docs
description: 了解 XQuery 最后 () 函数，该函数返回序列中最后一项的整数索引。
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
- last function [XQuery]
- fn:last function
ms.assetid: dc92086e-3b01-4b0b-9f54-3bbf306cf7ae
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fdc8ce4bf35bb630d0ed0b40892312f89a5285c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340523"
---
# <a name="context-functions---last-xquery"></a>上下文函数 - last (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回当前处理的序列中的项数。 具体来说，它返回序列中最后一项的整数索引。 序列中第一项的索引值为 1。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:last() as xs:integer  
```  
  
## <a name="remarks"></a>备注  
 在 SQL Server 中， **fn： last ()** 仅可用于上下文相关的谓词的上下文中。 特别要指出的是，它只能在方括号 (`[ ]`) 内使用。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-last-xquery-function-to-retrieve-the-last-two-manufacturing-steps"></a>A. 使用 last() XQuery 函数检索最后两个生产步骤  
 下面的查询检索某个特定生产模型的最后两个生产步骤。 在此查询中，将使用 **最后一个 ()** 函数返回的值、制造步骤数来检索最后两个生产步骤。  
  
```  
SELECT ProductModelID, Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  <LastTwoManuSteps>  
   <Last-1Step>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[(last()-1)]/text() }  
   </Last-1Step>  
   <LastStep>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[last()]/text() }  
   </LastStep>  
  </LastTwoManuSteps>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 在前面的查询中， **最后一个 ()** 函数在/中 `/AWMI:root//AWMI:Location)[1]/AWMI:step[last()]` 返回制造步骤的数目。 此值用于检索生产车间的最后一个生产步骤。  
  
 结果如下：  
  
```  
ProductModelID Result    
-------------- -------------------------------------  
7      <LastTwoManuSteps>  
         <Last-1Step>  
            When finished, inspect the forms for defects per   
            Inspection Specification .  
         </Last-1Step>  
         <LastStep>Remove the frames from the tool and place them   
            in the Completed or Rejected bin as appropriate.  
         </LastStep>  
       </LastTwoManuSteps>  
```  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
