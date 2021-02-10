---
description: EndOfRecordset 事件 (ADO)
title: EndOfRecordset 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EndOfRecordset
- Recordset::EndOfRecordset
helpviewer_keywords:
- EndOfRecordset event [ADO]
ms.assetid: 475de5e2-f634-4954-9edf-0027a6ba38d6
author: rothja
ms.author: jroth
ms.openlocfilehash: 9f917dbcf9f89edd22a06a8b95726333ffb342b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025131"
---
# <a name="endofrecordset-event-ado"></a>EndOfRecordset 事件 (ADO)
尝试移到超过 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)末尾的行时，将调用 **EndOfRecordset** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
EndOfRecordset fMoreData, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>参数  
 *fMoreData*  
 一个 **VARIANT_BOOL** 值，如果设置为 VARIANT_TRUE，则指示已添加到 **记录集** 的行数。  
  
 *adStatus*  
 [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md)状态值。  
  
 调用 **EndOfRecordset** 时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** 。 如果此事件无法请求取消导致此事件的操作，则将其设置为 **adStatusCantDeny** 。  
  
 在 **EndOfRecordset** 返回之前，将此参数设置为 **adStatusUnwantedEvent** ，以防止后续通知。  
  
 *pRecordset*  
 **记录集** 对象。 发生此事件的 **记录集** 。  
  
## <a name="remarks"></a>备注  
 如果 [MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)操作失败，则可能发生 **EndOfRecordset** 事件。  
  
 当尝试移过 **记录集** 对象的末尾时，将调用此事件处理程序，可能是由于调用 **MoveNext**。 但是，在此事件中，你可以从数据库中检索更多记录，并将它们追加到 **记录集** 的末尾。 在这种情况下，将 *fMoreData* 设置为 VARIANT_TRUE，并从 **EndOfRecordset** 返回。 然后再次调用 **MoveNext** 以访问新检索到的记录。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../../ado/guide/data/ado-event-handler-summary.md)
