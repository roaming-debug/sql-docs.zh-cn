---
title: )  (XQuery 的 min 函数 |Microsoft Docs
description: 了解 XQuery min () 函数，该函数返回一项序列中的某一项，其值小于其他值的值。
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
- fn:min function
- min function [XQuery]
ms.assetid: db0b7d94-3fa6-488f-96d6-6a9a7d6eda23
author: rothja
ms.author: jroth
ms.openlocfilehash: 19c4204e84759bab022d0f0765b9d6154d23d003
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341946"
---
# <a name="aggregate-functions---min"></a>聚合函数 - min
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  从一个原子值序列（ *$arg*）返回一个值小于所有其他值的项。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:min($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 要返回其最小值的一组数值。  
  
## <a name="remarks"></a>备注  
 传递给 **min ()** 的所有原子化值的类型都必须是同一基类型的子类型。 接受的基类型是支持 **gt** 操作的类型。 这些类型包括三种内置数值基类型、日期/时间基类型、xs:string、xs:boolean 和 xdt:untypedAtomic。 类型为 xdt:untypedAtomic 的值将转换为 xs:double。 如果有这些类型的混合，或者如果传递其他类型的其他值，则会引发静态错误。  
  
 **Min ()** 的结果接收传入类型的基类型，如 Xdt： untypedAtomic 的 xs： double。 如果输入静态为空，则隐含为空并返回静态错误。  
  
 **Min ()** 函数返回一个值，该值比输入序列中的任何其他值小。 对于 xs:string 值，则使用默认的 Unicode 码位排序规则。 如果无法将 xdt： untypedAtomic 值转换为 xs： double，则 *$arg* 输入序列中的值将被忽略。 如果输入是动态计算的空序列，则返回空序列。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-min-xquery-function-to-find-the-work-center-location-that-has-the-fewest-labor-hours"></a>A. 使用 min() XQuery 函数查找工时最少的生产车间  
 下面的查询检索产品型号 (ProductModelID=7) 生产过程中工时最少的所有生产车间。 通常返回单个位置，如下所示。 如果多个车间具有相同的最少工时数，则将它们全部返回。  
  
```  
select ProductModelID, Name, Instructions.query('  
  declare namespace AWMI=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  for   $Location in /AWMI:root/AWMI:Location  
  where $Location/@LaborHours =  
          min( /AWMI:root/AWMI:Location/@LaborHours )  
return  
  <Location WCID=     "{ $Location/@LocationID }"   
              LaborHrs= "{ $Location/@LaborHours }" />  
  ') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 请注意上述查询的以下方面：  
  
-   XQuery prolog 中的 **namespace** 关键字定义命名空间前缀。 然后，将此前缀用于 XQuery 主体中。  
  
 XQuery 体构造具有 \<Location> WCID 和 **LaborHrs** 属性的元素的 XML。  
  
-   该查询也检索 ProductModelID 和名称值。  
  
 结果如下：  
  
```  
ProductModelID   Name              Result  
---------------  ----------------  ---------------------------------  
7                HL Touring Frame  <Location WCID="45" LaborHrs="0.5"/>   
```  
  
## <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **Min ()** 函数将所有整数映射到 xs： decimal。  
  
-   不支持对类型为 xs： duration 的值执行 **min ()** 函数。  
  
-   不支持跨基类型边界混合类型的序列。  
  
-   不支持提供排序规则的语法选项。  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
