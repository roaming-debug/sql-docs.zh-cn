---
description: WillChangeField 和 FieldChangeComplete 事件 (ADO)
title: WillChangeField 和 FieldChangeComplete 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldChangeComplete
- Recordset::WillChangeField
- Recordset::FieldChangeComplete
- WillChangeField
helpviewer_keywords:
- WillChangeField event [ADO]
- fieldchangecomplete event [ADO]
ms.assetid: 3e49fb89-c45b-4d39-823e-3cc887c59b37
author: rothja
ms.author: jroth
ms.openlocfilehash: 4aef5291d7e4c539200eb0cfd34edf3174c44c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056132"
---
# <a name="willchangefield-and-fieldchangecomplete-events-ado"></a>WillChangeField 和 FieldChangeComplete 事件 (ADO)
在挂起的操作更改 [记录集中](./recordset-object-ado.md)一个或多个 [字段](./field-object.md)对象的值之前调用 **WillChangeField** 事件。 在一个或多个 **字段** 对象的值更改后调用 **FieldChangeComplete** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
WillChangeField cFields, Fields, adStatus, pRecordset  
FieldChangeComplete cFields, Fields, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>参数  
 *cFields*  
 Long 类型的 **值** ，指示字段中 **字段** 对象 *的数量*。  
  
 *Fields*  
 对于 **WillChangeField**，*字段* 参数是包含具有原始值的 **字段** 对象的 **变量** 的数组。 对于 **FieldChangeComplete**， *Fields* 参数是包含值已更改的 **字段** 对象的 **变量** 的数组。  
  
 *pError*  
 一个 [错误](./error-object.md) 对象。 它描述了 *adStatus* 的值为 **adStatusErrorsOccurred** 时所发生的错误;否则，不会设置。  
  
 *adStatus*  
 [EventStatusEnum](./eventstatusenum.md)状态值。  
  
 调用 **WillChangeField** 时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** 。 如果此事件无法请求取消挂起操作，则将其设置为 **adStatusCantDeny** 。  
  
 调用 **FieldChangeComplete** 时，如果导致事件的操作成功，则此参数设置为 **adStatusOK** ; 如果操作失败，则设置为 **adStatusErrorsOccurred** 。  
  
 在 **WillChangeField** 返回之前，将此参数设置为 **adStatusCancel** ，以请求取消挂起的操作。  
  
 在 **FieldChangeComplete** 返回之前，将此参数设置为 **adStatusUnwantedEvent** ，以防止后续通知。  
  
 *pRecordset*  
 **记录集** 对象。 发生此事件的 **记录集** 。  
  
## <a name="remarks"></a>备注  
 设置 [Value](./value-property-ado.md)属性并使用字段和值数组参数调用 [Update](./update-method.md)方法时，可能会发生 **WillChangeField** 或 **FieldChangeComplete** 事件。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)