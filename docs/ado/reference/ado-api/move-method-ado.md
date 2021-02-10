---
description: Move 方法 (ADO)
title: ADO)  (移动方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Move
- Recordset15::raw_Move
helpviewer_keywords:
- Move method [ADO]
ms.assetid: 13fe9381-d00b-4f4a-9162-83c3f21b3837
author: rothja
ms.author: jroth
ms.openlocfilehash: 848074390559e3c40e0cf1a4e8242344c70f6334
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041757"
---
# <a name="move-method-ado"></a>Move 方法 (ADO)
移动 [记录集](./recordset-object-ado.md) 对象中的当前记录的位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Move NumRecords, Start  
```  
  
#### <a name="parameters"></a>参数  
 *NumRecords*  
 一个有符号 **长** 表达式，指定当前记录位置移动的记录数。  
  
 *启动*  
 可选。 计算结果为书签的 **字符串** 值或 **变量** 。 还可以使用 [BookmarkEnum](./bookmarkenum.md) 值。  
  
## <a name="remarks"></a>备注  
 **Move** 方法在所有 **记录集** 对象上都受支持。  
  
 如果 *NumRecords* 参数大于零，则当前记录位置将向前移动 (向 **记录集** 的末尾) 。 如果 *NumRecords* 小于零，则当前记录位置 (向后) **记录集** 的开头向后移动。  
  
 如果 **移动** 调用会将当前记录位置移动到第一条记录之前的某个点，则 ADO 会将当前记录设置为记录集中第一条记录之前的位置 ([BOF](./bof-eof-properties-ado.md) 为 **True**) 。 当 **BOF** 属性为 **True** 时，尝试向后移动将生成错误。  
  
 如果 **移动** 调用会将当前记录位置移到最后一条记录之后的某个点，则 ADO 会将当前记录设置为记录集中最后一条记录之后的位置 ([EOF](./bof-eof-properties-ado.md) 为 **True**) 。 当 **EOF** 属性已 **为 True** 时，尝试向前移动将生成错误。  
  
 从空 **记录集** 对象调用 **Move** 方法会生成错误。  
  
 如果传递 *Start* 参数，则移动将相对于带有此书签的记录，假设 **Recordset** 对象支持书签。 如果未指定，则移动相对于当前记录。  
  
 如果使用 [CacheSize](./cachesize-property-ado.md) 属性在本地缓存来自提供程序的记录，并且传递的是在当前缓存记录组外部移动当前记录位置的 *NumRecords* 参数，则会强制 ADO 从目标记录开始检索一组新记录。 **CacheSize** 属性确定新检索到的组的大小，目标记录是检索的第一条记录。  
  
 如果 **记录集** 对象为 forward，则用户仍然可以传递一个小于零的 *NumRecords* 参数，前提是目标在当前缓存记录集中。 如果 **移动** 调用会在第一个缓存记录之前将当前记录位置移动到记录，则会发生错误。 因此，你可以使用记录缓存，该缓存支持对只支持向前滚动的访问接口进行完全滚动。 由于缓存的记录已加载到内存中，因此应避免缓存超过所需的记录。 即使只进 **记录集** 对象支持以这种方式向后移动，对任何只进 **Recordset** 对象调用 [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)方法仍将生成错误。  
  
> [!NOTE]
>  支持在只进 **记录集中** 向后移动不能预测，这取决于您的提供程序。 如果当前记录已位于 **记录集中** 的最后一条记录之后，则向后 **移动** 可能不会导致正确的当前位置。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 移动方法示例 ](./move-method-example-vb.md)   
 [ (VBScript) 移动方法示例 ](./move-method-example-vbscript.md)   
 [ (VC + +) 移动方法示例 ](./move-method-example-vc.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) ](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS) ](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord 方法 (ADO)](./moverecord-method-ado.md)