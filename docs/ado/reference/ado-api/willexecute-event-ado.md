---
description: WillExecute 事件 (ADO)
title: WillExecute 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- WillExecute
- Connection::WillExecute
helpviewer_keywords:
- WillExecute event [ADO]
ms.assetid: dd755e46-f589-48a3-93a9-51ff998d44b5
author: rothja
ms.author: jroth
ms.openlocfilehash: cffc2e0a79ff4bffd83eadac20cf522df6d0c9cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172363"
---
# <a name="willexecute-event-ado"></a>WillExecute 事件 (ADO)
在连接上执行挂起命令之前，就会调用 **WillExecute** 事件。  
  
## <a name="syntax"></a>语法  
  
```  
  
WillExecute Source, CursorType, LockType, Options, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 包含 SQL 命令或存储过程名称的 **字符串** 。  
  
 *CursorType*  
 一个 [CursorTypeEnum](./cursortypeenum.md) ，包含要打开的 **记录集** 的游标类型。 使用此参数，可以在 **记录集**[打开方法 (ADO 记录集)](./open-method-ado-recordset.md) 操作期间将光标更改为任意类型。 对于任何其他操作， *CursorType* 将被忽略。  
  
 *LockType*  
 一个 [LockTypeEnum](./locktypeenum.md) ，其中包含将打开的 **记录集** 的锁类型。 使用此参数，可以在 **RecordsetOpen** 操作过程中将锁定更改为任何类型。 对于任何其他操作， *LockType* 将被忽略。  
  
 *选项*  
 **Long** 值，指示可用于执行命令或打开 **记录集** 的选项。  
  
 *adStatus*  
 在调用此事件时， [EventStatusEnum](./eventstatusenum.md) 状态值可能是 **adStatusCantDeny** 或 **adStatusOK** 。 如果它是 **adStatusCantDeny**，则此事件可能不会请求取消挂起的操作。  
  
 *pCommand*  
 此事件通知适用 [ (ADO) 对象的命令对象 ](./command-object-ado.md) 。  
  
 *pRecordset*  
 此事件通知适用 [ (ADO) 对象的记录集对象 ](./recordset-object-ado.md) 。  
  
 *pConnection*  
 此事件通知适用 [ (ADO) 对象的连接对象 ](./connection-object-ado.md) 。  
  
## <a name="remarks"></a>备注  
 由于连接，可能会发生 **WillExecute** 事件。  [执行方法 (Ado 连接)](./execute-method-ado-connection.md)， [执行方法 (ado 命令)](./execute-method-ado-command.md)，或 [Open 方法 (Ado Recordset)](./open-method-ado-recordset.md) 方法， *pConnection* 参数应始终包含对 **连接** 对象的有效引用。 如果事件是 **Connection.Exe很漂亮** 的，则 *pRecordset* 和 *PCommand* 参数将设置为 **Nothing**。 如果事件是由 **recordset** 引起的，则 *pRecordset* 参数将引用 **recordset** 对象并将 *pCommand* 参数设置为 **Nothing**。 如果事件是 **Command.Exe很漂亮** 的，则 *pCommand* 参数将引用 **Command** 对象并将 *pRecordset* 参数设置为 **Nothing**。  
  
 **WillExecute** 允许你检查和修改挂起的执行参数。 此事件可能会返回一个请求，要求取消挂起的命令。  
  
> [!NOTE]
>  如果 **命令** 的原始源是 CommandStream 属性指定的流 [ (ADO)](./commandstream-property-ado.md) 属性，则将新字符串分配给 **WillExecute**_source_ 参数将更改 **命令** 的源。 **CommandStream** 属性将被清除，并且将用新的源更新 [ (ADO) 属性的 CommandText 属性](./commandtext-property-ado.md)。 **CommandStream** 指定的原始流将被释放，并且无法访问。  
  
 如果新的源字符串的方言不同于对应) 到 **CommandStream** 的 [方言属性](./dialect-property.md) (属性的原始设置，则必须通过设置 *pCommand* 引用的命令对象的 **方言** 属性来指定正确的方言。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)   
 [连接对象 (ADO)](./connection-object-ado.md)