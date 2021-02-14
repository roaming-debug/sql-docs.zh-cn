---
title: " (XQuery) 的天花板函数 |Microsoft Docs"
description: 了解如何使用 XQuery 天花板 () 函数返回不小于函数参数值的小数部分的最小数值。
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
- fn:ceiling function
- ceiling function [XQuery]
ms.assetid: 594f1dd0-3c27-41b3-b809-9ce6714c5a97
author: rothja
ms.author: jroth
ms.openlocfilehash: 1ea2dbaab164c57acf9e9bc166ee431e80783648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341586"
---
# <a name="numeric-values-functions---ceiling"></a>数值函数 - ceiling 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回不带小数部分并且不小于其参数值的最小数字。 如果参数是一个空序列，则返回空序列。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:ceiling ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 将应用该函数的数字。  
  
## <a name="remarks"></a>备注  
 如果 *$arg* 的类型为三个数值基类型之一 **： xs： float**、 **xs： double** 或 **xs： decimal**，则返回类型与 *$arg* 类型相同。  
  
 如果 *$arg* 的类型是派生自其中一个数值类型的类型，则返回类型为基本数值类型。  
  
 如果 fn： floor、fn：天花板或 fn： round 函数的输入为 **xdt： untypedAtomic**，则它将隐式转换为 **xs： double**。  
  
 任何其他类型都会生成静态错误。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-ceiling-xquery-function"></a>A. 使用 ceiling() XQuery 函数  
 对于产品型号 7，此查询返回产品型号生产过程中的生产车间的列表。 查询将返回每个生产车间的位置 ID、工时和批量大小（如果有记录）。 查询使用 **天花板** 函数将人工时间作为 **decimal** 类型的值返回。  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
     for $i in /AWMI:root/AWMI:Location  
     return   
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ ceiling($i/@LaborHours) }" >  
                    {   
                      $i/@LotSize  
                    }    
       </Location>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 请注意上述查询的以下方面：  
  
-   AWMI 命名空间前缀表示 Adventure Works 的生产说明。 此前缀引用被查询文档中使用的同一命名空间。  
  
-   **说明** 是一个 **xml** 类型列。 因此， [查询 () 方法 (XML 数据类型) ](../t-sql/xml/query-method-xml-data-type.md) 用于指定 XQuery。 XQuery 语句指定为该查询方法的参数。  
  
-   **对于 .。。返回** 为循环构造。 在查询中， **for** 循环标识 \<Location> 元素列表。 对于每个工作中心位置， **for** 循环中的 **return** 语句描述要生成的 XML：  
  
    -   \<Location>具有 LocationID 和 LaborHrs 特性的元素。 大括号 ({ }) 中对应的表达式从文档中检索所需的值。  
  
    -   {$ i/@LotSize } 表达式从文档中检索 LotSize 属性（如果存在）。  
  
    -   结果如下：  
  
```  
ProductModelID Result    
-------------- ------------------------------------------------------  
7      <Location LocationID="10" LaborHrs="3" LotSize="100"/>  
       <Location LocationID="20" LaborHrs="2" LotSize="1"/>     
       <Location LocationID="30" LaborHrs="1" LotSize="1"/>     
       <Location LocationID="45" LaborHrs="1" LotSize="20"/>  
       <Location LocationID="60" LaborHrs="3" LotSize="1"/>     
       <Location LocationID="60" LaborHrs="4" LotSize="1"/>  
```  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **上限 ()** 函数将所有整数值映射到 xs： decimal。  
  
## <a name="see-also"></a>另请参阅  
 [floor 函数 &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [&#40;XQuery&#41;循环函数 ](../xquery/numeric-values-functions-round.md)  
  
  
