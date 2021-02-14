---
title: " (XQuery) 循环函数 |Microsoft Docs"
description: 了解 XQuery 函数轮 () ，该函数返回与指定参数最接近的小数部分的数字。
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
- fn:round function
- round function [XQuery]
ms.assetid: 320b572f-bd5b-4055-95a6-dec5718c0041
author: rothja
ms.author: jroth
ms.openlocfilehash: f245b613c3e5e32fd2d4cc8eb09e969719ab1762
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347911"
---
# <a name="numeric-values-functions---round"></a>数值函数 - round
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回与参数最接近的整数。 如果有多个这样的数，将返回最接近正无穷的那个数。 例如：  
  
 如果参数为2.5，则 **round ()** 返回3。  
  
 如果参数为2.4999，则 **round ()** 返回2。  
  
 如果参数为-2.5，则 **舍入 ()** 返回-2。  
  
 如果参数是一个空序列，则 **round ()** 返回空序列。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:round ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 将应用该函数的数字。  
  
## <a name="remarks"></a>备注  
 如果 *$arg* 的类型为三个数值基类型之一 **： xs： float**、 **xs： double** 或 **xs： decimal**，则返回类型与 *$arg* 类型相同。 如果 *$arg* 的类型是派生自其中一个数值类型的类型，则返回类型为基本数值类型。  
  
 如果向 **fn： floor**、 **fn：天花板** 或 **fn： round** 函数的输入为 **xdt： untypedAtomic**，非类型化数据，则它将隐式转换为 **xs： double**。  
  
 任何其他类型都会生成静态错误。  
  
## <a name="examples"></a>示例  
 本主题提供了针对 AdventureWorks 数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例。  
  
 您可以使用天花板函数中的工作示例 [ (xquery)](../xquery/numeric-values-functions-ceiling.md) 用于 **轮 ()** xquery 函数。 只需将查询中的 **天花板 ()** 函数替换为 **round ()** 函数。  
  
## <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **舍入 ()** 函数将整数值映射到 xs： decimal。  
  
-   Xs： double 和 xs： float 值介于-0.5 e0 和-0e0 之间的 **round ()** 函数映射到0e0 而不是-0e0。  
  
## <a name="see-also"></a>另请参阅  
 [floor 函数 &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [天花板函数 &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)  
  
  
