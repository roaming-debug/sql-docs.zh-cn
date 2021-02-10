---
description: FetchComplete 事件 (ADO)
title: FetchComplete 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- FetchComplete event [ADO]
ms.assetid: a28d3858-566c-468d-b070-d1de4339fbea
author: rothja
ms.author: jroth
ms.openlocfilehash: a6fa6f1f5bb4a0289807c2d3ffd62c63d85013cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024751"
---
# <a name="fetchcomplete-event-ado"></a>FetchComplete 事件 (ADO)
当长时间异步操作中的所有记录都已检索到 [记录集中](../../../ado/reference/ado-api/recordset-object-ado.md)后，将调用 **FetchComplete** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
FetchComplete pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>参数  
 *pError*  
 一个 [错误](../../../ado/reference/ado-api/error-object.md) 对象。 它描述了 **adStatus** 的值为 **adStatusErrorsOccurred** 时所发生的错误;否则，不会设置。  
  
 *adStatus*  
 [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md)状态值。 当调用此事件时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** ; 如果操作失败，则设置为 **adStatusErrorsOccurred** 。  
  
 在此事件返回之前，将此参数设置为 **adStatusUnwantedEvent** 以防止后续通知。  
  
 *pRecordset*  
 **记录集** 对象。 为其检索记录的对象。  
  
## <a name="remarks"></a>备注  
 若要将 **FetchComplete** 与 Microsoft Visual Basic 一起使用，需要 Visual Basic 6.0 或更高版本。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../../ado/guide/data/ado-event-handler-summary.md)
