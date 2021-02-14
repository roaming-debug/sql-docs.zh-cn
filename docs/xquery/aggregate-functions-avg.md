---
title: )  (XQuery 的 avg 函数 |Microsoft Docs
description: 了解用于返回指定数值序列平均值的 XQuery 函数 avg () 。
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:avg function
- avg function [XQuery]
ms.assetid: 0cc60267-3c56-4a88-8ad7-bb07f0255d56
author: rothja
ms.author: jroth
ms.openlocfilehash: ee09ab89e1acb0fc8ddad23d002e2cd0e136bc59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340492"
---
# <a name="aggregate-functions---avg"></a>聚合函数 - avg
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  返回一组数值的平均值。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:avg($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 一组要计算平均值的原子值。  
  
## <a name="remarks"></a>备注  
 传递到 **avg ()** 的所有原子化值的类型必须是三个内置数值基类型或 Xdt： untypedAtomic 中的一个的子类型。 不能使用不同的数值类型。 类型为 xdt:untypedAtomic 的值视为 xs:double。 **Avg ()** 的结果接收传入类型的基类型，如 Xdt： untypedAtomic 的 xs： double。  
  
 如果输入在静态上为空，则暗示为空，并且会引发静态错误。  
  
 **Avg ()** 函数返回计算所得的数字的平均值。 例如：  
  
 **sum (** *$arg* **) div 计数 (** *$arg* **)**  
  
 如果 *$arg* 是空序列，则返回空序列。  
  
 如果无法将 xdt： untypedAtomic 值转换为 xs： double，则 *$arg* 输入序列中的值被忽略。  
  
 在所有其他情况下，函数均返回静态错误。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-avg-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-in-which-labor-hours-are-greater-than-the-average-for-all-work-center-locations"></a>A. 使用 avg() XQuery 函数来查找生产过程中工时大于所有生产车间的工时平均值的生产车间。  
 可以将 [min 函数 (XQuery](../xquery/aggregate-functions-min.md) 中提供的查询重写) 使用 **avg ()** 函数。  
  
## <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **Avg ()** 函数将所有整数映射到 xs： decimal。  
  
-   不支持对类型为 xs： duration 的值执行 **avg ()** 函数。  
  
-   不支持跨基类型边界混合类型的序列。  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
