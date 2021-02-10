---
description: 事件参数
title: 事件参数 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Error parameter [ADO]
- Object parameter [ADO]
- Status parameter [ADO]
- events [ADO], parameters
- Reason parameter [ADO]
- event parameters [ADO]
ms.assetid: bd5c5afa-d301-4899-acda-40f98a6afa4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f60d32ccd968c39da4fcea3978a46389e49ae49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033207"
---
# <a name="event-parameters"></a>事件参数
每个事件处理程序都有一个状态参数，用于控制事件处理程序。 对于完整事件，此参数还用于指示生成事件的操作是成功还是失败。 大多数完整事件还具有错误参数，以提供有关可能已发生的任何错误的信息，以及引用用于执行操作的 ADO 对象的一个或多个对象参数。 例如， [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md) 事件包含与事件关联的 **命令**、 **记录集** 和 **连接** 对象的对象参数。 在下面的 Microsoft® Visual Basic®示例中，可以看到 pCommand、pRecordset 和 pConnection 对象，这些对象表示 **Execute** 方法使用的 **命令**、**记录集** 和 **连接** 对象。  
  
```  
Private Sub connEvent_ExecuteComplete(ByVal RecordsAffected As Long, _  
     ByVal pError As ADODB.Error, _  
     adStatus As ADODB.EventStatusEnum, _  
     ByVal pCommand As ADODB.Command, _  
     ByVal pRecordset As ADODB.Recordset, _  
     ByVal pConnection As ADODB.Connection)  
```  
  
 除 **Error** 对象之外，将相同的参数传递给将事件。 这使你可以检查将在挂起的操作中使用的每个对象，并确定是否应允许该操作完成。  
  
 某些事件处理程序具有 *Reason* 参数，该参数提供有关事件发生原因的其他信息。 例如， **WillMove** 和 **MoveComplete** 事件可能是由于任何一个导航方法 (**MoveNext**、 **MovePrevious**，等等) 被调用，或者作为 requery 的结果。  
  
## <a name="status-parameter"></a>状态参数  
 调用事件处理程序例程后， *Status* 参数将设置为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**adStatusOK**|传递给和完成的事件。 此值表示导致事件的操作已成功完成。|  
|**adStatusErrorsOccurred**|仅传递到完成事件。 该值表示导致事件的操作未成功，或将事件取消了操作。 请检查 *错误* 参数以了解更多详细信息。|  
|**adStatusCantDeny**|传递给的只是事件。 此值表示操作无法由 "将" 事件取消。 必须执行此方法。|  
  
 如果在 "您的操作" 事件中确定操作应该继续，则将 *状态* 参数保持不变。 但只要传入状态参数未设置为 **adStatusCantDeny**，则可以通过将 *状态* 更改为 **adStatusCancel** 来取消挂起的操作。 执行此操作时，与操作关联的完整事件的 *Status* 参数将设置为 **adStatusErrorsOccurred**。 传递给完整事件的 **错误** 对象将包含值 **adErrOperationCancelled**。  
  
 如果不再想要处理事件，可以将 *状态* 设置为 **adStatusUnwantedEvent** ，应用程序将不再接收该事件的通知。 但请记住，某些事件可能会出于多种原因而引发。 在这种情况下，你必须为每个可能的原因指定 **adStatusUnwantedEvent** 。 例如，若要停止接收挂起的 **RecordChange** 事件的通知，必须在发生时将 **adRsnAddNew**、 **adRsnDelete**、 **adRsnUpdate**、 **adRsnUndoUpdate**、 **adRsnUndoAddNew**、 **adRsnUndoDelete** 和 **adRsnFirstChange** 的 *Status* 参数设置为 **adStatusUnwantedEvent** 。  
  
|值|说明|  
|-----------|-----------------|  
|**adStatusUnwantedEvent**|请求此事件处理程序没有收到进一步的通知。|  
|**adStatusCancel**|请求取消要发生的操作。|  
  
## <a name="error-parameter"></a>Error 参数  
 *错误* 参数是对 ADO [错误](../../reference/ado-api/error-object.md)对象的引用。 当 *Status* 参数设置为 **AdStatusErrorsOccurred** 时， **Error** 对象包含有关操作失败的原因的详细信息。 如果将与完整事件关联的 "事件" 事件已通过将 *Status* 参数设置为 **adStatusCancel** 而取消了该操作，则 error 对象始终设置为 **adErrOperationCancelled**。  
  
## <a name="object-parameter"></a>对象参数  
 每个事件都接收一个或多个对象，这些对象表示操作中涉及的对象。 例如， **ExecuteComplete** 事件接收 **命令** 对象、 **记录集** 对象和 **连接** 对象。  
  
## <a name="reason-parameter"></a>Reason 参数  
 *Reason* 参数 *adReason* 提供有关发生事件的原因的其他信息。 对于同一操作，具有 *adReason* 参数的事件可能会被调用多次，每次都不同。 例如，对要执行或撤消记录的插入、删除或修改操作调用 **WillChangeRecord** 事件处理程序。 如果只是出于特定原因而要处理事件发生，则可以使用 *adReason* 参数来筛选出不感兴趣的事件。 例如，如果你只想要在因添加记录而发生记录更改事件时进行处理，则可以使用如下所示的内容。  
  
```  
' BeginEventExampleVB01  
Private Sub rsTest_WillChangeRecord(ByVal adReason As ADODB.EventReasonEnum, ByVal cRecords As Long, adStatus As ADODB.EventStatusEnum, ByVal pRecordset As ADODB.Recordset)  
   If adReason = adRsnAddNew Then  
       ' Process event  
       '...  
   Else  
       ' Cancel event notification for all  
       ' other possible adReason values.  
       adStatus = adStatusUnwantedEvent  
   End If  
End Sub  
' EndEventExampleVB01  
```  
  
 在这种情况下，可能会因其他原因导致通知。 但是，对于每个原因只发生一次。 一旦每个原因发生一次通知，就会收到添加新记录的通知。  
  
 与此相反，您需要将 *adStatus* 设置为 **adStatusUnwantedEvent** ，以请求没有 **adReason** 参数的事件处理程序停止接收事件通知。  
  
## <a name="see-also"></a>另请参阅  
 [ADO 事件处理程序摘要](./ado-event-handler-summary.md)   
 [按语言的 ADO 事件实例化](./ado-event-instantiation-by-language.md)   
 [事件处理程序如何协同工作](./how-event-handlers-work-together.md)   
 [事件类型](./types-of-events.md)