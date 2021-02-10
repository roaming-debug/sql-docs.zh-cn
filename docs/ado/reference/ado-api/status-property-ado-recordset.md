---
description: Status 属性（ADO 记录集）
title: ADO 记录集)  (状态属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetStatus
- Recordset15::Status
helpviewer_keywords:
- Status property [ADO Recordset]
ms.assetid: 41d70d89-880f-4850-9d17-19d9790cc8eb
author: rothja
ms.author: jroth
ms.openlocfilehash: cf0bb7ad2a9fa07f0c04a60e685ac0349afd26f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040447"
---
# <a name="status-property-ado-recordset"></a>Status 属性（ADO 记录集）
指示与批更新或其他大容量操作有关的当前记录的状态。  
  
## <a name="return-value"></a>返回值  
 返回一个或多个 [RecordStatusEnum](./recordstatusenum.md) 值的总和。  
  
## <a name="remarks"></a>备注  
 使用 **Status** 属性可查看在批更新过程中修改的记录挂起的更改。 你还可以使用 **状态** 属性来查看在大容量操作过程中失败的记录的状态，例如，在 [记录集](./recordset-object-ado.md)对象上调用 [Resync](./resync-method.md)、 [UpdateBatch](./updatebatch-method.md)或 [CancelBatch](./cancelbatch-method-ado.md)方法时，或者将 **记录集** 对象的 [Filter](./filter-property.md)属性设置为书签的数组。 使用此属性，可以确定给定记录如何失败并相应地进行解决。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [状态属性示例 (记录集)  (VB) ](./status-property-example-recordset-vb.md)   
 [Status 属性示例 (VC++)](./status-property-example-vc.md)