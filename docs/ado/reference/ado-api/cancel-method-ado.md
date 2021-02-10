---
description: Cancel 方法 (ADO)
title: ) ADO (取消方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Cancel
- _Record::Cancel
- _Connection::Cancel
- Command25::Cancel
- _Stream::Cancel
helpviewer_keywords:
- Cancel method [ADO]
ms.assetid: e0db4e15-6787-41e2-8f13-9e9b524d620a
author: rothja
ms.author: jroth
ms.openlocfilehash: c33cedffcdd4eb9b738b952afaa060af9d99ff6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027483"
---
# <a name="cancel-method-ado"></a>Cancel 方法 (ADO)
取消执行挂起的异步方法调用。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.Cancel  
```  
  
## <a name="remarks"></a>备注  
 使用 **Cancel** 方法可终止异步方法调用的执行：即，使用 **adAsyncConnect**、 **adAsyncExecute** 或 **adAsyncFetch** 选项调用的方法。  
  
 下表显示了在特定类型的对象上使用 **Cancel** 方法时终止的任务。  
  
|如果 *对象* 为|对此方法的最后一次异步调用被终止|  
|----------------------|-------------------------------------------------------------|  
|[命令](./command-object-ado.md)|[执行](./execute-method-ado-command.md)|  
|[连接](./connection-object-ado.md)|[Execute](./execute-method-ado-connection.md) 或 [Open](./open-method-ado-connection.md)|  
|[记录](./record-object-ado.md)|[CopyRecord](./copyrecord-method-ado.md)、 [DeleteRecord](./deleterecord-method-ado.md)、 [MoveRecord](./moverecord-method-ado.md)或 [Open](./open-method-ado-record.md)|  
|[Recordset](./recordset-object-ado.md)|[打开](./open-method-ado-recordset.md)|  
|[Stream](./stream-object-ado.md)|[打开](./open-method-ado-stream.md)|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [命令对象 (ADO)](./command-object-ado.md)  
        [连接对象 (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录对象 (ADO)](./record-object-ado.md)  
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [流对象 (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [取消方法示例 (VB) ](./cancel-method-example-vb.md)   
 [取消方法示例 (VBScript) ](../rds-api/cancel-method-example-vbscript.md)   
 [取消方法示例 (VC + +) ](./cancel-method-example-vc.md)   
 [ (RDS) 的 Cancel 方法 ](../rds-api/cancel-method-rds.md)   
 [CancelBatch 方法 (ADO) ](./cancelbatch-method-ado.md)   
 [CancelUpdate 方法 (ADO) ](./cancelupdate-method-ado.md)   
 [CancelUpdate 方法 (RDS) ](../rds-api/cancelupdate-method-rds.md)   
 [ADO 命令 (执行方法) ](./execute-method-ado-command.md)   
 [ADO 连接 (执行方法) ](./execute-method-ado-connection.md)   
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [Open 方法（ADO 记录集）](./open-method-ado-recordset.md)