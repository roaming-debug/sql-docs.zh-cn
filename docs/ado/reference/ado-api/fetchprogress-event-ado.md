---
description: FetchProgress 事件 (ADO)
title: FetchProgress 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FetchProgress
- Recordset::FetchProgress
helpviewer_keywords:
- FetchProgress event [ADO]
ms.assetid: 301716fd-81fc-40eb-8a04-221ef7ab410e
author: rothja
ms.author: jroth
ms.openlocfilehash: fa10ddcef5adfd340d2cbd690ff54d8ab62f35ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034067"
---
# <a name="fetchprogress-event-ado"></a>FetchProgress 事件 (ADO)
在长时间异步操作期间会定期调用 **FetchProgress** 事件，以报告当前已检索到 [记录集中](../../../ado/reference/ado-api/recordset-object-ado.md)的行数。  
  
## <a name="syntax"></a>语法  
  
```  
  
FetchProgress Progress, MaxProgress, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>参数  
 *进度*  
 一个 **长整型** 值，指示提取操作当前检索到的记录数。  
  
 *MaxProgress*  
 一个 **长整型** 值，指示预期要检索的最大记录数。  
  
 *adStatus*  
 [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md)状态值。  
  
 *pRecordset*  
 作为要检索其记录的对象的 **记录集** 对象。  
  
## <a name="remarks"></a>备注  
 对子 **记录集** 使用 **FetchProgress** 时，请注意，*进度* 和 *MaxProgress* 参数值派生自基础 [游标服务](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)行集。 返回的值表示基础行集中的记录总数，而不仅仅是当前章节中的记录数。  
  
> [!NOTE]
>  若要将 **FetchProgress** 与 Microsoft Visual Basic 一起使用，需要 Visual Basic 6.0 或更高版本。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../../ado/guide/data/ado-event-handler-summary.md)
