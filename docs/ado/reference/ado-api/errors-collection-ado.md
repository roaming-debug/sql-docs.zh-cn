---
description: 错误集合 (ADO)
title: ADO)  (收集错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Errors
- Connection15::get_Errors
- Connection15::GetErrors
helpviewer_keywords:
- Errors collection [ADO]
ms.assetid: 290819e1-7b39-4e1e-a93b-801257138b00
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f75d1a068a3e86ecb9e0d5b1e27afae6ac4a6e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171186"
---
# <a name="errors-collection-ado"></a>错误集合 (ADO)
包含为响应单个提供程序相关的失败而创建的所有 [错误](../../../ado/reference/ado-api/error-object.md) 对象。  
  
## <a name="remarks"></a>备注  
 涉及 ADO 对象的任何操作都可能会生成一个或多个提供程序错误。 发生每个错误时，可以将一个或多个 **错误** 对象放置在 [连接](../../../ado/reference/ado-api/connection-object-ado.md)对象的 **Errors** 集合中。 当另一个 ADO 操作生成错误时，将清除 **错误** 集合，并将一组新的 **错误** 对象放置在 **错误** 集合中。  
  
 每个 **错误** 对象都表示特定提供程序错误，而不是 ADO 错误。 ADO 错误将公开给运行时异常处理机制。 例如，在 Microsoft Visual Basic 中，出现特定于 ADO 的错误将触发 [onError](../../../ado/reference/rds-api/onerror-event-rds.md) 事件，并出现在 **Err** 对象中。  
  
 不生成错误的 ADO 操作对 **错误** 集合没有影响。 使用 [Clear](../../../ado/reference/ado-api/clear-method-ado.md) 方法手动清除 **错误** 集合。  
  
 **错误** 集合中的 **错误** 对象集描述了在单个语句中发生的所有错误。 通过枚举 **错误** 集合中的特定错误，你的错误处理例程可以更准确地确定错误的原因和来源，并采取适当的措施进行恢复。  
  
 某些属性和方法会返回 **错误** 集合中显示为 **错误** 对象的警告，但不会停止执行程序。 在对 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)对象调用 [Resync](../../../ado/reference/ado-api/resync-method.md)、 [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)或 [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md)方法之前，在 **连接** 对象上调用 [Open](../../../ado/reference/ado-api/open-method-ado-connection.md)方法，或在 **recordset** 对象上设置 [Filter](../../../ado/reference/ado-api/filter-property.md)属性，对 **错误** 集合调用 **Clear** 方法。 这样一来，您可以读取 **Errors** 集合的 [Count](../../../ado/reference/ado-api/count-property-ado.md)属性来测试返回的警告。  
  
> [!NOTE]
>  有关单个 ADO 操作如何生成多个错误的更详细说明，请参阅 **Error** 对象主题。  
  
 本部分包含以下主题。  
  
-   [错误集合属性、方法和事件](../../../ado/reference/ado-api/errors-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [Error 对象](../../../ado/reference/ado-api/error-object.md)   
 [附录 A：提供程序](../../../ado/guide/appendixes/appendix-a-providers.md)
