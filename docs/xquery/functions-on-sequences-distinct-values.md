---
title: )  (XQuery 的非重复值函数 |Microsoft Docs
description: 了解如何使用 XQuery 中的 distinct 值函数从序列中删除重复值。
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
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
author: rothja
ms.author: jroth
ms.openlocfilehash: b9a3561c25ca0c64e01c2603b64656320d499518
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341906"
---
# <a name="functions-on-sequences---distinct-values"></a>基于序列的函数 - distinct-values
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  从 *$arg* 指定的序列中删除重复值。 如果 *$arg* 是空序列，则函数返回空序列。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 原子值序列。  
  
## <a name="remarks"></a>备注  
 传递给 **非重复值** 的所有类型的原子化值 () 必须是同一基类型的子类型。 接受的基类型是支持 **eq** 操作的类型。 这些类型包括三种内置数值基类型、日期/时间基类型、xs:string、xs:boolean 和 xdt:untypedAtomic。 类型 xdt:untypedAtomic 的值转换为 xs:string。 如果这些类型混合在一起，或者传递了其他类型的其他值，则会引发静态错误。  
  
 **() 的非重复值** 的结果接收传入类型的基类型，例如，Xdt： untypedAtomic 的 xs： string，其中包含原始基数。 如果输入在静态上为空，则暗示为空，并且会引发静态错误。  
  
 类型 xs:string 的值与 XQuery 默认 Unicode 码位排序规则进行比较。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>A. 使用 distinct-values() 函数从序列中删除重复的值  
 在此示例中，将包含电话号码的 XML 实例分配给 **xml** 类型变量。 针对此变量指定的 XQuery 将使用 **非重复值 ()** 函数来编译不包含重复项的电话号码列表。  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 结果如下：  
  
```  
111-111-1111 222-222-2222    
```  
  
 在下面的查询中， (1、1、2) 的数字序列传入 () 函数的 **非重复值** 。 然后函数将删除序列中重复的数字并返回剩下的两个数字。  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 该查询将返回 1 2。  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **非重复值 ()** 函数将整数值映射到 xs： decimal。  
  
-   **非重复值 ()** 函数仅支持前面提到的类型，不支持组合基类型。  
  
-   不支持在 xs： duration 值上 () 函数的 **非重复值** 。  
  
-   不支持提供排序规则的语法选项。  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
