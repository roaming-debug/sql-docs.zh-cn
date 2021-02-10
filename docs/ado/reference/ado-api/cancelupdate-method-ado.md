---
description: CancelUpdate 方法 (ADO)
title: " (ADO) 的 CancelUpdate 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CancelUpdate
helpviewer_keywords:
- CancelUpdate method [ADO]
ms.assetid: eaa856cc-c786-462e-890c-c896261b1741
author: rothja
ms.author: jroth
ms.openlocfilehash: 02d173995501e6c8bf2ae987ba88727f4e06aad9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027142"
---
# <a name="cancelupdate-method-ado"></a>CancelUpdate 方法 (ADO)
在调用[Update](./update-method.md)方法之前，取消对[Recordset](./recordset-object-ado.md)对象的当前行或新行所做的任何更改，或[记录](./record-object-ado.md)对象的[字段](./fields-collection-ado.md)集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.CancelUpdaterecord.Fields.CancelUpdate  
```  
  
## <a name="remarks"></a>备注  
  
## <a name="recordset"></a>记录集  
 使用 **CancelUpdate** 方法可以取消对当前行所做的任何更改，或者放弃新添加的行。 在调用 **Update** 方法之后，不能取消对当前行或新行所做的更改，除非这些更改是可使用 [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 方法回滚的事务的一部分，或者是批处理更新的一部分。 对于批更新，可以使用 **CancelUpdate** 或 [CancelBatch](./cancelbatch-method-ado.md)方法取消 **更新**。  
  
 如果在调用 **CancelUpdate** 方法时添加新行，则当前行将成为在 [AddNew](./addnew-method-ado.md) 调用之前的当前行。  
  
 如果处于编辑模式并想要移出当前记录 (例如，通过使用 [move](./move-method-ado.md)、 [NextRecordset](./nextrecordset-method-ado.md)或 [Close](./close-method-ado.md) 方法) ，可以使用 **CancelUpdate** 来取消任何挂起的更改。 如果更新无法成功发布到数据源，可能需要执行此操作。 例如，尝试删除因引用完整性冲突而失败时，会在调用 [delete](./delete-method-ado-recordset.md)后使 **记录集** 处于编辑模式。  
  
## <a name="record"></a>记录  
 **CancelUpdate** 方法取消 [字段](./field-object.md)对象的任何挂起的插入或删除操作，并取消现有字段的挂起更新并将其还原为原始值。 **字段** 集合中所有字段的 [Status](./status-property-ado-recordset.md)属性都设置为 **adFieldOK**。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [字段集合 (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [Update 和 CancelUpdate 方法示例 (VB) ](./update-and-cancelupdate-methods-example-vb.md)   
 [Update 和 CancelUpdate 方法示例 (VC + +) ](./update-and-cancelupdate-methods-example-vc.md)   
 [AddNew 方法 (ADO) ](./addnew-method-ado.md)   
 [ADO (取消方法) ](./cancel-method-ado.md)   
 [ (RDS) 的 Cancel 方法 ](../rds-api/cancel-method-rds.md)   
 [CancelBatch 方法 (ADO) ](./cancelbatch-method-ado.md)   
 [CancelUpdate 方法 (RDS) ](../rds-api/cancelupdate-method-rds.md)   
 [EditMode 属性](./editmode-property.md)   
 [Update 方法](./update-method.md)