---
description: 在记录集中移动的更多方法
title: 在记录集中移动的更多方式 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- Recordset object [ADO], moving
- MovePrevious method [ADO]
ms.assetid: 9f8cf1b2-3def-453f-a0ff-4646c5f15262
author: rothja
ms.author: jroth
ms.openlocfilehash: 78acb389a38896d05a684b0dbd940942a026655e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032709"
---
# <a name="more-ways-to-move-in-a-recordset"></a>在记录集中移动的更多方法
以下四个方法用于在 **记录集中** 移动或滚动记录： [MoveFirst、MoveLast、MoveNext 和 MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)。  (这些方法中的某些方法对于只进游标不可用。 )   
  
 **MoveFirst** 将当前记录位置更改为 **记录集中** 的第一条记录。 **MoveLast** 将当前记录位置更改为 **记录集中** 的最后一条记录。 若要使用 **MoveFirst** 或 **MoveLast**， **Recordset** 对象必须支持书签或后向光标移动;否则，此方法调用将生成错误。  
  
 **MoveNext** 将当前记录位置向前移动一个位置。 如果在调用 **MoveNext** 时在最后一条记录上， **EOF** 将设置为 **True**。 **MovePrevious** 将当前记录位置向后移动一个位置。 如果在调用 **MovePrevious** 时位于第一条记录上， **BOF** 将设置为 **True**。 在使用这些方法时，最好检查 **EOF** 和 **BOF** 属性，如果要移出 **记录集** 的结尾，请将光标移回有效的当前记录位置，如下所示：  
  
```  
. . .  
oRs.MoveNext  
If oRs.EOF Then oRs.MoveLast  
. . .   
```  
  
 或者，对于 **MovePrevious** 方法：  
  
```  
. . .   
oRs.MovePrevious  
If oRs.BOF Then oRs.MoveFirst  
. . .  
```  
  
 如果已对 **记录集** 进行筛选或排序，且当前记录的数据已更改，则位置也可能会更改。 在这种情况下， **MoveNext** 方法会正常运行，但请注意，位置是从新位置向前移动一条记录，而不是旧位置。 例如，更改当前记录中的数据，以便记录移到已排序 **记录集** 的末尾，这意味着调用 **MoveNext** 会导致 ADO 将当前记录设置为 **记录集** 最后一条记录之后的位置 (**EOF**  =  **True**) 。  
  
 **Recordset** 对象的各种移动方法的行为在一定程度上取决于 **记录集** 内的数据。 添加到 **记录集** 的新记录最初按特定顺序添加，该顺序是由数据源定义的，可以隐式或显式依赖于新记录中的数据。 例如，如果在填充 **记录集** 的查询中执行了排序或联接，则新记录将插入到 **记录集** 内的适当位置。 如果在创建 **记录集** 时未显式指定排序方式，则数据源实现中的更改可能导致返回的行的顺序不小心发生更改。 此外， **记录集** 的排序、筛选和编辑函数可能会影响顺序，并且可能会显示记录集中的哪些行。  
  
 因此， **MoveNext**、 **MovePrevious**、 **MoveFirst**、 **MoveLast** 和 **Move** 都对同一 **记录集中** 执行的其他操作是敏感的。 ADO 始终会尝试维持当前位置，直到你显式移动它，但有时，介入更改会使你难以理解后续移动的效果。 例如，如果您调用 **MoveFirst** 来定位已排序 **记录集** 的第一行，并将排序从升序改为降序，则仍处于同一行上，但现在它是 **记录集** 的最后一行。 **MoveFirst** 将转到 (新的第一行) 的其他行。  
  
 作为另一个示例，如果你定位在 **记录集** 中间的特定行上，并调用 **Delete** ，然后调用 **MoveNext**，则现在会立即对已删除的记录执行记录。 但如果调用 **MovePrevious** ，则会将记录置于删除当前记录的前一条记录，因为已删除的记录不再在 **记录集** 的活动成员身份中计数。  
  
 对于相对于当前 **MovePrevious**、 **MoveNext** 和 **移动** 的方法，在当前记录中更改数据的情况下，在所有提供程序中定义一致的移动语义尤其困难。 例如，如果您正在使用已排序的经过筛选的 **记录集**，并更改了当前记录中的数据，使其在所有其他记录之前，但您更改的数据也不再与筛选器匹配，则 **MoveNext** 操作应该将您带到哪个位置。 最安全的结论是， **记录集** 内的相对运动 riskier 绝对移动 (例如，使用 **MoveFirst** 或 **MoveLast**) 在编辑、添加或删除记录时更改数据。 排序和筛选应基于主键或 ID，因为此类型的值不应更改。