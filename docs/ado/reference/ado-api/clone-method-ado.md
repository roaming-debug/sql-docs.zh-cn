---
description: Clone 方法 (ADO)
title: ) ADO (克隆方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Clone
- Recordset20::raw_Clone
helpviewer_keywords:
- Clone method [ADO]
ms.assetid: ad49265f-1c05-4271-9bbf-7c00010ac18c
author: rothja
ms.author: jroth
ms.openlocfilehash: 57bf5f2a4e9134ffdd497133c44741af8a84f2ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027105"
---
# <a name="clone-method-ado"></a>Clone 方法 (ADO)
从现有 **记录集** 对象创建重复的 [记录集](./recordset-object-ado.md)对象。 还可以指定克隆为只读。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set rstDuplicate = rstOriginal.Clone (LockType)  
```  
  
## <a name="return-value"></a>返回值  
 返回 **记录集** 对象引用。  
  
#### <a name="parameters"></a>参数  
 *rstDuplicate*  
 标识要创建的重复 **记录集** 对象的对象变量。  
  
 *rstOriginal*  
 一个对象变量，用于标识要复制的 **记录集** 对象。  
  
 *LockType*  
 可选。 一个 [LockTypeEnum](./locktypeenum.md) 值，该值指定原始 **记录集** 的锁定类型或只读 **记录集**。 有效值为 **adLockUnspecified** 或 **adLockReadOnly**。  
  
## <a name="remarks"></a>备注  
 使用 **Clone** 方法可创建多个重复的 **记录集** 对象，尤其是在您想要在一组给定的记录中维护多个当前记录时。 使用 **Clone** 方法比创建和打开使用与原始方法相同的定义的新 **Recordset** 对象更有效。  
  
 原始 **记录集**（如果有）的 [筛选器](./filter-property.md)属性将不会应用于克隆。 设置新 **记录集** 的 "**筛选器**" 属性以筛选结果。 复制任何现有筛选器值的最简单方法是直接分配 **筛选器** 值，如下所示。  
  
```  
rsNew.Filter = rsOriginal.Filter  
```  
  
 新创建的克隆的当前记录设置为第一条记录。  
  
 对一个 **记录集** 对象所做的更改在其所有克隆中都可见，而不考虑游标类型。 但是，在对原始记录集执行 [Requery](./requery-method.md) 后，克隆将不再同步到原始 **记录集**。  
  
 关闭原始 **记录集** 不会关闭其副本，也不会关闭副本来关闭原始或任何其他副本。  
  
 只能克隆支持书签的 **记录集** 对象。 书签值可互换;也就是说，一个 **记录集** 对象的书签引用会引用其任何克隆中的同一记录。  
  
 触发的某些 **记录集** 事件也将出现在所有 **记录集** 克隆中。 但是，因为克隆的记录 **集** 的当前记录可能不同，所以事件对于克隆可能无效。 例如，如果更改字段的值，则 [WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md) 事件将在已更改的 **记录集中** 和所有克隆中发生。 克隆 **记录集** 的 **WillChangeField** 事件的 *fields* 参数 (未进行更改的) 将引用克隆的当前记录的字段，这可能是与发生更改的原始记录 **集** 的当前记录不同的记录。  
  
 下表提供了所有 **记录集** 事件的完整列表。 它指示它们是有效的，并且对于使用 **Clone** 方法生成的任何记录集克隆都是有效的。  
  
|事件|是否在克隆中触发？|  
|-----------|--------------------------|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|否|  
|[FetchComplete](./fetchcomplete-event-ado.md)|否|  
|[FetchProgress](./fetchprogress-event-ado.md)|否|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|是|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|否|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|是|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|否|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|是|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|是|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|否|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|否|  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Clone 方法示例 (VB) ](./clone-method-example-vb.md)   
 [ (VBScript) 的 Clone 方法示例 ](./clone-method-example-vbscript.md)   
 [Clone 方法示例 (VC++)](./clone-method-example-vc.md)