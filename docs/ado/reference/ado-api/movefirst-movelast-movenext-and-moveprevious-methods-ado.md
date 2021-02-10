---
description: 'MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) '
title: MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
author: rothja
ms.author: jroth
ms.openlocfilehash: a267298dd652a307d6e3365cb48e30f9fc667ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041727"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) 
移动到指定 [记录集](./recordset-object-ado.md) 对象中的第一条、最后一条、下一条记录或上一条记录，并使该记录成为当前记录。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>备注  
 使用 **MoveFirst** 方法将当前记录位置移动到 **记录集中** 的第一条记录。  
  
 使用 **MoveLast** 方法将当前记录位置移动到 **记录集中** 的最后一条记录。 **Recordset** 对象必须支持书签或后向光标移动;否则，此方法调用将生成错误。  
  
 当 **记录集** 为空时，对 **MoveFirst** 或 **MoveLast** 的调用 (**BOF** 和 **EOF** 均为 True) 会生成错误。  
  
 使用 **MoveNext** 方法将当前记录位置向前移动一条记录 (向 **记录集**) 的底部移动。 如果最后一条记录是当前记录并且您调用了 **MoveNext** 方法，则 ADO 会将当前记录设置为 **记录集中** 最后一条记录之后的位置 ([EOF](./bof-eof-properties-ado.md) 为 **True**) 。 当 **EOF** 属性已 **为 True** 时，尝试向前移动将生成错误。  
  
 在 ADO 2.5 和更高版本中，当对 **记录集** 进行筛选或排序并且当前记录的数据发生更改时，调用 **MoveNext** 方法会将游标从当前记录向前移动两条记录。 这是因为当更改当前记录时，下一条记录将成为新的当前记录。 更改后调用 **MoveNext** 会将游标从新的当前记录向前移动一条记录。 这不同于 ADO 2.1 和更早版本中的行为。 在这些早期版本中，更改已排序或已筛选的 **记录集中** 的当前记录的数据不会更改当前记录的位置，并且 **MoveNext** 会在当前记录之后立即将光标移到下一条记录。  
  
 使用 **MovePrevious** 方法将当前记录位置 (一条记录向下移动一条记录，直到记录 **集** 的顶部) 。 **Recordset** 对象必须支持书签或后向光标移动;否则，此方法调用将生成错误。 如果第一条记录是当前记录并且您调用了 **MovePrevious** 方法，则 ADO 会将当前记录设置为 **记录集中** 第一条记录之前的位置 ([BOF](./bof-eof-properties-ado.md) 为 **True**) 。 当 **BOF** 属性为 **True** 时，尝试向后移动将生成错误。 如果 **记录集** 对象不支持书签或后向光标移动，则 **MovePrevious** 方法将生成错误。  
  
 如果 **记录集** 是只进的并且您希望同时支持向前滚动和向后滚动，则可以使用 [CacheSize](./cachesize-property-ado.md) 属性创建一个记录缓存，该缓存将支持通过 [Move](./move-method-ado.md) 方法向后光标移动。 由于缓存的记录已加载到内存中，因此应避免缓存超过所需的记录。 可以在只进 **Recordset** 对象中调用 **MoveFirst** 方法;这样做可能会导致提供程序重新执行生成 **Recordset** 对象的命令。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法示例 (VB) ](./movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法示例 (VBScript) ](./movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法示例 (VC + +) ](./movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [ADO (移动方法) ](./move-method-ado.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS) ](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord 方法 (ADO)](./moverecord-method-ado.md)