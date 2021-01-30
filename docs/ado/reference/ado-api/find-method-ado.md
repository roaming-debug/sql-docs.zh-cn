---
description: Find 方法 (ADO)
title: Find 方法 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Find
- Recordset15::Find
helpviewer_keywords:
- Find method [ADO]
ms.assetid: 55c9810a-d8ca-46c2-a9dc-80e7ee7aa188
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f581b2c4b4861c852a9e5a24aec5a219ee098f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167314"
---
# <a name="find-method-ado"></a>Find 方法 (ADO)
在 [记录集中](./recordset-object-ado.md) 搜索满足指定条件的行。 或者，可以指定搜索方向、起始行和起始行的偏移量。 如果满足条件，则在找到的记录上设置当前行位置;否则，将位置设置为 **记录集** 的 (或开始) 。  
  
## <a name="syntax"></a>语法  
  
```  
  
Find (Criteria, SkipRows, SearchDirection, Start)  
```  
  
#### <a name="parameters"></a>参数  
 *条件*  
 一个包含语句的 **字符串** 值，该语句指定要在搜索中使用的列名称、比较运算符和值。  
  
 *SkipRows*  
 可选。 一个 **长整型** 值，其默认值为零，指定从当前行或 *开始* 书签开始搜索的行偏移量。 默认情况下，搜索将从当前行开始。  
  
 *SearchDirection*  
 可选。 一个 [SearchDirectionEnum](./searchdirectionenum.md) 值，指定搜索是否应在搜索方向上从当前行或下一个可用行开始。 如果值为 **adSearchForward**，将在 **记录集** 末尾停止搜索。 如果值为 **adSearchBackward**，则不成功的搜索将在 **记录集** 的开始处停止。  
  
 *启动*  
 可选。 作为搜索的起始位置的 **变量** 书签。  
  
## <a name="remarks"></a>备注  
 仅可在 *条件* 中指定单列名称。 此方法不支持多列搜索。  
  
 *条件* 中的比较运算符可以是 " **>** " (大于) ，"* * \<**" (less than), "=" (equal), "> =" (大于或等于) ，"<=" (小于或等于) ，"<>" (不等于) ，或 "like" (模式匹配) 。  
  
 *条件* 中的值可以是字符串、浮点数字或日期。 字符串值用单引号或 "#" 分隔 (数字符号) 标记， (例如，"state =" WA "" 或 "state = #WA #" ) 。 日期值用 "#" 分隔 (数字符号) 标记， (例如，"start_date > #7/22/97 #" ) 。 这些值可以包含小时、分钟和秒以指示时间戳，但不应包含毫秒或错误。  
  
 如果比较运算符为 "like"，则字符串值可能包含星号 ( * ) 以查找任意字符或子字符串的一个或多个匹配项。 例如，"Maine" 与 " \* " 匹配。 你还可以使用前导和尾随星号来查找值中包含的子字符串。 例如，"状态" 与 " \* as \* " "匹配阿拉斯加，阿肯色，和马萨诸塞州。  
  
 星号只能在条件字符串的末尾，或在条件字符串的开头和末尾使用，如上所示。 不能将星号用作前导通配符 ( "* str" ) ，或用作嵌入的通配符 ( 的 \* r ") 。 这将导致错误。  
  
> [!NOTE]
>  如果在调用 **Find** 之前未设置当前行位置，将会发生错误。 在调用 **Find** 之前，应调用设置行位置的任何方法，例如 [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)。  
  
> [!NOTE]
>  如果对记录集调用 **Find** 方法，并且记录集中的当前位置位于最后一条记录或文件尾 (EOF) ，则您将找不到任何内容。 需要调用 **MoveFirst** 方法，将当前位置/光标设置为记录集的开头。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Find 方法示例 (VB) ](./find-method-example-vb.md)   
 [Index 属性](./index-property.md)   
 [优化 Property-Dynamic (ADO) ](./optimize-property-dynamic-ado.md)   
 [Seek 方法](./seek-method.md)