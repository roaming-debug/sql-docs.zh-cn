---
description: EventStatusEnum
title: EventStatusEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EventStatusEnum
helpviewer_keywords:
- EventStatusEnum enumeration [ADO]
ms.assetid: ebfd4cda-4017-4873-9d28-38b1c7db12a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 917e88ace8b1d2e0abdbe084ebaf2eb71191f093
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025025"
---
# <a name="eventstatusenum"></a>EventStatusEnum
指定事件的当前执行状态。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adStatusCancel**|4|请求取消导致事件发生的操作。|  
|**adStatusCantDeny**|3|指示操作无法请求取消挂起的操作。|  
|**adStatusErrorsOccurred**|2|指示导致事件的操作由于错误或错误而失败。|  
|**adStatusOK**|1|指示导致事件的操作成功。|  
|**adStatusUnwantedEvent**|5|在事件方法完成执行之前，阻止后续通知。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. EventStatus|  
|AdoEnums. EventStatus. CANTDENY|  
|AdoEnums. EventStatus. ERRORSOCCURRED|  
|AdoEnums. EventStatus. OK|  
|AdoEnums. EventStatus. UNWANTEDEVENT|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [BeginTransComplete、CommitTransComplete 和 RollbackTransComplete 事件 (ADO) ](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)  

        [ConnectComplete 和 Disconnect 事件 (ADO)](../../../ado/reference/ado-api/connectcomplete-and-disconnect-events-ado.md)  

        [EndOfRecordset 事件 (ADO)](../../../ado/reference/ado-api/endofrecordset-event-ado.md)  

        [ExecuteComplete 事件 (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)  
    :::column-end:::
    :::column:::
        [FetchComplete 事件 (ADO)](../../../ado/reference/ado-api/fetchcomplete-event-ado.md)  

        [InfoMessage 事件 (ADO)](../../../ado/reference/ado-api/infomessage-event-ado.md)  

        [WillChangeField 和 FieldChangeComplete 事件 (ADO)](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)  

        [WillChangeRecord 和 RecordChangeComplete 事件 (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)  
    :::column-end:::
    :::column:::
        [WillChangeRecordset 和 RecordsetChangeComplete 事件 (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)  

        [WillConnect 事件 (ADO)](../../../ado/reference/ado-api/willconnect-event-ado.md)  

        [WillExecute 事件 (ADO)](../../../ado/reference/ado-api/willexecute-event-ado.md)  

        [WillMove 和 MoveComplete 事件 (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)  
    :::column-end:::
:::row-end:::
