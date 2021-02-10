---
description: InfoMessage 事件 (ADO)
title: InfoMessage 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::InfoMessage
- InfoMessage
helpviewer_keywords:
- InfoMessage event [ADO]
ms.assetid: 468c87dd-e3bc-4084-9941-94d10743d4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 68121629c35c6924304e81d9401dcbd53b51ddb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020638"
---
# <a name="infomessage-event-ado"></a>InfoMessage 事件 (ADO)
只要在 **ConnectionEvent** 操作过程中出现警告，就会调用 **InfoMessage** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
InfoMessage pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>参数  
 *pError*  
 一个 [错误](./error-object.md) 对象。 此参数包含返回的任何错误。 如果返回多个错误，则枚举 **错误** 集合以找到它们。  
  
 *adStatus*  
 [EventStatusEnum](./eventstatusenum.md)状态值。 如果出现警告， *adStatus* 将设置为 **AdStatusOK** ，而 *pError* 将包含警告。  
  
 在此事件返回之前，将此参数设置为 **adStatusUnwantedEvent** 以防止后续通知。  
  
 *pConnection*  
 [连接](./connection-object-ado.md)对象。 发生警告的连接。 例如，打开 **连接** 对象或对 **连接** 执行 [命令](./command-object-ado.md)时，可能会出现警告。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)   
 [连接对象 (ADO)](./connection-object-ado.md)