---
description: Index 属性
title: Index 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset21::Index
helpviewer_keywords:
- Index property
ms.assetid: 1c79e271-21ec-41a8-8163-c5e89f0001a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e8c346ac48092d7a5dcaf09068bb64d63316a38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170953"
---
# <a name="index-property"></a>Index 属性
指示当前对 [记录集](./recordset-object-ado.md) 对象有效的索引的名称。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值，该值是索引的名称。  
  
## <a name="remarks"></a>备注  
 **索引** 属性命名的索引之前必须在 **Recordset** 对象的基础表上声明。 也就是说，必须以编程方式将索引声明为 ADOX [索引](../adox-api/index-object-adox.md) 对象，或创建基表。  
  
 如果无法设置索引，则会发生运行时错误。 在下列条件下，不能设置 **Index** 属性：  
  
-   在 [WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) 或 **RecordsetChangeComplete** 事件处理程序中。  
  
-   如果 **记录集** 仍在执行操作 (可由 [状态](./state-property-ado.md) 属性) 确定。  
  
-   如果已对具有 [filter](./filter-property.md)属性的 **记录** 集设置筛选器，则为。  
  
 如果 **记录集** 已关闭，则始终可以成功设置 **Index** 属性，但如果基础提供程序不支持索引，则无法成功地打开该 **记录集** 或索引将无法使用。  
  
 如果可以设置索引，则当前行位置可能会更改。 这将导致更新 [AbsolutePosition](./absoluteposition-property-ado.md) 属性，并激发 **WillChangeRecordset**、 **RecordsetChangeComplete**、 [WillMove](./willmove-and-movecomplete-events-ado.md)和 [MoveComplete](./willmove-and-movecomplete-events-ado.md) 事件。  
  
 如果可以设置索引，并且 [LockType](./locktype-property-ado.md) 属性为 **adLockPessimistic** 或 **adLockOptimistic**，则执行隐式 [UpdateBatch](./updatebatch-method.md) 操作。 这将释放当前和受影响的组。 任何现有筛选器都将释放，并且当前行位置将更改为重新排序 **记录集** 的第一行。  
  
 **Index** 属性与 [Seek](./seek-method.md)方法一起使用。 如果基础提供程序不支持 **Index** 属性，因而是 **Seek** 方法，请考虑改用 [Find](./find-method-ado.md) 方法。 确定 **Recordset** 对象是否支持具有 [支持](./supports-method.md)**(a)** 方法的索引。  
  
 内置的 **索引** 属性与动态 [Optimize](./optimize-property-dynamic-ado.md) 属性无关，不过它们都处理索引。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Seek 方法和 Index 属性示例 (VB) ](./seek-method-and-index-property-example-vb.md)   
 [索引对象 (ADOX) ](../adox-api/index-object-adox.md)   
 [Seek 方法](./seek-method.md)