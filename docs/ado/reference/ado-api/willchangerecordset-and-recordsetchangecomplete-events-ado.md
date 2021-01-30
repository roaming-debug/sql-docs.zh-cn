---
description: WillChangeRecordset 和 RecordsetChangeComplete 事件 (ADO)
title: WillChangeRecordset 和 RecordsetChangeComplete 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset::RecordsetChangeComplete
- RecordsetChangeComplete
- Recordset::WillChangeRecordset
- WillChangeRecordset
helpviewer_keywords:
- RecordsetChangeComplete event [ADO]
- WillChangeRecordset event [ADO]
ms.assetid: d5d44659-e0d9-46d9-a297-99c43555082f
author: rothja
ms.author: jroth
ms.openlocfilehash: 6246eaa8dbdd92236a891fa56189e104b594b64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172356"
---
# <a name="willchangerecordset-and-recordsetchangecomplete-events-ado"></a>WillChangeRecordset 和 RecordsetChangeComplete 事件 (ADO)
在挂起的操作更改 [记录集](./recordset-object-ado.md)之前调用 **WillChangeRecordset** 事件。 更改 **记录集** 后，将调用 **RecordsetChangeComplete** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
WillChangeRecordset adReason, adStatus, pRecordset  
RecordsetChangeComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>参数  
 *adReason*  
 一个 [EventReasonEnum](./eventreasonenum.md) 值，该值指定此事件的原因。 其值可以是 **adRsnRequery**、 **adRsnResynch**、 **adRsnClose**、 **adRsnOpen**。  
  
 *adStatus*  
 [EventStatusEnum](./eventstatusenum.md)状态值。  
  
 调用 **WillChangeRecordset** 时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** 。 如果此事件无法请求取消挂起操作，则将其设置为 **adStatusCantDeny** 。  
  
 调用 **RecordsetChangeComplete** 时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** ; 如果操作失败，则设置为 **adStatusErrorsOccurred** ; 如果已取消与先前接受的 **WillChangeRecordset** 事件关联的操作，则设置为 **adStatusCancel** 。  
  
 在 **WillChangeRecordset** 返回之前，将此参数设置为 **adStatusCancel** ，以请求取消挂起的操作或将此参数设置为 adStatusUnwantedEvent，以防止后续通知。  
  
 在 **WillChangeRecordset** 或 **RecordsetChangeComplete** 返回之前，将此参数设置为 **adStatusUnwantedEvent** ，以防止后续通知。  
  
 *pError*  
 一个 [错误](./error-object.md) 对象。 它描述了 *adStatus* 的值为 **adStatusErrorsOccurred** 时所发生的错误;否则，不会设置。  
  
 *pRecordset*  
 **记录集** 对象。 发生此事件的 **记录集** 。  
  
## <a name="remarks"></a>备注  
 **WillChangeRecordset** 或 **RecordsetChangeComplete** 事件可能是由 **记录集** [Requery](./requery-method.md)或 [Open](./open-method-ado-recordset.md)方法导致的。  
  
 如果提供程序不支持书签，则每次从提供程序检索新行时都会发生 **RecordsetChange** 事件通知。 此事件的频率取决于 **RecordsetCacheSize** 属性。  
  
 必须为每个可能的 **adReason** 值将 **adStatus** 参数设置为 **adStatusUnwantedEvent** ，以完全停止包含 **adReason** 参数的任何事件的事件通知。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)