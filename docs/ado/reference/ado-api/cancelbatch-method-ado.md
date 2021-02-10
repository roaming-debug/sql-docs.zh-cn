---
description: CancelBatch 方法 (ADO)
title: " (ADO) 的 CancelBatch 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_CancelBatch
- Recordset15::CancelBatch
helpviewer_keywords:
- CancelBatch method [ADO]
ms.assetid: dbdc2574-e44e-4d95-b03d-4a5d9e9adf3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 4784dbd1de8cf29476b4873ce79b704925e06dfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035497"
---
# <a name="cancelbatch-method-ado"></a>CancelBatch 方法 (ADO)
取消挂起的批更新。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.CancelBatchAffectRecords  
```  
  
#### <a name="parameters"></a>参数  
 *AffectRecords*  
 可选。 一个 [AffectEnum](./affectenum.md) 值，指示 **CancelBatch** 方法将影响的记录数。  
  
## <a name="remarks"></a>备注  
 使用 **CancelBatch** 方法可取消批处理更新模式中 [记录集中](./recordset-object-ado.md) 的任何挂起的更新。 如果 **记录集** 处于立即更新模式，则在没有 **adAffectCurrent** 的情况下调用 **CancelBatch** 将生成错误。  
  
 如果要编辑当前记录或在调用 **CancelBatch** 时添加新记录，ADO 首先会调用 [CancelUpdate](./cancelupdate-method-ado.md) 方法来取消任何缓存的更改。 之后， **记录集中** 的所有挂起的更改都将被取消。  
  
 当前记录可以在 **CancelBatch** 调用后无法确定，尤其是在您正在添加新记录的过程中。 出于此原因，将当前记录位置设置为 **CancelBatch** 调用后 **记录集中** 的已知位置是明智的。 例如，调用 [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) 方法。  
  
 如果尝试取消挂起的更新由于与基础数据发生冲突而失败 (例如，如果记录已由其他用户) 删除，则该提供程序会将警告返回到 [错误](./errors-collection-ado.md) 集合，但不会暂停程序执行。 仅当所有请求的记录上发生冲突时才会发生运行时错误。 使用 " [筛选器](./filter-property.md) " 属性 (**AdFilterAffectedRecords**) "和" [状态](./status-property-ado-recordset.md) "属性来查找存在冲突的记录。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 的 UpdateBatch 和 CancelBatch 方法示例 ](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [ (VC + +) 的 UpdateBatch 和 CancelBatch 方法示例 ](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [ADO (取消方法) ](./cancel-method-ado.md)   
 [ (RDS) 的 Cancel 方法 ](../rds-api/cancel-method-rds.md)   
 [CancelUpdate 方法 (ADO) ](./cancelupdate-method-ado.md)   
 [CancelUpdate 方法 (RDS) ](../rds-api/cancelupdate-method-rds.md)   
 [ADO)  (Clear 方法 ](./clear-method-ado.md)   
 [LockType 属性 (ADO) ](./locktype-property-ado.md)   
 [UpdateBatch 方法](./updatebatch-method.md)